---
title: "aaaIntroduction tooresource felsökning i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över funktioner för hello Nätverksbevakaren resurs felsökning"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a><span data-ttu-id="39f49-103">Introduktion tooresource felsökning i Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="39f49-103">Introduction tooresource troubleshooting in Azure Network Watcher</span></span>

<span data-ttu-id="39f49-104">Virtuella Nätverksgatewayer anslutningsbarhet mellan lokala resurser och andra virtuella nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="39f49-104">Virtual Network Gateways provide connectivity between on-premises resources and other virtual networks within Azure.</span></span> <span data-ttu-id="39f49-105">Övervaka dessa gateways och deras anslutningar är kritiska tooensuring kommunikation är inte skadad.</span><span class="sxs-lookup"><span data-stu-id="39f49-105">Monitoring these gateways and their Connections is critical tooensuring communication is not broken.</span></span> <span data-ttu-id="39f49-106">Nätverksbevakaren ger hello kapaciteten tootroubleshoot virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="39f49-106">Network Watcher provides hello capability tootroubleshoot Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="39f49-107">Det kan anropas via hello portal, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="39f49-107">This can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="39f49-108">När den anropas, diagnostiserar Nätverksbevakaren hello hälsotillståndet för hello virtuell nätverksgateway eller anslutningen och returnerar hello rätt resultat.</span><span class="sxs-lookup"><span data-stu-id="39f49-108">When called, Network Watcher diagnoses hello health of hello virtual network gateway or connection and return hello appropriate results.</span></span> <span data-ttu-id="39f49-109">Denna begäran är en tidskrävande transaktion, returneras hello resultat när hello diagnos är klar.</span><span class="sxs-lookup"><span data-stu-id="39f49-109">This request is a long running transaction, hello results are returned once hello diagnosis is complete.</span></span>

![portal][2]

## <a name="results"></a><span data-ttu-id="39f49-111">Resultat</span><span class="sxs-lookup"><span data-stu-id="39f49-111">Results</span></span>

<span data-ttu-id="39f49-112">hello preliminära resultat returneras ger en övergripande bild av hello hälsotillstånd hello resurs.</span><span class="sxs-lookup"><span data-stu-id="39f49-112">hello preliminary results returned give an overall picture of hello health of hello resource.</span></span> <span data-ttu-id="39f49-113">Mer detaljerad information kan anges för resurser som visas i följande avsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="39f49-113">Deeper information can be provided for resources as shown in hello following section:</span></span>

<span data-ttu-id="39f49-114">hello följande lista finns hello-värden som returneras med hello felsöka API:</span><span class="sxs-lookup"><span data-stu-id="39f49-114">hello following list is hello values returned with hello troubleshoot API:</span></span>

* <span data-ttu-id="39f49-115">**startTime** -värdet är hello tid hello felsöka API-anrop som är igång.</span><span class="sxs-lookup"><span data-stu-id="39f49-115">**startTime** - This value is hello time hello troubleshoot API call started.</span></span>
* <span data-ttu-id="39f49-116">**endTime** -värdet är hello tidpunkt då hello felsökning avslutades.</span><span class="sxs-lookup"><span data-stu-id="39f49-116">**endTime** - This value is hello time when hello troubleshooting ended.</span></span>
* <span data-ttu-id="39f49-117">**koden** -värdet är ohälsosamma, om det finns ett enda diagnos-fel.</span><span class="sxs-lookup"><span data-stu-id="39f49-117">**code** - This value is UnHealthy, if there is a single diagnosis failure.</span></span>
* <span data-ttu-id="39f49-118">**resultaten** -resultat är en samling av resultaten på hello anslutning eller hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="39f49-118">**results** - Results is a collection of results returned on hello Connection or hello virtual network gateway.</span></span>
    * <span data-ttu-id="39f49-119">**ID** -värdet är hello feltypen.</span><span class="sxs-lookup"><span data-stu-id="39f49-119">**id** - This value is hello fault type.</span></span>
    * <span data-ttu-id="39f49-120">**Översikt över** -värdet är en sammanfattning av hello-fel.</span><span class="sxs-lookup"><span data-stu-id="39f49-120">**summary** - This value is a summary of hello fault.</span></span>
    * <span data-ttu-id="39f49-121">**detaljerad** -värdet ger en detaljerad beskrivning av hello-fel.</span><span class="sxs-lookup"><span data-stu-id="39f49-121">**detailed** - This value provides a detailed description of hello fault.</span></span>
    * <span data-ttu-id="39f49-122">**recommendedActions** -den här egenskapen är en uppsättning rekommenderade åtgärder tootake.</span><span class="sxs-lookup"><span data-stu-id="39f49-122">**recommendedActions** - This property is a collection of recommended actions tootake.</span></span>
      * <span data-ttu-id="39f49-123">**actionText** -värdet innehåller hello text som beskriver vilka åtgärden tootake.</span><span class="sxs-lookup"><span data-stu-id="39f49-123">**actionText** - This value contains hello text describing what action tootake.</span></span>
      * <span data-ttu-id="39f49-124">**actionUri** -värdet innehåller hello URI toodocumentation tooact.</span><span class="sxs-lookup"><span data-stu-id="39f49-124">**actionUri** - This value provides hello URI toodocumentation on how tooact.</span></span>
      * <span data-ttu-id="39f49-125">**actionUriText** -det här värdet är en kort beskrivning av hello åtgärd text.</span><span class="sxs-lookup"><span data-stu-id="39f49-125">**actionUriText** - This value is a short description of hello action text.</span></span>

<span data-ttu-id="39f49-126">hello följande tabeller visar hello olika fel typer (id under resultat från hello föregående lista) som är tillgängliga och om hello fel skapar loggar.</span><span class="sxs-lookup"><span data-stu-id="39f49-126">hello following tables show hello different fault types (id under results from hello preceding list) that are available and if hello fault creates logs.</span></span>

### <a name="gateway"></a><span data-ttu-id="39f49-127">Gateway</span><span class="sxs-lookup"><span data-stu-id="39f49-127">Gateway</span></span>

| <span data-ttu-id="39f49-128">Feltypen</span><span class="sxs-lookup"><span data-stu-id="39f49-128">Fault Type</span></span> | <span data-ttu-id="39f49-129">Orsak</span><span class="sxs-lookup"><span data-stu-id="39f49-129">Reason</span></span> | <span data-ttu-id="39f49-130">Logga</span><span class="sxs-lookup"><span data-stu-id="39f49-130">Log</span></span>|
|---|---|---|
| <span data-ttu-id="39f49-131">NoFault</span><span class="sxs-lookup"><span data-stu-id="39f49-131">NoFault</span></span> | <span data-ttu-id="39f49-132">Om inga fel har identifierats.</span><span class="sxs-lookup"><span data-stu-id="39f49-132">When no error is detected.</span></span> |<span data-ttu-id="39f49-133">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-133">Yes</span></span>|
| <span data-ttu-id="39f49-134">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="39f49-134">GatewayNotFound</span></span> | <span data-ttu-id="39f49-135">Det går inte att hitta Gateway eller Gateway inte har etablerats.</span><span class="sxs-lookup"><span data-stu-id="39f49-135">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="39f49-136">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-136">No</span></span>|
| <span data-ttu-id="39f49-137">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="39f49-137">PlannedMaintenance</span></span> |  <span data-ttu-id="39f49-138">Gateway-instans är under underhåll.</span><span class="sxs-lookup"><span data-stu-id="39f49-138">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="39f49-139">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-139">No</span></span>|
| <span data-ttu-id="39f49-140">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="39f49-140">UserDrivenUpdate</span></span> | <span data-ttu-id="39f49-141">När en användare uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="39f49-141">When a user update is in progress.</span></span> <span data-ttu-id="39f49-142">Detta kan vara en åtgärd för storleksändring.</span><span class="sxs-lookup"><span data-stu-id="39f49-142">This could be a resize operation.</span></span> | <span data-ttu-id="39f49-143">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-143">No</span></span> |
| <span data-ttu-id="39f49-144">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="39f49-144">VipUnResponsive</span></span> | <span data-ttu-id="39f49-145">Det går inte att nå hello primära instansen av hello Gateway.</span><span class="sxs-lookup"><span data-stu-id="39f49-145">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="39f49-146">Detta händer när hello hälsoavsökningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="39f49-146">This happens when hello health probe fails.</span></span> | <span data-ttu-id="39f49-147">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-147">No</span></span> |
| <span data-ttu-id="39f49-148">PlatformInActive</span><span class="sxs-lookup"><span data-stu-id="39f49-148">PlatformInActive</span></span> | <span data-ttu-id="39f49-149">Det finns ett problem med hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="39f49-149">There is an issue with hello platform.</span></span> | <span data-ttu-id="39f49-150">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-150">No</span></span>|
| <span data-ttu-id="39f49-151">ServiceNotRunning</span><span class="sxs-lookup"><span data-stu-id="39f49-151">ServiceNotRunning</span></span> | <span data-ttu-id="39f49-152">hello underliggande tjänst körs inte.</span><span class="sxs-lookup"><span data-stu-id="39f49-152">hello underlying service is not running.</span></span> | <span data-ttu-id="39f49-153">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-153">No</span></span>|
| <span data-ttu-id="39f49-154">NoConnectionsFoundForGateway</span><span class="sxs-lookup"><span data-stu-id="39f49-154">NoConnectionsFoundForGateway</span></span> | <span data-ttu-id="39f49-155">Det finns inga anslutningar på hello gateway.</span><span class="sxs-lookup"><span data-stu-id="39f49-155">No Connections exists on hello gateway.</span></span> <span data-ttu-id="39f49-156">Detta är endast en varning.</span><span class="sxs-lookup"><span data-stu-id="39f49-156">This is only a warning.</span></span>| <span data-ttu-id="39f49-157">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-157">No</span></span>|
| <span data-ttu-id="39f49-158">ConnectionsNotConnected</span><span class="sxs-lookup"><span data-stu-id="39f49-158">ConnectionsNotConnected</span></span> | <span data-ttu-id="39f49-159">Anslutningar är inte ansluten.</span><span class="sxs-lookup"><span data-stu-id="39f49-159">Connections are not connected.</span></span> <span data-ttu-id="39f49-160">Detta är endast en varning.</span><span class="sxs-lookup"><span data-stu-id="39f49-160">This is only a warning.</span></span>| <span data-ttu-id="39f49-161">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-161">Yes</span></span>|
| <span data-ttu-id="39f49-162">GatewayCPUUsageExceeded</span><span class="sxs-lookup"><span data-stu-id="39f49-162">GatewayCPUUsageExceeded</span></span> | <span data-ttu-id="39f49-163">hello Gateway processorkraft är > 95%.</span><span class="sxs-lookup"><span data-stu-id="39f49-163">hello current Gateway CPU usage is > 95%.</span></span> | <span data-ttu-id="39f49-164">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-164">Yes</span></span> |

### <a name="connection"></a><span data-ttu-id="39f49-165">Anslutning</span><span class="sxs-lookup"><span data-stu-id="39f49-165">Connection</span></span>

| <span data-ttu-id="39f49-166">Feltypen</span><span class="sxs-lookup"><span data-stu-id="39f49-166">Fault Type</span></span> | <span data-ttu-id="39f49-167">Orsak</span><span class="sxs-lookup"><span data-stu-id="39f49-167">Reason</span></span> | <span data-ttu-id="39f49-168">Logga</span><span class="sxs-lookup"><span data-stu-id="39f49-168">Log</span></span>|
|---|---|---|
| <span data-ttu-id="39f49-169">NoFault</span><span class="sxs-lookup"><span data-stu-id="39f49-169">NoFault</span></span> | <span data-ttu-id="39f49-170">Om inga fel har identifierats.</span><span class="sxs-lookup"><span data-stu-id="39f49-170">When no error is detected.</span></span> |<span data-ttu-id="39f49-171">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-171">Yes</span></span>|
| <span data-ttu-id="39f49-172">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="39f49-172">GatewayNotFound</span></span> | <span data-ttu-id="39f49-173">Det går inte att hitta Gateway eller Gateway inte har etablerats.</span><span class="sxs-lookup"><span data-stu-id="39f49-173">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="39f49-174">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-174">No</span></span>|
| <span data-ttu-id="39f49-175">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="39f49-175">PlannedMaintenance</span></span> | <span data-ttu-id="39f49-176">Gateway-instans är under underhåll.</span><span class="sxs-lookup"><span data-stu-id="39f49-176">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="39f49-177">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-177">No</span></span>|
| <span data-ttu-id="39f49-178">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="39f49-178">UserDrivenUpdate</span></span> | <span data-ttu-id="39f49-179">När en användare uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="39f49-179">When a user update is in progress.</span></span> <span data-ttu-id="39f49-180">Detta kan vara en åtgärd för storleksändring.</span><span class="sxs-lookup"><span data-stu-id="39f49-180">This could be a resize operation.</span></span>  | <span data-ttu-id="39f49-181">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-181">No</span></span> |
| <span data-ttu-id="39f49-182">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="39f49-182">VipUnResponsive</span></span> | <span data-ttu-id="39f49-183">Det går inte att nå hello primära instansen av hello Gateway.</span><span class="sxs-lookup"><span data-stu-id="39f49-183">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="39f49-184">Det händer när hello hälsoavsökningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="39f49-184">It happens when hello health probe fails.</span></span> | <span data-ttu-id="39f49-185">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-185">No</span></span> |
| <span data-ttu-id="39f49-186">ConnectionEntityNotFound</span><span class="sxs-lookup"><span data-stu-id="39f49-186">ConnectionEntityNotFound</span></span> | <span data-ttu-id="39f49-187">Konfigurationen för anslutningen saknas.</span><span class="sxs-lookup"><span data-stu-id="39f49-187">Connection configuration is missing.</span></span> | <span data-ttu-id="39f49-188">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-188">No</span></span> |
| <span data-ttu-id="39f49-189">ConnectionIsMarkedDisconnected</span><span class="sxs-lookup"><span data-stu-id="39f49-189">ConnectionIsMarkedDisconnected</span></span> | <span data-ttu-id="39f49-190">hello anslutning markeras ”frånkopplad”.</span><span class="sxs-lookup"><span data-stu-id="39f49-190">hello Connection is marked "disconnected".</span></span> |<span data-ttu-id="39f49-191">Nej</span><span class="sxs-lookup"><span data-stu-id="39f49-191">No</span></span>|
| <span data-ttu-id="39f49-192">ConnectionNotConfiguredOnGateway</span><span class="sxs-lookup"><span data-stu-id="39f49-192">ConnectionNotConfiguredOnGateway</span></span> | <span data-ttu-id="39f49-193">hello underliggande tjänsten har inte hello anslutning konfigureras.</span><span class="sxs-lookup"><span data-stu-id="39f49-193">hello underlying service does not have hello Connection configured.</span></span> | <span data-ttu-id="39f49-194">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-194">Yes</span></span> |
| <span data-ttu-id="39f49-195">ConnectionMarkedStandy</span><span class="sxs-lookup"><span data-stu-id="39f49-195">ConnectionMarkedStandy</span></span> | <span data-ttu-id="39f49-196">hello underliggande tjänsten har markerats som vänteläge.</span><span class="sxs-lookup"><span data-stu-id="39f49-196">hello underlying service is marked as standby.</span></span>| <span data-ttu-id="39f49-197">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-197">Yes</span></span>|
| <span data-ttu-id="39f49-198">Autentisering</span><span class="sxs-lookup"><span data-stu-id="39f49-198">Authentication</span></span> | <span data-ttu-id="39f49-199">I förväg delad nyckel matchar inte.</span><span class="sxs-lookup"><span data-stu-id="39f49-199">Preshared Key mismatch.</span></span> | <span data-ttu-id="39f49-200">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-200">Yes</span></span>|
| <span data-ttu-id="39f49-201">PeerReachability</span><span class="sxs-lookup"><span data-stu-id="39f49-201">PeerReachability</span></span> | <span data-ttu-id="39f49-202">hello peer-gateway kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="39f49-202">hello peer gateway is not reachable.</span></span> | <span data-ttu-id="39f49-203">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-203">Yes</span></span>|
| <span data-ttu-id="39f49-204">IkePolicyMismatch</span><span class="sxs-lookup"><span data-stu-id="39f49-204">IkePolicyMismatch</span></span> | <span data-ttu-id="39f49-205">hello peer-gateway har IKE-principer som inte stöds av Azure.</span><span class="sxs-lookup"><span data-stu-id="39f49-205">hello peer gateway has IKE policies that are not supported by Azure.</span></span> | <span data-ttu-id="39f49-206">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-206">Yes</span></span>|
| <span data-ttu-id="39f49-207">WfpParse fel</span><span class="sxs-lookup"><span data-stu-id="39f49-207">WfpParse Error</span></span> | <span data-ttu-id="39f49-208">Ett fel uppstod tolkning hello WFP-loggen.</span><span class="sxs-lookup"><span data-stu-id="39f49-208">An error occurred parsing hello WFP log.</span></span> |<span data-ttu-id="39f49-209">Ja</span><span class="sxs-lookup"><span data-stu-id="39f49-209">Yes</span></span>|

## <a name="supported-gateway-types"></a><span data-ttu-id="39f49-210">Gateway-typer som stöds</span><span class="sxs-lookup"><span data-stu-id="39f49-210">Supported Gateway types</span></span>

<span data-ttu-id="39f49-211">hello följande lista visar hello stöd visar vilka gateways och anslutningar stöds med Nätverksbevakaren felsökning.</span><span class="sxs-lookup"><span data-stu-id="39f49-211">hello following list shows hello support shows which gateways and connections are supported with Network Watcher troubleshooting.</span></span>
|  |  |
|---------|---------|
|<span data-ttu-id="39f49-212">**Gateway-typer**</span><span class="sxs-lookup"><span data-stu-id="39f49-212">**Gateway types**</span></span>   |         |
|<span data-ttu-id="39f49-213">VPN</span><span class="sxs-lookup"><span data-stu-id="39f49-213">VPN</span></span>      | <span data-ttu-id="39f49-214">Stöds</span><span class="sxs-lookup"><span data-stu-id="39f49-214">Supported</span></span>        |
|<span data-ttu-id="39f49-215">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="39f49-215">ExpressRoute</span></span> | <span data-ttu-id="39f49-216">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-216">Not Supported</span></span> |
|<span data-ttu-id="39f49-217">Hypernet</span><span class="sxs-lookup"><span data-stu-id="39f49-217">Hypernet</span></span> | <span data-ttu-id="39f49-218">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-218">Not Supported</span></span>|
|<span data-ttu-id="39f49-219">**VPN-typer**</span><span class="sxs-lookup"><span data-stu-id="39f49-219">**VPN types**</span></span> | |
|<span data-ttu-id="39f49-220">Vägen baserat</span><span class="sxs-lookup"><span data-stu-id="39f49-220">Route Based</span></span> | <span data-ttu-id="39f49-221">Stöds</span><span class="sxs-lookup"><span data-stu-id="39f49-221">Supported</span></span>|
|<span data-ttu-id="39f49-222">Principbaserad</span><span class="sxs-lookup"><span data-stu-id="39f49-222">Policy Based</span></span> | <span data-ttu-id="39f49-223">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-223">Not Supported</span></span>|
|<span data-ttu-id="39f49-224">**Anslutningstyper**</span><span class="sxs-lookup"><span data-stu-id="39f49-224">**Connection types**</span></span>||
|<span data-ttu-id="39f49-225">IPSec</span><span class="sxs-lookup"><span data-stu-id="39f49-225">IPSec</span></span>| <span data-ttu-id="39f49-226">Stöds</span><span class="sxs-lookup"><span data-stu-id="39f49-226">Supported</span></span>|
|<span data-ttu-id="39f49-227">VNet2Vnet</span><span class="sxs-lookup"><span data-stu-id="39f49-227">VNet2Vnet</span></span>| <span data-ttu-id="39f49-228">Stöds</span><span class="sxs-lookup"><span data-stu-id="39f49-228">Supported</span></span>|
|<span data-ttu-id="39f49-229">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="39f49-229">ExpressRoute</span></span>| <span data-ttu-id="39f49-230">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-230">Not Supported</span></span>|
|<span data-ttu-id="39f49-231">Hypernet</span><span class="sxs-lookup"><span data-stu-id="39f49-231">Hypernet</span></span>| <span data-ttu-id="39f49-232">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-232">Not Supported</span></span>|
|<span data-ttu-id="39f49-233">VPNClient</span><span class="sxs-lookup"><span data-stu-id="39f49-233">VPNClient</span></span>| <span data-ttu-id="39f49-234">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="39f49-234">Not Supported</span></span>|

## <a name="log-files"></a><span data-ttu-id="39f49-235">Loggfiler</span><span class="sxs-lookup"><span data-stu-id="39f49-235">Log files</span></span>

<span data-ttu-id="39f49-236">hello resurs felsökning loggfilerna lagras i ett lagringskonto efter resurs felsökning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="39f49-236">hello resource troubleshooting log files are stored in a storage account after resource troubleshooting is finished.</span></span> <span data-ttu-id="39f49-237">hello visar följande bild hello exempel innehållet i ett anrop som resulterade i ett fel.</span><span class="sxs-lookup"><span data-stu-id="39f49-237">hello following image shows hello example contents of a call that resulted in an error.</span></span>

![ZIP-filen][1]

> [!NOTE]
> <span data-ttu-id="39f49-239">I vissa fall kan skrivs endast en delmängd av hello loggfiler toostorage.</span><span class="sxs-lookup"><span data-stu-id="39f49-239">In some cases, only a subset of hello logs files is written toostorage.</span></span>

<span data-ttu-id="39f49-240">Anvisningar för hämtning av filer från azure storage-konton finns för[komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="39f49-240">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="39f49-241">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="39f49-241">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="39f49-242">Mer information om Lagringsutforskaren hittar du här på hello följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="39f49-242">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

### <a name="connectionstatstxt"></a><span data-ttu-id="39f49-243">ConnectionStats.txt</span><span class="sxs-lookup"><span data-stu-id="39f49-243">ConnectionStats.txt</span></span>

<span data-ttu-id="39f49-244">Hej **ConnectionStats.txt** filen innehåller övergripande statistik för hello anslutningen, inklusive ingående och utgående byte och anslutningsstatus hello tid hello anslutning har upprättats.</span><span class="sxs-lookup"><span data-stu-id="39f49-244">hello **ConnectionStats.txt** file contains overall stats of hello Connection, including ingress and egress bytes, Connection status, and hello time hello Connection was established.</span></span>

> [!NOTE]
> <span data-ttu-id="39f49-245">Om hello anropet toohello felsökning API returnerar felfri hello enda returneras i hello zip-filen är en **ConnectionStats.txt** fil.</span><span class="sxs-lookup"><span data-stu-id="39f49-245">If hello call toohello troubleshooting API returns healthy, hello only thing returned in hello zip file is a **ConnectionStats.txt** file.</span></span>

<span data-ttu-id="39f49-246">hello innehållet i den här filen är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="39f49-246">hello contents of this file are similar toohello following example:</span></span>

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a><span data-ttu-id="39f49-247">CPUStats.txt</span><span class="sxs-lookup"><span data-stu-id="39f49-247">CPUStats.txt</span></span>

<span data-ttu-id="39f49-248">Hej **CPUStats.txt** filen innehåller CPU-användning och minne för närvarande hello tester.</span><span class="sxs-lookup"><span data-stu-id="39f49-248">hello **CPUStats.txt** file contains CPU usage and memory available at hello time of testing.</span></span>  <span data-ttu-id="39f49-249">hello innehållet i den här filen är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="39f49-249">hello contents of this file is similar toohello following example:</span></span>

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a><span data-ttu-id="39f49-250">IKEErrors.txt</span><span class="sxs-lookup"><span data-stu-id="39f49-250">IKEErrors.txt</span></span>

<span data-ttu-id="39f49-251">Hej **IKEErrors.txt** filen innehåller några IKE-fel som identifierades under övervakning.</span><span class="sxs-lookup"><span data-stu-id="39f49-251">hello **IKEErrors.txt** file contains any IKE errors that were found during monitoring.</span></span>

<span data-ttu-id="39f49-252">hello visar följande exempel hello innehållet i en IKEErrors.txt-fil.</span><span class="sxs-lookup"><span data-stu-id="39f49-252">hello following example shows hello contents of an IKEErrors.txt file.</span></span> <span data-ttu-id="39f49-253">Felen kan vara olika beroende på hello problemet.</span><span class="sxs-lookup"><span data-stu-id="39f49-253">Your errors may be different depending on hello issue.</span></span>

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a><span data-ttu-id="39f49-254">Befordras wfpdiag.txt</span><span class="sxs-lookup"><span data-stu-id="39f49-254">Scrubbed-wfpdiag.txt</span></span>

<span data-ttu-id="39f49-255">Hej **Scrubbed wfpdiag.txt** loggfilen innehåller hello wfp-loggen.</span><span class="sxs-lookup"><span data-stu-id="39f49-255">hello **Scrubbed-wfpdiag.txt** log file contains hello wfp log.</span></span> <span data-ttu-id="39f49-256">Den här loggfilen innehåller loggning av paketet släpp och IKE/AuthIP fel.</span><span class="sxs-lookup"><span data-stu-id="39f49-256">This log contains logging of packet drop and IKE/AuthIP failures.</span></span>

<span data-ttu-id="39f49-257">hello visar följande exempel hello innehållet i hello Scrubbed wfpdiag.txt fil.</span><span class="sxs-lookup"><span data-stu-id="39f49-257">hello following example shows hello contents of hello Scrubbed-wfpdiag.txt file.</span></span> <span data-ttu-id="39f49-258">I det här exemplet hello delade nyckeln för en anslutning inte på rätt sätt kan ses från hello 3: e raden från hello längst ned.</span><span class="sxs-lookup"><span data-stu-id="39f49-258">In this example, hello shared key of a Connection was not correct as can be seen from hello 3rd line from hello bottom.</span></span> <span data-ttu-id="39f49-259">hello följande exempel är bara ett fragment för hello hela loggen, eftersom hello loggen kan vara tidskrävande beroende på hello problemet.</span><span class="sxs-lookup"><span data-stu-id="39f49-259">hello following example is just a snippet of hello entire log, as hello log can be lengthy depending on hello issue.</span></span>

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a><span data-ttu-id="39f49-260">wfpdiag.txt.SUM</span><span class="sxs-lookup"><span data-stu-id="39f49-260">wfpdiag.txt.sum</span></span>

<span data-ttu-id="39f49-261">Hej **wfpdiag.txt.sum** filen är en logg som visar hello buffertar och händelser som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="39f49-261">hello **wfpdiag.txt.sum** file is a log showing hello buffers and events processed.</span></span>

<span data-ttu-id="39f49-262">hello är följande exempel hello innehållet i en wfpdiag.txt.sum-fil.</span><span class="sxs-lookup"><span data-stu-id="39f49-262">hello following example is hello contents of a wfpdiag.txt.sum file.</span></span>
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a><span data-ttu-id="39f49-263">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39f49-263">Next steps</span></span>

<span data-ttu-id="39f49-264">Lär dig hur toodiagnose VPN-gatewayer och anslutningar via hello portal genom att besöka [Gateway felsökning – Azure-portalen](network-watcher-troubleshoot-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="39f49-264">Learn how toodiagnose VPN Gateways and Connections through hello portal by visiting [Gateway troubleshooting - Azure portal](network-watcher-troubleshoot-manage-portal.md).</span></span>
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
