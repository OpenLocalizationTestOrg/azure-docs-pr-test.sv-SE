---
title: "Konfigurera Tvingad tunneltrafik för Azure plats-till-plats-anslutningar: klassiska | Microsoft Docs"
description: "Så här dirigerar eller 'tvinga' alla Internet-bunden trafik tillbaka till den lokala platsen."
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
ms.openlocfilehash: 79bf6892c823da282c3e763921e830f986419854
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="configure-forced-tunneling-using-the-classic-deployment-model"></a><span data-ttu-id="379b1-103">Konfigurera framtvingad tunneling med den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="379b1-103">Configure forced tunneling using the classic deployment model</span></span>

<span data-ttu-id="379b1-104">Tvingad tunneling kan du omdirigera eller ”force” alla Internet-bunden trafik tillbaka till din lokala plats via en plats-till-plats VPN-tunnel för kontroll och granskning.</span><span class="sxs-lookup"><span data-stu-id="379b1-104">Forced tunneling lets you redirect or "force" all Internet-bound traffic back to your on-premises location via a Site-to-Site VPN tunnel for inspection and auditing.</span></span> <span data-ttu-id="379b1-105">Detta är en kritisk säkerhetskraven för de flesta företags IT principer.</span><span class="sxs-lookup"><span data-stu-id="379b1-105">This is a critical security requirement for most enterprise IT policies.</span></span> <span data-ttu-id="379b1-106">Utan Tvingad tunneling ska Internet-bunden trafik från din virtuella datorer i Azure alltid passerar från Azure nätverksinfrastruktur direkt ut till Internet, utan att du vill granska eller granska trafiken.</span><span class="sxs-lookup"><span data-stu-id="379b1-106">Without forced tunneling, Internet-bound traffic from your VMs in Azure will always traverse from Azure network infrastructure directly out to the Internet, without the option to allow you to inspect or audit the traffic.</span></span> <span data-ttu-id="379b1-107">Obehörig Internetåtkomst kan leda till avslöjande av information och andra typer av säkerhetsintrång.</span><span class="sxs-lookup"><span data-stu-id="379b1-107">Unauthorized Internet access can potentially lead to information disclosure or other types of security breaches.</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

<span data-ttu-id="379b1-108">Den här artikeln beskriver hur du konfigurerar Tvingad tunneltrafik för virtuella nätverk som skapats med den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="379b1-108">This article walks you through configuring forced tunneling for virtual networks created using the classic deployment model.</span></span> <span data-ttu-id="379b1-109">Tvingad tunneltrafik kan konfigureras med hjälp av PowerShell inte via portalen.</span><span class="sxs-lookup"><span data-stu-id="379b1-109">Forced tunneling can be configured by using PowerShell, not through the portal.</span></span> <span data-ttu-id="379b1-110">Om du vill konfigurera Tvingad tunneling för Resource Manager-distributionsmodellen Välj klassiska artikel från listan följande:</span><span class="sxs-lookup"><span data-stu-id="379b1-110">If you want to configure forced tunneling for the Resource Manager deployment model, select classic article from the following dropdown list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="379b1-111">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="379b1-111">PowerShell - Classic</span></span>](vpn-gateway-about-forced-tunneling.md)
> * [<span data-ttu-id="379b1-112">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="379b1-112">PowerShell - Resource Manager</span></span>](vpn-gateway-forced-tunneling-rm.md)
> 
> 

## <a name="requirements-and-considerations"></a><span data-ttu-id="379b1-113">Krav och överväganden</span><span class="sxs-lookup"><span data-stu-id="379b1-113">Requirements and considerations</span></span>
<span data-ttu-id="379b1-114">Tvingad tunneling i Azure konfigureras via virtuella nätverk användardefinierade vägar (UDR).</span><span class="sxs-lookup"><span data-stu-id="379b1-114">Forced tunneling in Azure is configured via virtual network user-defined routes (UDR).</span></span> <span data-ttu-id="379b1-115">Omdirigera trafik till en lokal plats uttrycks som en standardväg till Azure VPN-gatewayen.</span><span class="sxs-lookup"><span data-stu-id="379b1-115">Redirecting traffic to an on-premises site is expressed as a Default Route to the Azure VPN gateway.</span></span> <span data-ttu-id="379b1-116">Följande avsnitt innehåller aktuella begränsningen på Routning och vägar för ett Azure Virtual Network:</span><span class="sxs-lookup"><span data-stu-id="379b1-116">The following section lists the current limitation of the routing table and routes for an Azure Virtual Network:</span></span>

* <span data-ttu-id="379b1-117">Varje undernät för virtuellt nätverk har en inbyggd, system-routningstabell.</span><span class="sxs-lookup"><span data-stu-id="379b1-117">Each virtual network subnet has a built-in, system routing table.</span></span> <span data-ttu-id="379b1-118">Routningstabellen för systemet har följande tre grupper av vägar:</span><span class="sxs-lookup"><span data-stu-id="379b1-118">The system routing table has the following three groups of routes:</span></span>

  * <span data-ttu-id="379b1-119">**Lokal VNet vägar:** direkt till målet virtuella datorer i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-119">**Local VNet routes:** Directly to the destination VMs in the same virtual network.</span></span>
  * <span data-ttu-id="379b1-120">**Lokala vägar:** till Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="379b1-120">**On-premises routes:** To the Azure VPN gateway.</span></span>
  * <span data-ttu-id="379b1-121">**Standardvägen:** direkt till Internet.</span><span class="sxs-lookup"><span data-stu-id="379b1-121">**Default route:** Directly to the Internet.</span></span> <span data-ttu-id="379b1-122">Paket avsedda för de privata IP-adresser som inte omfattas av föregående två vägar tas bort.</span><span class="sxs-lookup"><span data-stu-id="379b1-122">Packets destined to the private IP addresses not covered by the previous two routes will be dropped.</span></span>
* <span data-ttu-id="379b1-123">Med lanseringen av användardefinierade vägar, kan du skapa en routningstabell för att lägga till en standardväg och sedan associerar routningstabellen för din VNet-adressundernät för att aktivera Tvingad tunneltrafik på dessa undernät.</span><span class="sxs-lookup"><span data-stu-id="379b1-123">With the release of user-defined routes, you can create a routing table to add a default route, and then associate the routing table to your VNet subnet(s) to enable forced tunneling on those subnets.</span></span>
* <span data-ttu-id="379b1-124">Du måste ange en ”standardwebbplats” bland de lokala mellan lokala platserna anslutna till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="379b1-124">You need to set a "default site" among the cross-premises local sites connected to the virtual network.</span></span>
* <span data-ttu-id="379b1-125">Tvingad tunneling måste vara kopplad till ett virtuellt nätverk som har en dynamisk routning VPN-gateway (inte en statisk gateway).</span><span class="sxs-lookup"><span data-stu-id="379b1-125">Forced tunneling must be associated with a VNet that has a dynamic routing VPN gateway (not a static gateway).</span></span>
* <span data-ttu-id="379b1-126">ExpressRoute Tvingad tunneltrafik konfigureras inte via den här mekanismen, men har aktiverats genom att annonsera en standardväg via ExpressRoute BGP-peeringsessioner i stället.</span><span class="sxs-lookup"><span data-stu-id="379b1-126">ExpressRoute forced tunneling is not configured via this mechanism, but instead, is enabled by advertising a default route via the ExpressRoute BGP peering sessions.</span></span> <span data-ttu-id="379b1-127">Finns det [ExpressRoute dokumentation](https://azure.microsoft.com/documentation/services/expressroute/) för mer information.</span><span class="sxs-lookup"><span data-stu-id="379b1-127">Please see the [ExpressRoute Documentation](https://azure.microsoft.com/documentation/services/expressroute/) for more information.</span></span>

## <a name="configuration-overview"></a><span data-ttu-id="379b1-128">Konfigurationsöversikt</span><span class="sxs-lookup"><span data-stu-id="379b1-128">Configuration overview</span></span>
<span data-ttu-id="379b1-129">I följande exempel tunneldata klientdel undernät inte tvingas.</span><span class="sxs-lookup"><span data-stu-id="379b1-129">In the following example, the Frontend subnet is not forced tunneled.</span></span> <span data-ttu-id="379b1-130">Arbetsbelastningar i undernätet Frontend kan fortsätta att acceptera och svara på kundernas önskemål direkt från Internet.</span><span class="sxs-lookup"><span data-stu-id="379b1-130">The workloads in the Frontend subnet can continue to accept and respond to customer requests from the Internet directly.</span></span> <span data-ttu-id="379b1-131">Halva nivå och Backend-undernät tvingas tunnel.</span><span class="sxs-lookup"><span data-stu-id="379b1-131">The Mid-tier and Backend subnets are forced tunneled.</span></span> <span data-ttu-id="379b1-132">Alla utgående anslutningar från dessa två undernät till Internet ska tvingas eller omdirigeras till en lokal plats via en S2S VPN-tunnlar.</span><span class="sxs-lookup"><span data-stu-id="379b1-132">Any outbound connections from these two subnets to the Internet will be forced or redirected back to an on-premises site via one of the S2S VPN tunnels.</span></span>

<span data-ttu-id="379b1-133">På så sätt kan du begränsa och inspektera Internetåtkomst från dina virtuella datorer eller molntjänster i Azure, medan du aktivera din flernivåtjänst arkitektur som krävs.</span><span class="sxs-lookup"><span data-stu-id="379b1-133">This allows you to restrict and inspect Internet access from your virtual machines or cloud services in Azure, while continuing to enable your multi-tier service architecture required.</span></span> <span data-ttu-id="379b1-134">Du kan också använda Tvingad tunneling hela virtuella nätverk, om det finns ingen Internet-riktade arbetsbelastningar i ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-134">You also can apply forced tunneling to the entire virtual networks if there are no Internet-facing workloads in your virtual networks.</span></span>

![Forcerade tunnlar](./media/vpn-gateway-about-forced-tunneling/forced-tunnel.png)

## <a name="before-you-begin"></a><span data-ttu-id="379b1-136">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="379b1-136">Before you begin</span></span>
<span data-ttu-id="379b1-137">Kontrollera att du har följande innan du påbörjar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="379b1-137">Verify that you have the following items before beginning configuration.</span></span>

* <span data-ttu-id="379b1-138">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="379b1-138">An Azure subscription.</span></span> <span data-ttu-id="379b1-139">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="379b1-139">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="379b1-140">Konfigurerade virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-140">A configured virtual network.</span></span> 
* <span data-ttu-id="379b1-141">Den senaste versionen av Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="379b1-141">The latest version of the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="379b1-142">Mer information om hur man installerar PowerShell-cmdletar finns i [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="379b1-142">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for more information about installing the PowerShell cmdlets.</span></span>

## <a name="configure-forced-tunneling"></a><span data-ttu-id="379b1-143">Konfigurera tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="379b1-143">Configure forced tunneling</span></span>
<span data-ttu-id="379b1-144">Med hjälp av följande procedur kan du ange Tvingad tunneling för ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-144">The following procedure will help you specify forced tunneling for a virtual network.</span></span> <span data-ttu-id="379b1-145">Konfigurationssteg motsvarar konfigurationsfilen VNet nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-145">The configuration steps correspond to the VNet network configuration file.</span></span>

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

<span data-ttu-id="379b1-146">I det här exemplet har tre undernät i det virtuella nätverket MultiTier-VNet: 'Klientdel', 'Midtier' och 'Backend-undernät med fyra mellan lokala anslutningar: 'DefaultSiteHQ' och tre filialer.</span><span class="sxs-lookup"><span data-stu-id="379b1-146">In this example, the virtual network 'MultiTier-VNet' has three subnets: 'Frontend', 'Midtier', and 'Backend' subnets, with four cross premises connections: 'DefaultSiteHQ', and three Branches.</span></span> 

<span data-ttu-id="379b1-147">Stegen kommer DefaultSiteHQ som standard plats-anslutning för Tvingad tunneltrafik, och konfigurera Midtier och Backend-undernät att använda Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="379b1-147">The steps will set the 'DefaultSiteHQ' as the default site connection for forced tunneling, and configure the Midtier and Backend subnets to use forced tunneling.</span></span>

1. <span data-ttu-id="379b1-148">Skapa en routningstabell.</span><span class="sxs-lookup"><span data-stu-id="379b1-148">Create a routing table.</span></span> <span data-ttu-id="379b1-149">Använd följande cmdlet för att skapa din routningstabellen.</span><span class="sxs-lookup"><span data-stu-id="379b1-149">Use the following cmdlet to create your route table.</span></span>

  ```powershell
  New-AzureRouteTable –Name "MyRouteTable" –Label "Routing Table for Forced Tunneling" –Location "North Europe"
  ```
2. <span data-ttu-id="379b1-150">Lägg till en standardväg i routningstabellen.</span><span class="sxs-lookup"><span data-stu-id="379b1-150">Add a default route to the routing table.</span></span> 

  <span data-ttu-id="379b1-151">I följande exempel läggs en standardväg i routningstabellen skapade i steg 1.</span><span class="sxs-lookup"><span data-stu-id="379b1-151">The following example adds a default route to the routing table created in Step 1.</span></span> <span data-ttu-id="379b1-152">Observera att endast vägen som stöds är målprefix för ”0.0.0.0/0” till ”VPNGateway” NextHop.</span><span class="sxs-lookup"><span data-stu-id="379b1-152">Note that the only route supported is the destination prefix of "0.0.0.0/0" to the "VPNGateway" NextHop.</span></span>

  ```powershell
  Get-AzureRouteTable -Name "MyRouteTable" | Set-AzureRoute –RouteTable "MyRouteTable" –RouteName "DefaultRoute" –AddressPrefix "0.0.0.0/0" –NextHopType VPNGateway
  ```
3. <span data-ttu-id="379b1-153">Associera routningstabellen i undernät.</span><span class="sxs-lookup"><span data-stu-id="379b1-153">Associate the routing table to the subnets.</span></span> 

  <span data-ttu-id="379b1-154">När en routningstabell skapas och lägga till en väg, Använd följande exempel för att lägga till eller associera routningstabellen till ett undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-154">After a routing table is created and a route added, use the following example to add or associate the route table to a VNet subnet.</span></span> <span data-ttu-id="379b1-155">Exemplet lägger till vägtabellen ”MyRouteTable” Midtier och Backend-undernät i VNet MultiTier-VNet.</span><span class="sxs-lookup"><span data-stu-id="379b1-155">The example adds the route table "MyRouteTable" to the Midtier and Backend subnets of VNet MultiTier-VNet.</span></span>

  ```powershell
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Midtier" -RouteTableName "MyRouteTable"
  Set-AzureSubnetRouteTable -VirtualNetworkName "MultiTier-VNet" -SubnetName "Backend" -RouteTableName "MyRouteTable"
  ```
4. <span data-ttu-id="379b1-156">Tilldela en standardplats för Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="379b1-156">Assign a default site for forced tunneling.</span></span> 

  <span data-ttu-id="379b1-157">I det föregående steget, cmdlet exempelskript skapas routningstabellen och associerade routningstabellen med två undernät för virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="379b1-157">In the preceding step, the sample cmdlet scripts created the routing table and associated the route table to two of the VNet subnets.</span></span> <span data-ttu-id="379b1-158">Återstående steg är att välja en lokal plats bland flera plats-anslutningar för det virtuella nätverket som standardwebbplatsen eller tunnel.</span><span class="sxs-lookup"><span data-stu-id="379b1-158">The remaining step is to select a local site among the multi-site connections of the virtual network as the default site or tunnel.</span></span>

  ```powershell
  $DefaultSite = @("DefaultSiteHQ")
  Set-AzureVNetGatewayDefaultSite –VNetName "MultiTier-VNet" –DefaultSite "DefaultSiteHQ"
  ```

## <a name="additional-powershell-cmdlets"></a><span data-ttu-id="379b1-159">Ytterligare PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="379b1-159">Additional PowerShell cmdlets</span></span>
### <a name="to-delete-a-route-table"></a><span data-ttu-id="379b1-160">Ta bort en routningstabell</span><span class="sxs-lookup"><span data-stu-id="379b1-160">To delete a route table</span></span>

```powershell
Remove-AzureRouteTable -Name <routeTableName>
```
  
### <a name="to-list-a-route-table"></a><span data-ttu-id="379b1-161">Att visa en routningstabell</span><span class="sxs-lookup"><span data-stu-id="379b1-161">To list a route table</span></span>

```powershell
Get-AzureRouteTable [-Name <routeTableName> [-DetailLevel <detailLevel>]]
```

### <a name="to-delete-a-route-from-a-route-table"></a><span data-ttu-id="379b1-162">Ta bort en väg från en routningstabell</span><span class="sxs-lookup"><span data-stu-id="379b1-162">To delete a route from a route table</span></span>

```powershell
Remove-AzureRouteTable –Name <routeTableName>
```

### <a name="to-remove-a-route-from-a-subnet"></a><span data-ttu-id="379b1-163">Ta bort en väg från ett undernät</span><span class="sxs-lookup"><span data-stu-id="379b1-163">To remove a route from a subnet</span></span>

```powershell
Remove-AzureSubnetRouteTable –VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-list-the-route-table-associated-with-a-subnet"></a><span data-ttu-id="379b1-164">Att visa routningstabellen som är associerad med ett undernät</span><span class="sxs-lookup"><span data-stu-id="379b1-164">To list the route table associated with a subnet</span></span>

```powershell
Get-AzureSubnetRouteTable -VirtualNetworkName <virtualNetworkName> -SubnetName <subnetName>
```

### <a name="to-remove-a-default-site-from-a-vnet-vpn-gateway"></a><span data-ttu-id="379b1-165">Ta bort en standardwebbplats från ett VNet VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="379b1-165">To remove a default site from a VNet VPN gateway</span></span>

```powershell
Remove-AzureVnetGatewayDefaultSite -VNetName <virtualNetworkName>
```