---
title: aaaHybrid utformning av DRM subsystem(s) med Azure Media Services | Microsoft Docs
description: "Det här avsnittet beskrivs hybridutformning av DRM subsystem(s) med Azure Media Services."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 4206248420ccd4dbfc9a87a86f4763534c6254a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-design-of-drm-subsystems"></a><span data-ttu-id="3ae94-103">Hybridutformning av DRM subsystem(s)</span><span class="sxs-lookup"><span data-stu-id="3ae94-103">Hybrid design of DRM subsystem(s)</span></span>

<span data-ttu-id="3ae94-104">Det här avsnittet beskrivs hybridutformning av DRM subsystem(s) med Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="3ae94-104">This topic discusses hybrid design of DRM subsystem(s) using Azure Media Services.</span></span>

## <a name="overview"></a><span data-ttu-id="3ae94-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="3ae94-105">Overview</span></span>

<span data-ttu-id="3ae94-106">Azure Media Services tillhandahåller support för hello följande tre DRM-systemet:</span><span class="sxs-lookup"><span data-stu-id="3ae94-106">Azure Media Services provides support for hello following three DRM system:</span></span>

* <span data-ttu-id="3ae94-107">PlayReady</span><span class="sxs-lookup"><span data-stu-id="3ae94-107">PlayReady</span></span>
* <span data-ttu-id="3ae94-108">Widevine (modulära)</span><span class="sxs-lookup"><span data-stu-id="3ae94-108">Widevine (Modular)</span></span>
* <span data-ttu-id="3ae94-109">FairPlay</span><span class="sxs-lookup"><span data-stu-id="3ae94-109">FairPlay</span></span>

<span data-ttu-id="3ae94-110">hello DRM stödet ingår DRM-kryptering (dynamisk kryptering) och licensleverans med Azure Media Player stöder alla 3 DRMs som en webbläsare spelare SDK.</span><span class="sxs-lookup"><span data-stu-id="3ae94-110">hello DRM support includes DRM encryption (dynamic encryption) and license delivery, with Azure Media Player supporting all 3 DRMs as a browser player SDK.</span></span>

<span data-ttu-id="3ae94-111">En detaljerad behandling av DRM/CENC undersystemet design och implementering finns i dokumentet hello [CENC med Multi-DRM och Access Control](media-services-cenc-with-multidrm-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="3ae94-111">For a detailed treatment of DRM/CENC subsystem design and implementation, please see hello document titled [CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md).</span></span>

<span data-ttu-id="3ae94-112">Även om vi har fullt stöd för tre DRM-system, måste ibland kunder toouse olika delar av sin egen infrastruktur/delsystem i tillägg tooAzure Media Services toobuild hybrid DRM-undersystemet.</span><span class="sxs-lookup"><span data-stu-id="3ae94-112">Although we offer complete support for three DRM systems, sometimes customers need toouse various parts of their own infrastructure/subsystems in addition tooAzure Media Services toobuild a hybrid DRM subsystem.</span></span>

<span data-ttu-id="3ae94-113">Nedan är några vanliga frågor och svar av kunder:</span><span class="sxs-lookup"><span data-stu-id="3ae94-113">Below are some common questions asked by customers:</span></span>

* <span data-ttu-id="3ae94-114">”Kan jag använda min egen DRM licensservrar”?</span><span class="sxs-lookup"><span data-stu-id="3ae94-114">"Can I use my own DRM license servers?"</span></span> <span data-ttu-id="3ae94-115">(I det här fallet kunder har investerat i DRM-licens serverkluster med inbäddade affärslogik).</span><span class="sxs-lookup"><span data-stu-id="3ae94-115">(In this case, customers have invested in DRM license server farm with embedded business logic).</span></span>
* <span data-ttu-id="3ae94-116">”Kan jag använda bara din leverans för DRM-licens i Azure Media Services utan värd för innehållet i AMS”?</span><span class="sxs-lookup"><span data-stu-id="3ae94-116">"Can I use only your DRM license delivery in Azure Media Services without hosting content in AMS?"</span></span>

## <a name="modularity-of-hello-ams-drm-platform"></a><span data-ttu-id="3ae94-117">Modularitet av hello DRM AMS-plattformen</span><span class="sxs-lookup"><span data-stu-id="3ae94-117">Modularity of hello AMS DRM platform</span></span>

<span data-ttu-id="3ae94-118">Som en del av en omfattande video molnplattform har Azure Media Services DRM en design med flexibiliteten och modulariteten i åtanke.</span><span class="sxs-lookup"><span data-stu-id="3ae94-118">As part of a comprehensive cloud video platform, Azure Media Services DRM has a design with flexibility and modularity in mind.</span></span> <span data-ttu-id="3ae94-119">Du kan använda Azure Media Services med någon av hello efter olika kombinationer som beskrivs i hello nedan (följer en förklaring av hello notation som används i hello tabell).</span><span class="sxs-lookup"><span data-stu-id="3ae94-119">You can use Azure Media Services with any of hello following different combinations described in hello table below (an explanation of hello notation used in hello table follows).</span></span> 

|<span data-ttu-id="3ae94-120">**Värd för innehåll & ursprung**</span><span class="sxs-lookup"><span data-stu-id="3ae94-120">**Content hosting & origin**</span></span>|<span data-ttu-id="3ae94-121">**Innehåll kryptering**</span><span class="sxs-lookup"><span data-stu-id="3ae94-121">**Content encryption**</span></span>|<span data-ttu-id="3ae94-122">**DRM-licensleverans**</span><span class="sxs-lookup"><span data-stu-id="3ae94-122">**DRM license delivery**</span></span>|
|---|---|---|
|<span data-ttu-id="3ae94-123">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-123">AMS</span></span>|<span data-ttu-id="3ae94-124">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-124">AMS</span></span>|<span data-ttu-id="3ae94-125">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-125">AMS</span></span>|
|<span data-ttu-id="3ae94-126">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-126">AMS</span></span>|<span data-ttu-id="3ae94-127">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-127">AMS</span></span>|<span data-ttu-id="3ae94-128">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-128">Third-party</span></span>|
|<span data-ttu-id="3ae94-129">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-129">AMS</span></span>|<span data-ttu-id="3ae94-130">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-130">Third-party</span></span>|<span data-ttu-id="3ae94-131">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-131">AMS</span></span>|
|<span data-ttu-id="3ae94-132">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-132">AMS</span></span>|<span data-ttu-id="3ae94-133">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-133">Third-party</span></span>|<span data-ttu-id="3ae94-134">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-134">Third-party</span></span>|
|<span data-ttu-id="3ae94-135">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-135">Third-party</span></span>|<span data-ttu-id="3ae94-136">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-136">Third-party</span></span>|<span data-ttu-id="3ae94-137">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-137">AMS</span></span>|

### <a name="content-hosting--origin"></a><span data-ttu-id="3ae94-138">Värd för innehåll & ursprung</span><span class="sxs-lookup"><span data-stu-id="3ae94-138">Content hosting & origin</span></span>

* <span data-ttu-id="3ae94-139">AMS: videotillgång finns i AMS och strömning är via AMS strömningsslutpunkter (men inte nödvändigtvis dynamisk paketering).</span><span class="sxs-lookup"><span data-stu-id="3ae94-139">AMS: video asset is hosted in AMS and streaming is through AMS streaming endpoints (but not necessarily dynamic packaging).</span></span>
* <span data-ttu-id="3ae94-140">Från tredje part: video är värdbaserad och levereras på en tredje parts strömmande plattform utanför AMS.</span><span class="sxs-lookup"><span data-stu-id="3ae94-140">Third-party: video is hosted and delivered on a third-party streaming platform outside of AMS.</span></span>

### <a name="content-encryption"></a><span data-ttu-id="3ae94-141">Innehåll kryptering</span><span class="sxs-lookup"><span data-stu-id="3ae94-141">Content encryption</span></span>

* <span data-ttu-id="3ae94-142">AMS: kryptering av innehåll är utförs dynamiskt/på begäran av AMS dynamisk kryptering.</span><span class="sxs-lookup"><span data-stu-id="3ae94-142">AMS: content encryption is performed dynamically/on-demand by AMS dynamic encryption.</span></span>
* <span data-ttu-id="3ae94-143">Från tredje part: innehåll kryptering utförs utanför AMS med före bearbetning arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="3ae94-143">Third-party: content encryption is performed outside of AMS using a pre-processing workflow.</span></span>

### <a name="drm-license-delivery"></a><span data-ttu-id="3ae94-144">DRM-licensleverans</span><span class="sxs-lookup"><span data-stu-id="3ae94-144">DRM license delivery</span></span>

* <span data-ttu-id="3ae94-145">AMS: DRM-licens levereras av AMS-Licenstjänsten leverans.</span><span class="sxs-lookup"><span data-stu-id="3ae94-145">AMS: DRM license is delivered by AMS license delivery service.</span></span>
* <span data-ttu-id="3ae94-146">Från tredje part: DRM-licens levereras av en tredje parts DRM licensserver utanför AMS.</span><span class="sxs-lookup"><span data-stu-id="3ae94-146">Third-party: DRM license is delivered by a third-party DRM license server outside of AMS.</span></span>

## <a name="configure-based-on-your-hybrid-scenario"></a><span data-ttu-id="3ae94-147">Konfigurera baserat på din hybridscenario</span><span class="sxs-lookup"><span data-stu-id="3ae94-147">Configure based on your hybrid scenario</span></span>

### <a name="content-key"></a><span data-ttu-id="3ae94-148">Innehållsnyckel</span><span class="sxs-lookup"><span data-stu-id="3ae94-148">Content key</span></span>

<span data-ttu-id="3ae94-149">Du kan styra följande attribut för både AMS dynamisk kryptering och AMS-Licenstjänsten leverans hello genom konfigurationen av en innehållsnyckel:</span><span class="sxs-lookup"><span data-stu-id="3ae94-149">Through configuration of a content key, you can control hello following attributes of both AMS dynamic encryption and AMS license delivery service:</span></span>

* <span data-ttu-id="3ae94-150">hello innehållsnyckeln används för dynamisk DRM-kryptering.</span><span class="sxs-lookup"><span data-stu-id="3ae94-150">hello content key used for dynamic DRM encryption.</span></span>
* <span data-ttu-id="3ae94-151">DRM-licens innehåll toobe levereras av licensleveranstjänster: rättigheter, innehållsnyckeln och begränsningar.</span><span class="sxs-lookup"><span data-stu-id="3ae94-151">DRM license content toobe delivered by license delivery services: rights, content key and restrictions.</span></span>
* <span data-ttu-id="3ae94-152">Typ av **innehåll begränsning för auktorisering av innehållsnyckel princip**: Öppna IP- eller tokenbegränsningar.</span><span class="sxs-lookup"><span data-stu-id="3ae94-152">Type of **content key authorization policy restriction**: open, IP, or token restriction.</span></span>
* <span data-ttu-id="3ae94-153">Om **token** typ av **begränsning för auktorisering av innehållsnyckel principen används**, hello **innehåll begränsning för auktorisering av innehållsnyckel princip** måste vara uppfyllda innan en licens utfärdas.</span><span class="sxs-lookup"><span data-stu-id="3ae94-153">If **token** type of **content key authorization policy restriction is used**, hello **content key authorization policy restriction** must be met before a license is issued.</span></span>

### <a name="asset-delivery-policy"></a><span data-ttu-id="3ae94-154">Principen för tillgångsleverans</span><span class="sxs-lookup"><span data-stu-id="3ae94-154">Asset delivery policy</span></span>

<span data-ttu-id="3ae94-155">Du kan styra hello följande attribut som används av AMS dynamiska Paketeraren och dynamisk kryptering av ett AMS strömmande slutpunkten genom konfigurationen av en tillgångsleveransprincip:</span><span class="sxs-lookup"><span data-stu-id="3ae94-155">Through configuration of an asset delivery policy, you can control hello following attributes used by AMS dynamic packager and dynamic encryption of an AMS streaming endpoint:</span></span>

* <span data-ttu-id="3ae94-156">Direktuppspelning DRM kryptering kombinationen och protokoll, till exempel STRECK under CENC (PlayReady och Widevine), jämna strömning under PlayReady HLS under Widevine eller PlayReady.</span><span class="sxs-lookup"><span data-stu-id="3ae94-156">Streaming protocol and DRM encryption combination, such as DASH under CENC (PlayReady and Widevine), smooth streaming under PlayReady, HLS under Widevine or PlayReady.</span></span>
* <span data-ttu-id="3ae94-157">hello standard/inbäddade licens leverans URL: er för varje hello berörda DRMs.</span><span class="sxs-lookup"><span data-stu-id="3ae94-157">hello default/embedded license delivery URLs for each of hello involved DRMs.</span></span>
* <span data-ttu-id="3ae94-158">Licens om förvärv URL: er (LA_URLs) i DASH MPD eller HLS spelningslista innehåller frågesträngen nyckel (KID) för Widevine och FairPlay, respektive.</span><span class="sxs-lookup"><span data-stu-id="3ae94-158">Whether license acquisition URLs (LA_URLs) in DASH MPD or HLS playlist contain query string of key ID (KID) for Widevine and FairPlay, respectively.</span></span>

## <a name="scenarios-and-samples"></a><span data-ttu-id="3ae94-159">Scenarier och exempel</span><span class="sxs-lookup"><span data-stu-id="3ae94-159">Scenarios and samples</span></span>

<span data-ttu-id="3ae94-160">Baserat på hello förklaringar hello föregående avsnitt, hello följande fem hybridscenarion använda respektive **innehållsnyckeln**-**tillgångsleveransprincip** configuration kombinationer (hello exempel som nämns i hello sista kolumnen så hello tabell):</span><span class="sxs-lookup"><span data-stu-id="3ae94-160">Based on hello explanations in hello previous section, hello following five hybrid scenarios use respective **Content key**-**Asset delivery policy** configuration combinations (hello samples mentioned in hello last column follow hello table):</span></span>

|<span data-ttu-id="3ae94-161">**Värd för innehåll & ursprung**</span><span class="sxs-lookup"><span data-stu-id="3ae94-161">**Content hosting & origin**</span></span>|<span data-ttu-id="3ae94-162">**DRM-kryptering**</span><span class="sxs-lookup"><span data-stu-id="3ae94-162">**DRM encryption**</span></span>|<span data-ttu-id="3ae94-163">**DRM-licensleverans**</span><span class="sxs-lookup"><span data-stu-id="3ae94-163">**DRM license delivery**</span></span>|<span data-ttu-id="3ae94-164">**Konfigurera innehållsnyckeln**</span><span class="sxs-lookup"><span data-stu-id="3ae94-164">**Configure content key**</span></span>|<span data-ttu-id="3ae94-165">**Konfigurera tillgångsleveransprincip**</span><span class="sxs-lookup"><span data-stu-id="3ae94-165">**Configure asset delivery policy**</span></span>|<span data-ttu-id="3ae94-166">**Exempel**</span><span class="sxs-lookup"><span data-stu-id="3ae94-166">**Sample**</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="3ae94-167">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-167">AMS</span></span>|<span data-ttu-id="3ae94-168">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-168">AMS</span></span>|<span data-ttu-id="3ae94-169">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-169">AMS</span></span>|<span data-ttu-id="3ae94-170">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-170">Yes</span></span>|<span data-ttu-id="3ae94-171">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-171">Yes</span></span>|<span data-ttu-id="3ae94-172">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="3ae94-172">Sample 1</span></span>|
|<span data-ttu-id="3ae94-173">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-173">AMS</span></span>|<span data-ttu-id="3ae94-174">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-174">AMS</span></span>|<span data-ttu-id="3ae94-175">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-175">Third-party</span></span>|<span data-ttu-id="3ae94-176">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-176">Yes</span></span>|<span data-ttu-id="3ae94-177">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-177">Yes</span></span>|<span data-ttu-id="3ae94-178">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="3ae94-178">Sample 2</span></span>|
|<span data-ttu-id="3ae94-179">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-179">AMS</span></span>|<span data-ttu-id="3ae94-180">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-180">Third-party</span></span>|<span data-ttu-id="3ae94-181">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-181">AMS</span></span>|<span data-ttu-id="3ae94-182">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-182">Yes</span></span>|<span data-ttu-id="3ae94-183">Nej</span><span class="sxs-lookup"><span data-stu-id="3ae94-183">No</span></span>|<span data-ttu-id="3ae94-184">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="3ae94-184">Sample 3</span></span>|
|<span data-ttu-id="3ae94-185">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-185">AMS</span></span>|<span data-ttu-id="3ae94-186">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-186">Third-party</span></span>|<span data-ttu-id="3ae94-187">Utanför</span><span class="sxs-lookup"><span data-stu-id="3ae94-187">Outside</span></span>|<span data-ttu-id="3ae94-188">Nej</span><span class="sxs-lookup"><span data-stu-id="3ae94-188">No</span></span>|<span data-ttu-id="3ae94-189">Nej</span><span class="sxs-lookup"><span data-stu-id="3ae94-189">No</span></span>|<span data-ttu-id="3ae94-190">Exempel 4</span><span class="sxs-lookup"><span data-stu-id="3ae94-190">Sample 4</span></span>|
|<span data-ttu-id="3ae94-191">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-191">Third-party</span></span>|<span data-ttu-id="3ae94-192">Från tredje part</span><span class="sxs-lookup"><span data-stu-id="3ae94-192">Third-party</span></span>|<span data-ttu-id="3ae94-193">AMS</span><span class="sxs-lookup"><span data-stu-id="3ae94-193">AMS</span></span>|<span data-ttu-id="3ae94-194">Ja</span><span class="sxs-lookup"><span data-stu-id="3ae94-194">Yes</span></span>|<span data-ttu-id="3ae94-195">Nej</span><span class="sxs-lookup"><span data-stu-id="3ae94-195">No</span></span>|    

<span data-ttu-id="3ae94-196">I hello prover PlayReady skydd fungerar för både DASH och smooth streaming.</span><span class="sxs-lookup"><span data-stu-id="3ae94-196">In hello samples, PlayReady protection works for both DASH and smooth streaming.</span></span> <span data-ttu-id="3ae94-197">hello video adresserna nedan är smooth streaming URL: er.</span><span class="sxs-lookup"><span data-stu-id="3ae94-197">hello video URLs below are smooth streaming URLs.</span></span> <span data-ttu-id="3ae94-198">tooget Hej motsvarande DASH URL: er, bara lägga till ”(format = mpd-tid-csf)”.</span><span class="sxs-lookup"><span data-stu-id="3ae94-198">tooget hello corresponding DASH URLs, just append "(format=mpd-time-csf)".</span></span> <span data-ttu-id="3ae94-199">Du kan använda hello [azure media testa player](http://aka.ms/amtest) tootest i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="3ae94-199">You could use hello [azure media test player](http://aka.ms/amtest) tootest in a browser.</span></span> <span data-ttu-id="3ae94-200">Det gör att du tooconfigure som strömning protokollet toouse under vilka teknisk.</span><span class="sxs-lookup"><span data-stu-id="3ae94-200">It allows you tooconfigure which streaming protocol toouse, under which tech.</span></span> <span data-ttu-id="3ae94-201">Stöder PlayReady via EME IE11 och MS-Edge på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="3ae94-201">IE11 and MS Edge on Windows 10 support PlayReady through EME.</span></span> <span data-ttu-id="3ae94-202">Mer information finns i [information om hello testa verktyget](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span><span class="sxs-lookup"><span data-stu-id="3ae94-202">For more information, see [details about hello test tool](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span></span>

### <a name="sample-1"></a><span data-ttu-id="3ae94-203">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="3ae94-203">Sample 1</span></span>

* <span data-ttu-id="3ae94-204">Datakälla (bas)-URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="3ae94-204">Source (base) URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span></span> 
* <span data-ttu-id="3ae94-205">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="3ae94-205">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 
* <span data-ttu-id="3ae94-206">Widevine LA_URL (bindestreck): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span><span class="sxs-lookup"><span data-stu-id="3ae94-206">Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span></span> 
* <span data-ttu-id="3ae94-207">FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span><span class="sxs-lookup"><span data-stu-id="3ae94-207">FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span></span> 

### <a name="sample-2"></a><span data-ttu-id="3ae94-208">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="3ae94-208">Sample 2</span></span>

* <span data-ttu-id="3ae94-209">Datakälla (bas)-URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="3ae94-209">Source (base) URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span></span> 
* <span data-ttu-id="3ae94-210">PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span><span class="sxs-lookup"><span data-stu-id="3ae94-210">PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span></span> 

### <a name="sample-3"></a><span data-ttu-id="3ae94-211">Exempel 3</span><span class="sxs-lookup"><span data-stu-id="3ae94-211">Sample 3</span></span>

* <span data-ttu-id="3ae94-212">Käll-URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="3ae94-212">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="3ae94-213">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="3ae94-213">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 

### <a name="sample-4"></a><span data-ttu-id="3ae94-214">Exempel 4</span><span class="sxs-lookup"><span data-stu-id="3ae94-214">Sample 4</span></span>

* <span data-ttu-id="3ae94-215">Käll-URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="3ae94-215">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="3ae94-216">PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span><span class="sxs-lookup"><span data-stu-id="3ae94-216">PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span></span> 

## <a name="summary"></a><span data-ttu-id="3ae94-217">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3ae94-217">Summary</span></span>

<span data-ttu-id="3ae94-218">Sammanfattningsvis Azure Media Services DRM-komponenterna är flexibla, du kan använda dem i ett hybridscenario genom att konfigurera innehållsnyckeln och tillgångsleveransprincip, enligt beskrivningen i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3ae94-218">In summary, Azure Media Services DRM components are flexible, you can use them in a hybrid scenario by properly configuring content key and asset delivery policy, as described in this topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ae94-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3ae94-219">Next steps</span></span>
<span data-ttu-id="3ae94-220">Visa Media Services utbildningsvägar.</span><span class="sxs-lookup"><span data-stu-id="3ae94-220">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3ae94-221">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="3ae94-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

