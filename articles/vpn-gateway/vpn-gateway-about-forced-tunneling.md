---
title: "Konfigurera Tvingad tunneltrafik för Azure plats-till-plats-anslutningar: klassiska | Microsoft Docs"
description: Hur tooredirect eller 'tvinga' alla Internet-bunden trafik tillbaka tooyour lokal plats.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 5c0177f1-540c-4474-9b80-f541fa44240b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 35b3a9ea370f9f962572629a69cc9aed16a87837
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-forced-tunneling-using-hello-classic-deployment-model"></a><span data-ttu-id="e7e6d-103">Konfigurera Tvingad tunneling med hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="e7e6d-103">Configure forced tunneling using hello classic deployment model</span></span>

<span data-ttu-id="e7e6d-104">Tvingad tunneltrafik kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka tooyour lokal plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back tooyour on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="e7e6d-105">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="e7e6d-106">Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut toohello Internet, utan hello alternativet tooallow du tooinspect eller audit hello-trafik.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out toohello Internet, without hello option tooallow you tooinspect or audit hello traffic.</span></span> <span data-ttu-id="e7e6d-107">Obehörig Internetåtkomst kan leda tooinformation avslöjas eller andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-107">Unauthorized Internet access can potentially lead tooinformation disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="e7e6d-108">Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-108">This article walks you through configuring forced tunneling for virtual networks created using hello classic deployment model.</span></span> <span data-ttu-id="e7e6d-109">Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-109">Forced tunneling can be configured by using PowerShell, not through hello portal.</span></span> <span data-ttu-id="e7e6d-110">Om du vill tooconfigure Tvingad tunneltrafik för hello Resource Manager-modellen, Välj klassiska artikel hello följande listrutan:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-110">If you want tooconfigure forced tunneling for hello Resource Manager deployment model, select classic article from hello following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e7e6d-111">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="e7e6d-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="e7e6d-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e7e6d-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="e7e6d-113">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="e7e6d-113">Requirements and considerations</span></span>
<span data-ttu-id="e7e6d-114">Tvingad tunneling i Azure konfigureras via virtuella nätverk användardefinierade vägar (UDR).</span><span class="sxs-lookup"><span data-stu-id="e7e6d-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="e7e6d-115">Omdirigera trafik tooan lokal plats uttrycks som en standardväg toohello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-115">Redirecting traffic tooan on-premises site is expressed as a Default Route toohello Azure VPN gateway.</span></span> <span data-ttu-id="e7e6d-116">hello innehåller följande avsnitt hello aktuella begränsning av hello routningstabellen och vägar för ett Azure Virtual Network:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-116">hello following section lists hello current limitation of hello routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="e7e6d-117">Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="e7e6d-118">routningstabellen för hello system har hello följande tre grupper av vägar:</span><span class="sxs-lookup"><span data-stu-id="e7e6d-118">hello system routing table has hello following three groups of routes:</span></span>

  * <span data-ttu-id="e7e6d-119">**Lokal VNet vägar:** direkt toohello mål virtuella datorer i hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-119">**Local VNet routes:** Directly toohello destination VMs in hello same virtual network.</span></span>
  * <span data-ttu-id="e7e6d-120">**Lokala vägar:** toohello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-120">**On-premises routes:** toohello Azure VPN gateway.</span></span>
  * <span data-ttu-id="e7e6d-121">**Standardvägen:** direkt toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-121">**Default route:** Directly toohello Internet.</span></span> <span data-ttu-id="e7e6d-122">Paket avsedda toohello privata IP-adresser som inte omfattas av hello föregående två vägar kommer att tas bort.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-122">Packets destined toohello private IP addresses not covered by hello previous two routes will be dropped.</span></span>
* <span data-ttu-id="e7e6d-123">Hello versionen av användardefinierade vägar kan du skapa en routning tabell tooadd en standardväg och sedan associerar hello routning tabell tooyour VNet adressundernät tooenable Tvingad tunneltrafik på dessa undernät.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-123">With hello release of user-defined routes, you can create a routing table tooadd a default route, and then associate hello routing table tooyour VNet subnet(s) tooenable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="e7e6d-124">Du måste tooset en ”standardwebbplats” bland hello lokala mellan lokala platser anslutna toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-124">You need tooset a "default site" among hello cross-premises local sites connected toohello virtual network.</span></span>
* <span data-ttu-id="e7e6d-125">Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en dynamisk routning VPN-gateway (inte en statisk gateway).</span><span class="sxs-lookup"><span data-stu-id="e7e6d-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="e7e6d-126">ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen men i stället har aktiverats genom att annonsera en standardväg via hello ExpressRoute BGP peeringsessioner.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via hello ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="e7e6d-127">Se hello [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-127">Please see hello [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="e7e6d-128">Konfigurationsöversikt</span><span class="sxs-lookup"><span data-stu-id="e7e6d-128">Configuration overview</span></span>
<span data-ttu-id="e7e6d-129">I följande exempel hello, tunneldata hello klientdel undernät inte tvingas.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-129">In hello following example, hello Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="e7e6d-130">hello arbetsbelastningar i hello klientdel undernät kan fortsätta tooaccept och svara toocustomer begäranden från hello Internet direkt.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-130">hello workloads in hello Frontend subnet can continue tooaccept and respond toocustomer requests from hello Internet directly.</span></span> <span data-ttu-id="e7e6d-131">hello halva nivå och Backend-undernät tvingas tunnel.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-131">hello Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="e7e6d-132">Alla utgående anslutningar från dessa två undernät toohello Internet blir framtvingad eller omdirigerade tillbaka tooan lokal plats via en hello S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-132">Any outbound connections from these two subnets toohello Internet will be forced or redirected back tooan on-premises site via one of hello S2S VPN tunnels.</span></span>

<span data-ttu-id="e7e6d-133">Detta ger dig toorestrict och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan tooenable flernivåtjänst arkitekturen krävs.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-133">This allows you toorestrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing tooenable your multi-tier service architecture required.</span></span> <span data-ttu-id="e7e6d-134">Du kan också använda Tvingad tunneltrafik toohello hela virtuella nätverk om det finns ingen Internet-riktade arbetsbelastningar i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-134">You also can apply forced tunneling toohello entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Forcerade tunnlar](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="e7e6d-136">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="e7e6d-136">Before you begin</span></span>
<span data-ttu-id="e7e6d-137">Kontrollera att du har följande objekt innan du börjar configuration hello.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-137">Verify that you have hello following items before beginning configuration.</span></span>

* <span data-ttu-id="e7e6d-138">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-138">An Azure subscription.</span></span> <span data-ttu-id="e7e6d-139">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e7e6d-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="e7e6d-140">Konfigurerade virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-140">A configured virtual network.</span></span> 
* <span data-ttu-id="e7e6d-141">hello senaste versionen av hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-141">hello latest version of hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="e7e6d-142">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information om hur du installerar hello PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-142">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for more information about installing hello PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="e7e6d-143">Konfigurera tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="e7e6d-143">Configure forced tunneling</span></span>
<span data-ttu-id="e7e6d-144">hello nedan hjälper dig att ange Tvingad tunneling för ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-144">hello following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="e7e6d-145">hello konfigurationssteg motsvarar toohello VNet nätverk konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-145">hello configuration steps correspond toohello VNet network configuration file.</span></span>

```
<VirtualNetworkSite name="MultiTier-VNet" Location="North Europe">
     <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="DefaultSiteHQ">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch1">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch2">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Branch3">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
        </Gateway>
      </VirtualNetworkSite>
    </VirtualNetworkSite>
```

<span data-ttu-id="e7e6d-146">I det här exemplet hello virtuella nätverket MultiTier-VNet har tre undernät: 'Klientdel', 'Midtier' och 'Backend-undernät med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-146">In this example, hello virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="e7e6d-147">hello steg ange hello 'DefaultSiteHQ' hello standard plats-anslutning för Tvingad tunneling och konfigurera hello Midtier och Backend-undernät toouse Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-147">hello steps will set hello 'DefaultSiteHQ' as hello default site connection for forced tunneling, and configure hello Midtier and Backend subnets toouse forced tunneling.</span></span>

1. <span data-ttu-id="e7e6d-148">Skapa en routningstabell.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-148">Create a routing table.</span></span> <span data-ttu-id="e7e6d-149">Använd följande cmdlet toocreate hello din routningstabellen.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-149">Use hello following cmdlet toocreate your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="e7e6d-150">Lägg till en standard väg toohello routningstabell.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-150">Add a default route toohello routing table.</span></span> 

  <span data-ttu-id="e7e6d-151">hello följande exempel lägger till en standard väg toohello routningstabell skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-151">hello following example adds a default route toohello routing table created in Step 1.</span></span> <span data-ttu-id="e7e6d-152">Observera att hello endast väg som stöds är hello målprefix för ”0.0.0.0/0” toohello ”VPNGateway” NextHop.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-152">Note that hello only route supported is hello destination prefix of "0.0.0.0/0" toohello "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="e7e6d-153">Associera hello routning tabell toohello undernät.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-153">Associate hello routing table toohello subnets.</span></span> 

  <span data-ttu-id="e7e6d-154">När en routningstabell skapas och lägga till en väg, Använd följande exempel tooadd hello eller associera hello flödet tabell tooa VNet subnet.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-154">After a routing table is created and a route added, use hello following example tooadd or associate hello route table tooa VNet subnet.</span></span> <span data-ttu-id="e7e6d-155">hello exempel läggs hello flödet tabell ”MyRouteTable” toohello Midtier och Backend-undernät i VNet MultiTier-VNet.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-155">hello example adds hello route table "MyRouteTable" toohello Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="e7e6d-156">Tilldela en standardplats för Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="e7e6d-157">I föregående steg hello, hello cmdlet exempelskript skapas hello routningstabellen och associerade hello flödet tabell tootwo hello VNet-undernät.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-157">In hello preceding step, hello sample cmdlet scripts created hello routing table and associated hello route table tootwo of hello VNet subnets.</span></span> <span data-ttu-id="e7e6d-158">hello återstående steg är tooselect en lokal plats bland hello anslutningar på flera platser i hello virtuellt nätverk som hello standardwebbplatsen eller tunnel.</span><span class="sxs-lookup"><span data-stu-id="e7e6d-158">hello remaining step is tooselect a local site among hello multi-site connections of hello virtual network as hello default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="e7e6d-159">Ytterligare PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="e7e6d-159">Additional PowerShell cmdlets</span></span>
### <a name="toodelete-a-route-table"></a><span data-ttu-id="e7e6d-160">toodelete en routningstabell</span><span class="sxs-lookup"><span data-stu-id="e7e6d-160">toodelete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="toolist-a-route-table"></a><span data-ttu-id="e7e6d-161">toolist en routningstabell</span><span class="sxs-lookup"><span data-stu-id="e7e6d-161">toolist a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="toodelete-a-route-from-a-route-table"></a><span data-ttu-id="e7e6d-162">toodelete en väg från en routningstabell</span><span class="sxs-lookup"><span data-stu-id="e7e6d-162">toodelete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="tooremove-a-route-from-a-subnet"></a><span data-ttu-id="e7e6d-163">tooremove en väg från ett undernät</span><span class="sxs-lookup"><span data-stu-id="e7e6d-163">tooremove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="toolist-hello-route-table-associated-with-a-subnet"></a><span data-ttu-id="e7e6d-164">toolist hello vägtabell associerad med ett undernät</span><span class="sxs-lookup"><span data-stu-id="e7e6d-164">toolist hello route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="tooremove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="e7e6d-165">tooremove en standardwebbplats från ett VNet VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="e7e6d-165">tooremove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```