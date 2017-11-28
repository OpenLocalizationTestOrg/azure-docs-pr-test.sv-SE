---
title: "Introduktion till resursen felsökning i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över funktioner för felsökning av Nätverksbevakaren resurs"
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
ms.openlocfilehash: 0d5091b682d1b25c47b224394bcc2c46366eeb2a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a><span data-ttu-id="2c2dc-103">Introduktion till felsökning i Azure Nätverksbevakaren resurs</span><span class="sxs-lookup"><span data-stu-id="2c2dc-103">Introduction to resource troubleshooting in Azure Network Watcher</span></span>

<span data-ttu-id="2c2dc-104">Virtuella Nätverksgatewayer anslutningsbarhet mellan lokala resurser och andra virtuella nätverk i Azure.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-104">Virtual Network Gateways provide connectivity between on-premises resources and other virtual networks within Azure.</span></span> <span data-ttu-id="2c2dc-105">Övervakning av dessa gateways och deras anslutningar är avgörande för att säkerställa kommunikationen är inte skadad.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-105">Monitoring these gateways and their Connections is critical to ensuring communication is not broken.</span></span> <span data-ttu-id="2c2dc-106">Nätverksbevakaren ger möjlighet att felsöka virtuella Nätverksgatewayer och anslutningar.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-106">Network Watcher provides the capability to troubleshoot Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="2c2dc-107">Det kan anropas via portalen, PowerShell, CLI eller REST API.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-107">This can be called through the portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="2c2dc-108">När den anropas, Nätverksbevakaren diagnostiserar hälsotillståndet för den virtuella nätverksgatewayen eller anslutning och rätt resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-108">When called, Network Watcher diagnoses the health of the virtual network gateway or connection and return the appropriate results.</span></span> <span data-ttu-id="2c2dc-109">Denna begäran är en tidskrävande transaktion, returneras resultaten när diagnosen är klar.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-109">This request is a long running transaction, the results are returned once the diagnosis is complete.</span></span>

![portal][2]

## <a name="results"></a><span data-ttu-id="2c2dc-111">Resultat</span><span class="sxs-lookup"><span data-stu-id="2c2dc-111">Results</span></span>

<span data-ttu-id="2c2dc-112">Preliminär resultaten ger en övergripande bild av hälsotillståndet för resursen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-112">The preliminary results returned give an overall picture of the health of the resource.</span></span> <span data-ttu-id="2c2dc-113">Mer detaljerad information kan anges för resurser som visas i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-113">Deeper information can be provided for resources as shown in the following section:</span></span>

<span data-ttu-id="2c2dc-114">Följande lista innehåller de värden som returneras med felsökning API:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-114">The following list is the values returned with the troubleshoot API:</span></span>

* <span data-ttu-id="2c2dc-115">**startTime** -värdet är felsöka API-anropet startades.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-115">**startTime** - This value is the time the troubleshoot API call started.</span></span>
* <span data-ttu-id="2c2dc-116">**endTime** -värdet är den tid då felsökning avslutades.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-116">**endTime** - This value is the time when the troubleshooting ended.</span></span>
* <span data-ttu-id="2c2dc-117">**koden** -värdet är ohälsosamma, om det finns ett enda diagnos-fel.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-117">**code** - This value is UnHealthy, if there is a single diagnosis failure.</span></span>
* <span data-ttu-id="2c2dc-118">**resultaten** -resultat är en samling av resultaten på anslutningen eller den virtuella nätverksgatewayen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-118">**results** - Results is a collection of results returned on the Connection or the virtual network gateway.</span></span>
    * <span data-ttu-id="2c2dc-119">**ID** -värdet är feltypen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-119">**id** - This value is the fault type.</span></span>
    * <span data-ttu-id="2c2dc-120">**Översikt över** -värdet är en sammanfattning av felet.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-120">**summary** - This value is a summary of the fault.</span></span>
    * <span data-ttu-id="2c2dc-121">**detaljerad** -värdet ger en detaljerad beskrivning av felet.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-121">**detailed** - This value provides a detailed description of the fault.</span></span>
    * <span data-ttu-id="2c2dc-122">**recommendedActions** -den här egenskapen är en uppsättning rekommenderade åtgärder som ska vidtas.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-122">**recommendedActions** - This property is a collection of recommended actions to take.</span></span>
      * <span data-ttu-id="2c2dc-123">**actionText** -värdet innehåller den text som beskriver åtgärd att vidta.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-123">**actionText** - This value contains the text describing what action to take.</span></span>
      * <span data-ttu-id="2c2dc-124">**actionUri** -värdet innehåller URI-dokumentationen om hur fungerar.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-124">**actionUri** - This value provides the URI to documentation on how to act.</span></span>
      * <span data-ttu-id="2c2dc-125">**actionUriText** -det här värdet är en kort beskrivning av åtgärden text.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-125">**actionUriText** - This value is a short description of the action text.</span></span>

<span data-ttu-id="2c2dc-126">Följande tabeller visar vilka typer av olika fel (id under resultat från föregående lista) som är tillgängliga och om felet skapar loggar.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-126">The following tables show the different fault types (id under results from the preceding list) that are available and if the fault creates logs.</span></span>

### <a name="gateway"></a><span data-ttu-id="2c2dc-127">Gateway</span><span class="sxs-lookup"><span data-stu-id="2c2dc-127">Gateway</span></span>

| <span data-ttu-id="2c2dc-128">Feltypen</span><span class="sxs-lookup"><span data-stu-id="2c2dc-128">Fault Type</span></span> | <span data-ttu-id="2c2dc-129">Orsak</span><span class="sxs-lookup"><span data-stu-id="2c2dc-129">Reason</span></span> | <span data-ttu-id="2c2dc-130">Logga</span><span class="sxs-lookup"><span data-stu-id="2c2dc-130">Log</span></span>|
|---|---|---|
| <span data-ttu-id="2c2dc-131">NoFault</span><span class="sxs-lookup"><span data-stu-id="2c2dc-131">NoFault</span></span> | <span data-ttu-id="2c2dc-132">Om inga fel har identifierats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-132">When no error is detected.</span></span> |<span data-ttu-id="2c2dc-133">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-133">Yes</span></span>|
| <span data-ttu-id="2c2dc-134">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="2c2dc-134">GatewayNotFound</span></span> | <span data-ttu-id="2c2dc-135">Det går inte att hitta Gateway eller Gateway inte har etablerats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-135">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="2c2dc-136">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-136">No</span></span>|
| <span data-ttu-id="2c2dc-137">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="2c2dc-137">PlannedMaintenance</span></span> |  <span data-ttu-id="2c2dc-138">Gateway-instans är under underhåll.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-138">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="2c2dc-139">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-139">No</span></span>|
| <span data-ttu-id="2c2dc-140">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="2c2dc-140">UserDrivenUpdate</span></span> | <span data-ttu-id="2c2dc-141">När en användare uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-141">When a user update is in progress.</span></span> <span data-ttu-id="2c2dc-142">Detta kan vara en åtgärd för storleksändring.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-142">This could be a resize operation.</span></span> | <span data-ttu-id="2c2dc-143">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-143">No</span></span> |
| <span data-ttu-id="2c2dc-144">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="2c2dc-144">VipUnResponsive</span></span> | <span data-ttu-id="2c2dc-145">Det går inte att nå den primära instansen av Gateway.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-145">Cannot reach the primary instance of the Gateway.</span></span> <span data-ttu-id="2c2dc-146">Detta händer när hälsoavsökningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-146">This happens when the health probe fails.</span></span> | <span data-ttu-id="2c2dc-147">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-147">No</span></span> |
| <span data-ttu-id="2c2dc-148">PlatformInActive</span><span class="sxs-lookup"><span data-stu-id="2c2dc-148">PlatformInActive</span></span> | <span data-ttu-id="2c2dc-149">Det finns ett problem med plattformen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-149">There is an issue with the platform.</span></span> | <span data-ttu-id="2c2dc-150">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-150">No</span></span>|
| <span data-ttu-id="2c2dc-151">ServiceNotRunning</span><span class="sxs-lookup"><span data-stu-id="2c2dc-151">ServiceNotRunning</span></span> | <span data-ttu-id="2c2dc-152">Den underliggande tjänsten körs inte.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-152">The underlying service is not running.</span></span> | <span data-ttu-id="2c2dc-153">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-153">No</span></span>|
| <span data-ttu-id="2c2dc-154">NoConnectionsFoundForGateway</span><span class="sxs-lookup"><span data-stu-id="2c2dc-154">NoConnectionsFoundForGateway</span></span> | <span data-ttu-id="2c2dc-155">Det finns inga anslutningar på gateway.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-155">No Connections exists on the gateway.</span></span> <span data-ttu-id="2c2dc-156">Detta är endast en varning.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-156">This is only a warning.</span></span>| <span data-ttu-id="2c2dc-157">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-157">No</span></span>|
| <span data-ttu-id="2c2dc-158">ConnectionsNotConnected</span><span class="sxs-lookup"><span data-stu-id="2c2dc-158">ConnectionsNotConnected</span></span> | <span data-ttu-id="2c2dc-159">Anslutningar är inte ansluten.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-159">Connections are not connected.</span></span> <span data-ttu-id="2c2dc-160">Detta är endast en varning.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-160">This is only a warning.</span></span>| <span data-ttu-id="2c2dc-161">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-161">Yes</span></span>|
| <span data-ttu-id="2c2dc-162">GatewayCPUUsageExceeded</span><span class="sxs-lookup"><span data-stu-id="2c2dc-162">GatewayCPUUsageExceeded</span></span> | <span data-ttu-id="2c2dc-163">Den aktuella Gateway CPU-användningen är > 95%.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-163">The current Gateway CPU usage is > 95%.</span></span> | <span data-ttu-id="2c2dc-164">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-164">Yes</span></span> |

### <a name="connection"></a><span data-ttu-id="2c2dc-165">Anslutning</span><span class="sxs-lookup"><span data-stu-id="2c2dc-165">Connection</span></span>

| <span data-ttu-id="2c2dc-166">Feltypen</span><span class="sxs-lookup"><span data-stu-id="2c2dc-166">Fault Type</span></span> | <span data-ttu-id="2c2dc-167">Orsak</span><span class="sxs-lookup"><span data-stu-id="2c2dc-167">Reason</span></span> | <span data-ttu-id="2c2dc-168">Logga</span><span class="sxs-lookup"><span data-stu-id="2c2dc-168">Log</span></span>|
|---|---|---|
| <span data-ttu-id="2c2dc-169">NoFault</span><span class="sxs-lookup"><span data-stu-id="2c2dc-169">NoFault</span></span> | <span data-ttu-id="2c2dc-170">Om inga fel har identifierats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-170">When no error is detected.</span></span> |<span data-ttu-id="2c2dc-171">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-171">Yes</span></span>|
| <span data-ttu-id="2c2dc-172">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="2c2dc-172">GatewayNotFound</span></span> | <span data-ttu-id="2c2dc-173">Det går inte att hitta Gateway eller Gateway inte har etablerats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-173">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="2c2dc-174">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-174">No</span></span>|
| <span data-ttu-id="2c2dc-175">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="2c2dc-175">PlannedMaintenance</span></span> | <span data-ttu-id="2c2dc-176">Gateway-instans är under underhåll.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-176">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="2c2dc-177">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-177">No</span></span>|
| <span data-ttu-id="2c2dc-178">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="2c2dc-178">UserDrivenUpdate</span></span> | <span data-ttu-id="2c2dc-179">När en användare uppdatering pågår.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-179">When a user update is in progress.</span></span> <span data-ttu-id="2c2dc-180">Detta kan vara en åtgärd för storleksändring.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-180">This could be a resize operation.</span></span>  | <span data-ttu-id="2c2dc-181">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-181">No</span></span> |
| <span data-ttu-id="2c2dc-182">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="2c2dc-182">VipUnResponsive</span></span> | <span data-ttu-id="2c2dc-183">Det går inte att nå den primära instansen av Gateway.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-183">Cannot reach the primary instance of the Gateway.</span></span> <span data-ttu-id="2c2dc-184">Det händer när hälsoavsökningen misslyckas.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-184">It happens when the health probe fails.</span></span> | <span data-ttu-id="2c2dc-185">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-185">No</span></span> |
| <span data-ttu-id="2c2dc-186">ConnectionEntityNotFound</span><span class="sxs-lookup"><span data-stu-id="2c2dc-186">ConnectionEntityNotFound</span></span> | <span data-ttu-id="2c2dc-187">Konfigurationen för anslutningen saknas.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-187">Connection configuration is missing.</span></span> | <span data-ttu-id="2c2dc-188">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-188">No</span></span> |
| <span data-ttu-id="2c2dc-189">ConnectionIsMarkedDisconnected</span><span class="sxs-lookup"><span data-stu-id="2c2dc-189">ConnectionIsMarkedDisconnected</span></span> | <span data-ttu-id="2c2dc-190">Anslutningen har markerats ”frånkopplad”.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-190">The Connection is marked "disconnected".</span></span> |<span data-ttu-id="2c2dc-191">Nej</span><span class="sxs-lookup"><span data-stu-id="2c2dc-191">No</span></span>|
| <span data-ttu-id="2c2dc-192">ConnectionNotConfiguredOnGateway</span><span class="sxs-lookup"><span data-stu-id="2c2dc-192">ConnectionNotConfiguredOnGateway</span></span> | <span data-ttu-id="2c2dc-193">Den underliggande tjänsten har inte konfigurerats anslutningen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-193">The underlying service does not have the Connection configured.</span></span> | <span data-ttu-id="2c2dc-194">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-194">Yes</span></span> |
| <span data-ttu-id="2c2dc-195">ConnectionMarkedStandy</span><span class="sxs-lookup"><span data-stu-id="2c2dc-195">ConnectionMarkedStandy</span></span> | <span data-ttu-id="2c2dc-196">Den underliggande tjänsten har markerats som vänteläge.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-196">The underlying service is marked as standby.</span></span>| <span data-ttu-id="2c2dc-197">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-197">Yes</span></span>|
| <span data-ttu-id="2c2dc-198">Autentisering</span><span class="sxs-lookup"><span data-stu-id="2c2dc-198">Authentication</span></span> | <span data-ttu-id="2c2dc-199">I förväg delad nyckel matchar inte.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-199">Preshared Key mismatch.</span></span> | <span data-ttu-id="2c2dc-200">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-200">Yes</span></span>|
| <span data-ttu-id="2c2dc-201">PeerReachability</span><span class="sxs-lookup"><span data-stu-id="2c2dc-201">PeerReachability</span></span> | <span data-ttu-id="2c2dc-202">Peer-gateway kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-202">The peer gateway is not reachable.</span></span> | <span data-ttu-id="2c2dc-203">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-203">Yes</span></span>|
| <span data-ttu-id="2c2dc-204">IkePolicyMismatch</span><span class="sxs-lookup"><span data-stu-id="2c2dc-204">IkePolicyMismatch</span></span> | <span data-ttu-id="2c2dc-205">Peer-gateway har IKE-principer som inte stöds av Azure.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-205">The peer gateway has IKE policies that are not supported by Azure.</span></span> | <span data-ttu-id="2c2dc-206">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-206">Yes</span></span>|
| <span data-ttu-id="2c2dc-207">WfpParse fel</span><span class="sxs-lookup"><span data-stu-id="2c2dc-207">WfpParse Error</span></span> | <span data-ttu-id="2c2dc-208">Ett fel uppstod vid parsning WFP-loggen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-208">An error occurred parsing the WFP log.</span></span> |<span data-ttu-id="2c2dc-209">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2dc-209">Yes</span></span>|

## <a name="supported-gateway-types"></a><span data-ttu-id="2c2dc-210">Gateway-typer som stöds</span><span class="sxs-lookup"><span data-stu-id="2c2dc-210">Supported Gateway types</span></span>

<span data-ttu-id="2c2dc-211">I följande lista visas stödet visar vilka gateways och anslutningar stöds med Nätverksbevakaren felsökning.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-211">The following list shows the support shows which gateways and connections are supported with Network Watcher troubleshooting.</span></span>
|  |  |
|---------|---------|
|<span data-ttu-id="2c2dc-212">**Gateway-typer**</span><span class="sxs-lookup"><span data-stu-id="2c2dc-212">**Gateway types**</span></span>   |         |
|<span data-ttu-id="2c2dc-213">VPN</span><span class="sxs-lookup"><span data-stu-id="2c2dc-213">VPN</span></span>      | <span data-ttu-id="2c2dc-214">Stöds</span><span class="sxs-lookup"><span data-stu-id="2c2dc-214">Supported</span></span>        |
|<span data-ttu-id="2c2dc-215">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="2c2dc-215">ExpressRoute</span></span> | <span data-ttu-id="2c2dc-216">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-216">Not Supported</span></span> |
|<span data-ttu-id="2c2dc-217">Hypernet</span><span class="sxs-lookup"><span data-stu-id="2c2dc-217">Hypernet</span></span> | <span data-ttu-id="2c2dc-218">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-218">Not Supported</span></span>|
|<span data-ttu-id="2c2dc-219">**VPN-typer**</span><span class="sxs-lookup"><span data-stu-id="2c2dc-219">**VPN types**</span></span> | |
|<span data-ttu-id="2c2dc-220">Vägen baserat</span><span class="sxs-lookup"><span data-stu-id="2c2dc-220">Route Based</span></span> | <span data-ttu-id="2c2dc-221">Stöds</span><span class="sxs-lookup"><span data-stu-id="2c2dc-221">Supported</span></span>|
|<span data-ttu-id="2c2dc-222">Principbaserad</span><span class="sxs-lookup"><span data-stu-id="2c2dc-222">Policy Based</span></span> | <span data-ttu-id="2c2dc-223">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-223">Not Supported</span></span>|
|<span data-ttu-id="2c2dc-224">**Anslutningstyper**</span><span class="sxs-lookup"><span data-stu-id="2c2dc-224">**Connection types**</span></span>||
|<span data-ttu-id="2c2dc-225">IPSec</span><span class="sxs-lookup"><span data-stu-id="2c2dc-225">IPSec</span></span>| <span data-ttu-id="2c2dc-226">Stöds</span><span class="sxs-lookup"><span data-stu-id="2c2dc-226">Supported</span></span>|
|<span data-ttu-id="2c2dc-227">VNet2Vnet</span><span class="sxs-lookup"><span data-stu-id="2c2dc-227">VNet2Vnet</span></span>| <span data-ttu-id="2c2dc-228">Stöds</span><span class="sxs-lookup"><span data-stu-id="2c2dc-228">Supported</span></span>|
|<span data-ttu-id="2c2dc-229">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="2c2dc-229">ExpressRoute</span></span>| <span data-ttu-id="2c2dc-230">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-230">Not Supported</span></span>|
|<span data-ttu-id="2c2dc-231">Hypernet</span><span class="sxs-lookup"><span data-stu-id="2c2dc-231">Hypernet</span></span>| <span data-ttu-id="2c2dc-232">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-232">Not Supported</span></span>|
|<span data-ttu-id="2c2dc-233">VPNClient</span><span class="sxs-lookup"><span data-stu-id="2c2dc-233">VPNClient</span></span>| <span data-ttu-id="2c2dc-234">Stöds inte</span><span class="sxs-lookup"><span data-stu-id="2c2dc-234">Not Supported</span></span>|

## <a name="log-files"></a><span data-ttu-id="2c2dc-235">Loggfiler</span><span class="sxs-lookup"><span data-stu-id="2c2dc-235">Log files</span></span>

<span data-ttu-id="2c2dc-236">Resursen felsökning loggfilerna lagras i ett lagringskonto efter resurs felsökning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-236">The resource troubleshooting log files are stored in a storage account after resource troubleshooting is finished.</span></span> <span data-ttu-id="2c2dc-237">Följande bild visar exempel innehållet i ett anrop som resulterade i ett fel.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-237">The following image shows the example contents of a call that resulted in an error.</span></span>

![ZIP-filen][1]

> [!NOTE]
> <span data-ttu-id="2c2dc-239">I vissa fall skrivs endast en delmängd av loggfiler till lagring.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-239">In some cases, only a subset of the logs files is written to storage.</span></span>

<span data-ttu-id="2c2dc-240">Anvisningar för att hämta filer från azure storage-konton, referera till [komma igång med Azure Blob storage med hjälp av .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="2c2dc-240">For instructions on downloading files from azure storage accounts, refer to [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="2c2dc-241">Ett annat verktyg som kan användas är Lagringsutforskaren.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-241">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="2c2dc-242">Mer information om Lagringsutforskaren hittar du här på följande länk: [Lagringsutforskaren](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="2c2dc-242">More information about Storage Explorer can be found here at the following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

### <a name="connectionstatstxt"></a><span data-ttu-id="2c2dc-243">ConnectionStats.txt</span><span class="sxs-lookup"><span data-stu-id="2c2dc-243">ConnectionStats.txt</span></span>

<span data-ttu-id="2c2dc-244">Den **ConnectionStats.txt** filen innehåller övergripande statistik för anslutningen, inklusive ingående och utgående byte, anslutningsstatus och den tid som anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-244">The **ConnectionStats.txt** file contains overall stats of the Connection, including ingress and egress bytes, Connection status, and the time the Connection was established.</span></span>

> [!NOTE]
> <span data-ttu-id="2c2dc-245">Om anropet till felsökning API returnerar felfri, det enda som returneras i zip-filen är en **ConnectionStats.txt** fil.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-245">If the call to the troubleshooting API returns healthy, the only thing returned in the zip file is a **ConnectionStats.txt** file.</span></span>

<span data-ttu-id="2c2dc-246">Innehållet i den här filen liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-246">The contents of this file are similar to the following example:</span></span>

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a><span data-ttu-id="2c2dc-247">CPUStats.txt</span><span class="sxs-lookup"><span data-stu-id="2c2dc-247">CPUStats.txt</span></span>

<span data-ttu-id="2c2dc-248">Den **CPUStats.txt** filen innehåller CPU-användning och minne vid tidpunkten för testning.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-248">The **CPUStats.txt** file contains CPU usage and memory available at the time of testing.</span></span>  <span data-ttu-id="2c2dc-249">Innehållet i den här filen liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2c2dc-249">The contents of this file is similar to the following example:</span></span>

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a><span data-ttu-id="2c2dc-250">IKEErrors.txt</span><span class="sxs-lookup"><span data-stu-id="2c2dc-250">IKEErrors.txt</span></span>

<span data-ttu-id="2c2dc-251">Den **IKEErrors.txt** filen innehåller några IKE-fel som identifierades under övervakning.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-251">The **IKEErrors.txt** file contains any IKE errors that were found during monitoring.</span></span>

<span data-ttu-id="2c2dc-252">I följande exempel visar innehållet i en IKEErrors.txt-fil.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-252">The following example shows the contents of an IKEErrors.txt file.</span></span> <span data-ttu-id="2c2dc-253">Felen kan vara olika beroende på problemet.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-253">Your errors may be different depending on the issue.</span></span>

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a><span data-ttu-id="2c2dc-254">Befordras wfpdiag.txt</span><span class="sxs-lookup"><span data-stu-id="2c2dc-254">Scrubbed-wfpdiag.txt</span></span>

<span data-ttu-id="2c2dc-255">Den **Scrubbed wfpdiag.txt** loggfilen innehåller wfp-loggen.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-255">The **Scrubbed-wfpdiag.txt** log file contains the wfp log.</span></span> <span data-ttu-id="2c2dc-256">Den här loggfilen innehåller loggning av paketet släpp och IKE/AuthIP fel.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-256">This log contains logging of packet drop and IKE/AuthIP failures.</span></span>

<span data-ttu-id="2c2dc-257">I följande exempel visar innehållet i filen Scrubbed wfpdiag.txt.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-257">The following example shows the contents of the Scrubbed-wfpdiag.txt file.</span></span> <span data-ttu-id="2c2dc-258">I det här exemplet var den delade nyckeln för en anslutning inte på rätt sätt kan ses från 3: e raden längst ned.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-258">In this example, the shared key of a Connection was not correct as can be seen from the 3rd line from the bottom.</span></span> <span data-ttu-id="2c2dc-259">I följande exempel är bara ett fragment av hela loggen eftersom loggen kan vara tidskrävande beroende på problemet.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-259">The following example is just a snippet of the entire log, as the log can be lengthy depending on the issue.</span></span>

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
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

### <a name="wfpdiagtxtsum"></a><span data-ttu-id="2c2dc-260">wfpdiag.txt.SUM</span><span class="sxs-lookup"><span data-stu-id="2c2dc-260">wfpdiag.txt.sum</span></span>

<span data-ttu-id="2c2dc-261">Den **wfpdiag.txt.sum** filen är en logg som innehåller buffertar och händelser som har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-261">The **wfpdiag.txt.sum** file is a log showing the buffers and events processed.</span></span>

<span data-ttu-id="2c2dc-262">I följande exempel är innehållet i en wfpdiag.txt.sum-fil.</span><span class="sxs-lookup"><span data-stu-id="2c2dc-262">The following example is the contents of a wfpdiag.txt.sum file.</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="2c2dc-263">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c2dc-263">Next steps</span></span>

<span data-ttu-id="2c2dc-264">Lär dig att diagnostisera VPN-gatewayer och anslutningar via portalen genom att besöka [Gateway felsökning – Azure-portalen](network-watcher-troubleshoot-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2c2dc-264">Learn how to diagnose VPN Gateways and Connections through the portal by visiting [Gateway troubleshooting - Azure portal](network-watcher-troubleshoot-manage-portal.md).</span></span>
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
