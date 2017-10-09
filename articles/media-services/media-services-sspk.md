---
title: "aaaLicensing Microsoft® Smooth Streaming klienten portera Kit"
description: "Läs mer om hur toolicensing hello Microsoft® Smooth Streaming klienten portera Kit."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: e3b488e7-8428-4c10-a072-eb3af46c82ad
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: xpouyat
ms.openlocfilehash: 56c3dccda73dd02207bb4dbe8109ba6fda917a6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a><span data-ttu-id="11c40-103">Licensiering Microsoft® Smooth Streaming klienten portera Kit</span><span class="sxs-lookup"><span data-stu-id="11c40-103">Licensing Microsoft® Smooth Streaming Client Porting Kit</span></span>
## <a name="overview"></a><span data-ttu-id="11c40-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="11c40-104">Overview</span></span>
<span data-ttu-id="11c40-105">Microsoft Smooth Streaming klienten portera Kit (**SSPK** för kort) är en Smooth Streaming-klient-implementering som är optimerade toohelp inbäddade enhetstillverkare, kabel och mobila operatorer, innehåll leverantörer, luren tillverkare, oberoende programvaruleverantörer (ISV) och solution providers toocreate produkter och tjänster för direktuppspelning av anpassningsbar strömning innehåll i Smooth Streaming-format.</span><span class="sxs-lookup"><span data-stu-id="11c40-105">Microsoft Smooth Streaming Client Porting Kit (**SSPK** for short) is a Smooth Streaming client implementation that is optimized toohelp embedded device manufacturers, cable and mobile operators, content service providers, handset manufacturers, independent software vendors (ISVs), and solution providers toocreate products and services for streaming adaptive streaming content in Smooth Streaming format.</span></span> <span data-ttu-id="11c40-106">SSPK är en enhet och plattform oberoende implementering av Smooth Streaming-klient som kan överföras av hello licenstagaren tooany enheten och plattform.</span><span class="sxs-lookup"><span data-stu-id="11c40-106">SSPK is a device and platform independent implementation of Smooth Streaming client that can be ported by hello licensee tooany device and platform.</span></span> 

<span data-ttu-id="11c40-107">Med nedan är en hög nivå arkitektur och IIS Smooth Streaming portera Kit är hello Smooth Streaming klienten implementering som tillhandahålls av Microsoft och innehåller alla hello core logik för uppspelning av Smooth Streaming-innehåll.</span><span class="sxs-lookup"><span data-stu-id="11c40-107">Included below is a high level architecture and IIS Smooth Streaming Porting Kit box is hello Smooth Streaming Client implementation provided by Microsoft and includes all hello core logic for playback of Smooth Streaming content.</span></span> <span data-ttu-id="11c40-108">Detta är portar av partner för en viss enhet eller plattformen genom att implementera rätt gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="11c40-108">This is then ported by partners for a specific device or platform by implementing appropriate interfaces.</span></span> 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a><span data-ttu-id="11c40-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="11c40-110">Description</span></span>
<span data-ttu-id="11c40-111">SSPK licensieras på villkor som ger utmärkt affärsvärde.</span><span class="sxs-lookup"><span data-stu-id="11c40-111">SSPK is licensed on terms that offer excellent business value.</span></span> <span data-ttu-id="11c40-112">SSPK licens ger hello branschen med:</span><span class="sxs-lookup"><span data-stu-id="11c40-112">SSPK license provides hello industry with:</span></span>

* <span data-ttu-id="11c40-113">Smooth Streaming portera Kit källa i C++</span><span class="sxs-lookup"><span data-stu-id="11c40-113">Smooth Streaming Porting Kit source in C++</span></span> 
  * <span data-ttu-id="11c40-114">implementerar Smooth Streaming klientfunktioner</span><span class="sxs-lookup"><span data-stu-id="11c40-114">implements Smooth Streaming Client functionality</span></span>
  * <span data-ttu-id="11c40-115">lägger till formatet parsning heuristik, buffring logik osv.</span><span class="sxs-lookup"><span data-stu-id="11c40-115">adds format parsing, heuristics, buffering logic, etc.</span></span>
* <span data-ttu-id="11c40-116">Programmet Player API: er</span><span class="sxs-lookup"><span data-stu-id="11c40-116">Player application APIs</span></span> 
  * <span data-ttu-id="11c40-117">programmeringsgränssnitt för interaktion med ett media player-program</span><span class="sxs-lookup"><span data-stu-id="11c40-117">programming interfaces for interaction with a media player application</span></span>
* <span data-ttu-id="11c40-118">Plattform Abstraction Layer (PAL) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="11c40-118">Platform Abstraction Layer (PAL) Interface</span></span> 
  * <span data-ttu-id="11c40-119">programmeringsgränssnitt för interaktion med hello operativsystem (trådar, sockets)</span><span class="sxs-lookup"><span data-stu-id="11c40-119">programming interfaces for interaction with hello operating system (threads, sockets)</span></span>
* <span data-ttu-id="11c40-120">Hardware Abstraction Layer (HAL) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="11c40-120">Hardware Abstraction Layer (HAL) Interface</span></span> 
  * <span data-ttu-id="11c40-121">programmeringsgränssnitt för interaktion med A / V-avkodare (avkoda, återgivning)</span><span class="sxs-lookup"><span data-stu-id="11c40-121">programming interfaces for interaction with hardware A/V decoders (decoding, rendering)</span></span>
* <span data-ttu-id="11c40-122">Digital Rights Management (DRM) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="11c40-122">Digital Rights Management (DRM) Interface</span></span> 
  * <span data-ttu-id="11c40-123">programmeringsgränssnitt för hantering av DRM via hello DRM Abstraction Layer (DAL)</span><span class="sxs-lookup"><span data-stu-id="11c40-123">programming interfaces for handling DRM through hello DRM Abstraction Layer (DAL)</span></span>
  * <span data-ttu-id="11c40-124">Microsoft PlayReady portera Kit levereras separat, men integrerar via det här gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="11c40-124">Microsoft PlayReady Porting Kit ships separately but integrates through this interface.</span></span> <span data-ttu-id="11c40-125">Mer information om Microsoft PlayReady Device licensiering, klickar du på [här](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span><span class="sxs-lookup"><span data-stu-id="11c40-125">For more details on Microsoft PlayReady Device licensing, click [here](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span></span>
* <span data-ttu-id="11c40-126">Exempel på implementering</span><span class="sxs-lookup"><span data-stu-id="11c40-126">Implementation samples</span></span> 
  * <span data-ttu-id="11c40-127">exempel på PAL implementering för Linux</span><span class="sxs-lookup"><span data-stu-id="11c40-127">sample PAL implementation for Linux</span></span>
  * <span data-ttu-id="11c40-128">exempel på HAL implementering för GStreamer</span><span class="sxs-lookup"><span data-stu-id="11c40-128">sample HAL implementation for GStreamer</span></span>

## <a name="licensing-options"></a><span data-ttu-id="11c40-129">Licensieringsalternativ</span><span class="sxs-lookup"><span data-stu-id="11c40-129">Licensing Options</span></span>
<span data-ttu-id="11c40-130">Microsoft Smooth Streaming klienten portera Kit görs tillgängliga toolicensees under två distinkta licensavtal: en för att utveckla Smooth Streaming klienten Interim produkter och en annan för att distribuera Smooth Streaming klienten slutliga produkter tooend användare.</span><span class="sxs-lookup"><span data-stu-id="11c40-130">Microsoft Smooth Streaming Client Porting Kit is made available toolicensees under two distinct license agreements: one for developing Smooth Streaming Client Interim Products and another for distributing Smooth Streaming Client Final Products tooend users.</span></span>

* <span data-ttu-id="11c40-131">För kretsuppsättning tillverkare, systemintegrerare eller oberoende leverantörer (ISV) som kräver en källa kod portera sats toodevelop Interim produkter, Microsoft Smooth Streaming klienten portera Kit **Interim produktlicensen** ska köras.</span><span class="sxs-lookup"><span data-stu-id="11c40-131">For chipset manufacturers, system integrators, or independent software vendors (ISVs) who require a source code porting kit toodevelop Interim Products, a Microsoft Smooth Streaming Client Porting Kit **Interim Product License** should be executed.</span></span>
* <span data-ttu-id="11c40-132">För enhetstillverkare eller ISV: er som behöver distributionsrättigheter för Smooth Streaming klienten slutliga produkter tooend användare hello Microsoft Smooth Streaming klienten portera Kit **slutliga produktlicensen** ska köras.</span><span class="sxs-lookup"><span data-stu-id="11c40-132">For device manufacturers or ISVs who require distribution rights for Smooth Streaming Client Final Products tooend users, hello Microsoft Smooth Streaming Client Porting Kit **Final Product License** should be executed.</span></span>

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a><span data-ttu-id="11c40-133">Microsoft Smooth Streaming klienten portera Kit Interim produktlicens</span><span class="sxs-lookup"><span data-stu-id="11c40-133">Microsoft Smooth Streaming Client Porting Kit Interim Product License</span></span>
<span data-ttu-id="11c40-134">Under denna licens, Microsoft erbjuder ett Smooth Streaming klienten portera Kit och hello nödvändiga immateriella rättigheter toodevelop och distribuera Smooth Streaming klienten Interim produkter tooother Smooth Streaming klienten portera Kit enheten licenstagare som distribuera Smooth Streaming klienten slutliga produkter.</span><span class="sxs-lookup"><span data-stu-id="11c40-134">Under this license, Microsoft offers a Smooth Streaming Client Porting Kit and hello necessary intellectual property rights toodevelop and distribute Smooth Streaming Client Interim Products tooother Smooth Streaming Client Porting Kit device licensees that distribute Smooth Streaming Client Final Products.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="11c40-135">Avgift struktur</span><span class="sxs-lookup"><span data-stu-id="11c40-135">Fee structure</span></span>
<span data-ttu-id="11c40-136">50 000 USA enstaka licens avgift ger åtkomst toohello Smooth Streaming klienten portera Kit.</span><span class="sxs-lookup"><span data-stu-id="11c40-136">A U.S. $50,000 one-time license fee provides access toohello Smooth Streaming Client Porting Kit.</span></span> 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a><span data-ttu-id="11c40-137">Microsoft Smooth Streaming klienten portera Kit Final produktlicens</span><span class="sxs-lookup"><span data-stu-id="11c40-137">Microsoft Smooth Streaming Client Porting Kit Final Product License</span></span>
<span data-ttu-id="11c40-138">Under denna licens erbjuder Microsoft all nödvändig immateriella rättigheter tooreceive Smooth Streaming klienten Interim produkter från andra Smooth Streaming klienten portera Kit licenstagare och toodistribute företagsanpassad Smooth Streaming klienten slutliga Produkter tooend användare.</span><span class="sxs-lookup"><span data-stu-id="11c40-138">Under this license, Microsoft offers all necessary intellectual property rights tooreceive Smooth Streaming Client Interim Products from other Smooth Streaming Client Porting Kit licensees and toodistribute company-branded Smooth Streaming Client Final Products tooend users.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="11c40-139">Avgift struktur</span><span class="sxs-lookup"><span data-stu-id="11c40-139">Fee structure</span></span>
<span data-ttu-id="11c40-140">hello Smooth Streaming klienten färdiga produkten erbjuds under en royalty modell som under:</span><span class="sxs-lookup"><span data-stu-id="11c40-140">hello Smooth Streaming Client Final Product is offered under a royalty model as under:</span></span>

* <span data-ttu-id="11c40-141">$0.10 per enhet implementering levererade</span><span class="sxs-lookup"><span data-stu-id="11c40-141">$0.10 per device implementation shipped</span></span>
* <span data-ttu-id="11c40-142">hello royalty är begränsad till 50 000 varje år</span><span class="sxs-lookup"><span data-stu-id="11c40-142">hello royalty is capped at $50,000 each year</span></span>
* <span data-ttu-id="11c40-143">Ingen royalty för första 10 000 enheter implementeringar varje år</span><span class="sxs-lookup"><span data-stu-id="11c40-143">No royalty for first 10,000 device implementations each year</span></span> 

## <a name="licensing-procedure-and-sspk-access"></a><span data-ttu-id="11c40-144">Licensiering proceduren och SSPK åtkomst</span><span class="sxs-lookup"><span data-stu-id="11c40-144">Licensing Procedure and SSPK access</span></span>
<span data-ttu-id="11c40-145">Kontakta e- [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) licensiering för alla frågor.</span><span class="sxs-lookup"><span data-stu-id="11c40-145">Please email [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) for all licensing queries.</span></span>

<span data-ttu-id="11c40-146">Hej [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) är tillgänglig tooregistered Interim licenstagare.</span><span class="sxs-lookup"><span data-stu-id="11c40-146">hello [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is accessible tooregistered Interim licensees.</span></span>

<span data-ttu-id="11c40-147">Interimistisk och slutlig SSPK licenstagare kan skicka tekniska frågor för[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="11c40-147">Interim and Final SSPK licensees can submit technical questions too[smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span></span>

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a><span data-ttu-id="11c40-148">Microsoft Smooth Streaming klienten tillfälliga produkten avtal licenstagare</span><span class="sxs-lookup"><span data-stu-id="11c40-148">Microsoft Smooth Streaming Client Interim Product Agreement Licensees</span></span>
* <span data-ttu-id="11c40-149">Adroit Business-lösningar, Inc</span><span class="sxs-lookup"><span data-stu-id="11c40-149">Adroit Business Solutions, Inc</span></span>
* <span data-ttu-id="11c40-150">Avancerade Digital Broadcast SA</span><span class="sxs-lookup"><span data-stu-id="11c40-150">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="11c40-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="11c40-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="11c40-152">Albis Technologies Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-152">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="11c40-153">Alticast Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-153">Alticast Corporation</span></span>
* <span data-ttu-id="11c40-154">Amazon Digital tjänster, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-154">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="11c40-155">Arion Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-155">Arion Technology, Inc.</span></span>
* <span data-ttu-id="11c40-156">AVC Multimedia programvara Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-156">AVC Multimedia Software Co., Ltd.</span></span>
* <span data-ttu-id="11c40-157">Cavium, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-157">Cavium, Inc.</span></span>
* <span data-ttu-id="11c40-158">EchoStar köp Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-158">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="11c40-159">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-159">Enseo, Inc.</span></span>
* <span data-ttu-id="11c40-160">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="11c40-160">Fluendo S.A.</span></span>
* <span data-ttu-id="11c40-161">HANDAN BroadInfoCom Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-161">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="11c40-162">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="11c40-162">Infomir GMBH</span></span>
* <span data-ttu-id="11c40-163">Irdeto USA Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-163">Irdeto USA Inc.</span></span>
* <span data-ttu-id="11c40-164">iWEDIA S.A.</span><span class="sxs-lookup"><span data-stu-id="11c40-164">iWEDIA S.A.</span></span> 
* <span data-ttu-id="11c40-165">Liberty globala tjänster BV</span><span class="sxs-lookup"><span data-stu-id="11c40-165">Liberty Global Services BV</span></span>
* <span data-ttu-id="11c40-166">MediaTek Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-166">MediaTek Inc.</span></span>
* <span data-ttu-id="11c40-167">MStar Co, Ltd</span><span class="sxs-lookup"><span data-stu-id="11c40-167">MStar Co, Ltd</span></span>
* <span data-ttu-id="11c40-168">NINTENDO Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-168">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="11c40-169">OpenTV, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-169">OpenTV, Inc.</span></span>
* <span data-ttu-id="11c40-170">T.ex Digital begränsad</span><span class="sxs-lookup"><span data-stu-id="11c40-170">Saffron Digital Limited</span></span>
* <span data-ttu-id="11c40-171">Av Changhong elektriska Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="11c40-171">Sichuan Changhong Electric Co., Ltd</span></span>
* <span data-ttu-id="11c40-172">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="11c40-172">SoftAtHome</span></span>
* <span data-ttu-id="11c40-173">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-173">Sony Corporation</span></span>
* <span data-ttu-id="11c40-174">Tatung Technology Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-174">Tatung Technology Inc.</span></span>
* <span data-ttu-id="11c40-175">TCL Technoly Electronics (Huizhou) Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span></span>
* <span data-ttu-id="11c40-176">TOP Victory investeringar, Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-176">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="11c40-177">Vestel Elektronik Sanayi para Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="11c40-177">Vestel Elektronik Sanayi ve Ticaret A.S.</span></span>
* <span data-ttu-id="11c40-178">VisualOn, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-178">VisualOn, Inc.</span></span>
* <span data-ttu-id="11c40-179">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-179">ZTE Corporation</span></span>

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a><span data-ttu-id="11c40-180">Microsoft Smooth Streaming klienten färdiga produkten avtal licenstagare</span><span class="sxs-lookup"><span data-stu-id="11c40-180">Microsoft Smooth Streaming Client Final Product Agreement Licensees</span></span>
* <span data-ttu-id="11c40-181">Avancerade Digital Broadcast SA</span><span class="sxs-lookup"><span data-stu-id="11c40-181">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="11c40-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="11c40-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="11c40-183">Albis Technologies Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-183">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="11c40-184">Amazon Digital tjänster, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-184">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="11c40-185">AmTRAN teknik Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-185">AmTRAN Technology Co., Ltd.</span></span>
* <span data-ttu-id="11c40-186">Arcadyan Technology Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-186">Arcadyan Technology Corporation</span></span>
* <span data-ttu-id="11c40-187">Arion Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-187">Arion Technology, Inc.</span></span>
* <span data-ttu-id="11c40-188">ATMACA ELEKTRONİK SAN.</span><span class="sxs-lookup"><span data-stu-id="11c40-188">ATMACA ELEKTRONİK SAN.</span></span> <span data-ttu-id="11c40-189">PARA TİC.</span><span class="sxs-lookup"><span data-stu-id="11c40-189">VE TİC.</span></span> <span data-ttu-id="11c40-190">A.Ş</span><span class="sxs-lookup"><span data-stu-id="11c40-190">A.Ş</span></span>
* <span data-ttu-id="11c40-191">Brittiska Sky Broadcasting begränsad</span><span class="sxs-lookup"><span data-stu-id="11c40-191">British Sky Broadcasting Limited</span></span>
* <span data-ttu-id="11c40-192">CastPal Technology Inc., Shenzhen</span><span class="sxs-lookup"><span data-stu-id="11c40-192">CastPal Technology Inc., Shenzhen</span></span>
* <span data-ttu-id="11c40-193">Compal Electronic, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-193">Compal Electronics, Inc.</span></span>
* <span data-ttu-id="11c40-194">Dongguan Digital AV teknik Corp., Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-194">Dongguan Digital AV Technology Corp., Ltd.</span></span>
* <span data-ttu-id="11c40-195">EchoStar köp Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-195">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="11c40-196">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-196">Enseo, Inc.</span></span>
* <span data-ttu-id="11c40-197">Filmflex filmer begränsad</span><span class="sxs-lookup"><span data-stu-id="11c40-197">Filmflex Movies Limited</span></span>
* <span data-ttu-id="11c40-198">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="11c40-198">Fluendo S.A.</span></span>
* <span data-ttu-id="11c40-199">Gibson innovationer begränsad</span><span class="sxs-lookup"><span data-stu-id="11c40-199">Gibson Innovations Limited</span></span>
* <span data-ttu-id="11c40-200">Haier Information Applicantion S.R.L</span><span class="sxs-lookup"><span data-stu-id="11c40-200">Haier Information Applicantion S.R.L</span></span>
* <span data-ttu-id="11c40-201">HANDAN BroadInfoCom Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-201">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="11c40-202">Hisense internationella Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-202">Hisense International Co., Ltd.</span></span> 
* <span data-ttu-id="11c40-203">Homecast Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="11c40-203">Homecast Co.,Ltd</span></span>
* <span data-ttu-id="11c40-204">Hon Hai Precision branschen Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-204">Hon Hai Precision Industry Co., Ltd.</span></span>
* <span data-ttu-id="11c40-205">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="11c40-205">Infomir GMBH</span></span>
* <span data-ttu-id="11c40-206">Kaonmedia Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-206">Kaonmedia Co., Ltd.</span></span>
* <span data-ttu-id="11c40-207">KDDI Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-207">KDDI Corporation</span></span>
* <span data-ttu-id="11c40-208">NINTENDO Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-208">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="11c40-209">Orange SA</span><span class="sxs-lookup"><span data-stu-id="11c40-209">Orange SA</span></span>
* <span data-ttu-id="11c40-210">T.ex Digital begränsad</span><span class="sxs-lookup"><span data-stu-id="11c40-210">Saffron Digital Limited</span></span>
* <span data-ttu-id="11c40-211">Sagemcom bredband SAS</span><span class="sxs-lookup"><span data-stu-id="11c40-211">Sagemcom Broadband SAS</span></span>
* <span data-ttu-id="11c40-212">Shenzhen Coship Electronics CO., LTD</span><span class="sxs-lookup"><span data-stu-id="11c40-212">Shenzhen Coship Electronics CO., LTD</span></span>
* <span data-ttu-id="11c40-213">Shenzhen Jiuzhou elektriska Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="11c40-213">Shenzhen Jiuzhou Electric Co.,Ltd</span></span>
* <span data-ttu-id="11c40-214">Shenzhen Skyworth Digital teknik Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="11c40-214">Shenzhen Skyworth Digital Technology Co., Ltd</span></span>
* <span data-ttu-id="11c40-215">Av Changhong elektriska Companys</span><span class="sxs-lookup"><span data-stu-id="11c40-215">Sichuan Changhong Electric Co., Ltd.</span></span>
* <span data-ttu-id="11c40-216">Skardin industriell Corp.</span><span class="sxs-lookup"><span data-stu-id="11c40-216">Skardin Industrial Corp.</span></span>
* <span data-ttu-id="11c40-217">Sky Deutschland Fernsehen GmbH & Co. KG</span><span class="sxs-lookup"><span data-stu-id="11c40-217">Sky Deutschland Fernsehen GmbH & Co. KG</span></span>
* <span data-ttu-id="11c40-218">SmarDTV S.A.</span><span class="sxs-lookup"><span data-stu-id="11c40-218">SmarDTV S.A.</span></span>
* <span data-ttu-id="11c40-219">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="11c40-219">SoftAtHome</span></span>
* <span data-ttu-id="11c40-220">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-220">Sony Corporation</span></span>
* <span data-ttu-id="11c40-221">Begränsad TCL utomeuropeiska marknadsföring (Offshore Macao kommersiell)</span><span class="sxs-lookup"><span data-stu-id="11c40-221">TCL Overseas Marketing (Macao Commercial Offshore) Limited</span></span>
* <span data-ttu-id="11c40-222">SAS-Technicolor leverans tekniker</span><span class="sxs-lookup"><span data-stu-id="11c40-222">Technicolor Delivery Technologies, SAS</span></span>
* <span data-ttu-id="11c40-223">Tongfang globala Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-223">Tongfang Global Ltd.</span></span>
* <span data-ttu-id="11c40-224">TOP Victory investeringar, Ltd.</span><span class="sxs-lookup"><span data-stu-id="11c40-224">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="11c40-225">Toshiba Lifestyle produkter och tjänster Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-225">Toshiba Lifestyle Products & Services Corporation</span></span>
* <span data-ttu-id="11c40-226">Universal Media Corporation /Slovakia/ s.r.o.</span><span class="sxs-lookup"><span data-stu-id="11c40-226">Universal Media Corporation /Slovakia/ s.r.o.</span></span>
* <span data-ttu-id="11c40-227">VIZIO, Inc.</span><span class="sxs-lookup"><span data-stu-id="11c40-227">VIZIO, Inc.</span></span>
* <span data-ttu-id="11c40-228">Wistron Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-228">Wistron Corporation</span></span>
* <span data-ttu-id="11c40-229">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="11c40-229">ZTE Corporation</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="11c40-230">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="11c40-230">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="11c40-231">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="11c40-231">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

