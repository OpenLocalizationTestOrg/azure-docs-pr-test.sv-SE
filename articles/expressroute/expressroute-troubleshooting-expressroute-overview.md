---
title: "Verifiera anslutningsbarhet: Azure ExpressRoute felsökningsguide för | Microsoft Docs"
description: "Den här sidan innehåller instruktioner för felsökning och verifiera slutet tooend anslutningen för en ExpressRoute-krets."
documentationcenter: na
services: expressroute
author: rambk
manager: tracsman
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/01/2017
ms.author: cherylmc
ms.openlocfilehash: 713c39c7eafd77a4380b2a91902a9686f2ce1d85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="verifying-expressroute-connectivity"></a><span data-ttu-id="33998-103">Verifiera en ExpressRoute-anslutning</span><span class="sxs-lookup"><span data-stu-id="33998-103">Verifying ExpressRoute connectivity</span></span>
<span data-ttu-id="33998-104">ExpressRoute som utökar ett lokalt nätverk till hello Microsoft cloud över en privat anslutning som möjliggörs med hjälp av en provider för anslutningen, omfattar följande tre separata nätverkszoner hello:</span><span class="sxs-lookup"><span data-stu-id="33998-104">ExpressRoute, which extends an on-premises network into hello Microsoft cloud over a private connection that is facilitated by a connectivity provider, involves hello following three distinct network zones:</span></span>

-   <span data-ttu-id="33998-105">Kundens nätverk</span><span class="sxs-lookup"><span data-stu-id="33998-105">Customer Network</span></span>
-   <span data-ttu-id="33998-106">Providern nätverk</span><span class="sxs-lookup"><span data-stu-id="33998-106">Provider Network</span></span>
-   <span data-ttu-id="33998-107">Microsoft-Datacenter</span><span class="sxs-lookup"><span data-stu-id="33998-107">Microsoft Datacenter</span></span>

<span data-ttu-id="33998-108">hello syftet med det här dokumentet är toohelp användaren tooidentify där (eller även om) ett anslutningsproblem finns och i vilken zon vilket tooseek hjälp från rätt team tooresolve hello problemet.</span><span class="sxs-lookup"><span data-stu-id="33998-108">hello purpose of this document is toohelp user tooidentify where (or even if) a connectivity issue exists and within which zone, thereby tooseek help from appropriate team tooresolve hello issue.</span></span> <span data-ttu-id="33998-109">Öppna ett supportärende med om Microsoft-supporten nödvändiga tooresolve problem [Microsoft-supporten][Support].</span><span class="sxs-lookup"><span data-stu-id="33998-109">If Microsoft support is needed tooresolve an issue, open a support ticket with [Microsoft Support][Support].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33998-110">Det här dokumentet är avsett toohelp diagnostisera och åtgärda problem med enkel.</span><span class="sxs-lookup"><span data-stu-id="33998-110">This document is intended toohelp diagnosing and fixing simple issues.</span></span> <span data-ttu-id="33998-111">Det är inte avsedda toobe en ersättning för Microsoft-supporten.</span><span class="sxs-lookup"><span data-stu-id="33998-111">It is not intended toobe a replacement for Microsoft support.</span></span> <span data-ttu-id="33998-112">Öppna ett supportärende med [Microsoft-supporten] [ Support] om du toosolve hello problemet med hjälp av hello riktlinjer som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="33998-112">Open a support ticket with [Microsoft Support][Support] if you are unable toosolve hello problem using hello guidance provided.</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="33998-113">Översikt</span><span class="sxs-lookup"><span data-stu-id="33998-113">Overview</span></span>
<span data-ttu-id="33998-114">hello visar följande diagram hello logiska anslutningen för en kund nätverk tooMicrosoft nätverk med hjälp av ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="33998-114">hello following diagram shows hello logical connectivity of a customer network tooMicrosoft network using ExpressRoute.</span></span>
<span data-ttu-id="33998-115">[![1]][1]</span><span class="sxs-lookup"><span data-stu-id="33998-115">[![1]][1]</span></span>

<span data-ttu-id="33998-116">I föregående diagram hello, visar hello nummer viktiga nätverket punkter.</span><span class="sxs-lookup"><span data-stu-id="33998-116">In hello preceding diagram, hello numbers indicate key network points.</span></span> <span data-ttu-id="33998-117">hello nätverket punkter refereras ofta via den här artikeln av deras associerade nummer.</span><span class="sxs-lookup"><span data-stu-id="33998-117">hello network points are referenced often through this article by their associated number.</span></span>

<span data-ttu-id="33998-118">Beroende på hello ExpressRoute-anslutningen modellen (moln Exchange samordning, Point-to-Point Ethernet-anslutning eller alla-till-alla (IPVPN)) hello nätverket punkter 3 och 4 kan vara växlar (nivå 2-enheter).</span><span class="sxs-lookup"><span data-stu-id="33998-118">Depending on hello ExpressRoute connectivity model (Cloud Exchange Co-location, Point-to-Point Ethernet Connection, or Any-to-any (IPVPN)) hello network points 3 and 4 may be switches (Layer 2 devices).</span></span> <span data-ttu-id="33998-119">hello viktiga nätverket punkter visas är följande:</span><span class="sxs-lookup"><span data-stu-id="33998-119">hello key network points illustrated are as follows:</span></span>

1.  <span data-ttu-id="33998-120">Kunden beräkning enheten (exempelvis en server eller dator)</span><span class="sxs-lookup"><span data-stu-id="33998-120">Customer compute device (for example, a server or PC)</span></span>
2.  <span data-ttu-id="33998-121">CEs: Klientens gränsroutrar</span><span class="sxs-lookup"><span data-stu-id="33998-121">CEs: Customer edge routers</span></span> 
3.  <span data-ttu-id="33998-122">Sämsta (CE inriktad): providern edge routrar/växlar som är riktade mot klientens gränsroutrar.</span><span class="sxs-lookup"><span data-stu-id="33998-122">PEs (CE facing): Provider edge routers/switches that are facing customer edge routers.</span></span> <span data-ttu-id="33998-123">Kallas tooas PE CEs i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="33998-123">Referred tooas PE-CEs in this document.</span></span>
4.  <span data-ttu-id="33998-124">Sämsta (MSEE inriktad): providern edge routrar/växlar som är riktade mot MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-124">PEs (MSEE facing): Provider edge routers/switches that are facing MSEEs.</span></span> <span data-ttu-id="33998-125">Kallas tooas PE MSEEs i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="33998-125">Referred tooas PE-MSEEs in this document.</span></span>
5.  <span data-ttu-id="33998-126">MSEEs: Microsoft Enterprise kant (MSEE) ExpressRoute routrar</span><span class="sxs-lookup"><span data-stu-id="33998-126">MSEEs: Microsoft Enterprise Edge (MSEE) ExpressRoute routers</span></span>
6.  <span data-ttu-id="33998-127">Gateway för virtuellt nätverk (VNet)</span><span class="sxs-lookup"><span data-stu-id="33998-127">Virtual Network (VNet) Gateway</span></span>
7.  <span data-ttu-id="33998-128">Compute-enheten på hello Azure VNet</span><span class="sxs-lookup"><span data-stu-id="33998-128">Compute device on hello Azure VNet</span></span>

<span data-ttu-id="33998-129">Om hello molnet Exchange samplacering eller Point-to-Point Ethernet-anslutning anslutningen modeller används skulle hello klientens gränsrouter (2) upprätta BGP-peering med MSEEs (5).</span><span class="sxs-lookup"><span data-stu-id="33998-129">If hello Cloud Exchange Co-location or Point-to-Point Ethernet Connection connectivity models are used, hello customer edge router (2) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="33998-130">Nätverket punkterna 3 och 4 skulle fortfarande finns men är något transparent som Lager2-enheter.</span><span class="sxs-lookup"><span data-stu-id="33998-130">Network points 3 and 4 would still exist but be somewhat transparent as Layer 2 devices.</span></span>

<span data-ttu-id="33998-131">Om hello alla-till-alla (IPVPN) anslutningen mall används hello PEs (MSEE inriktad) (4) skulle upprätta BGP-peering med MSEEs (5).</span><span class="sxs-lookup"><span data-stu-id="33998-131">If hello Any-to-any (IPVPN) connectivity model is used, hello PEs (MSEE facing) (4) would establish BGP peering with MSEEs (5).</span></span> <span data-ttu-id="33998-132">Vägar skulle sedan sprider tillbaka toohello kundens nätverk via hello IPVPN service provider nätverk.</span><span class="sxs-lookup"><span data-stu-id="33998-132">Routes would then propagate back toohello customer network via hello IPVPN service provider network.</span></span>

>[!NOTE]
><span data-ttu-id="33998-133">Microsoft kräver ett redundant BGP-sessioner mellan MSEEs (5) och PE-MSEEs (4)-par för ExpressRoute med hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="33998-133">For ExpressRoute high availability, Microsoft requires a redundant pair of BGP sessions between MSEEs (5) and PE-MSEEs (4).</span></span> <span data-ttu-id="33998-134">Ett redundant par nätverkssökvägar rekommenderas också mellan kundens nätverk och PE CEs.</span><span class="sxs-lookup"><span data-stu-id="33998-134">A redundant pair of network paths is also encouraged between customer network and PE-CEs.</span></span> <span data-ttu-id="33998-135">I modellen för alla-till-alla (IPVPN)-anslutning kanske en enskild CE-enhet (2) men anslutna tooone eller mer PEs (3).</span><span class="sxs-lookup"><span data-stu-id="33998-135">However, in Any-to-any (IPVPN) connection model, a single CE device (2) may be connected tooone or more PEs (3).</span></span>
>
>

<span data-ttu-id="33998-136">toovalidate en ExpressRoute-krets hello följande steg beskrivs (med hello nätverkspunkt som anges av hello associerade nummer):</span><span class="sxs-lookup"><span data-stu-id="33998-136">toovalidate an ExpressRoute circuit, hello following steps are covered (with hello network point indicated by hello associated number):</span></span>
1. [<span data-ttu-id="33998-137">Validera kretsetablering och tillstånd (5)</span><span class="sxs-lookup"><span data-stu-id="33998-137">Validate circuit provisioning and state (5)</span></span>](#validate-circuit-provisioning-and-state)
2. [<span data-ttu-id="33998-138">Verifiera minst en ExpressRoute-peering är konfigurerad (5)</span><span class="sxs-lookup"><span data-stu-id="33998-138">Validate at least one ExpressRoute peering is configured (5)</span></span>](#validate-peering-configuration)
3. [<span data-ttu-id="33998-139">Validera ARP mellan Microsoft och hello-leverantör (länk mellan 4 och 5)</span><span class="sxs-lookup"><span data-stu-id="33998-139">Validate ARP between Microsoft and hello service provider (link between 4 and 5)</span></span>](#validate-arp-between-microsoft-and-the-service-provider)
4. [<span data-ttu-id="33998-140">Validera BGP och vägar på hello MSEE (BGP mellan too5 4 och 5 too6 om ett virtuellt nätverk är ansluten)</span><span class="sxs-lookup"><span data-stu-id="33998-140">Validate BGP and routes on hello MSEE (BGP between 4 too5, and 5 too6 if a VNet is connected)</span></span>](#validate-bgp-and-routes-on-the-msee)
5. [<span data-ttu-id="33998-141">Kontrollera hello trafik statistik (trafik som passerar genom 5)</span><span class="sxs-lookup"><span data-stu-id="33998-141">Check hello Traffic Statistics (Traffic passing through 5)</span></span>](#check-the-traffic-statistics)

<span data-ttu-id="33998-142">Mer verifieringar och kontroller ska läggas till i hello framtida Kom tillbaka varje månad!</span><span class="sxs-lookup"><span data-stu-id="33998-142">More validations and checks will be added in hello future, check back monthly!</span></span>

##<a name="validate-circuit-provisioning-and-state"></a><span data-ttu-id="33998-143">Validera kretsetablering och tillstånd</span><span class="sxs-lookup"><span data-stu-id="33998-143">Validate circuit provisioning and state</span></span>
<span data-ttu-id="33998-144">Oavsett hello anslutningen modellen har en ExpressRoute-krets toobe skapas och därmed en tjänst nyckeln genereras för kretsetablering.</span><span class="sxs-lookup"><span data-stu-id="33998-144">Regardless of hello connectivity model, an ExpressRoute circuit has toobe created and thus a service key generated for circuit provisioning.</span></span> <span data-ttu-id="33998-145">Etablera en ExpressRoute-krets upprättar en redundant nivå 2-anslutningar mellan PE-MSEEs (4) och MSEEs (5).</span><span class="sxs-lookup"><span data-stu-id="33998-145">Provisioning an ExpressRoute circuit establishes a redundant Layer 2 connections between PE-MSEEs (4) and MSEEs (5).</span></span> <span data-ttu-id="33998-146">Mer information om hur toocreate, ändra, konfigurera och verifiera en ExpressRoute-krets finns hello artikel [skapa och ändra en ExpressRoute-krets][CreateCircuit].</span><span class="sxs-lookup"><span data-stu-id="33998-146">For more information on how toocreate, modify, provision, and verify an ExpressRoute circuit, see hello article [Create and modify an ExpressRoute circuit][CreateCircuit].</span></span>

>[!TIP]
><span data-ttu-id="33998-147">Nyckeln för tjänsten identifierar en ExpressRoute-krets.</span><span class="sxs-lookup"><span data-stu-id="33998-147">A service key uniquely identifies an ExpressRoute circuit.</span></span> <span data-ttu-id="33998-148">Den här nyckeln är obligatorisk för de flesta hello powershell-kommandon som nämns i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="33998-148">This key is required for most of hello powershell commands mentioned in this document.</span></span> <span data-ttu-id="33998-149">Dessutom bör du behöver hjälp från Microsoft eller från en ExpressRoute partner tootroubleshoot ExpressRoute problemet betjäna hello viktiga tooreadily identifiera hello krets.</span><span class="sxs-lookup"><span data-stu-id="33998-149">Also, should you need assistance from Microsoft or from an ExpressRoute partner tootroubleshoot an ExpressRoute issue, provide hello service key tooreadily identify hello circuit.</span></span>
>
>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="33998-150">Verifiering via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33998-150">Verification via hello Azure portal</span></span>
<span data-ttu-id="33998-151">I hello Azure-portalen, hello status för en ExpressRoute-krets kan kontrolleras genom att välja ![2][2] hello hello vänster sidorutan-menyn och sedan välja ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="33998-151">In hello Azure portal, hello status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="33998-152">Om du markerar en ExpressRoute öppnas krets som anges under ”alla resurser” hello ExpressRoute-kretsen bladet.</span><span class="sxs-lookup"><span data-stu-id="33998-152">Selecting an ExpressRoute circuit listed under "All resources" opens hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="33998-153">I hello ![3][3] avsnitt i hello bladet hello ExpressRoute essentials listas som visas i följande skärmbild som visar hello:</span><span class="sxs-lookup"><span data-stu-id="33998-153">In hello ![3][3] section of hello blade, hello ExpressRoute essentials are listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="33998-154">![4][4]</span><span class="sxs-lookup"><span data-stu-id="33998-154">![4][4]</span></span>    

<span data-ttu-id="33998-155">I hello ExpressRoute Essentials *krets status* anger hello kretsstatusen hello på hello Microsoft sida.</span><span class="sxs-lookup"><span data-stu-id="33998-155">In hello ExpressRoute Essentials, *Circuit status* indicates hello status of hello circuit on hello Microsoft side.</span></span> <span data-ttu-id="33998-156">*Providerstatus* visar om hello kretsen har *etablerad ej etablerad* på hello-leverantör sida.</span><span class="sxs-lookup"><span data-stu-id="33998-156">*Provider status* indicates if hello circuit has been *Provisioned/Not provisioned* on hello service-provider side.</span></span> 

<span data-ttu-id="33998-157">En ExpressRoute-kretsen toobe operativa, hello *krets status* måste vara *aktiverad* och hello *Providerstatus* måste vara *etablerad*.</span><span class="sxs-lookup"><span data-stu-id="33998-157">For an ExpressRoute circuit toobe operational, hello *Circuit status* must be *Enabled* and hello *Provider status* must be *Provisioned*.</span></span>

>[!NOTE]
><span data-ttu-id="33998-158">Om hello *krets status* är inte aktiverad, kontakta [Microsoft-supporten][Support].</span><span class="sxs-lookup"><span data-stu-id="33998-158">If hello *Circuit status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="33998-159">Om hello *Providerstatus* har inte etablerats kontakta tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="33998-159">If hello *Provider status* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="33998-160">Verifiering via PowerShell</span><span class="sxs-lookup"><span data-stu-id="33998-160">Verification via PowerShell</span></span>
<span data-ttu-id="33998-161">toolist alla hello ExpressRoute-kretsar i en resursgrupp, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-161">toolist all hello ExpressRoute circuits in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG"

>[!TIP]
><span data-ttu-id="33998-162">Du kan hämta din resursgruppens namn via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="33998-162">You can get your resource group name through hello Azure portal.</span></span> <span data-ttu-id="33998-163">Se hello föregående avsnitt i det här dokumentet och Observera att hello resursgruppens namn anges i hello exempel skärmbild.</span><span class="sxs-lookup"><span data-stu-id="33998-163">See hello previous subsection of this document and note that hello resource group name is listed in hello example screen shot.</span></span>
>
>

<span data-ttu-id="33998-164">tooselect en viss ExpressRoute-krets i en resursgrupp, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-164">tooselect a particular ExpressRoute circuit in a Resource Group, use hello following command:</span></span>

    Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"

<span data-ttu-id="33998-165">En exempelsvar är:</span><span class="sxs-lookup"><span data-stu-id="33998-165">A sample response is:</span></span>

    Name                             : Test-ER-Ckt
    ResourceGroupName                : Test-ER-RG
    Location                         : westus2
    Id                               : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                        "Name": "Standard_UnlimitedData",
                                        "Tier": "Standard",
                                        "Family": "UnlimitedData"
                                        }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             : 
    ServiceProviderProperties        : {
                                        "ServiceProviderName": "****",
                                        "PeeringLocation": "******",
                                        "BandwidthInMbps": 100
                                        }
    ServiceKey                       : **************************************
    Peerings                         : []
    Authorizations                   : []

<span data-ttu-id="33998-166">tooconfirm om en ExpressRoute-krets fungerar betala närmare toohello följande fält:</span><span class="sxs-lookup"><span data-stu-id="33998-166">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields:</span></span>

    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned

>[!NOTE]
><span data-ttu-id="33998-167">Om hello *CircuitProvisioningState* är inte aktiverad, kontakta [Microsoft-supporten][Support].</span><span class="sxs-lookup"><span data-stu-id="33998-167">If hello *CircuitProvisioningState* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="33998-168">Om hello *ServiceProviderProvisioningState* har inte etablerats kontakta tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="33998-168">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

###<a name="verification-via-powershell-classic"></a><span data-ttu-id="33998-169">Verifiering via PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="33998-169">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="33998-170">toolist alla hello ExpressRoute-kretsar under en prenumeration, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-170">toolist all hello ExpressRoute circuits under a subscription, use hello following command:</span></span>

    Get-AzureDedicatedCircuit

<span data-ttu-id="33998-171">tooselect en viss ExpressRoute-kretsen använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-171">tooselect a particular ExpressRoute circuit, use hello following command:</span></span>

    Get-AzureDedicatedCircuit -ServiceKey **************************************

<span data-ttu-id="33998-172">En exempelsvar är:</span><span class="sxs-lookup"><span data-stu-id="33998-172">A sample response is:</span></span>

    andwidth                         : 100
    BillingType                      : UnlimitedData
    CircuitName                      : Test-ER-Ckt
    Location                         : westus2
    ServiceKey                       : **************************************
    ServiceProviderName              : ****
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

<span data-ttu-id="33998-173">tooconfirm om en ExpressRoute-krets fungerar betala närmare toohello följande fält: ServiceProviderProvisioningState: etablerats Status: aktiverat</span><span class="sxs-lookup"><span data-stu-id="33998-173">tooconfirm if an ExpressRoute circuit is operational, pay particular attention toohello following fields: ServiceProviderProvisioningState : Provisioned Status                           : Enabled</span></span>

>[!NOTE]
><span data-ttu-id="33998-174">Om hello *Status* är inte aktiverad, kontakta [Microsoft-supporten][Support].</span><span class="sxs-lookup"><span data-stu-id="33998-174">If hello *Status* is not enabled, contact [Microsoft Support][Support].</span></span> <span data-ttu-id="33998-175">Om hello *ServiceProviderProvisioningState* har inte etablerats kontakta tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="33998-175">If hello *ServiceProviderProvisioningState* is not provisioned, contact your service provider.</span></span>
>
>

##<a name="validate-peering-configuration"></a><span data-ttu-id="33998-176">Verifiera Peering konfiguration</span><span class="sxs-lookup"><span data-stu-id="33998-176">Validate Peering Configuration</span></span>
<span data-ttu-id="33998-177">När hello-leverantör har slutförts hello etablering hello ExpressRoute-krets, kan du skapa en routningskonfiguration över hello ExpressRoute-krets mellan MSEE fso (4) och MSEEs (5).</span><span class="sxs-lookup"><span data-stu-id="33998-177">After hello service provider has completed hello provisioning hello ExpressRoute circuit, a routing configuration can be created over hello ExpressRoute circuit between MSEE-PRs (4) and MSEEs (5).</span></span> <span data-ttu-id="33998-178">Varje ExpressRoute-kretsen kan ha en, två eller tre routning kontexter aktiverad: privat Azure-peering (trafik tooprivate virtuella nätverk i Azure), offentlig Azure-peering (trafik toopublic IP-adresser i Azure) och Microsoft-peering (trafik tooOffice 365 och Dynamics 365).</span><span class="sxs-lookup"><span data-stu-id="33998-178">Each ExpressRoute circuit can have one, two, or three routing contexts enabled: Azure private peering (traffic tooprivate virtual networks in Azure), Azure public peering (traffic toopublic IP addresses in Azure), and Microsoft peering (traffic tooOffice 365 and Dynamics 365).</span></span> <span data-ttu-id="33998-179">Mer information om hur toocreate och ändra konfigurationen för routning, se hello artikel [skapa och ändra routning för en ExpressRoute-krets][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="33998-179">For more information on how toocreate and modify routing configuration, see hello article [Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>

###<a name="verification-via-hello-azure-portal"></a><span data-ttu-id="33998-180">Verifiering via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33998-180">Verification via hello Azure portal</span></span>
>[!IMPORTANT]
><span data-ttu-id="33998-181">Det finns ett känt fel i hello Azure-portalen där ExpressRoute peerkopplingar är *inte* visas i hello portal om hello-leverantör har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="33998-181">There is a known bug in hello Azure portal wherein ExpressRoute peerings are *NOT* shown in hello portal if configured by hello service provider.</span></span> <span data-ttu-id="33998-182">Lägga till ExpressRoute peerkopplingar via hello-portalen eller PowerShell *skriver över hello providern tjänstinställningar*.</span><span class="sxs-lookup"><span data-stu-id="33998-182">Adding ExpressRoute peerings via hello portal or PowerShell *overwrites hello service provider settings*.</span></span> <span data-ttu-id="33998-183">Den här åtgärden bryter hello routning på hello ExpressRoute-krets och kräver hello stöd av hello providern toorestore hello tjänstinställningar och återupprätta normal routning.</span><span class="sxs-lookup"><span data-stu-id="33998-183">This action breaks hello routing on hello ExpressRoute circuit and requires hello support of hello service provider toorestore hello settings and reestablish normal routing.</span></span> <span data-ttu-id="33998-184">Ändra bara hello ExpressRoute peerkopplingar om det är säkert att hello-leverantören tillhandahåller nivå 2-tjänster!</span><span class="sxs-lookup"><span data-stu-id="33998-184">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="33998-185">Om nivå 3 har angetts av hello service provider och hello peerkopplingar är tomma i hello portal, vara PowerShell används toosee hello providern konfigurerade inställningar.</span><span class="sxs-lookup"><span data-stu-id="33998-185">If layer 3 is provided by hello service provider and hello peerings are blank in hello portal, PowerShell can be used toosee hello service provider configured settings.</span></span>
>
>

<span data-ttu-id="33998-186">I hello Azure-portalen, status för en ExpressRoute-krets kan kontrolleras genom att välja ![2][2] hello hello vänster sidorutan-menyn och sedan välja ExpressRoute-kretsen.</span><span class="sxs-lookup"><span data-stu-id="33998-186">In hello Azure portal, status of an ExpressRoute circuit can be checked by selecting ![2][2] on hello left-side-bar menu and then selecting hello ExpressRoute circuit.</span></span> <span data-ttu-id="33998-187">Att välja en ExpressRoute öppnas krets som anges under ”alla resurser” hello ExpressRoute-kretsen bladet.</span><span class="sxs-lookup"><span data-stu-id="33998-187">Selecting an ExpressRoute circuit listed under "All resources" would open hello ExpressRoute circuit blade.</span></span> <span data-ttu-id="33998-188">I hello ![3][3] avsnitt i hello bladet hello ExpressRoute essentials anges som visas i följande skärmbild som visar hello:</span><span class="sxs-lookup"><span data-stu-id="33998-188">In hello ![3][3] section of hello blade, hello ExpressRoute essentials would be listed as shown in hello following screen shot:</span></span>

<span data-ttu-id="33998-189">![5][5]</span><span class="sxs-lookup"><span data-stu-id="33998-189">![5][5]</span></span>

<span data-ttu-id="33998-190">I föregående exempel hello, som noterats Azure är privat peering routning kontext aktiverad, medan Azure offentliga och Microsoft peering routning kontexter inte är aktiverade.</span><span class="sxs-lookup"><span data-stu-id="33998-190">In hello preceding example, as noted Azure private peering routing context is enabled, whereas Azure public and Microsoft peering routing contexts are not enabled.</span></span> <span data-ttu-id="33998-191">En kontext som har aktiverats peering har också hello primära och sekundära point-to-point (krävs för BGP)-undernät i listan.</span><span class="sxs-lookup"><span data-stu-id="33998-191">A successfully enabled peering context would also have hello primary and secondary point-to-point (required for BGP) subnets listed.</span></span> <span data-ttu-id="33998-192">Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-192">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span> 

>[!NOTE]
><span data-ttu-id="33998-193">Om en peering inte är aktiverat, kontrollera om hello primära och sekundära undernät som tilldelats matcha hello konfigurationen på PE MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-193">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on PE-MSEEs.</span></span> <span data-ttu-id="33998-194">Om inte, toochange hello konfiguration på MSEE routrar, se för[skapa och ändra routning för en ExpressRoute-krets][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="33998-194">If not, toochange hello configuration on MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>
>
>

###<a name="verification-via-powershell"></a><span data-ttu-id="33998-195">Verifiering via PowerShell</span><span class="sxs-lookup"><span data-stu-id="33998-195">Verification via PowerShell</span></span>
<span data-ttu-id="33998-196">tooget hello Azure privat peering konfigurationsinformation, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="33998-196">tooget hello Azure private peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt

<span data-ttu-id="33998-197">En exempelsvar för en har konfigurerats privat peering, är:</span><span class="sxs-lookup"><span data-stu-id="33998-197">A sample response, for a successfully configured private peering, is:</span></span>

    Name                       : AzurePrivatePeering
    Id                         : /subscriptions/***************************/resourceGroups/Test-ER-RG/providers/***********/expressRouteCircuits/Test-ER-Ckt/peerings/AzurePrivatePeering
    Etag                       : W/"################################"
    PeeringType                : AzurePrivatePeering
    AzureASN                   : 12076
    PeerASN                    : ####
    PrimaryPeerAddressPrefix   : 172.16.0.0/30
    SecondaryPeerAddressPrefix : 172.16.0.4/30
    PrimaryAzurePort           : 
    SecondaryAzurePort         : 
    SharedKey                  : 
    VlanId                     : 300
    MicrosoftPeeringConfig     : null
    ProvisioningState          : Succeeded

 <span data-ttu-id="33998-198">En kontext som har aktiverats peering skulle ha hello primära och sekundära adressprefix i listan.</span><span class="sxs-lookup"><span data-stu-id="33998-198">A successfully enabled peering context would have hello primary and secondary address prefixes listed.</span></span> <span data-ttu-id="33998-199">Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-199">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="33998-200">tooget hello Azure offentlig peering konfigurationsinformation, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="33998-200">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt

<span data-ttu-id="33998-201">tooget hello Microsoft peering konfigurationsinformation, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="33998-201">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    $ckt = Get-AzureRmExpressRouteCircuit -ResourceGroupName "Test-ER-RG" -Name "Test-ER-Ckt"
    Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -Circuit $ckt

<span data-ttu-id="33998-202">Om en peering inte har konfigurerats, skulle det finnas ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="33998-202">If a peering is not configured, there would be an error message.</span></span> <span data-ttu-id="33998-203">Exempelsvar när hello anges peering (offentlig Azure-peering i det här exemplet) har inte konfigurerats i hello krets:</span><span class="sxs-lookup"><span data-stu-id="33998-203">A sample response, when hello stated peering (Azure Public peering in this example) is not configured within hello circuit:</span></span>

    Get-AzureRmExpressRouteCircuitPeeringConfig : Sequence contains no matching element
    At line:1 char:1
        + Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering ...
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
            + CategoryInfo          : CloseError: (:) [Get-AzureRmExpr...itPeeringConfig], InvalidOperationException
            + FullyQualifiedErrorId : Microsoft.Azure.Commands.Network.GetAzureExpressRouteCircuitPeeringConfigCommand


<p/>
>[!NOTE]
><span data-ttu-id="33998-204">Om en peering inte är aktiverad, kan du kontrollera om hello primära och sekundära undernät som tilldelats matchar hello konfigurationen på hello länkat PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-204">If a peering is not enabled, check if hello primary and secondary subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-205">Även om hello korrigera *VlanId*, *AzureASN*, och *PeerASN* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-205">Also check if hello correct *VlanId*, *AzureASN*, and *PeerASN* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-206">Om du väljer MD5-hash ska hello delad nyckel vara samma på MSEE och PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-206">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="33998-207">toochange hello konfigurationen på hello MSEE routrar finns för [skapa och ändra routning för en ExpressRoute-krets] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="33998-207">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>  
>
>

### <a name="verification-via-powershell-classic"></a><span data-ttu-id="33998-208">Verifiering via PowerShell (klassisk)</span><span class="sxs-lookup"><span data-stu-id="33998-208">Verification via PowerShell (Classic)</span></span>
<span data-ttu-id="33998-209">tooget hello Azure privat peering konfigurationsinformation, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-209">tooget hello Azure private peering configuration details, use hello following command:</span></span>

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"

<span data-ttu-id="33998-210">En exempelsvar för en har konfigurerats privat peering är:</span><span class="sxs-lookup"><span data-stu-id="33998-210">A sample response, for a successfully configured private peering is:</span></span>

    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : ####
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100

<span data-ttu-id="33998-211">A, aktiverats peering kontexten skulle ha hello primära och sekundära peer-undernät i listan.</span><span class="sxs-lookup"><span data-stu-id="33998-211">A successfully, enabled peering context would have hello primary and secondary peer subnets listed.</span></span> <span data-ttu-id="33998-212">Hej /30 undernät används för hello IP-gränssnitt hello MSEEs och PE MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-212">hello /30 subnets are used for hello interface IP address of hello MSEEs and PE-MSEEs.</span></span>

<span data-ttu-id="33998-213">tooget hello Azure offentlig peering konfigurationsinformation, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="33998-213">tooget hello Azure public peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

<span data-ttu-id="33998-214">tooget hello Microsoft peering konfigurationsinformation, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="33998-214">tooget hello Microsoft peering configuration details, use hello following commands:</span></span>

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

>[!IMPORTANT]
><span data-ttu-id="33998-215">Om nivå 3 peerkopplingar ställts in av hello-leverantör, ange hello ExpressRoute peerkopplingar via hello-portalen eller PowerShell skriver över hello tjänstinställningar för providern.</span><span class="sxs-lookup"><span data-stu-id="33998-215">If layer 3 peerings were set by hello service provider, setting hello ExpressRoute peerings via hello portal or PowerShell overwrites hello service provider settings.</span></span> <span data-ttu-id="33998-216">Återställer hello sida peering providerinställningar kräver hello stöd av hello-leverantör.</span><span class="sxs-lookup"><span data-stu-id="33998-216">Resetting hello provider side peering settings requires hello support of hello service provider.</span></span> <span data-ttu-id="33998-217">Ändra bara hello ExpressRoute peerkopplingar om det är säkert att hello-leverantören tillhandahåller nivå 2-tjänster!</span><span class="sxs-lookup"><span data-stu-id="33998-217">Only modify hello ExpressRoute peerings if it is certain that hello service provider is providing layer 2 services only!</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="33998-218">Om en peering inte är aktiverad, kan du kontrollera om hello primära och sekundära peer-undernät som tilldelats matchar hello konfigurationen på hello länkat PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-218">If a peering is not enabled, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-219">Även om hello korrigera *VlanId*, *AzureAsn*, och *PeerAsn* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-219">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-220">toochange hello konfigurationen på hello MSEE routrar finns för [skapa och ändra routning för en ExpressRoute-krets] [CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="33998-220">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

## <a name="validate-arp-between-microsoft-and-hello-service-provider"></a><span data-ttu-id="33998-221">Validera ARP mellan Microsoft och hello-leverantör</span><span class="sxs-lookup"><span data-stu-id="33998-221">Validate ARP between Microsoft and hello service provider</span></span>
<span data-ttu-id="33998-222">Det här avsnittet använder PowerShell (klassisk)-kommandon.</span><span class="sxs-lookup"><span data-stu-id="33998-222">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="33998-223">Om du har använt Azure Resource Manager för PowerShell-kommandon, kontrollera att du har åtkomst till admin-/ medadministratör toohello prenumeration via [klassiska Azure-portalen][OldPortal].</span><span class="sxs-lookup"><span data-stu-id="33998-223">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal].</span></span> <span data-ttu-id="33998-224">Felsöka med Azure Resource Manager kommandon finns toohello [komma ARP-tabeller i hello Resource Manager-distributionsmodellen] [ ARP] dokumentet.</span><span class="sxs-lookup"><span data-stu-id="33998-224">For troubleshooting using Azure Resource Manager commands please refer toohello [Getting ARP tables in hello Resource Manager deployment model][ARP] document.</span></span>

>[!NOTE]
><span data-ttu-id="33998-225">tooget ARP, både hello Azure-portalen och Azure Resource Manager PowerShell-kommandon kan användas.</span><span class="sxs-lookup"><span data-stu-id="33998-225">tooget ARP, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="33998-226">Om det uppstår fel med hello Azure Resource Manager PowerShell-kommandon, bör klassiska PowerShell-kommandon fungera som klassiska PowerShell kommandon också fungera med Azure Resource Manager ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="33998-226">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as Classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="33998-227">tooget hello ARP-tabell från hello primära MSEE router för privata hello-peering använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-227">tooget hello ARP table from hello primary MSEE router for hello private peering, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringArpInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="33998-228">Ett exempelsvar för hello-kommando i hello lyckade scenariot:</span><span class="sxs-lookup"><span data-stu-id="33998-228">An example response for hello command, in hello successful scenario:</span></span>

    ARP Info:

                 Age           Interface           IpAddress          MacAddress
                 113             On-Prem       10.0.0.1           e8ed.f335.4ca9
                   0           Microsoft       10.0.0.2           7c0e.ce85.4fc9

<span data-ttu-id="33998-229">På liknande sätt kan du kontrollera hello ARP-tabell från hello MSEE i hello *primära*/*sekundära* sökvägen för *privata* /  *Offentliga*/*Microsoft* peerkopplingar.</span><span class="sxs-lookup"><span data-stu-id="33998-229">Similarly, you can check hello ARP table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* peerings.</span></span>

<span data-ttu-id="33998-230">hello finns följande exempel visar hello svar hello-kommando för en peering inte.</span><span class="sxs-lookup"><span data-stu-id="33998-230">hello following example shows hello response of hello command for a peering does not exist.</span></span>

    ARP Info:
       
>[!NOTE]
><span data-ttu-id="33998-231">Om hello ARP-tabell saknar mappas IP-adresser för hello gränssnitt tooMAC adresser, granska hello följande information:</span><span class="sxs-lookup"><span data-stu-id="33998-231">If hello ARP table does not have IP addresses of hello interfaces mapped tooMAC addresses, review hello following information:</span></span>
>1. <span data-ttu-id="33998-232">Om hello första IP-adressen för hello /30 undernät tilldelas för hello länken mellan hello MSEE PR och MSEE används hello-gränssnittet på MSEE PR.</span><span class="sxs-lookup"><span data-stu-id="33998-232">If hello first IP address of hello /30 subnet assigned for hello link between hello MSEE-PR and MSEE is used on hello interface of MSEE-PR.</span></span> <span data-ttu-id="33998-233">Azure använder alltid hello andra IP-adressen för MSEEs.</span><span class="sxs-lookup"><span data-stu-id="33998-233">Azure always uses hello second IP address for MSEEs.</span></span>
>2. <span data-ttu-id="33998-234">Kontrollera om hello kund (C-tagg) och service (S-tagg) VLAN-taggarna matchar både på MSEE PR och MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-234">Verify if hello customer (C-Tag) and service (S-Tag) VLAN tags match both on MSEE-PR and MSEE pair.</span></span>
>
>

## <a name="validate-bgp-and-routes-on-hello-msee"></a><span data-ttu-id="33998-235">Validera BGP och vägar på hello MSEE</span><span class="sxs-lookup"><span data-stu-id="33998-235">Validate BGP and routes on hello MSEE</span></span>
<span data-ttu-id="33998-236">Det här avsnittet använder PowerShell (klassisk)-kommandon.</span><span class="sxs-lookup"><span data-stu-id="33998-236">This section uses PowerShell (Classic) commands.</span></span> <span data-ttu-id="33998-237">Om du har använt Azure Resource Manager för PowerShell-kommandon, kontrollera att du har åtkomst till admin-/ medadministratör toohello prenumeration via [klassiska Azure-portalen][OldPortal]</span><span class="sxs-lookup"><span data-stu-id="33998-237">If you have been using PowerShell Azure Resource Manager commands, ensure that you have admin/co-admin access toohello subscription via [Azure classic portal][OldPortal]</span></span>

>[!NOTE]
><span data-ttu-id="33998-238">tooget BGP information, både hello Azure-portalen och Azure Resource Manager PowerShell-kommandon kan användas.</span><span class="sxs-lookup"><span data-stu-id="33998-238">tooget BGP information, both hello Azure portal and Azure Resource Manager PowerShell commands can be used.</span></span> <span data-ttu-id="33998-239">Om det uppstår fel med hello Azure Resource Manager PowerShell-kommandon, bör klassiska PowerShell-kommandon fungera som klassiska PowerShell kommandon också fungera med Azure Resource Manager ExpressRoute-kretsar.</span><span class="sxs-lookup"><span data-stu-id="33998-239">If errors are encountered with hello Azure Resource Manager PowerShell commands, classic PowerShell commands should work as classic PowerShell commands also work with Azure Resource Manager ExpressRoute circuits.</span></span>
>
>

<span data-ttu-id="33998-240">tooget hello routningstabellen (BGP-Grannens) sammanfattning för en viss routning kontext, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-240">tooget hello routing table (BGP neighbor) summary for a particular routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableSummary -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="33998-241">Ett exempelsvar är:</span><span class="sxs-lookup"><span data-stu-id="33998-241">An example response is:</span></span>

    Route Table Summary:

            Neighbor                   V                  AS              UpDown         StatePfxRcd
            10.0.0.1                   4                ####                8w4d                  50

<span data-ttu-id="33998-242">I föregående exempel hello visas hello-kommandot är användbara toodetermine för hur länge hello routning kontexten har upprättats.</span><span class="sxs-lookup"><span data-stu-id="33998-242">As shown in hello preceding example, hello command is useful toodetermine for how long hello routing context has been established.</span></span> <span data-ttu-id="33998-243">Den anger också antalet väg prefix annonseras av hello peering router.</span><span class="sxs-lookup"><span data-stu-id="33998-243">It also indicates number of route prefixes advertised by hello peering router.</span></span>

>[!NOTE]
><span data-ttu-id="33998-244">Kontrollera om hello primära och sekundära peer-undernät som tilldelats matchar hello konfigurationen på hello länkade PE MSEE om hello tillstånd är i aktivt eller inaktivt.</span><span class="sxs-lookup"><span data-stu-id="33998-244">If hello state is in Active or Idle, check if hello primary and secondary peer subnets assigned match hello configuration on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-245">Även om hello korrigera *VlanId*, *AzureAsn*, och *PeerAsn* används på MSEEs och om du värdena mappar toohello som används på hello länkad PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-245">Also check if hello correct *VlanId*, *AzureAsn*, and *PeerAsn* are used on MSEEs and if these values maps toohello ones used on hello linked PE-MSEE.</span></span> <span data-ttu-id="33998-246">Om du väljer MD5-hash ska hello delad nyckel vara samma på MSEE och PE MSEE.</span><span class="sxs-lookup"><span data-stu-id="33998-246">If MD5 hashing is chosen, hello shared key should be same on MSEE and PE-MSEE pair.</span></span> <span data-ttu-id="33998-247">toochange hello konfigurationen på hello MSEE routrar finns för[skapa och ändra routning för en ExpressRoute-krets][CreatePeering].</span><span class="sxs-lookup"><span data-stu-id="33998-247">toochange hello configuration on hello MSEE routers, refer too[Create and modify routing for an ExpressRoute circuit][CreatePeering].</span></span>
>
>

<p/>
>[!NOTE]
><span data-ttu-id="33998-248">Om vissa mål inte kan nås via en viss peering, kontrollera hello routningstabellen för hello MSEEs tillhörande toohello särskilda peering sammanhang.</span><span class="sxs-lookup"><span data-stu-id="33998-248">If certain destinations are not reachable over a particular peering, check hello route table of hello MSEEs belonging toohello particular peering context.</span></span> <span data-ttu-id="33998-249">Om ett matchande prefix (kan vara NATed IP) finns i routningstabellen för hello, kontrollerar du om det finns ACL-brandväggar/NSG på hello sökväg och om de tillåter hello trafik.</span><span class="sxs-lookup"><span data-stu-id="33998-249">If a matching prefix (could be NATed IP) is present in hello routing table, then check if there are firewalls/NSG/ACLs on hello path and if they permit hello traffic.</span></span>
>
>

<span data-ttu-id="33998-250">tooget hello fullständig routningstabellen från MSEE på hello *primära* sökvägen för hello viss *privata* routning kontext, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-250">tooget hello full routing table from MSEE on hello *Primary* path for hello particular *Private* routing context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitPeeringRouteTableInfo -AccessType Private -Path Primary -ServiceKey "*********************************"

<span data-ttu-id="33998-251">Exempel lyckade resultat för hello-kommandot är:</span><span class="sxs-lookup"><span data-stu-id="33998-251">An example successful outcome for hello command is:</span></span>

    Route Table Info:

             Network             NextHop              LocPrf              Weight                Path
         10.1.0.0/16            10.0.0.1                                       0    #### ##### #####     
         10.2.0.0/16            10.0.0.1                                       0    #### ##### #####
    ...

<span data-ttu-id="33998-252">På liknande sätt kan du kontrollera hello routningstabellen från hello MSEE i hello *primära*/*sekundära* sökvägen för *privata* / *Offentliga*/*Microsoft* en peering-kontext.</span><span class="sxs-lookup"><span data-stu-id="33998-252">Similarly, you can check hello routing table from hello MSEE in hello *Primary*/*Secondary* path, for *Private*/*Public*/*Microsoft* a peering context.</span></span>

<span data-ttu-id="33998-253">hello finns följande exempel visar hello svar hello-kommando för en peering inte:</span><span class="sxs-lookup"><span data-stu-id="33998-253">hello following example shows hello response of hello command for a peering does not exist:</span></span>

    Route Table Info:

##<a name="check-hello-traffic-statistics"></a><span data-ttu-id="33998-254">Kontrollera hello trafik statistik</span><span class="sxs-lookup"><span data-stu-id="33998-254">Check hello Traffic Statistics</span></span>
<span data-ttu-id="33998-255">tooget hello kombineras primära och sekundära sökväg trafik statistik--byte in och ut – för en peering kontext, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="33998-255">tooget hello combined primary and secondary path traffic statistics--bytes in and out--of a peering context, use hello following command:</span></span>

    Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf683b3a6e5c -AccessType Private

<span data-ttu-id="33998-256">Ett exempel på utdata för hello-kommandot är:</span><span class="sxs-lookup"><span data-stu-id="33998-256">A sample output of hello command is:</span></span>

    PrimaryBytesIn PrimaryBytesOut SecondaryBytesIn SecondaryBytesOut
    -------------- --------------- ---------------- -----------------
         240780020       239863857        240565035         239628474

<span data-ttu-id="33998-257">Ett exempel på utdata hello-kommando för en obefintlig peering är:</span><span class="sxs-lookup"><span data-stu-id="33998-257">A sample output of hello command for a non-existent peering is:</span></span>

    Get-AzureDedicatedCircuitStats : ResourceNotFound: Can not find any subinterface for peering type 'Public' for circuit '97f85950-01dd-4d30-a73c-bf683b3a6e5c' .
    At line:1 char:1
    + Get-AzureDedicatedCircuitStats -ServiceKey 97f85950-01dd-4d30-a73c-bf ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Get-AzureDedicatedCircuitStats], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.GetAzureDedicatedCircuitPeeringStatsCommand

## <a name="next-steps"></a><span data-ttu-id="33998-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33998-258">Next Steps</span></span>
<span data-ttu-id="33998-259">Mer information eller hjälp, ta en titt hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="33998-259">For more information or help, check out hello following links:</span></span>

- <span data-ttu-id="33998-260">[Microsoft Support][Support]</span><span class="sxs-lookup"><span data-stu-id="33998-260">[Microsoft Support][Support]</span></span>
- <span data-ttu-id="33998-261">[Skapa och ändra en ExpressRoute-krets][CreateCircuit]</span><span class="sxs-lookup"><span data-stu-id="33998-261">[Create and modify an ExpressRoute circuit][CreateCircuit]</span></span>
- <span data-ttu-id="33998-262">[Skapa och ändra routning för en ExpressRoute-krets][CreatePeering]</span><span class="sxs-lookup"><span data-stu-id="33998-262">[Create and modify routing for an ExpressRoute circuit][CreatePeering]</span></span>

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-expressroute-overview/expressroute-logical-diagram.png "Logisk Expressroute-anslutning"
[2]: ./media/expressroute-troubleshooting-expressroute-overview/portal-all-resources.png "Ikon för alla resurser"
[3]: ./media/expressroute-troubleshooting-expressroute-overview/portal-overview.png "Översikt över ikonen"
[4]: ./media/expressroute-troubleshooting-expressroute-overview/portal-circuit-status.png "ExpressRoute Essentials exempel skärmbild"
[5]: ./media/expressroute-troubleshooting-expressroute-overview/portal-private-peering.png "ExpressRoute Essentials exempel skärmbild"

<!--Link References-->
[Support]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[CreateCircuit]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-circuit-portal-resource-manager 
[CreatePeering]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-routing-portal-resource-manager
[OldPortal]: https://manage.windowsazure.com
[ARP]: https://docs.microsoft.com/en-us/azure/expressroute/expressroute-troubleshooting-arp-resource-manager






