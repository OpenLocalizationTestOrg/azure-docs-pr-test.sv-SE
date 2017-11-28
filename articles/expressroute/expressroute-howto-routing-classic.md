---
title: "Hur tooconfigure routning (peering) för en ExpressRoute-krets: Azure: klassisk | Microsoft Docs"
description: "Den här artikeln vägleder dig genom hello steg för att skapa och etablera hello private, offentlig och Microsoft-peering i ExpressRoute-kretsen. Den här artikeln beskriver också hur toocheck hello status, uppdatera eller ta bort peerkopplingar för kretsen."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: a4bd39d2-373a-467a-8b06-36cfcc1027d2
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: dc5bcc4b86c3bc965a88281b6c2a660f3bc58eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit-classic"></a><span data-ttu-id="cfd29-104">Skapa och ändra peering för en ExpressRoute-krets (klassisk)</span><span class="sxs-lookup"><span data-stu-id="cfd29-104">Create and modify peering for an ExpressRoute circuit (classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cfd29-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cfd29-105">Azure portal</span></span>](expressroute-howto-routing-portal-resource-manager.md)
> * [<span data-ttu-id="cfd29-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cfd29-106">PowerShell</span></span>](expressroute-howto-routing-arm.md)
> * [<span data-ttu-id="cfd29-107">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="cfd29-107">Azure CLI</span></span>](howto-routing-cli.md)
> * [<span data-ttu-id="cfd29-108">Video - privat peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-108">Video - Private peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="cfd29-109">Video - offentlig peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-109">Video - Public peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-public-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="cfd29-110">Video - Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-110">Video - Microsoft peering</span></span>](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [<span data-ttu-id="cfd29-111">PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="cfd29-111">PowerShell (classic)</span></span>](expressroute-howto-routing-classic.md)
> 

<span data-ttu-id="cfd29-112">Den här artikeln vägleder dig genom hello steg toocreate och hantera routningskonfiguration för en ExpressRoute-krets med PowerShell och hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-112">This article walks you through hello steps toocreate and manage routing configuration for an ExpressRoute circuit using PowerShell and hello classic deployment model.</span></span> <span data-ttu-id="cfd29-113">hello stegen nedan visas också hur toocheck hello status, uppdatera, eller ta bort och ta bort etableringen av peerkopplingar för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-113">hello steps below will also show you how toocheck hello status, update, or delete and deprovision peerings for an ExpressRoute circuit.</span></span>

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

<span data-ttu-id="cfd29-114">**Om distributionsmodeller för Azure**</span><span class="sxs-lookup"><span data-stu-id="cfd29-114">**About Azure deployment models**</span></span>

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="configuration-prerequisites"></a><span data-ttu-id="cfd29-115">Förutsättningar för konfiguration</span><span class="sxs-lookup"><span data-stu-id="cfd29-115">Configuration prerequisites</span></span>
* <span data-ttu-id="cfd29-116">Du behöver hello senaste versionen av hello Azure Service Management (SM) PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-116">You will need hello latest version of hello Azure Service Management (SM) PowerShell cmdlets.</span></span> <span data-ttu-id="cfd29-117">Mer information finns i [komma igång med Azure PowerShell-cmdlets](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cfd29-117">For more information, see [Getting started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>  
* <span data-ttu-id="cfd29-118">Kontrollera att du har granskat hello [krav](expressroute-prerequisites.md) sidan hello [routningskrav](expressroute-routing.md) sida och hello [arbetsflöden](expressroute-workflows.md) sidan innan du börjar konfigurera.</span><span class="sxs-lookup"><span data-stu-id="cfd29-118">Make sure that you have reviewed hello [prerequisites](expressroute-prerequisites.md) page, hello [routing requirements](expressroute-routing.md) page, and hello [workflows](expressroute-workflows.md) page before you begin configuration.</span></span>
* <span data-ttu-id="cfd29-119">Du måste ha en aktiv ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-119">You must have an active ExpressRoute circuit.</span></span> <span data-ttu-id="cfd29-120">Följ instruktionerna för hello för[skapar du en ExpressRoute-krets](expressroute-howto-circuit-classic.md) och ha hello krets aktiveras med anslutningsleverantören innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="cfd29-120">Follow hello instructions too[create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have hello circuit enabled by your connectivity provider before you proceed.</span></span> <span data-ttu-id="cfd29-121">Hej ExpressRoute-krets måste vara i ett etablerat och aktiverade tillstånd du toobe kan toorun hello-cmdlets som beskrivs nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-121">hello ExpressRoute circuit must be in a provisioned and enabled state for you toobe able toorun hello cmdlets described below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfd29-122">Dessa anvisningar gäller endast toocircuits som skapats med leverantörer som erbjuder tjänster nivå 2.</span><span class="sxs-lookup"><span data-stu-id="cfd29-122">These instructions only apply toocircuits created with service providers offering Layer 2 connectivity services.</span></span> <span data-ttu-id="cfd29-123">Om du använder en Internet-leverantör som erbjuder hanteringstjänster till Layer 3 (vanligtvis en IPVPN, t.ex. MPLS) kommer anslutningsleverantören konfigurera och hantera routning för dig.</span><span class="sxs-lookup"><span data-stu-id="cfd29-123">If you are using a service provider offering managed Layer 3 services (typically an IPVPN, like MPLS), your connectivity provider will configure and manage routing for you.</span></span>
> 
> 

<span data-ttu-id="cfd29-124">Du kan konfigurera en, två eller alla tre peerings (Azure privat, Azure offentlig och Microsoft) för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-124">You can configure one, two, or all three peerings (Azure private, Azure public and Microsoft) for an ExpressRoute circuit.</span></span> <span data-ttu-id="cfd29-125">Du kan konfigurera peerings i valfri ordning.</span><span class="sxs-lookup"><span data-stu-id="cfd29-125">You can configure peerings in any order you choose.</span></span> <span data-ttu-id="cfd29-126">Du måste dock se till att du har slutfört hello konfigurationen för varje peering en i taget.</span><span class="sxs-lookup"><span data-stu-id="cfd29-126">However, you must make sure that you complete hello configuration of each peering one at a time.</span></span>


### <a name="log-in-tooyour-azure-account-and-select-a-subscription"></a><span data-ttu-id="cfd29-127">Logga in tooyour Azure-konto och välj en prenumeration</span><span class="sxs-lookup"><span data-stu-id="cfd29-127">Log in tooyour Azure account and select a subscription</span></span>
1. <span data-ttu-id="cfd29-128">Öppna PowerShell-konsol med utökade behörigheter och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="cfd29-128">Open your PowerShell console with elevated rights and connect tooyour account.</span></span> <span data-ttu-id="cfd29-129">Använd hello följande exempel toohelp du ansluta:</span><span class="sxs-lookup"><span data-stu-id="cfd29-129">Use hello following example toohelp you connect:</span></span>

        Login-AzureRmAccount

2. <span data-ttu-id="cfd29-130">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="cfd29-130">Check hello subscriptions for hello account.</span></span>

        Get-AzureRmSubscription

3. <span data-ttu-id="cfd29-131">Om du har mer än en prenumeration väljer du hello prenumeration som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="cfd29-131">If you have more than one subscription, select hello subscription that you want toouse.</span></span>

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

4. <span data-ttu-id="cfd29-132">Använd sedan följande cmdlet tooadd hello tooPowerShell din Azure-prenumeration för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-132">Next, use hello following cmdlet tooadd your Azure subscription tooPowerShell for hello classic deployment model.</span></span>

        Add-AzureAccount


## <a name="azure-private-peering"></a><span data-ttu-id="cfd29-133">Azures privata peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-133">Azure private peering</span></span>
<span data-ttu-id="cfd29-134">Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Azure privat peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-134">This section provides instructions on how toocreate, get, update, and delete hello Azure private peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-azure-private-peering"></a><span data-ttu-id="cfd29-135">toocreate privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-135">toocreate Azure private peering</span></span>
1. <span data-ttu-id="cfd29-136">**Importera hello PowerShell-modulen för ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-136">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="cfd29-137">Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="cfd29-137">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="cfd29-138">Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-138">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="cfd29-139">**Skapa en ExpressRoute-krets.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-139">**Create an ExpressRoute circuit.**</span></span>
   
    <span data-ttu-id="cfd29-140">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="cfd29-140">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="cfd29-141">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="cfd29-141">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="cfd29-142">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cfd29-142">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="cfd29-143">Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-143">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span> 
3. <span data-ttu-id="cfd29-144">**Kontrollera hello ExpressRoute-kretsen tooensure den har etablerats.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-144">**Check hello ExpressRoute circuit tooensure it is provisioned.**</span></span>
   
    <span data-ttu-id="cfd29-145">Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cfd29-145">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="cfd29-146">Se hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-146">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="cfd29-147">Kontrollera att kretsen hello visas som etablerad och aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cfd29-147">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="cfd29-148">Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.</span><span class="sxs-lookup"><span data-stu-id="cfd29-148">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="cfd29-149">**Konfigurera privat Azure-peering för hello krets.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-149">**Configure Azure private peering for hello circuit.**</span></span>
   
    <span data-ttu-id="cfd29-150">Kontrollera att du har följande objekt innan du fortsätter med nästa steg i hello hello:</span><span class="sxs-lookup"><span data-stu-id="cfd29-150">Make sure that you have hello following items before you proceed with hello next steps:</span></span>
   
   * <span data-ttu-id="cfd29-151">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-151">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="cfd29-152">Detta får inte vara en del av något adressutrymme som reserverats för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="cfd29-152">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="cfd29-153">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-153">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="cfd29-154">Detta får inte vara en del av något adressutrymme som reserverats för virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="cfd29-154">This must not be part of any address space reserved for virtual networks.</span></span>
   * <span data-ttu-id="cfd29-155">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="cfd29-155">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="cfd29-156">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="cfd29-156">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="cfd29-157">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="cfd29-157">AS number for peering.</span></span> <span data-ttu-id="cfd29-158">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="cfd29-158">You can use both 2-byte and 4-byte AS numbers.</span></span> <span data-ttu-id="cfd29-159">Du kan använda ett privat AS-tal för den här peeringen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-159">You can use a private AS number for this peering.</span></span> <span data-ttu-id="cfd29-160">Se till att du inte använder 65515.</span><span class="sxs-lookup"><span data-stu-id="cfd29-160">Ensure that you are not using 65515.</span></span>
   * <span data-ttu-id="cfd29-161">En MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="cfd29-161">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="cfd29-162">**Det här är valfritt**.</span><span class="sxs-lookup"><span data-stu-id="cfd29-162">**This is optional**.</span></span>
     
    <span data-ttu-id="cfd29-163">Du kan köra följande cmdlet tooconfigure privat Azure-peering för kretsen hello.</span><span class="sxs-lookup"><span data-stu-id="cfd29-163">You can run hello following cmdlet tooconfigure Azure private peering for your circuit.</span></span>
     
        <span data-ttu-id="cfd29-164">Nya AzureBGPPeering - AccessType privat - ServiceKey ”***” - PrimaryPeerSubnet ”10.0.0.0/30” - SecondaryPeerSubnet ”10.0.0.4/30” - PeerAsn 1234 - VlanId 100</span><span class="sxs-lookup"><span data-stu-id="cfd29-164">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100</span></span>
     
    <span data-ttu-id="cfd29-165">Du kan använda hello-cmdlet nedan om du väljer toouse en MD5-hash.</span><span class="sxs-lookup"><span data-stu-id="cfd29-165">You can use hello cmdlet below if you choose toouse an MD5 hash.</span></span>
     
        <span data-ttu-id="cfd29-166">Nya AzureBGPPeering - AccessType privat - ServiceKey ”***” - PrimaryPeerSubnet ”10.0.0.0/30” - SecondaryPeerSubnet ”10.0.0.4/30” - PeerAsn 1234 - VlanId 100 - SharedKey ”A1B2C3D4”</span><span class="sxs-lookup"><span data-stu-id="cfd29-166">New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="cfd29-167">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="cfd29-167">Ensure that you specify your AS number as peering ASN, not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-private-peering-details"></a><span data-ttu-id="cfd29-168">tooview Azure privat peering information</span><span class="sxs-lookup"><span data-stu-id="cfd29-168">tooview Azure private peering details</span></span>
<span data-ttu-id="cfd29-169">Du kan hämta konfigurationsinformation med följande cmdlet hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-169">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="tooupdate-azure-private-peering-configuration"></a><span data-ttu-id="cfd29-170">tooupdate Azure privat peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="cfd29-170">tooupdate Azure private peering configuration</span></span>
<span data-ttu-id="cfd29-171">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cfd29-171">You can update any part of hello configuration using hello following cmdlet.</span></span> <span data-ttu-id="cfd29-172">I hello exemplet nedan uppdateras hello VLAN-ID för hello krets från 100 too500.</span><span class="sxs-lookup"><span data-stu-id="cfd29-172">In hello example below, hello VLAN ID of hello circuit is being updated from 100 too500.</span></span>

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="toodelete-azure-private-peering"></a><span data-ttu-id="cfd29-173">toodelete privat Azure-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-173">toodelete Azure private peering</span></span>
<span data-ttu-id="cfd29-174">Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="cfd29-174">You can remove your peering configuration by running hello following cmdlet.</span></span>

> [!WARNING]
> <span data-ttu-id="cfd29-175">Du måste se till att alla virtuella nätverk avlänkas från hello ExpressRoute-krets innan du kör denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cfd29-175">You must ensure that all virtual networks are unlinked from hello ExpressRoute circuit before running this cmdlet.</span></span> 
> 
> 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a><span data-ttu-id="cfd29-176">Azures offentliga peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-176">Azure public peering</span></span>
<span data-ttu-id="cfd29-177">Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Azure offentlig peering-konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-177">This section provides instructions on how toocreate, get, update and delete hello Azure public peering configuration for an ExpressRoute circuit.</span></span>

### <a name="toocreate-azure-public-peering"></a><span data-ttu-id="cfd29-178">toocreate offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-178">toocreate Azure public peering</span></span>
1. <span data-ttu-id="cfd29-179">**Importera hello PowerShell-modulen för ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-179">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="cfd29-180">Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="cfd29-180">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="cfd29-181">Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-181">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span> 
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="cfd29-182">**Skapa en ExpressRoute-krets**</span><span class="sxs-lookup"><span data-stu-id="cfd29-182">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="cfd29-183">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="cfd29-183">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="cfd29-184">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure offentlig peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="cfd29-184">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure public peering for you.</span></span> <span data-ttu-id="cfd29-185">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cfd29-185">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="cfd29-186">Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-186">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="cfd29-187">**Kontrollera ExpressRoute-kretsen tooensure den har etablerats**</span><span class="sxs-lookup"><span data-stu-id="cfd29-187">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="cfd29-188">Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverats.</span><span class="sxs-lookup"><span data-stu-id="cfd29-188">You must first check toosee if hello ExpressRoute circuit is Provisioned and also Enabled.</span></span> <span data-ttu-id="cfd29-189">Se hello exemplet nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-189">See hello example below.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="cfd29-190">Kontrollera att kretsen hello visas som etablerad och aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cfd29-190">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="cfd29-191">Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.</span><span class="sxs-lookup"><span data-stu-id="cfd29-191">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="cfd29-192">**Konfigurera offentlig Azure-peering för hello-krets**</span><span class="sxs-lookup"><span data-stu-id="cfd29-192">**Configure Azure public peering for hello circuit**</span></span>
   
    <span data-ttu-id="cfd29-193">Se till att du har följande information innan du fortsätter ytterligare hello.</span><span class="sxs-lookup"><span data-stu-id="cfd29-193">Ensure that you have hello following information before you proceed further.</span></span>
   
   * <span data-ttu-id="cfd29-194">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-194">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="cfd29-195">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="cfd29-195">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="cfd29-196">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-196">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="cfd29-197">Detta måste vara ett giltigt offentligt IPv4-prefix.</span><span class="sxs-lookup"><span data-stu-id="cfd29-197">This must be a valid public IPv4 prefix.</span></span>
   * <span data-ttu-id="cfd29-198">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="cfd29-198">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="cfd29-199">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="cfd29-199">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="cfd29-200">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="cfd29-200">AS number for peering.</span></span> <span data-ttu-id="cfd29-201">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="cfd29-201">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="cfd29-202">En MD5-hash om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="cfd29-202">An MD5 hash if you choose toouse one.</span></span> <span data-ttu-id="cfd29-203">**Det här är valfritt**.</span><span class="sxs-lookup"><span data-stu-id="cfd29-203">**This is optional**.</span></span>
     
    <span data-ttu-id="cfd29-204">Du kan köra följande cmdlet tooconfigure offentlig Azure-peering för kretsen hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-204">You can run hello following cmdlet tooconfigure Azure public peering for your circuit</span></span>
     
        <span data-ttu-id="cfd29-205">Nya AzureBGPPeering - AccessType offentliga - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - PeerAsn 1234 - VlanId 200</span><span class="sxs-lookup"><span data-stu-id="cfd29-205">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200</span></span>
     
    <span data-ttu-id="cfd29-206">Du kan använda hello-cmdlet nedan om du väljer toouse en MD5-hash</span><span class="sxs-lookup"><span data-stu-id="cfd29-206">You can use hello cmdlet below if you choose toouse an MD5 hash</span></span>
     
        <span data-ttu-id="cfd29-207">Nya AzureBGPPeering - AccessType offentliga - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - PeerAsn 1234 - VlanId 200 - SharedKey ”A1B2C3D4”</span><span class="sxs-lookup"><span data-stu-id="cfd29-207">New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"</span></span>
     
     > [!IMPORTANT]
     > <span data-ttu-id="cfd29-208">Kontrollera att du anger AS-talet som peering-ASN, inte kund-ASN.</span><span class="sxs-lookup"><span data-stu-id="cfd29-208">Ensure that you specify your AS number as peering ASN and not customer ASN.</span></span>
     > 
     > 

### <a name="tooview-azure-public-peering-details"></a><span data-ttu-id="cfd29-209">tooview Azure offentlig peering information</span><span class="sxs-lookup"><span data-stu-id="cfd29-209">tooview Azure public peering details</span></span>
<span data-ttu-id="cfd29-210">Du kan hämta konfigurationsinformation med följande cmdlet hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-210">You can get configuration details using hello following cmdlet</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="tooupdate-azure-public-peering-configuration"></a><span data-ttu-id="cfd29-211">tooupdate Azure offentlig peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="cfd29-211">tooupdate Azure public peering configuration</span></span>
<span data-ttu-id="cfd29-212">Du kan uppdatera någon del av hello-konfigurationen med hjälp av följande cmdlet hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-212">You can update any part of hello configuration using hello following cmdlet</span></span>

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

<span data-ttu-id="cfd29-213">hello VLAN-ID för hello krets uppdateras från 200 too600 i hello ovan exempel.</span><span class="sxs-lookup"><span data-stu-id="cfd29-213">hello VLAN ID of hello circuit is being updated from 200 too600 in hello above example.</span></span>

### <a name="toodelete-azure-public-peering"></a><span data-ttu-id="cfd29-214">toodelete offentlig Azure-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-214">toodelete Azure public peering</span></span>
<span data-ttu-id="cfd29-215">Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-215">You can remove your peering configuration by running hello following cmdlet</span></span>

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a><span data-ttu-id="cfd29-216">Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-216">Microsoft peering</span></span>
<span data-ttu-id="cfd29-217">Det här avsnittet innehåller instruktioner om hur toocreate, hämta, uppdatera och ta bort hello Microsoft peering konfiguration för en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="cfd29-217">This section provides instructions on how toocreate, get, update and delete hello Microsoft peering configuration for an ExpressRoute circuit.</span></span> 

### <a name="toocreate-microsoft-peering"></a><span data-ttu-id="cfd29-218">toocreate Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-218">toocreate Microsoft peering</span></span>
1. <span data-ttu-id="cfd29-219">**Importera hello PowerShell-modulen för ExpressRoute.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-219">**Import hello PowerShell module for ExpressRoute.**</span></span>
   
    <span data-ttu-id="cfd29-220">Du måste importera hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen i ordning toostart med hello ExpressRoute-cmdletar.</span><span class="sxs-lookup"><span data-stu-id="cfd29-220">You must import hello Azure and ExpressRoute modules into hello PowerShell session in order toostart using hello ExpressRoute cmdlets.</span></span> <span data-ttu-id="cfd29-221">Kör hello följande kommandon tooimport hello Azure och ExpressRoute-moduler i hello PowerShell-sessionen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-221">Run hello following commands tooimport hello Azure and ExpressRoute modules into hello PowerShell session.</span></span>  
   
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'
2. <span data-ttu-id="cfd29-222">**Skapa en ExpressRoute-krets**</span><span class="sxs-lookup"><span data-stu-id="cfd29-222">**Create an ExpressRoute circuit**</span></span>
   
    <span data-ttu-id="cfd29-223">Följ hello instruktioner toocreate en [ExpressRoute-krets](expressroute-howto-circuit-classic.md) och har etablerats av hello anslutning providern.</span><span class="sxs-lookup"><span data-stu-id="cfd29-223">Follow hello instructions toocreate an [ExpressRoute circuit](expressroute-howto-circuit-classic.md) and have it provisioned by hello connectivity provider.</span></span> <span data-ttu-id="cfd29-224">Om anslutningsleverantören erbjuder hanteringstjänster Layer 3, kan du begära din anslutning providern tooenable Azure privat peering åt dig.</span><span class="sxs-lookup"><span data-stu-id="cfd29-224">If your connectivity provider offers managed Layer 3 services, you can request your connectivity provider tooenable Azure private peering for you.</span></span> <span data-ttu-id="cfd29-225">Du behöver inte i så fall toofollow instruktioner som anges i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cfd29-225">In that case, you won't need toofollow instructions listed in hello next sections.</span></span> <span data-ttu-id="cfd29-226">Men om anslutningsleverantören inte kan hantera routning av du, när du har skapat din krets Följ hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="cfd29-226">However, if your connectivity provider does not manage routing for you, after creating your circuit, follow hello instructions below.</span></span>
3. <span data-ttu-id="cfd29-227">**Kontrollera ExpressRoute-kretsen tooensure den har etablerats**</span><span class="sxs-lookup"><span data-stu-id="cfd29-227">**Check ExpressRoute circuit tooensure it is provisioned**</span></span>
   
    <span data-ttu-id="cfd29-228">Du måste först kontrollera toosee om hello ExpressRoute-kretsen är etablerad och aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cfd29-228">You must first check toosee if hello ExpressRoute circuit is in Provisioned and Enabled state.</span></span>
   
        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"
   
        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled
   
    <span data-ttu-id="cfd29-229">Kontrollera att kretsen hello visas som etablerad och aktiverad.</span><span class="sxs-lookup"><span data-stu-id="cfd29-229">Make sure that hello circuit shows as Provisioned and Enabled.</span></span> <span data-ttu-id="cfd29-230">Om det inte fungera med din anslutning providern tooget din krets toohello krävs tillstånd och status.</span><span class="sxs-lookup"><span data-stu-id="cfd29-230">If it doesn't, work with your connectivity provider tooget your circuit toohello required state and status.</span></span>
   
        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled
4. <span data-ttu-id="cfd29-231">**Konfigurera Microsoft-peering för hello-krets**</span><span class="sxs-lookup"><span data-stu-id="cfd29-231">**Configure Microsoft peering for hello circuit**</span></span>
   
    <span data-ttu-id="cfd29-232">Kontrollera att du har hello följande information innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="cfd29-232">Make sure that you have hello following information before you proceed.</span></span>
   
   * <span data-ttu-id="cfd29-233">Ett/30-undernät för hello primära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-233">A /30 subnet for hello primary link.</span></span> <span data-ttu-id="cfd29-234">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="cfd29-234">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="cfd29-235">Ett/30-undernät för hello sekundära länken.</span><span class="sxs-lookup"><span data-stu-id="cfd29-235">A /30 subnet for hello secondary link.</span></span> <span data-ttu-id="cfd29-236">Detta måste vara ett giltigt offentligt IPv4-prefix som du äger och har registrerat i en RIR/IR.</span><span class="sxs-lookup"><span data-stu-id="cfd29-236">This must be a valid public IPv4 prefix owned by you and registered in an RIR / IRR.</span></span>
   * <span data-ttu-id="cfd29-237">Ett giltigt VLAN-ID-tooestablish denna peering på.</span><span class="sxs-lookup"><span data-stu-id="cfd29-237">A valid VLAN ID tooestablish this peering on.</span></span> <span data-ttu-id="cfd29-238">Kontrollera att ingen annan peering-session i kretsen hello hello använder samma VLAN-ID.</span><span class="sxs-lookup"><span data-stu-id="cfd29-238">Ensure that no other peering in hello circuit uses hello same VLAN ID.</span></span>
   * <span data-ttu-id="cfd29-239">AS-tal för peering.</span><span class="sxs-lookup"><span data-stu-id="cfd29-239">AS number for peering.</span></span> <span data-ttu-id="cfd29-240">Du kan använda både 2 byte och 4 byte som AS-tal.</span><span class="sxs-lookup"><span data-stu-id="cfd29-240">You can use both 2-byte and 4-byte AS numbers.</span></span>
   * <span data-ttu-id="cfd29-241">Annonserade prefix: du måste ange en lista över alla prefix du planerar tooadvertise över hello BGP-sessionen.</span><span class="sxs-lookup"><span data-stu-id="cfd29-241">Advertised prefixes: You must provide a list of all prefixes you plan tooadvertise over hello BGP session.</span></span> <span data-ttu-id="cfd29-242">Endast offentliga IP-adressprefix accepteras.</span><span class="sxs-lookup"><span data-stu-id="cfd29-242">Only public IP address prefixes are accepted.</span></span> <span data-ttu-id="cfd29-243">Du kan skicka en kommaavgränsad lista om du planerar toosend en uppsättning prefix.</span><span class="sxs-lookup"><span data-stu-id="cfd29-243">You can send a comma separated list if you plan toosend a set of prefixes.</span></span> <span data-ttu-id="cfd29-244">Dessa prefix måste vara registrerade tooyou i en RIR / IRR.</span><span class="sxs-lookup"><span data-stu-id="cfd29-244">These prefixes must be registered tooyou in an RIR / IRR.</span></span>
   * <span data-ttu-id="cfd29-245">Kund ASN: Om du annonserar prefix som inte är registrerade toohello peering som du kan ange hello som antalet toowhich som de har registrerats.</span><span class="sxs-lookup"><span data-stu-id="cfd29-245">Customer ASN: If you are advertising prefixes that are not registered toohello peering AS number, you can specify hello AS number toowhich they are registered.</span></span> <span data-ttu-id="cfd29-246">**Det här är valfritt**.</span><span class="sxs-lookup"><span data-stu-id="cfd29-246">**This is optional**.</span></span>
   * <span data-ttu-id="cfd29-247">Routning namn i registret: Du kan ange hello RIR / IRR mot vilken hello som antalet och prefix har registrerats.</span><span class="sxs-lookup"><span data-stu-id="cfd29-247">Routing Registry Name: You can specify hello RIR / IRR against which hello AS number and prefixes are registered.</span></span>
   * <span data-ttu-id="cfd29-248">En MD5-hash, om du väljer toouse en.</span><span class="sxs-lookup"><span data-stu-id="cfd29-248">An MD5 hash, if you choose toouse one.</span></span> <span data-ttu-id="cfd29-249">**Det här är valfritt.**</span><span class="sxs-lookup"><span data-stu-id="cfd29-249">**This is optional.**</span></span>
     
    <span data-ttu-id="cfd29-250">Du kan köra följande cmdlet tooconfigure Microsoft pering för kretsen hello</span><span class="sxs-lookup"><span data-stu-id="cfd29-250">You can run hello following cmdlet tooconfigure Microsoft pering for your circuit</span></span>
     
        <span data-ttu-id="cfd29-251">Nya AzureBGPPeering - AccessType Microsoft - ServiceKey ”***” - PrimaryPeerSubnet ”131.107.0.0/30” - SecondaryPeerSubnet ”131.107.0.4/30” - VlanId 300 - PeerAsn 1234 - CustomerAsn 2245 - AdvertisedPublicPrefixes ”123.0.0.0/30” - RoutingRegistryName ”ARIN” - SharedKey ”A1B2C3D4”</span><span class="sxs-lookup"><span data-stu-id="cfd29-251">New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"</span></span>

### <a name="tooview-microsoft-peering-details"></a><span data-ttu-id="cfd29-252">tooview Microsoft peering information</span><span class="sxs-lookup"><span data-stu-id="cfd29-252">tooview Microsoft peering details</span></span>
<span data-ttu-id="cfd29-253">Du kan hämta konfigurationsinformation med hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cfd29-253">You can get configuration details using hello following cmdlet.</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="tooupdate-microsoft-peering-configuration"></a><span data-ttu-id="cfd29-254">tooupdate Microsoft peering-konfiguration</span><span class="sxs-lookup"><span data-stu-id="cfd29-254">tooupdate Microsoft peering configuration</span></span>
<span data-ttu-id="cfd29-255">Du kan uppdatera någon del av hello-konfigurationen med hjälp av hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="cfd29-255">You can update any part of hello configuration using hello following cmdlet.</span></span>

    Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="toodelete-microsoft-peering"></a><span data-ttu-id="cfd29-256">toodelete Microsoft-peering</span><span class="sxs-lookup"><span data-stu-id="cfd29-256">toodelete Microsoft peering</span></span>
<span data-ttu-id="cfd29-257">Du kan ta bort peering konfigurationen genom att köra följande cmdlet hello.</span><span class="sxs-lookup"><span data-stu-id="cfd29-257">You can remove your peering configuration by running hello following cmdlet.</span></span>

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a><span data-ttu-id="cfd29-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cfd29-258">Next steps</span></span>
<span data-ttu-id="cfd29-259">Nästa [länka VNet-tooan ExpressRoute-krets](expressroute-howto-linkvnet-classic.md).</span><span class="sxs-lookup"><span data-stu-id="cfd29-259">Next, [Link a VNet tooan ExpressRoute circuit](expressroute-howto-linkvnet-classic.md).</span></span>

* <span data-ttu-id="cfd29-260">Mer information om arbetsflöden finns [ExpressRoute-arbetsflöden](expressroute-workflows.md).</span><span class="sxs-lookup"><span data-stu-id="cfd29-260">For more information about workflows, see [ExpressRoute workflows](expressroute-workflows.md).</span></span>
* <span data-ttu-id="cfd29-261">Mer information om krets-peering finns i [ExpressRoute-kretsar och routningsdomäner](expressroute-circuit-peerings.md).</span><span class="sxs-lookup"><span data-stu-id="cfd29-261">For more information about circuit peering, see [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md).</span></span>

