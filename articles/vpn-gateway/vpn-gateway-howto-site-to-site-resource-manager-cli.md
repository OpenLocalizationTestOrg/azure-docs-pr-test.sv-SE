---
title: "Ansluta din lokala nätverk tooan virtuella Azure-nätverket: plats-till-plats-VPN: CLI | Microsoft Docs"
description: "Steg toocreate en IPSec-anslutning från ditt lokala nätverk tooan virtuella Azure-nätverket via hello offentliga Internet. Dessa steg hjälper dig att skapa en plats-till-plats-anslutning med VPN-gateway med hjälp av CLI."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: c652cf2caf3928cdeb19d7dc329f6db101e5ed90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a><span data-ttu-id="a5dff-104">Skapa ett virtuellt nätverk med en VPN-anslutning från plats till plats med CLI</span><span class="sxs-lookup"><span data-stu-id="a5dff-104">Create a virtual network with a Site-to-Site VPN connection using CLI</span></span>

<span data-ttu-id="a5dff-105">Den här artikeln visar hur hello toouse Azure CLI toocreate plats-till-plats VPN-gateway-anslutningen från ditt lokala nätverk toohello VNet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-105">This article shows you how toouse hello Azure CLI toocreate a Site-to-Site VPN gateway connection from your on-premises network toohello VNet.</span></span> <span data-ttu-id="a5dff-106">hello stegen i den här artikeln gäller toohello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="a5dff-106">hello steps in this article apply toohello Resource Manager deployment model.</span></span> <span data-ttu-id="a5dff-107">Du kan också skapa den här konfigurationen med hjälp av en annan distributionsverktyget eller distributionsmodell genom att välja ett annat alternativ hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="a5dff-107">You can also create this configuration using a different deployment tool or deployment model by selecting a different option from hello following list:</span></span><br>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a5dff-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a5dff-108">Azure portal</span></span>](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="a5dff-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a5dff-109">PowerShell</span></span>](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [<span data-ttu-id="a5dff-110">CLI</span><span class="sxs-lookup"><span data-stu-id="a5dff-110">CLI</span></span>](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [<span data-ttu-id="a5dff-111">Azure Portal (klassisk)</span><span class="sxs-lookup"><span data-stu-id="a5dff-111">Azure portal (classic)</span></span>](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>


![Diagram över plats-till-plats-anslutning med VPN-gateway](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-diagram.png)

<span data-ttu-id="a5dff-113">En plats-till-plats VPN-gateway-anslutningen har använt tooconnect ditt lokala nätverk tooan virtuella Azure-nätverket via en IPsec/IKE (IKEv1 eller IKEv2) VPN-tunnel.</span><span class="sxs-lookup"><span data-stu-id="a5dff-113">A Site-to-Site VPN gateway connection is used tooconnect your on-premises network tooan Azure virtual network over an IPsec/IKE (IKEv1 or IKEv2) VPN tunnel.</span></span> <span data-ttu-id="a5dff-114">Den här typen av anslutning kräver en VPN-enhet som finns lokalt som har ett externt Internetriktade offentliga IP-adress som tilldelats tooit.</span><span class="sxs-lookup"><span data-stu-id="a5dff-114">This type of connection requires a VPN device located on-premises that has an externally facing public IP address assigned tooit.</span></span> <span data-ttu-id="a5dff-115">Mer information om VPN-gatewayer finns i [Om VPN-gateway](vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="a5dff-115">For more information about VPN gateways, see [About VPN gateway](vpn-gateway-about-vpngateways.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a5dff-116">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a5dff-116">Before you begin</span></span>

<span data-ttu-id="a5dff-117">Kontrollera att du har uppfyllt hello följande villkor innan du påbörjar konfiguration:</span><span class="sxs-lookup"><span data-stu-id="a5dff-117">Verify that you have met hello following criteria before beginning configuration:</span></span>

* <span data-ttu-id="a5dff-118">Kontrollera att du har en kompatibel VPN-enhet och någon som kan tooconfigure den.</span><span class="sxs-lookup"><span data-stu-id="a5dff-118">Make sure you have a compatible VPN device and someone who is able tooconfigure it.</span></span> <span data-ttu-id="a5dff-119">Se [Om VPN-enheter](vpn-gateway-about-vpn-devices.md) för mer information om kompatibla VPN-enheter och enhetskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="a5dff-119">For more information about compatible VPN devices and device configuration, see [About VPN Devices](vpn-gateway-about-vpn-devices.md).</span></span>
* <span data-ttu-id="a5dff-120">Kontrollera att du har en extern offentlig IPv4-adress för VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="a5dff-120">Verify that you have an externally facing public IPv4 address for your VPN device.</span></span> <span data-ttu-id="a5dff-121">Den här IP-adressen får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="a5dff-121">This IP address cannot be located behind a NAT.</span></span>
* <span data-ttu-id="a5dff-122">Om du är bekant med hello IP-adressintervall som finns i ditt lokala nätverk konfiguration måste du toocoordinate med någon som kan ge de detaljer du.</span><span class="sxs-lookup"><span data-stu-id="a5dff-122">If you are unfamiliar with hello IP address ranges located in your on-premises network configuration, you need toocoordinate with someone who can provide those details for you.</span></span> <span data-ttu-id="a5dff-123">När du skapar den här konfigurationen måste du ange hello IP-adressintervall adressprefix Azure dirigerar tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="a5dff-123">When you create this configuration, you must specify hello IP address range prefixes that Azure will route tooyour on-premises location.</span></span> <span data-ttu-id="a5dff-124">Ingen av hello undernät i ditt lokala nätverk kan över lap med hello virtuella undernät som du vill tooconnect till.</span><span class="sxs-lookup"><span data-stu-id="a5dff-124">None of hello subnets of your on-premises network can over lap with hello virtual network subnets that you want tooconnect to.</span></span>
* <span data-ttu-id="a5dff-125">Kontrollera att du har installerat senaste versionen av hello CLI-kommandona (2.0 eller senare).</span><span class="sxs-lookup"><span data-stu-id="a5dff-125">Verify that you have installed latest version of hello CLI commands (2.0 or later).</span></span> <span data-ttu-id="a5dff-126">Information om hur du installerar hello CLI-kommandon finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli) och [Kom igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a5dff-126">For information about installing hello CLI commands, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli) and [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli).</span></span>

### <span data-ttu-id="a5dff-127"><a name="example"></a>Exempelvärden</span><span class="sxs-lookup"><span data-stu-id="a5dff-127"><a name="example"></a>Example values</span></span>

<span data-ttu-id="a5dff-128">Du kan använda följande värden toocreate en testmiljö hello eller referera toothese värden toobetter förstå hello exemplen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="a5dff-128">You can use hello following values toocreate a test environment, or refer toothese values toobetter understand hello examples in this article:</span></span>

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.11.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.11.0.0/24 
GatewaySubnet           = 10.11.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <span data-ttu-id="a5dff-129"><a name="Login"></a>1. Ansluta tooyour prenumeration</span><span class="sxs-lookup"><span data-stu-id="a5dff-129"><a name="Login"></a>1. Connect tooyour subscription</span></span>

[!INCLUDE [CLI login](../../includes/vpn-gateway-cli-login-include.md)]

## <span data-ttu-id="a5dff-130"><a name="rg"></a>2. Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a5dff-130"><a name="rg"></a>2. Create a resource group</span></span>

<span data-ttu-id="a5dff-131">hello skapas följande exempel en resursgrupp med namnet 'TestRG1' hello 'eastus' plats.</span><span class="sxs-lookup"><span data-stu-id="a5dff-131">hello following example creates a resource group named 'TestRG1' in hello 'eastus' location.</span></span> <span data-ttu-id="a5dff-132">Om du redan har en resursgrupp i hello region som du vill toocreate ditt VNet, kan du använda den i stället.</span><span class="sxs-lookup"><span data-stu-id="a5dff-132">If you already have a resource group in hello region that you want toocreate your VNet, you can use that one instead.</span></span>

```azurecli
az group create --name TestRG1 --location eastus
```

## <span data-ttu-id="a5dff-133"><a name="VNet"></a>3. Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="a5dff-133"><a name="VNet"></a>3. Create a virtual network</span></span>

<span data-ttu-id="a5dff-134">Om du inte redan har ett virtuellt nätverk, skapar du en med hjälp av hello [az network vnet skapa](/cli/azure/network/vnet#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a5dff-134">If you don't already have a virtual network, create one using hello [az network vnet create](/cli/azure/network/vnet#create) command.</span></span> <span data-ttu-id="a5dff-135">När du skapar ett virtuellt nätverk, se till att hello adressutrymmen som du anger inte överlappa hello-adressutrymmen som finns på ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="a5dff-135">When creating a virtual network, make sure that hello address spaces you specify don't overlap any of hello address spaces that you have on your on-premises network.</span></span>

<span data-ttu-id="a5dff-136">hello skapas följande exempel ett virtuellt nätverk med namnet 'TestVNet1' och ett undernät, 'Undernät1'.</span><span class="sxs-lookup"><span data-stu-id="a5dff-136">hello following example creates a virtual network named 'TestVNet1' and a subnet, 'Subnet1'.</span></span>

```azurecli
az network vnet create --name TestVNet1 --resource-group TestRG1 --address-prefix 10.11.0.0/16 --location eastus --subnet-name Subnet1 --subnet-prefix 10.11.0.0/24
```

## <span data-ttu-id="a5dff-137">4. <a name="gwsub"></a>Skapa hello gateway-undernät</span><span class="sxs-lookup"><span data-stu-id="a5dff-137">4. <a name="gwsub"></a>Create hello gateway subnet</span></span>

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

<span data-ttu-id="a5dff-138">För den här konfigurationen behöver du också ett gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="a5dff-138">For this configuration, you also need a gateway subnet.</span></span> <span data-ttu-id="a5dff-139">hello virtuell nätverksgateway använder ett gateway-undernät som innehåller hello IP-adresser som används av hello VPN gateway-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a5dff-139">hello virtual network gateway uses a gateway subnet that contains hello IP addresses that are used by hello VPN gateway services.</span></span> <span data-ttu-id="a5dff-140">När du skapar ett gateway-undernät måste det få namnet ”GatewaySubnet”.</span><span class="sxs-lookup"><span data-stu-id="a5dff-140">When you create a gateway subnet, it must be named 'GatewaySubnet'.</span></span> <span data-ttu-id="a5dff-141">Om du ger det något annat namn kan du skapa undernätet, men Azure behandlar det inte som ett gateway-undernät.</span><span class="sxs-lookup"><span data-stu-id="a5dff-141">If you name it something else, you create a subnet, but Azure won't treat it as a gateway subnet.</span></span>

<span data-ttu-id="a5dff-142">hello storleken på hello gateway-undernätet som du anger beror på hello VPN gateway-konfigurationen som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="a5dff-142">hello size of hello gateway subnet that you specify depends on hello VPN gateway configuration that you want toocreate.</span></span> <span data-ttu-id="a5dff-143">Det är möjligt toocreate ett gatewayundernät så liten som /29, rekommenderar vi att du skapar ett större undernät som innehåller flera adresser genom att välja minst/27 eller /28.</span><span class="sxs-lookup"><span data-stu-id="a5dff-143">While it is possible toocreate a gateway subnet as small as /29, we recommend that you create a larger subnet that includes more addresses by selecting /27 or /28.</span></span> <span data-ttu-id="a5dff-144">Med ett större gateway-undernät tillåter tillräckligt med IP-adresser tooaccommodate framtida konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a5dff-144">Using a larger gateway subnet allows for enough IP addresses tooaccommodate possible future configurations.</span></span>

<span data-ttu-id="a5dff-145">Använd hello [az undernät för virtuellt nätverk skapa](/cli/azure/network/vnet/subnet#create) kommandot toocreate hello gateway-undernätet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-145">Use hello [az network vnet subnet create](/cli/azure/network/vnet/subnet#create) command toocreate hello gateway subnet.</span></span>

```azurecli
az network vnet subnet create --address-prefix 10.11.255.0/27 --name GatewaySubnet --resource-group TestRG1 --vnet-name TestVNet1
```

## <span data-ttu-id="a5dff-146"><a name="localnet"></a>5. Skapa hello lokal nätverksgateway</span><span class="sxs-lookup"><span data-stu-id="a5dff-146"><a name="localnet"></a>5. Create hello local network gateway</span></span>

<span data-ttu-id="a5dff-147">hello lokal nätverksgateway refererar vanligtvis tooyour lokal plats.</span><span class="sxs-lookup"><span data-stu-id="a5dff-147">hello local network gateway typically refers tooyour on-premises location.</span></span> <span data-ttu-id="a5dff-148">Du namnge hello plats som Azure kan se tooit och sedan ange hello IP-adress för hello lokala VPN-enhet toowhich ska du skapa en anslutning.</span><span class="sxs-lookup"><span data-stu-id="a5dff-148">You give hello site a name by which Azure can refer tooit, then specify hello IP address of hello on-premises VPN device toowhich you will create a connection.</span></span> <span data-ttu-id="a5dff-149">Du kan också ange hello IP-adressprefix som vidarebefordras via hello VPN-gateway toohello VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-149">You also specify hello IP address prefixes that will be routed through hello VPN gateway toohello VPN device.</span></span> <span data-ttu-id="a5dff-150">hello-adressprefix som du anger är hello prefix finns i ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="a5dff-150">hello address prefixes you specify are hello prefixes located on your on-premises network.</span></span> <span data-ttu-id="a5dff-151">Om ditt lokala nätverk ändras kan uppdatera du enkelt hello prefix.</span><span class="sxs-lookup"><span data-stu-id="a5dff-151">If your on-premises network changes, you can easily update hello prefixes.</span></span>

<span data-ttu-id="a5dff-152">Använd hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="a5dff-152">Use hello following values:</span></span>

* <span data-ttu-id="a5dff-153">Hej *--gateway-ip-adress* är hello IP-adressen för din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-153">hello *--gateway-ip-address* is hello IP address of your on-premises VPN device.</span></span> <span data-ttu-id="a5dff-154">VPN-enheten får inte finnas bakom en NAT.</span><span class="sxs-lookup"><span data-stu-id="a5dff-154">Your VPN device cannot be located behind a NAT.</span></span>
* <span data-ttu-id="a5dff-155">Hej *--lokala adressprefix* är din lokala adressutrymmen.</span><span class="sxs-lookup"><span data-stu-id="a5dff-155">hello *--local-address-prefixes* are your on-premises address spaces.</span></span>

<span data-ttu-id="a5dff-156">Använd hello [az lokala-nätverksgateway skapa](/cli/azure/network/local-gateway#create) kommandot tooadd en lokal nätverksgateway med flera adressprefix:</span><span class="sxs-lookup"><span data-stu-id="a5dff-156">Use hello [az network local-gateway create](/cli/azure/network/local-gateway#create) command tooadd a local network gateway with multiple address prefixes:</span></span>

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 --resource-group TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <span data-ttu-id="a5dff-157"><a name="PublicIP"></a>6. Begär en offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="a5dff-157"><a name="PublicIP"></a>6. Request a Public IP address</span></span>

<span data-ttu-id="a5dff-158">En VPN-gateway måste ha en offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a5dff-158">A VPN gateway must have a Public IP address.</span></span> <span data-ttu-id="a5dff-159">Du först begära hello IP-adressresurs och läser sedan jobbhistorikposten tooit när du skapar din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="a5dff-159">You first request hello IP address resource, and then refer tooit when creating your virtual network gateway.</span></span> <span data-ttu-id="a5dff-160">hello IP-adressen tilldelas dynamiskt toohello resurs när hello VPN-gateway har skapats.</span><span class="sxs-lookup"><span data-stu-id="a5dff-160">hello IP address is dynamically assigned toohello resource when hello VPN gateway is created.</span></span> <span data-ttu-id="a5dff-161">VPN-gateway stöder för närvarande endast *dynamisk* offentlig IP-adressallokering.</span><span class="sxs-lookup"><span data-stu-id="a5dff-161">VPN Gateway currently only supports *Dynamic* Public IP address allocation.</span></span> <span data-ttu-id="a5dff-162">Du kan inte begära en statisk offentlig IP-adresstilldelning.</span><span class="sxs-lookup"><span data-stu-id="a5dff-162">You cannot request a Static Public IP address assignment.</span></span> <span data-ttu-id="a5dff-163">Detta innebär dock inte att hello IP-adressen ändras när den har tilldelats tooyour VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="a5dff-163">However, this does not mean that hello IP address changes after it has been assigned tooyour VPN gateway.</span></span> <span data-ttu-id="a5dff-164">hello endast tid hello offentliga IP-adressändringarna är när hello gateway bort och återskapas.</span><span class="sxs-lookup"><span data-stu-id="a5dff-164">hello only time hello Public IP address changes is when hello gateway is deleted and re-created.</span></span> <span data-ttu-id="a5dff-165">Den ändras inte vid storleksändring, återställning eller annat internt underhåll/uppgraderingar av din VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="a5dff-165">It doesn't change across resizing, resetting, or other internal maintenance/upgrades of your VPN gateway.</span></span>

<span data-ttu-id="a5dff-166">Använd hello [az nätverket offentliga IP-skapa](/cli/azure/network/public-ip#create) kommandot toorequest en dynamiska offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="a5dff-166">Use hello [az network public-ip create](/cli/azure/network/public-ip#create) command toorequest a Dynamic Public IP address.</span></span>

```azurecli
az network public-ip create --name VNet1GWIP --resource-group TestRG1 --allocation-method Dynamic
```

## <span data-ttu-id="a5dff-167"><a name="CreateGateway"></a>7. Skapa hello VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="a5dff-167"><a name="CreateGateway"></a>7. Create hello VPN gateway</span></span>

<span data-ttu-id="a5dff-168">Skapa hello VPN-gateway för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a5dff-168">Create hello virtual network VPN gateway.</span></span> <span data-ttu-id="a5dff-169">Skapa en VPN-gateway kan ta upp too45 minuter eller mer toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a5dff-169">Creating a VPN gateway can take up too45 minutes or more toocomplete.</span></span>

<span data-ttu-id="a5dff-170">Använd hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="a5dff-170">Use hello following values:</span></span>

* <span data-ttu-id="a5dff-171">Hej *--gateway-typen* för plats-till-plats konfigurationen är *Vpn*.</span><span class="sxs-lookup"><span data-stu-id="a5dff-171">hello *--gateway-type* for a Site-to-Site configuration is *Vpn*.</span></span> <span data-ttu-id="a5dff-172">hello gateway-typ är alltid specifika toohello konfiguration som du implementerar.</span><span class="sxs-lookup"><span data-stu-id="a5dff-172">hello gateway type is always specific toohello configuration that you are implementing.</span></span> <span data-ttu-id="a5dff-173">Se [Gatewaytyper](vpn-gateway-about-vpn-gateway-settings.md#gwtype) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a5dff-173">For more information, see [Gateway types](vpn-gateway-about-vpn-gateway-settings.md#gwtype).</span></span>
* <span data-ttu-id="a5dff-174">Hej *--vpn-typ* kan vara *RouteBased* (kallas tooas en dynamisk Gateway i viss dokumentation) eller *PolicyBased* (kallas tooas en statisk Gateway i vissa dokumentationen).</span><span class="sxs-lookup"><span data-stu-id="a5dff-174">hello *--vpn-type* can be *RouteBased* (referred tooas a Dynamic Gateway in some documentation), or *PolicyBased* (referred tooas a Static Gateway in some documentation).</span></span> <span data-ttu-id="a5dff-175">hello-inställningen är särskilda toorequirements för hello-enhet som du ansluter till.</span><span class="sxs-lookup"><span data-stu-id="a5dff-175">hello setting is specific toorequirements of hello device that you are connecting to.</span></span> <span data-ttu-id="a5dff-176">Mer information om VPN-gatewaytyper finns i [Om konfigurationsinställningar för VPN-gateway](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span><span class="sxs-lookup"><span data-stu-id="a5dff-176">For more information about VPN gateway types, see [About VPN Gateway configuration settings](vpn-gateway-about-vpn-gateway-settings.md#vpntype).</span></span>
* <span data-ttu-id="a5dff-177">Välj hello Gateway-SKU som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="a5dff-177">Select hello Gateway SKU that you want toouse.</span></span> <span data-ttu-id="a5dff-178">Det finns konfigurationsbegränsningar för vissa SKU:er.</span><span class="sxs-lookup"><span data-stu-id="a5dff-178">There are configuration limitations for certain SKUs.</span></span> <span data-ttu-id="a5dff-179">Se [Gateway SKU:er](vpn-gateway-about-vpn-gateway-settings.md#gwsku) för mer information.</span><span class="sxs-lookup"><span data-stu-id="a5dff-179">For more information, see [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span>

<span data-ttu-id="a5dff-180">Skapa hello VPN-gateway med hello [az vnet-nätverksgateway skapa](/cli/azure/network/vnet-gateway#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a5dff-180">Create hello VPN gateway using hello [az network vnet-gateway create](/cli/azure/network/vnet-gateway#create) command.</span></span> <span data-ttu-id="a5dff-181">Om du kör det här kommandot med hello – inget - wait-parameter, kan du inte ser några feedback eller utdata.</span><span class="sxs-lookup"><span data-stu-id="a5dff-181">If you run this command using hello '--no-wait' parameter, you don't see any feedback or output.</span></span> <span data-ttu-id="a5dff-182">Den här parametern kan hello gateway toocreate i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="a5dff-182">This parameter allows hello gateway toocreate in hello background.</span></span> <span data-ttu-id="a5dff-183">Det tar cirka 45 minuter toocreate en gateway.</span><span class="sxs-lookup"><span data-stu-id="a5dff-183">It takes around 45 minutes toocreate a gateway.</span></span>

```azurecli
az network vnet-gateway create --name VNet1GW --public-ip-address VNet1GWIP --resource-group TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku VpnGw1 --no-wait 
```

## <span data-ttu-id="a5dff-184"><a name="VPNDevice"></a>8. Konfigurera din VPN-enhet</span><span class="sxs-lookup"><span data-stu-id="a5dff-184"><a name="VPNDevice"></a>8. Configure your VPN device</span></span>

<span data-ttu-id="a5dff-185">Plats-till-platsanslutningar tooan lokalt nätverk kräver en VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-185">Site-to-Site connections tooan on-premises network require a VPN device.</span></span> <span data-ttu-id="a5dff-186">I det här steget konfigurerar du VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="a5dff-186">In this step, you configure your VPN device.</span></span> <span data-ttu-id="a5dff-187">När du konfigurerar VPN-enhet behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a5dff-187">When configuring your VPN device, you need hello following:</span></span>

- <span data-ttu-id="a5dff-188">En delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="a5dff-188">A shared key.</span></span> <span data-ttu-id="a5dff-189">Detta är hello samma delade nyckel som du anger när du skapar din plats-till-plats VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="a5dff-189">This is hello same shared key that you specify when creating your Site-to-Site VPN connection.</span></span> <span data-ttu-id="a5dff-190">I vårt exempel använder vi en enkel delad nyckel.</span><span class="sxs-lookup"><span data-stu-id="a5dff-190">In our examples, we use a basic shared key.</span></span> <span data-ttu-id="a5dff-191">Vi rekommenderar att du generera en mer komplex viktiga toouse.</span><span class="sxs-lookup"><span data-stu-id="a5dff-191">We recommend that you generate a more complex key toouse.</span></span>
- <span data-ttu-id="a5dff-192">hello offentliga IP-adressen för din virtuella nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="a5dff-192">hello Public IP address of your virtual network gateway.</span></span> <span data-ttu-id="a5dff-193">Du kan visa hello offentliga IP-adressen med hjälp av hello Azure-portalen, PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="a5dff-193">You can view hello public IP address by using hello Azure portal, PowerShell, or CLI.</span></span> <span data-ttu-id="a5dff-194">toofind hello offentliga IP-adressen för din virtuella nätverksgateway, Använd hello [az offentliga ip-lista över](/cli/azure/network/public-ip#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="a5dff-194">toofind hello public IP address of your virtual network gateway, use hello [az network public-ip list](/cli/azure/network/public-ip#list) command.</span></span> <span data-ttu-id="a5dff-195">För att enkelt läsa är hello utdata formaterad toodisplay hello lista över offentliga IP-adresser i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="a5dff-195">For easy reading, hello output is formatted toodisplay hello list of public IPs in table format.</span></span>

  ```azurecli
  az network public-ip list --resource-group TestRG1 --output table
  ```


[!INCLUDE [Configure VPN device](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]


## <span data-ttu-id="a5dff-196"><a name="CreateConnection"></a>9. Skapa hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="a5dff-196"><a name="CreateConnection"></a>9. Create hello VPN connection</span></span>

<span data-ttu-id="a5dff-197">Skapa hello plats-till-plats VPN-anslutning mellan din virtuella nätverksgateway och din lokala VPN-enhet.</span><span class="sxs-lookup"><span data-stu-id="a5dff-197">Create hello Site-to-Site VPN connection between your virtual network gateway and your on-premises VPN device.</span></span> <span data-ttu-id="a5dff-198">Betala per närmare toohello delade nyckelvärde, vilket måste matcha hello konfigurerade delade nyckelvärdet för VPN-enheten.</span><span class="sxs-lookup"><span data-stu-id="a5dff-198">Pay particular attention toohello shared key value, which must match hello configured shared key value for your VPN device.</span></span>

<span data-ttu-id="a5dff-199">Skapa hello anslutning med hello [az vpn-nätverksanslutning skapa](/cli/azure/network/vpn-connection#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a5dff-199">Create hello connection using hello [az network vpn-connection create](/cli/azure/network/vpn-connection#create) command.</span></span>

```azurecli
az network vpn-connection create --name VNet1toSite2 -resource-group TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

<span data-ttu-id="a5dff-200">Efter en kort när hello upprättas anslutning.</span><span class="sxs-lookup"><span data-stu-id="a5dff-200">After a short while, hello connection will be established.</span></span>

## <span data-ttu-id="a5dff-201"><a name="toverify"></a>10. Kontrollera hello VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="a5dff-201"><a name="toverify"></a>10. Verify hello VPN connection</span></span>

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)]

<span data-ttu-id="a5dff-202">Om du vill toouse en annan metod tooverify din anslutning finns [verifiera en anslutning för VPN-Gateway](vpn-gateway-verify-connection-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a5dff-202">If you want toouse another method tooverify your connection, see [Verify a VPN Gateway connection](vpn-gateway-verify-connection-resource-manager.md).</span></span>

## <span data-ttu-id="a5dff-203"><a name="connectVM"></a>tooconnect tooa virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a5dff-203"><a name="connectVM"></a>tooconnect tooa virtual machine</span></span>

[!INCLUDE [Connect tooa VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <span data-ttu-id="a5dff-204"><a name="tasks"></a>Vanliga åtgärder</span><span class="sxs-lookup"><span data-stu-id="a5dff-204"><a name="tasks"></a>Common tasks</span></span>

<span data-ttu-id="a5dff-205">Det här avsnittet tar upp vanliga kommandon som är användbara när du arbetar med plats-till-plats-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="a5dff-205">This section contains common commands that are helpful when working with site-to-site configurations.</span></span> <span data-ttu-id="a5dff-206">Hello fullständig lista över nätverk CLI-kommandon, se [Azure CLI - nätverk](/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="a5dff-206">For hello full list of CLI networking commands, see [Azure CLI - Networking](/cli/azure/network).</span></span>

[!INCLUDE [local network gateway common tasks](../../includes/vpn-gateway-common-tasks-cli-include.md)]

## <a name="next-steps"></a><span data-ttu-id="a5dff-207">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5dff-207">Next steps</span></span>

* <span data-ttu-id="a5dff-208">När anslutningen är klar kan du lägga till virtuella datorer tooyour virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a5dff-208">Once your connection is complete, you can add virtual machines tooyour virtual networks.</span></span> <span data-ttu-id="a5dff-209">Mer information finns i [Virtuella datorer](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span><span class="sxs-lookup"><span data-stu-id="a5dff-209">For more information, see [Virtual Machines](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).</span></span>
* <span data-ttu-id="a5dff-210">Information om BGP finns hello [BGP översikt](vpn-gateway-bgp-overview.md) och [hur tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a5dff-210">For information about BGP, see hello [BGP Overview](vpn-gateway-bgp-overview.md) and [How tooconfigure BGP](vpn-gateway-bgp-resource-manager-ps.md).</span></span>
* <span data-ttu-id="a5dff-211">Information om tvingad tunneltrafik finns i [Om forcerade tunnlar](vpn-gateway-forced-tunneling-rm.md).</span><span class="sxs-lookup"><span data-stu-id="a5dff-211">For information about Forced Tunneling, see [About Forced Tunneling](vpn-gateway-forced-tunneling-rm.md).</span></span>
* <span data-ttu-id="a5dff-212">Mer information om aktiv-aktiv-anslutningar med hög tillgänglighet finns i [Anslutning med hög tillgänglighet på flera platser och VNet-till-VNet-anslutning](vpn-gateway-highlyavailable.md).</span><span class="sxs-lookup"><span data-stu-id="a5dff-212">For information about Highly Available Active-Active connections, see [Highly Available cross-premises and VNet-to-VNet connectivity](vpn-gateway-highlyavailable.md).</span></span>
* <span data-ttu-id="a5dff-213">Om du vill ha en lista över Azure CLI-nätverkskommandon finns det i [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span><span class="sxs-lookup"><span data-stu-id="a5dff-213">For a list of networking Azure CLI commands, see [Azure CLI](https://docs.microsoft.com/cli/azure/network).</span></span>