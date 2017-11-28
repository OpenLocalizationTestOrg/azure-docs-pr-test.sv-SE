---
title: "Konfigurera routning (peering) för en ExpressRoute-krets: hanteraren för filserverresurser: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom stegen för att skapa och etablera privat, offentlig och Microsoft-peering av en ExpressRoute-krets. I artikeln får du även se hur man kontrollerar status, uppdaterar eller tar bort peerings för din krets."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a036d51-77ae-4fee-9ddb-35f040fbdcdf
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: af68955b78239832e413e1b59e033d7d3da8d599
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="2fddd-104">Skapa och ändra peering för en ExpressRoute-krets med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="2fddd-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="2fddd-105">Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i Resource Manager-distributionsmodellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2fddd-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in the Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="2fddd-106">Du kan också kontrollera status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-106">You can also check the status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="2fddd-107">Om du vill använda en annan metod för att arbeta med kretsen väljer du en artikel från följande lista:</span><span class="sxs-lookup"><span data-stu-id="2fddd-107">If you want to use a different method to work with your circuit, select an article from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2fddd-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2fddd-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="2fddd-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2fddd-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="2fddd-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2fddd-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="2fddd-111">Video - privat peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2fddd-112">Video - offentlig peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2fddd-113">Video - Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="2fddd-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="2fddd-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="2fddd-115">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="2fddd-115">Configuration prerequisites</span></span>

* <span data-ttu-id="2fddd-116">Du behöver den senaste versionen av Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-116">You will need the latest version of the Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="2fddd-117">Mer information finns i [Installera och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2fddd-117">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="2fddd-118">Kontrollera att du har granskat sidorna [Förutsättningar](expressroute-prerequisites.md), [Routningskrav](expressroute-routing.md) sidan och [Arbetsflöden](expressroute-workflows.md) sidan innan du påbörjar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-118">Make sure that you have reviewed the [prerequisites](expressroute-prerequisites.md) page, the [routing requirements](expressroute-routing.md) page, and the [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="2fddd-119">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="2fddd-120">Följ anvisningarna för att [Skapa en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och aktivera kretsen av anslutningsprovidern innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="2fddd-120">Follow the instructions to [Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have the circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="2fddd-121">ExpressRoute-kretsen måste vara i ett etablerad och aktiverade tillstånd att kunna köra cmdlets i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2fddd-121">The ExpressRoute circuit must be in a provisioned and enabled state for you to be able to run the cmdlets in this article.</span></span>

<span data-ttu-id="2fddd-122">Dessa anvisningar gäller endast för kretsar som skapats med tjänstleverantörer som erbjuder tjänster för Layer 2-anslutning.</span><span class="sxs-lookup"><span data-stu-id="2fddd-122">These instructions only apply to circuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="2fddd-123">Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="2fddd-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2fddd-124">Vi gör för närvarande inte reklam för peerings som konfigurerats av tjänstleverantörer via tjänsthanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-124">We currently do not advertise peerings configured by service providers through the service management portal.</span></span> <span data-ttu-id="2fddd-125">Vi arbetar på att kunna aktivera den här funktionen snart.</span><span class="sxs-lookup"><span data-stu-id="2fddd-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="2fddd-126">Kontrollera med leverantören innan du konfigurerar BGP peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="2fddd-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="2fddd-127">Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="2fddd-128">Du kan konfigurera peerings i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="2fddd-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="2fddd-129">Dock måste du se till att du slutför konfigurationen av en peering i taget.</span><span class="sxs-lookup"><span data-stu-id="2fddd-129">However, you must make sure that you complete the configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="2fddd-130">Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-130">Azure private peering</span></span>

<span data-ttu-id="2fddd-131">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort Azure privat peering konfigurationen för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-131">This section helps you create, get, update, and delete the Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-private-peering"></a><span data-ttu-id="2fddd-132">Så här skapar du Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-132">To create Azure private peering</span></span>

1. <span data-ttu-id="2fddd-133">Importera PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2fddd-133">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="2fddd-134">Du måste installera den senaste versionen av PowerShell-installeraren från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera modulerna i Azure Resource Manager i PowerShell-sessionen för att kunna börja använda ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="2fddd-134">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="2fddd-135">Du måste köra PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="2fddd-135">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="2fddd-136">Importera alla AzureRM.* modulerna inom intervallet semantiska versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-136">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="2fddd-137">Du kan också importera en väljer modul inom intervallet för semantisk versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-137">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="2fddd-138">Logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2fddd-138">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="2fddd-139">Välj den prenumeration som du vill skapa ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-139">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="2fddd-140">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="2fddd-141">Följ anvisningarna för att skapa en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och etablera den med anslutningsprovidern.</span><span class="sxs-lookup"><span data-stu-id="2fddd-141">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2fddd-142">Om din anslutningsleverantör erbjuder hanteringstjänster för Layer 3, kan du begära att anslutningsleverantören aktiverar Azures privata peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="2fddd-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="2fddd-143">I så fall behöver du inte följa anvisningarna i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2fddd-143">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2fddd-144">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med nästa steg.</span><span class="sxs-lookup"><span data-stu-id="2fddd-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="2fddd-145">Kontrollera ExpressRoute-krets och kontrollera att den är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="2fddd-145">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="2fddd-146">Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-146">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="2fddd-147">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-147">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="2fddd-148">Konfigurera Azures privata peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-148">Configure Azure private peering for the circuit.</span></span> <span data-ttu-id="2fddd-149">Kontrollera att du har följande objekt innan du fortsätter med nästa steg:</span><span class="sxs-lookup"><span data-stu-id="2fddd-149">Make sure that you have the following items before you proceed with the next steps:</span></span>

  * <span data-ttu-id="2fddd-150">Ett /30 undernät för den primära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-150">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2fddd-151">Undernätet får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2fddd-151">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="2fddd-152">Ett /30 undernät för den sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-152">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2fddd-153">Undernätet får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="2fddd-153">The subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="2fddd-154">Ett giltigt VLAN-ID att upprätta denna peering på.</span><span class="sxs-lookup"><span data-stu-id="2fddd-154">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2fddd-155">Se till att ingen annan peering i kretsen använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="2fddd-155">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2fddd-156">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="2fddd-156">AS number for peering.</span></span> <span data-ttu-id="2fddd-157">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="2fddd-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="2fddd-158">Du kan använda ett privat AS-tal för den här peeringen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="2fddd-159">Se till att du inte använder 65515.</span><span class="sxs-lookup"><span data-stu-id="2fddd-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="2fddd-160">**Valfritt -** en MD5-hash om du väljer att använda en.</span><span class="sxs-lookup"><span data-stu-id="2fddd-160">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="2fddd-161">Använd följande exempel för att konfigurera privat Azure-peering för kretsen:</span><span class="sxs-lookup"><span data-stu-id="2fddd-161">Use the following example to configure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="2fddd-162">Om du väljer att använda en MD5-hash, Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-162">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="2fddd-163">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="2fddd-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-private-peering-details"></a><span data-ttu-id="2fddd-164">Så här visar du Azures privata peering-information</span><span class="sxs-lookup"><span data-stu-id="2fddd-164">To view Azure private peering details</span></span>

<span data-ttu-id="2fddd-165">Du kan få konfigurationsinformation genom att använda följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-165">You can get configuration details by using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="to-update-azure-private-peering-configuration"></a><span data-ttu-id="2fddd-166">Uppdatera konfigurationen av Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-166">To update Azure private peering configuration</span></span>

<span data-ttu-id="2fddd-167">Du kan uppdatera någon del av konfigurationen med följande exempel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-167">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="2fddd-168">I det här exemplet uppdateras VLAN-ID för kretsen från 100 till 500.</span><span class="sxs-lookup"><span data-stu-id="2fddd-168">In this example, the VLAN ID of the circuit is being updated from 100 to 500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-private-peering"></a><span data-ttu-id="2fddd-169">Så här tar du bort Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-169">To delete Azure private peering</span></span>

<span data-ttu-id="2fddd-170">Du kan ta bort peering konfigurationen genom att köra följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-170">You can remove your peering configuration by running the following example:</span></span>

> [!WARNING]
> <span data-ttu-id="2fddd-171">Du måste se till att alla virtuella nätverk avlänkas från ExpressRoute-kretsen innan du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="2fddd-171">You must ensure that all virtual networks are unlinked from the ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="2fddd-172">Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-172">Azure public peering</span></span>

<span data-ttu-id="2fddd-173">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort Azure offentlig peering konfigurationen för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-173">This section helps you create, get, update, and delete the Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="to-create-azure-public-peering"></a><span data-ttu-id="2fddd-174">Så här skapar du Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-174">To create Azure public peering</span></span>

1. <span data-ttu-id="2fddd-175">Importera PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2fddd-175">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="2fddd-176">Du måste installera den senaste versionen av PowerShell-installeraren från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera modulerna i Azure Resource Manager i PowerShell-sessionen för att kunna börja använda ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="2fddd-176">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="2fddd-177">Du måste köra PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="2fddd-177">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="2fddd-178">Importera alla AzureRM.* modulerna inom intervallet semantiska versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-178">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="2fddd-179">Du kan också importera en väljer modul inom intervallet för semantisk versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-179">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="2fddd-180">Logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2fddd-180">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="2fddd-181">Välj den prenumeration som du vill skapa ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-181">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="2fddd-182">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="2fddd-183">Följ anvisningarna för att skapa en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och etablera den med anslutningsprovidern.</span><span class="sxs-lookup"><span data-stu-id="2fddd-183">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2fddd-184">Om din anslutningsleverantör erbjuder hanteringstjänster för Layer 3, kan du begära att anslutningsleverantören aktiverar Azures privata peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="2fddd-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="2fddd-185">I så fall behöver du inte följa anvisningarna i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2fddd-185">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2fddd-186">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med nästa steg.</span><span class="sxs-lookup"><span data-stu-id="2fddd-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="2fddd-187">Kontrollera ExpressRoute-kretsen om du vill att den är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="2fddd-187">Check the ExpressRoute circuit to ensure it is provisioned and also enabled.</span></span> <span data-ttu-id="2fddd-188">Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-188">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="2fddd-189">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-189">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                      "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="2fddd-190">Konfigurera Azures offentliga peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-190">Configure Azure public peering for the circuit.</span></span> <span data-ttu-id="2fddd-191">Kontrollera att du har följande information innan du ska kunna fortsätta.</span><span class="sxs-lookup"><span data-stu-id="2fddd-191">Make sure that you have the following information before you proceed further.</span></span>

  * <span data-ttu-id="2fddd-192">Ett /30 undernät för den primära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-192">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2fddd-193">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="2fddd-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="2fddd-194">Ett /30 undernät för den sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-194">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2fddd-195">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="2fddd-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="2fddd-196">Ett giltigt VLAN-ID att upprätta denna peering på.</span><span class="sxs-lookup"><span data-stu-id="2fddd-196">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2fddd-197">Se till att ingen annan peering i kretsen använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="2fddd-197">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2fddd-198">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="2fddd-198">AS number for peering.</span></span> <span data-ttu-id="2fddd-199">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="2fddd-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="2fddd-200">**Valfritt -** en MD5-hash om du väljer att använda en.</span><span class="sxs-lookup"><span data-stu-id="2fddd-200">**Optional -** An MD5 hash if you choose to use one.</span></span>

  <span data-ttu-id="2fddd-201">Kör följande exempel för att konfigurera offentlig Azure-peering för kretsen</span><span class="sxs-lookup"><span data-stu-id="2fddd-201">Run the following example to configure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="2fddd-202">Om du väljer att använda en MD5-hash, Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-202">If you choose to use an MD5 hash, use the following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="2fddd-203">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="2fddd-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="to-view-azure-public-peering-details"></a><span data-ttu-id="2fddd-204">Så här visar du Azures offentliga peering-information</span><span class="sxs-lookup"><span data-stu-id="2fddd-204">To view Azure public peering details</span></span>

<span data-ttu-id="2fddd-205">Du kan hämta konfigurationsinformation med följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="2fddd-205">You can get configuration details using the following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="to-update-azure-public-peering-configuration"></a><span data-ttu-id="2fddd-206">Så här uppdaterar du konfigurationen av Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-206">To update Azure public peering configuration</span></span>

<span data-ttu-id="2fddd-207">Du kan uppdatera någon del av konfigurationen med följande exempel.</span><span class="sxs-lookup"><span data-stu-id="2fddd-207">You can update any part of the configuration using the following example.</span></span> <span data-ttu-id="2fddd-208">I det här exemplet uppdateras VLAN-ID för kretsen från 200 till 600.</span><span class="sxs-lookup"><span data-stu-id="2fddd-208">In this example, the VLAN ID of the circuit is being updated from 200 to 600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-public-peering"></a><span data-ttu-id="2fddd-209">Så här tar du bort Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-209">To delete Azure public peering</span></span>

<span data-ttu-id="2fddd-210">Du kan ta bort peering konfigurationen genom att köra följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-210">You can remove your peering configuration by running the following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="2fddd-211">Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-211">Microsoft peering</span></span>

<span data-ttu-id="2fddd-212">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort Microsoft peering konfigurationen för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-212">This section helps you create, get, update, and delete the Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2fddd-213">Microsoft-peering i ExpressRoute-kretsar som konfigurerades före den 1 augusti 2017 kommer att ha alla service prefix annonserade via Microsoft peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="2fddd-213">Microsoft peering of ExpressRoute circuits that were configured prior to August 1, 2017 will have all service prefixes advertised through the Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="2fddd-214">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonseras förrän ett filter för flödet är kopplat till kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached to the circuit.</span></span> <span data-ttu-id="2fddd-215">Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="to-create-microsoft-peering"></a><span data-ttu-id="2fddd-216">Så här skapar du Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-216">To create Microsoft peering</span></span>

1. <span data-ttu-id="2fddd-217">Importera PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="2fddd-217">Import the PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="2fddd-218">Du måste installera den senaste versionen av PowerShell-installeraren från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera modulerna i Azure Resource Manager i PowerShell-sessionen för att kunna börja använda ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="2fddd-218">You must install the latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import the Azure Resource Manager modules into the PowerShell session in order to start using the ExpressRoute cmdlets.</span></span> <span data-ttu-id="2fddd-219">Du måste köra PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="2fddd-219">You will need to run PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="2fddd-220">Importera alla AzureRM.* modulerna inom intervallet semantiska versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-220">Import all of the AzureRM.* modules within the known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="2fddd-221">Du kan också importera en väljer modul inom intervallet för semantisk versionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-221">You can also just import a select module within the known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="2fddd-222">Logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="2fddd-222">Sign in to your account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="2fddd-223">Välj den prenumeration som du vill skapa ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-223">Select the subscription you want to create ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="2fddd-224">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="2fddd-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="2fddd-225">Följ anvisningarna för att skapa en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och etablera den med anslutningsprovidern.</span><span class="sxs-lookup"><span data-stu-id="2fddd-225">Follow the instructions to create an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by the connectivity provider.</span></span>

  <span data-ttu-id="2fddd-226">Om din anslutningsleverantör erbjuder hanteringstjänster för Layer 3, kan du begära att anslutningsleverantören aktiverar Azures privata peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="2fddd-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider to enable Azure private peering for you.</span></span> <span data-ttu-id="2fddd-227">I så fall behöver du inte följa anvisningarna i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2fddd-227">In that case, you won't need to follow instructions listed in the next sections.</span></span> <span data-ttu-id="2fddd-228">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med nästa steg.</span><span class="sxs-lookup"><span data-stu-id="2fddd-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using the next steps.</span></span>
3. <span data-ttu-id="2fddd-229">Kontrollera ExpressRoute-krets och kontrollera att den är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="2fddd-229">Check the ExpressRoute circuit to make sure it is provisioned and also enabled.</span></span> <span data-ttu-id="2fddd-230">Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-230">Use the following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="2fddd-231">Svaret liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-231">The response is similar to the following example:</span></span>

  ```
  Name                             : ExpressRouteARMCircuit
  ResourceGroupName                : ExpressRouteResourceGroup
  Location                         : westus
  Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
  Etag                             : W/"################################"
  ProvisioningState                : Succeeded
  Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
  CircuitProvisioningState         : Enabled
  ServiceProviderProvisioningState : Provisioned
  ServiceProviderNotes             : 
  ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
  ServiceKey                       : **************************************
  Peerings                         : []
  ```
4. <span data-ttu-id="2fddd-232">Konfigurera Microsoft-peering för kretsen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-232">Configure Microsoft peering for the circuit.</span></span> <span data-ttu-id="2fddd-233">Kontrollera att du har följande information innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="2fddd-233">Make sure that you have the following information before you proceed.</span></span>

  * <span data-ttu-id="2fddd-234">Ett /30 undernät för den primära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-234">A /30 subnet for the primary link.</span></span> <span data-ttu-id="2fddd-235">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="2fddd-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="2fddd-236">Ett /30 undernät för den sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="2fddd-236">A /30 subnet for the secondary link.</span></span> <span data-ttu-id="2fddd-237">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="2fddd-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="2fddd-238">Ett giltigt VLAN-ID att upprätta denna peering på.</span><span class="sxs-lookup"><span data-stu-id="2fddd-238">A valid VLAN ID to establish this peering on.</span></span> <span data-ttu-id="2fddd-239">Se till att ingen annan peering i kretsen använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="2fddd-239">Ensure that no other peering in the circuit uses the same VLAN ID.</span></span>
  * <span data-ttu-id="2fddd-240">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="2fddd-240">AS number for peering.</span></span> <span data-ttu-id="2fddd-241">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="2fddd-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="2fddd-242">Annonserade prefix: Du måste ange en lista över alla prefix som du planerar att annonsera i BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="2fddd-242">Advertised prefixes: You must provide a list of all prefixes you plan to advertise over the BGP session.</span></span> <span data-ttu-id="2fddd-243">Endast offentliga IP-adressprefix accepteras.</span><span class="sxs-lookup"><span data-stu-id="2fddd-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="2fddd-244">Om du planerar att skicka en uppsättning prefix, kan du skicka en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="2fddd-244">If you plan to send a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="2fddd-245">Dessa prefix måste vara registrerade åt dig i ett RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="2fddd-245">These prefixes must be registered to you in an RIR / IRR.</span></span>
  * <span data-ttu-id="2fddd-246">**Valfritt -** kunden ASN: Om du är reklam prefix som inte har registrerats peering som tal, kan du ange numret som de har registrerats.</span><span class="sxs-lookup"><span data-stu-id="2fddd-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered to the peering AS number, you can specify the AS number to which they are registered.</span></span>
  * <span data-ttu-id="2fddd-247">Routningens registernamn: Du kan ange den RIR/IR mot vilken AS-numret och prefixet är registrerade.</span><span class="sxs-lookup"><span data-stu-id="2fddd-247">Routing Registry Name: You can specify the RIR / IRR against which the AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="2fddd-248">**Valfritt -** en MD5-hash om du väljer att använda en.</span><span class="sxs-lookup"><span data-stu-id="2fddd-248">**Optional -** An MD5 hash if you choose to use one.</span></span>

   <span data-ttu-id="2fddd-249">Använd följande exempel för att konfigurera Microsoft-peering för kretsen:</span><span class="sxs-lookup"><span data-stu-id="2fddd-249">Use the following example to configure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="to-get-microsoft-peering-details"></a><span data-ttu-id="2fddd-250">Så här hämtar du Microsofts peering-information</span><span class="sxs-lookup"><span data-stu-id="2fddd-250">To get Microsoft peering details</span></span>

<span data-ttu-id="2fddd-251">Du kan hämta konfigurationsinformation enligt följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-251">You can get configuration details using the following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-microsoft-peering-configuration"></a><span data-ttu-id="2fddd-252">Så här uppdaterar du Microsofts peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="2fddd-252">To update Microsoft peering configuration</span></span>

<span data-ttu-id="2fddd-253">Du kan uppdatera någon del av konfigurationen med följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2fddd-253">You can update any part of the configuration using the following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-microsoft-peering"></a><span data-ttu-id="2fddd-254">Så här tar du bort Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="2fddd-254">To delete Microsoft peering</span></span>

<span data-ttu-id="2fddd-255">Du kan ta bort peering konfigurationen genom att köra följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="2fddd-255">You can remove your peering configuration by running the following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="2fddd-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2fddd-256">Next steps</span></span>

<span data-ttu-id="2fddd-257">Nästa steg [Länka ett VNet till en ExpressRoute-krets](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-257">Next step, [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="2fddd-258">Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="2fddd-259">Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="2fddd-260">Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2fddd-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
