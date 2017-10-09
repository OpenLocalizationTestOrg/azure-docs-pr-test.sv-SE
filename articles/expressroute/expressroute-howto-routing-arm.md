---
title: "Hur tooconfigure routning (peering) för en ExpressRoute-krets: hanteraren för filserverresurser: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
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
ms.openlocfilehash: eb3ddf5c05a086ac3e22c64417e51381ef465921
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a><span data-ttu-id="a8334-104">Skapa och ändra peering för en ExpressRoute-krets med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8334-104">Create and modify peering for an ExpressRoute circuit using PowerShell</span></span>

<span data-ttu-id="a8334-105">Den här artikeln hjälper dig att skapa och hantera routningskonfiguration för en ExpressRoute-krets i hello Resource Manager-distributionsmodellen med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a8334-105">This article helps you create and manage routing configuration for an ExpressRoute circuit in hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="a8334-106">Du kan också kontrollera hello status, uppdatera eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-106">You can also check hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span> <span data-ttu-id="a8334-107">Om du vill toouse en annan metod toowork med kretsen, väljer du en artikel från hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="a8334-107">If you want toouse a different method toowork with your circuit, select an article from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a8334-108">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a8334-108">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="a8334-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8334-109">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="a8334-110">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a8334-110">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="a8334-111">Video - privat peering</span><span class="sxs-lookup"><span data-stu-id="a8334-111">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="a8334-112">Video - offentlig peering</span><span class="sxs-lookup"><span data-stu-id="a8334-112">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="a8334-113">Video - Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-113">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="a8334-114">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="a8334-114">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

## <a name="configuration-prerequisites"></a><span data-ttu-id="a8334-115">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="a8334-115">Configuration prerequisites</span></span>

* <span data-ttu-id="a8334-116">Du behöver hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="a8334-116">You will need hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="a8334-117">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a8334-117">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> 
* <span data-ttu-id="a8334-118">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) sidan hello [routningskrav](expressroute-routing.md) sida och hello [arbetsflöden](expressroute-workflows.md) sidan innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="a8334-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="a8334-119">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="a8334-120">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-arm.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a8334-120">Follow hello instructions too[Create an ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="a8334-121">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd du toobe kan toorun hello-cmdlets i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a8334-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets in this article.</span></span>

<span data-ttu-id="a8334-122">Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2.</span><span class="sxs-lookup"><span data-stu-id="a8334-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="a8334-123">Om du använder en tjänstprovider som erbjuder hanterade Layer 3-tjänster (vanligtvis en IPVPN, t.ex. MPLS), anslutningsleverantören kan konfigurera och hantera routning av du.</span><span class="sxs-lookup"><span data-stu-id="a8334-123">If you are using a service provider that offers managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8334-124">Vi annonseras för närvarande inte peerkopplingar som konfigurerats av leverantörer via hello service management portal.</span><span class="sxs-lookup"><span data-stu-id="a8334-124">We currently do not advertise peerings configured by service providers through hello service management portal.</span></span> <span data-ttu-id="a8334-125">Vi arbetar på att kunna aktivera den här funktionen snart.</span><span class="sxs-lookup"><span data-stu-id="a8334-125">We are working on enabling this capability soon.</span></span> <span data-ttu-id="a8334-126">Kontrollera med leverantören innan du konfigurerar BGP peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="a8334-126">Check with your service provider before configuring BGP peerings.</span></span>
> 
> 

<span data-ttu-id="a8334-127">Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-127">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="a8334-128">Du kan konfigurera peerings i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="a8334-128">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="a8334-129">Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.</span><span class="sxs-lookup"><span data-stu-id="a8334-129">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span> 

## <a name="azure-private-peering"></a><span data-ttu-id="a8334-130">Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="a8334-130">Azure private peering</span></span>

<span data-ttu-id="a8334-131">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-131">This section helps you create, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="a8334-132">toocreate privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-132">toocreate Azure private peering</span></span>

1. <span data-ttu-id="a8334-133">Importera hello PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="a8334-133">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="a8334-134">Du måste installera hello senaste PowerShell installer från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera hello Azure Resource Manager moduler till hello PowerShell-session i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a8334-134">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="a8334-135">Du behöver toorun PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8334-135">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM
  Install-AzureRM
  ```

  <span data-ttu-id="a8334-136">Importera alla hello AzureRM.* moduler i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-136">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="a8334-137">Du kan också importera en väljer modul i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-137">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network 
  ```

  <span data-ttu-id="a8334-138">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="a8334-138">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="a8334-139">Välj hello-prenumeration som du vill toocreate ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a8334-139">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="a8334-140">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-140">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="a8334-141">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="a8334-141">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="a8334-142">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="a8334-142">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="a8334-143">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8334-143">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="a8334-144">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a8334-144">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="a8334-145">Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a8334-145">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="a8334-146">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-146">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="a8334-147">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-147">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="a8334-148">Konfigurera privat Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-148">Configure Azure private peering for hello circuit.</span></span> <span data-ttu-id="a8334-149">Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-149">Make sure that you have hello following items before you proceed with hello next steps:</span></span>

  * <span data-ttu-id="a8334-150">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-150">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="a8334-151">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a8334-151">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="a8334-152">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-152">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="a8334-153">hello undernät får inte vara en del av alla adressutrymme som är reserverade för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="a8334-153">hello subnet must not be part of any address space reserved for virtual networks.</span></span>
  * <span data-ttu-id="a8334-154">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="a8334-154">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="a8334-155">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="a8334-155">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="a8334-156">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="a8334-156">AS number for peering.</span></span> <span data-ttu-id="a8334-157">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="a8334-157">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="a8334-158">Du kan använda ett privat AS-tal för den här peeringen.</span><span class="sxs-lookup"><span data-stu-id="a8334-158">You can use a private AS number for this peering.</span></span> <span data-ttu-id="a8334-159">Se till att du inte använder 65515.</span><span class="sxs-lookup"><span data-stu-id="a8334-159">Ensure that you are not using 65515.</span></span>
  * <span data-ttu-id="a8334-160">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="a8334-160">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="a8334-161">Använd följande exempel tooconfigure Azure privat peering för kretsen hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-161">Use hello following example tooconfigure Azure private peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="a8334-162">Om du väljer toouse en MD5-hash, Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-162">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="a8334-163">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="a8334-163">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="a8334-164">tooview Azure privat peering information</span><span class="sxs-lookup"><span data-stu-id="a8334-164">tooview Azure private peering details</span></span>

<span data-ttu-id="a8334-165">Du kan få konfigurationsinformation genom att använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-165">You can get configuration details by using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt
```

### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="a8334-166">tooupdate Azure privat peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="a8334-166">tooupdate Azure private peering configuration</span></span>

<span data-ttu-id="a8334-167">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8334-167">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="a8334-168">I det här exemplet uppdateras hello VLAN-ID för hello krets från 100 too500.</span><span class="sxs-lookup"><span data-stu-id="a8334-168">In this example, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="a8334-169">toodelete privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-169">toodelete Azure private peering</span></span>

<span data-ttu-id="a8334-170">Du kan ta bort peering konfigurationen genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-170">You can remove your peering configuration by running hello following example:</span></span>

> [!WARNING]
> <span data-ttu-id="a8334-171">Du måste se till att alla virtuella nätverk avlänkas från hello ExpressRoute-krets innan du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a8334-171">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this example.</span></span> 
> 
> 

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-public-peering"></a><span data-ttu-id="a8334-172">Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="a8334-172">Azure public peering</span></span>

<span data-ttu-id="a8334-173">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-173">This section helps you create, get, update, and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="a8334-174">toocreate offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-174">toocreate Azure public peering</span></span>

1. <span data-ttu-id="a8334-175">Importera hello PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="a8334-175">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="a8334-176">Du måste installera hello senaste PowerShell installer från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera hello Azure Resource Manager moduler till hello PowerShell-session i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a8334-176">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="a8334-177">Du behöver toorun PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8334-177">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
```

  <span data-ttu-id="a8334-178">Importera alla hello AzureRM.* moduler i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-178">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="a8334-179">Du kan också importera en väljer modul i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-179">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
```

  <span data-ttu-id="a8334-180">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="a8334-180">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="a8334-181">Välj hello-prenumeration som du vill toocreate ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a8334-181">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
  Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="a8334-182">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-182">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="a8334-183">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="a8334-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="a8334-184">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="a8334-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="a8334-185">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8334-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="a8334-186">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a8334-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="a8334-187">Kontrollera hello ExpressRoute-kretsen tooensure etablerats och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a8334-187">Check hello ExpressRoute circuit tooensure it is provisioned and also enabled.</span></span> <span data-ttu-id="a8334-188">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-188">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="a8334-189">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-189">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="a8334-190">Konfigurera offentlig Azure-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-190">Configure Azure public peering for hello circuit.</span></span> <span data-ttu-id="a8334-191">Kontrollera att du har följande information innan du fortsätter ytterligare hello.</span><span class="sxs-lookup"><span data-stu-id="a8334-191">Make sure that you have hello following information before you proceed further.</span></span>

  * <span data-ttu-id="a8334-192">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-192">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="a8334-193">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="a8334-193">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="a8334-194">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-194">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="a8334-195">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="a8334-195">This must be a valid public IPv4 prefix.</span></span>
  * <span data-ttu-id="a8334-196">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="a8334-196">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="a8334-197">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="a8334-197">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="a8334-198">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="a8334-198">AS number for peering.</span></span> <span data-ttu-id="a8334-199">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="a8334-199">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="a8334-200">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="a8334-200">**Optional -** An MD5 hash if you choose toouse one.</span></span>

  <span data-ttu-id="a8334-201">Kör följande exempel tooconfigure Azure offentlig peering för kretsen hello</span><span class="sxs-lookup"><span data-stu-id="a8334-201">Run hello following example tooconfigure Azure public peering for your circuit</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  <span data-ttu-id="a8334-202">Om du väljer toouse en MD5-hash, Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-202">If you choose toouse an MD5 hash, use hello following example:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

  > [!IMPORTANT]
  > <span data-ttu-id="a8334-203">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="a8334-203">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
  > 
  >

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="a8334-204">tooview Azure offentlig peering information</span><span class="sxs-lookup"><span data-stu-id="a8334-204">tooview Azure public peering details</span></span>

<span data-ttu-id="a8334-205">Du kan hämta konfigurationsinformation med hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a8334-205">You can get configuration details using hello following cmdlet:</span></span>

```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

  Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt
  ```

### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="a8334-206">tooupdate Azure offentlig peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="a8334-206">tooupdate Azure public peering configuration</span></span>

<span data-ttu-id="a8334-207">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="a8334-207">You can update any part of hello configuration using hello following example.</span></span> <span data-ttu-id="a8334-208">I det här exemplet uppdateras hello VLAN-ID för hello krets från 200 too600.</span><span class="sxs-lookup"><span data-stu-id="a8334-208">In this example, hello VLAN ID of hello circuit is being updated from 200 too600.</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="a8334-209">toodelete offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-209">toodelete Azure public peering</span></span>

<span data-ttu-id="a8334-210">Du kan ta bort peering konfigurationen genom att köra hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-210">You can remove your peering configuration by running hello following example:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="microsoft-peering"></a><span data-ttu-id="a8334-211">Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-211">Microsoft peering</span></span>

<span data-ttu-id="a8334-212">Det här avsnittet hjälper dig att skapa, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-212">This section helps you create, get, update, and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8334-213">Microsoft-peering i ExpressRoute-kretsar som har konfigurerats tidigare tooAugust 1, 2017 kommer ha alla service prefix annonserade via Microsoft hello-peering, även om filter routning inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="a8334-213">Microsoft peering of ExpressRoute circuits that were configured prior tooAugust 1, 2017 will have all service prefixes advertised through hello Microsoft peering, even if route filters are not defined.</span></span> <span data-ttu-id="a8334-214">Microsoft-peering i ExpressRoute-kretsar som är konfigurerade på eller efter den 1 augusti 2017 har inte alla prefix annonserade tills ett flöde filter ansluts toohello krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-214">Microsoft peering of ExpressRoute circuits that are configured on or after August 1, 2017 will not have any prefixes advertised until a route filter is attached toohello circuit.</span></span> <span data-ttu-id="a8334-215">Mer information finns i [konfigurera route filter för Microsoft-peering](how-to-routefilter-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a8334-215">For more information, see [Configure a route filter for Microsoft peering](how-to-routefilter-powershell.md).</span></span>
> 
> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="a8334-216">toocreate Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-216">toocreate Microsoft peering</span></span>

1. <span data-ttu-id="a8334-217">Importera hello PowerShell-modulen för ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="a8334-217">Import hello PowerShell module for ExpressRoute.</span></span>

  <span data-ttu-id="a8334-218">Du måste installera hello senaste PowerShell installer från [PowerShell-galleriet](http://www.powershellgallery.com/) och importera hello Azure Resource Manager moduler till hello PowerShell-session i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="a8334-218">You must install hello latest PowerShell installer from [PowerShell Gallery](http://www.powershellgallery.com/) and import hello Azure Resource Manager modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="a8334-219">Du behöver toorun PowerShell som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8334-219">You will need toorun PowerShell as an Administrator.</span></span>

  ```powershell
  Install-Module AzureRM

  Install-AzureRM
  ```

  <span data-ttu-id="a8334-220">Importera alla hello AzureRM.* moduler i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-220">Import all of hello AzureRM.* modules within hello known semantic version range.</span></span>

  ```powershell
  Import-AzureRM
  ```

  <span data-ttu-id="a8334-221">Du kan också importera en väljer modul i hello kända semantiska versionsintervall.</span><span class="sxs-lookup"><span data-stu-id="a8334-221">You can also just import a select module within hello known semantic version range.</span></span>

  ```powershell
  Import-Module AzureRM.Network
  ```

  <span data-ttu-id="a8334-222">Logga in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="a8334-222">Sign in tooyour account.</span></span>

  ```powershell
  Login-AzureRmAccount
  ```

  <span data-ttu-id="a8334-223">Välj hello-prenumeration som du vill toocreate ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="a8334-223">Select hello subscription you want toocreate ExpressRoute circuit.</span></span>

  ```powershell
Select-AzureRmSubscription -SubscriptionId "<subscription ID>"
  ```
2. <span data-ttu-id="a8334-224">Skapa en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-224">Create an ExpressRoute circuit.</span></span>

  <span data-ttu-id="a8334-225">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-arm.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="a8334-225">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-arm.md) and have it provisioned by hello connectivity provider.</span></span>

  <span data-ttu-id="a8334-226">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="a8334-226">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="a8334-227">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8334-227">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="a8334-228">Om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets fortsätta konfigurationen med hjälp av hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="a8334-228">However, if your connectivity provider does not manage routing for you, after creating your circuit, continue your configuration using hello next steps.</span></span>
3. <span data-ttu-id="a8334-229">Kontrollera hello ExpressRoute-kretsen toomake är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a8334-229">Check hello ExpressRoute circuit toomake sure it is provisioned and also enabled.</span></span> <span data-ttu-id="a8334-230">Använd följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-230">Use hello following example:</span></span>

  ```powershell
  Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```

  <span data-ttu-id="a8334-231">hello svaret är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-231">hello response is similar toohello following example:</span></span>

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
4. <span data-ttu-id="a8334-232">Konfigurera Microsoft-peering för hello krets.</span><span class="sxs-lookup"><span data-stu-id="a8334-232">Configure Microsoft peering for hello circuit.</span></span> <span data-ttu-id="a8334-233">Kontrollera att du har hello följande information innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="a8334-233">Make sure that you have hello following information before you proceed.</span></span>

  * <span data-ttu-id="a8334-234">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-234">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="a8334-235">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="a8334-235">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="a8334-236">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="a8334-236">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="a8334-237">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="a8334-237">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
  * <span data-ttu-id="a8334-238">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="a8334-238">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="a8334-239">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="a8334-239">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
  * <span data-ttu-id="a8334-240">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="a8334-240">AS number for peering.</span></span> <span data-ttu-id="a8334-241">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="a8334-241">You can use both 2-byte and 4-byte AS numbers.</span></span>
  * <span data-ttu-id="a8334-242">Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="a8334-242">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="a8334-243">Endast offentliga IP-adressprefix accepteras.</span><span class="sxs-lookup"><span data-stu-id="a8334-243">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="a8334-244">Om du planerar toosend en uppsättning prefix, kan du skicka en kommaavgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="a8334-244">If you plan toosend a set of prefixes, you can send a comma-separated list.</span></span> <span data-ttu-id="a8334-245">Dessa prefix måste vara registrerade tooyou i en RIR / IRR.</span><span class="sxs-lookup"><span data-stu-id="a8334-245">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
  * <span data-ttu-id="a8334-246">**Valfritt -** kunden ASN: Om du är reklam prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.</span><span class="sxs-lookup"><span data-stu-id="a8334-246">**Optional -** Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span>
  * <span data-ttu-id="a8334-247">Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.</span><span class="sxs-lookup"><span data-stu-id="a8334-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
  * <span data-ttu-id="a8334-248">**Valfritt -** en MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="a8334-248">**Optional -** An MD5 hash if you choose toouse one.</span></span>

   <span data-ttu-id="a8334-249">Använd följande exempel tooconfigure Microsoft-peering för kretsen hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-249">Use hello following example tooconfigure Microsoft peering for your circuit:</span></span>

  ```powershell
  Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```

### <a name="tooget-microsoft-peering-details"></a><span data-ttu-id="a8334-250">tooget Microsoft peering information</span><span class="sxs-lookup"><span data-stu-id="a8334-250">tooget Microsoft peering details</span></span>

<span data-ttu-id="a8334-251">Du kan hämta konfigurationsinformation med hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-251">You can get configuration details using hello following example:</span></span>

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="a8334-252">tooupdate Microsoft peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="a8334-252">tooupdate Microsoft peering configuration</span></span>

<span data-ttu-id="a8334-253">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a8334-253">You can update any part of hello configuration using hello following example:</span></span>

```powershell
Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="a8334-254">toodelete Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="a8334-254">toodelete Microsoft peering</span></span>

<span data-ttu-id="a8334-255">Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello:</span><span class="sxs-lookup"><span data-stu-id="a8334-255">You can remove your peering configuration by running hello following cmdlet:</span></span>

```powershell
Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a><span data-ttu-id="a8334-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8334-256">Next steps</span></span>

<span data-ttu-id="a8334-257">Nästa steg [länka VNet-tooan ExpressRoute-krets](expressroute-howto-linkvnet-arm.md).</span><span class="sxs-lookup"><span data-stu-id="a8334-257">Next step, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-arm.md).</span></span>

* <span data-ttu-id="a8334-258">Mer information om ExpressRoute-arbetsflöden finns i [ExpressRoute-arbetsflöden](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="a8334-258">For more information about ExpressRoute workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="a8334-259">Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="a8334-259">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>
* <span data-ttu-id="a8334-260">Mer information om hur du arbetar med virtuella nätverk finns i [Översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a8334-260">For more information about working with virtual networks, see [Virtual network overview](../virtual-network/virtual-networks-overview.md).</span></span>
