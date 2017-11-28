---
title: "Licensiering Microsoft® Smooth Streaming klienten portera Kit"
description: "Mer information om hur för Microsoft® Smooth Streaming klienten portera Kit."
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
ms.openlocfilehash: b5a36ac6771bef220afe29446cd56c1b65a498d9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a><span data-ttu-id="066ef-103">Licensiering Microsoft® Smooth Streaming klienten portera Kit</span><span class="sxs-lookup"><span data-stu-id="066ef-103">Licensing Microsoft® Smooth Streaming Client Porting Kit</span></span>
## <a name="overview"></a><span data-ttu-id="066ef-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="066ef-104">Overview</span></span>
<span data-ttu-id="066ef-105">Microsoft Smooth Streaming klienten portera Kit (**SSPK** för kort) är en Smooth Streaming-klient-implementering som är optimerad för att hjälpa inbäddade enhetstillverkare, kabel och mobila operatorer, innehåll leverantörer, luren tillverkare oberoende programvaruleverantörer (ISV) och lösningsleverantörer att skapa produkter och tjänster för direktuppspelning av anpassningsbar strömning innehåll i Smooth Streaming-format.</span><span class="sxs-lookup"><span data-stu-id="066ef-105">Microsoft Smooth Streaming Client Porting Kit (**SSPK** for short) is a Smooth Streaming client implementation that is optimized to help embedded device manufacturers, cable and mobile operators, content service providers, handset manufacturers, independent software vendors (ISVs), and solution providers to create products and services for streaming adaptive streaming content in Smooth Streaming format.</span></span> <span data-ttu-id="066ef-106">SSPK är en enhet och plattform oberoende implementering av Smooth Streaming-klient som kan överföras av licenstagaren till enheten och plattform.</span><span class="sxs-lookup"><span data-stu-id="066ef-106">SSPK is a device and platform independent implementation of Smooth Streaming client that can be ported by the licensee to any device and platform.</span></span> 

<span data-ttu-id="066ef-107">Med nedan är en hög nivå arkitektur och IIS Smooth Streaming portera Kit är klienten för Smooth Streaming-implementering som tillhandahålls av Microsoft och innehåller alla kärnlogik för uppspelning av Smooth Streaming-innehåll.</span><span class="sxs-lookup"><span data-stu-id="066ef-107">Included below is a high level architecture and IIS Smooth Streaming Porting Kit box is the Smooth Streaming Client implementation provided by Microsoft and includes all the core logic for playback of Smooth Streaming content.</span></span> <span data-ttu-id="066ef-108">Detta är portar av partner för en viss enhet eller plattformen genom att implementera rätt gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="066ef-108">This is then ported by partners for a specific device or platform by implementing appropriate interfaces.</span></span> 

![SSPK](./media/media-services-sspk/sspk-arch.png)

## <a name="description"></a><span data-ttu-id="066ef-110">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="066ef-110">Description</span></span>
<span data-ttu-id="066ef-111">SSPK licensieras på villkor som ger utmärkt affärsvärde.</span><span class="sxs-lookup"><span data-stu-id="066ef-111">SSPK is licensed on terms that offer excellent business value.</span></span> <span data-ttu-id="066ef-112">SSPK licens får branschen:</span><span class="sxs-lookup"><span data-stu-id="066ef-112">SSPK license provides the industry with:</span></span>

* <span data-ttu-id="066ef-113">Smooth Streaming portera Kit källa i C++</span><span class="sxs-lookup"><span data-stu-id="066ef-113">Smooth Streaming Porting Kit source in C++</span></span> 
  * <span data-ttu-id="066ef-114">implementerar Smooth Streaming klientfunktioner</span><span class="sxs-lookup"><span data-stu-id="066ef-114">implements Smooth Streaming Client functionality</span></span>
  * <span data-ttu-id="066ef-115">lägger till formatet parsning heuristik, buffring logik osv.</span><span class="sxs-lookup"><span data-stu-id="066ef-115">adds format parsing, heuristics, buffering logic, etc.</span></span>
* <span data-ttu-id="066ef-116">Programmet Player API: er</span><span class="sxs-lookup"><span data-stu-id="066ef-116">Player application APIs</span></span> 
  * <span data-ttu-id="066ef-117">programmeringsgränssnitt för interaktion med ett media player-program</span><span class="sxs-lookup"><span data-stu-id="066ef-117">programming interfaces for interaction with a media player application</span></span>
* <span data-ttu-id="066ef-118">Plattform Abstraction Layer (PAL) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="066ef-118">Platform Abstraction Layer (PAL) Interface</span></span> 
  * <span data-ttu-id="066ef-119">programmeringsgränssnitt för interaktion med operativsystemet (trådar, sockets)</span><span class="sxs-lookup"><span data-stu-id="066ef-119">programming interfaces for interaction with the operating system (threads, sockets)</span></span>
* <span data-ttu-id="066ef-120">Hardware Abstraction Layer (HAL) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="066ef-120">Hardware Abstraction Layer (HAL) Interface</span></span> 
  * <span data-ttu-id="066ef-121">programmeringsgränssnitt för interaktion med A / V-avkodare (avkoda, återgivning)</span><span class="sxs-lookup"><span data-stu-id="066ef-121">programming interfaces for interaction with hardware A/V decoders (decoding, rendering)</span></span>
* <span data-ttu-id="066ef-122">Digital Rights Management (DRM) gränssnitt</span><span class="sxs-lookup"><span data-stu-id="066ef-122">Digital Rights Management (DRM) Interface</span></span> 
  * <span data-ttu-id="066ef-123">programmeringsgränssnitt för hantering av DRM via DRM Abstraction Layer (DAL)</span><span class="sxs-lookup"><span data-stu-id="066ef-123">programming interfaces for handling DRM through the DRM Abstraction Layer (DAL)</span></span>
  * <span data-ttu-id="066ef-124">Microsoft PlayReady portera Kit levereras separat, men integrerar via det här gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="066ef-124">Microsoft PlayReady Porting Kit ships separately but integrates through this interface.</span></span> <span data-ttu-id="066ef-125">Mer information om Microsoft PlayReady Device licensiering, klickar du på [här](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span><span class="sxs-lookup"><span data-stu-id="066ef-125">For more details on Microsoft PlayReady Device licensing, click [here](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).</span></span>
* <span data-ttu-id="066ef-126">Exempel på implementering</span><span class="sxs-lookup"><span data-stu-id="066ef-126">Implementation samples</span></span> 
  * <span data-ttu-id="066ef-127">exempel på PAL implementering för Linux</span><span class="sxs-lookup"><span data-stu-id="066ef-127">sample PAL implementation for Linux</span></span>
  * <span data-ttu-id="066ef-128">exempel på HAL implementering för GStreamer</span><span class="sxs-lookup"><span data-stu-id="066ef-128">sample HAL implementation for GStreamer</span></span>

## <a name="licensing-options"></a><span data-ttu-id="066ef-129">Licensieringsalternativ</span><span class="sxs-lookup"><span data-stu-id="066ef-129">Licensing Options</span></span>
<span data-ttu-id="066ef-130">Microsoft Smooth Streaming klienten portera Kit görs tillgängligt för licenstagare enligt två distinkta licensavtal: en för att utveckla Smooth Streaming klienten Interim produkter och en annan för att distribuera Smooth Streaming klienten slutliga produkter till slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="066ef-130">Microsoft Smooth Streaming Client Porting Kit is made available to licensees under two distinct license agreements: one for developing Smooth Streaming Client Interim Products and another for distributing Smooth Streaming Client Final Products to end users.</span></span>

* <span data-ttu-id="066ef-131">För kretsuppsättning tillverkare, systemintegrerare eller oberoende leverantörer (ISV) som kräver en källa code portera kit för att utveckla Interim produkter, Microsoft Smooth Streaming klienten portera Kit **Interim produktlicensen** ska köras.</span><span class="sxs-lookup"><span data-stu-id="066ef-131">For chipset manufacturers, system integrators, or independent software vendors (ISVs) who require a source code porting kit to develop Interim Products, a Microsoft Smooth Streaming Client Porting Kit **Interim Product License** should be executed.</span></span>
* <span data-ttu-id="066ef-132">För enhetstillverkare eller ISV: er som behöver distributionsrättigheter för Smooth Streaming klienten slutliga produkter till slutanvändare, Microsoft Smooth Streaming klienten portera Kit **slutliga produktlicensen** ska köras.</span><span class="sxs-lookup"><span data-stu-id="066ef-132">For device manufacturers or ISVs who require distribution rights for Smooth Streaming Client Final Products to end users, the Microsoft Smooth Streaming Client Porting Kit **Final Product License** should be executed.</span></span>

### <a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a><span data-ttu-id="066ef-133">Microsoft Smooth Streaming klienten portera Kit Interim produktlicens</span><span class="sxs-lookup"><span data-stu-id="066ef-133">Microsoft Smooth Streaming Client Porting Kit Interim Product License</span></span>
<span data-ttu-id="066ef-134">Under denna licens, erbjuder Microsoft en Smooth Streaming klienten portera Kit och nödvändiga immateriella rättigheter att utveckla och distribuera Smooth Streaming klienten Interim produkter till andra Smooth Streaming klienten portera Kit enheten licenstagare som distribuera Smooth Streaming klienten slutliga produkter.</span><span class="sxs-lookup"><span data-stu-id="066ef-134">Under this license, Microsoft offers a Smooth Streaming Client Porting Kit and the necessary intellectual property rights to develop and distribute Smooth Streaming Client Interim Products to other Smooth Streaming Client Porting Kit device licensees that distribute Smooth Streaming Client Final Products.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="066ef-135">Avgift struktur</span><span class="sxs-lookup"><span data-stu-id="066ef-135">Fee structure</span></span>
<span data-ttu-id="066ef-136">USA 50 000 enstaka licens avgift ger åtkomst till den Smooth Streaming klienten portera Kit.</span><span class="sxs-lookup"><span data-stu-id="066ef-136">A U.S. $50,000 one-time license fee provides access to the Smooth Streaming Client Porting Kit.</span></span> 

### <a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a><span data-ttu-id="066ef-137">Microsoft Smooth Streaming klienten portera Kit Final produktlicens</span><span class="sxs-lookup"><span data-stu-id="066ef-137">Microsoft Smooth Streaming Client Porting Kit Final Product License</span></span>
<span data-ttu-id="066ef-138">Under denna licens erbjuder Microsoft alla nödvändiga immateriella rättigheter får Smooth Streaming klienten Interim produkter från andra Smooth Streaming klienten portera Kit licenstagare och distribuera företagsanpassad Smooth Streaming klienten slutliga Produkter för slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="066ef-138">Under this license, Microsoft offers all necessary intellectual property rights to receive Smooth Streaming Client Interim Products from other Smooth Streaming Client Porting Kit licensees and to distribute company-branded Smooth Streaming Client Final Products to end users.</span></span>

#### <a name="fee-structure"></a><span data-ttu-id="066ef-139">Avgift struktur</span><span class="sxs-lookup"><span data-stu-id="066ef-139">Fee structure</span></span>
<span data-ttu-id="066ef-140">Smooth Streaming klienten slutliga produkten erbjuds under en royalty modell som under:</span><span class="sxs-lookup"><span data-stu-id="066ef-140">The Smooth Streaming Client Final Product is offered under a royalty model as under:</span></span>

* <span data-ttu-id="066ef-141">$0.10 per enhet implementering levererade</span><span class="sxs-lookup"><span data-stu-id="066ef-141">$0.10 per device implementation shipped</span></span>
* <span data-ttu-id="066ef-142">Royalty är begränsad till 50 000 varje år</span><span class="sxs-lookup"><span data-stu-id="066ef-142">The royalty is capped at $50,000 each year</span></span>
* <span data-ttu-id="066ef-143">Ingen royalty för första 10 000 enheter implementeringar varje år</span><span class="sxs-lookup"><span data-stu-id="066ef-143">No royalty for first 10,000 device implementations each year</span></span> 

## <a name="licensing-procedure-and-sspk-access"></a><span data-ttu-id="066ef-144">Licensiering proceduren och SSPK åtkomst</span><span class="sxs-lookup"><span data-stu-id="066ef-144">Licensing Procedure and SSPK access</span></span>
<span data-ttu-id="066ef-145">Kontakta e- [ sspkinfo@microsoft.com ](mailto:sspkinfo@microsoft.com) licensiering för alla frågor.</span><span class="sxs-lookup"><span data-stu-id="066ef-145">Please email [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) for all licensing queries.</span></span>

<span data-ttu-id="066ef-146">Den [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) är tillgänglig för den registrerade Interim licenstagare.</span><span class="sxs-lookup"><span data-stu-id="066ef-146">The [SSPK Distribution portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) is accessible to registered Interim licensees.</span></span>

<span data-ttu-id="066ef-147">Interimistisk och slutlig SSPK licenstagare kan skicka tekniska frågor till [ smoothpk@microsoft.com ](mailto:smoothpk@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="066ef-147">Interim and Final SSPK licensees can submit technical questions to [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).</span></span>

## <a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a><span data-ttu-id="066ef-148">Microsoft Smooth Streaming klienten tillfälliga produkten avtal licenstagare</span><span class="sxs-lookup"><span data-stu-id="066ef-148">Microsoft Smooth Streaming Client Interim Product Agreement Licensees</span></span>
* <span data-ttu-id="066ef-149">Adroit Business-lösningar, Inc</span><span class="sxs-lookup"><span data-stu-id="066ef-149">Adroit Business Solutions, Inc</span></span>
* <span data-ttu-id="066ef-150">Avancerade Digital Broadcast SA</span><span class="sxs-lookup"><span data-stu-id="066ef-150">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="066ef-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="066ef-151">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="066ef-152">Albis Technologies Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-152">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="066ef-153">Alticast Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-153">Alticast Corporation</span></span>
* <span data-ttu-id="066ef-154">Amazon Digital tjänster, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-154">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="066ef-155">Arion Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-155">Arion Technology, Inc.</span></span>
* <span data-ttu-id="066ef-156">AVC Multimedia programvara Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-156">AVC Multimedia Software Co., Ltd.</span></span>
* <span data-ttu-id="066ef-157">Cavium, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-157">Cavium, Inc.</span></span>
* <span data-ttu-id="066ef-158">EchoStar köp Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-158">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="066ef-159">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-159">Enseo, Inc.</span></span>
* <span data-ttu-id="066ef-160">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="066ef-160">Fluendo S.A.</span></span>
* <span data-ttu-id="066ef-161">HANDAN BroadInfoCom Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-161">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="066ef-162">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="066ef-162">Infomir GMBH</span></span>
* <span data-ttu-id="066ef-163">Irdeto USA Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-163">Irdeto USA Inc.</span></span>
* <span data-ttu-id="066ef-164">iWEDIA S.A.</span><span class="sxs-lookup"><span data-stu-id="066ef-164">iWEDIA S.A.</span></span> 
* <span data-ttu-id="066ef-165">Liberty globala tjänster BV</span><span class="sxs-lookup"><span data-stu-id="066ef-165">Liberty Global Services BV</span></span>
* <span data-ttu-id="066ef-166">MediaTek Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-166">MediaTek Inc.</span></span>
* <span data-ttu-id="066ef-167">MStar Co, Ltd</span><span class="sxs-lookup"><span data-stu-id="066ef-167">MStar Co, Ltd</span></span>
* <span data-ttu-id="066ef-168">NINTENDO Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-168">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="066ef-169">OpenTV, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-169">OpenTV, Inc.</span></span>
* <span data-ttu-id="066ef-170">T.ex Digital begränsad</span><span class="sxs-lookup"><span data-stu-id="066ef-170">Saffron Digital Limited</span></span>
* <span data-ttu-id="066ef-171">Av Changhong elektriska Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="066ef-171">Sichuan Changhong Electric Co., Ltd</span></span>
* <span data-ttu-id="066ef-172">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="066ef-172">SoftAtHome</span></span>
* <span data-ttu-id="066ef-173">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-173">Sony Corporation</span></span>
* <span data-ttu-id="066ef-174">Tatung Technology Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-174">Tatung Technology Inc.</span></span>
* <span data-ttu-id="066ef-175">TCL Technoly Electronics (Huizhou) Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-175">TCL Technoly Electronics (Huizhou) Co., Ltd.</span></span>
* <span data-ttu-id="066ef-176">TOP Victory investeringar, Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-176">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="066ef-177">Vestel Elektronik Sanayi para Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="066ef-177">Vestel Elektronik Sanayi ve Ticaret A.S.</span></span>
* <span data-ttu-id="066ef-178">VisualOn, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-178">VisualOn, Inc.</span></span>
* <span data-ttu-id="066ef-179">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-179">ZTE Corporation</span></span>

## <a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a><span data-ttu-id="066ef-180">Microsoft Smooth Streaming klienten färdiga produkten avtal licenstagare</span><span class="sxs-lookup"><span data-stu-id="066ef-180">Microsoft Smooth Streaming Client Final Product Agreement Licensees</span></span>
* <span data-ttu-id="066ef-181">Avancerade Digital Broadcast SA</span><span class="sxs-lookup"><span data-stu-id="066ef-181">Advanced Digital Broadcast SA</span></span>
* <span data-ttu-id="066ef-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret a. s.</span><span class="sxs-lookup"><span data-stu-id="066ef-182">AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.</span></span>
* <span data-ttu-id="066ef-183">Albis Technologies Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-183">Albis Technologies Ltd.</span></span>
* <span data-ttu-id="066ef-184">Amazon Digital tjänster, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-184">Amazon Digital Services, Inc.</span></span>
* <span data-ttu-id="066ef-185">AmTRAN teknik Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-185">AmTRAN Technology Co., Ltd.</span></span>
* <span data-ttu-id="066ef-186">Arcadyan Technology Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-186">Arcadyan Technology Corporation</span></span>
* <span data-ttu-id="066ef-187">Arion Technology, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-187">Arion Technology, Inc.</span></span>
* <span data-ttu-id="066ef-188">ATMACA ELEKTRONİK SAN.</span><span class="sxs-lookup"><span data-stu-id="066ef-188">ATMACA ELEKTRONİK SAN.</span></span> <span data-ttu-id="066ef-189">PARA TİC.</span><span class="sxs-lookup"><span data-stu-id="066ef-189">VE TİC.</span></span> <span data-ttu-id="066ef-190">A.Ş</span><span class="sxs-lookup"><span data-stu-id="066ef-190">A.Ş</span></span>
* <span data-ttu-id="066ef-191">Brittiska Sky Broadcasting begränsad</span><span class="sxs-lookup"><span data-stu-id="066ef-191">British Sky Broadcasting Limited</span></span>
* <span data-ttu-id="066ef-192">CastPal Technology Inc., Shenzhen</span><span class="sxs-lookup"><span data-stu-id="066ef-192">CastPal Technology Inc., Shenzhen</span></span>
* <span data-ttu-id="066ef-193">Compal Electronic, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-193">Compal Electronics, Inc.</span></span>
* <span data-ttu-id="066ef-194">Dongguan Digital AV teknik Corp., Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-194">Dongguan Digital AV Technology Corp., Ltd.</span></span>
* <span data-ttu-id="066ef-195">EchoStar köp Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-195">EchoStar Purchasing Corporation</span></span>
* <span data-ttu-id="066ef-196">Enseo, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-196">Enseo, Inc.</span></span>
* <span data-ttu-id="066ef-197">Filmflex filmer begränsad</span><span class="sxs-lookup"><span data-stu-id="066ef-197">Filmflex Movies Limited</span></span>
* <span data-ttu-id="066ef-198">Fluendo S.A.</span><span class="sxs-lookup"><span data-stu-id="066ef-198">Fluendo S.A.</span></span>
* <span data-ttu-id="066ef-199">Gibson innovationer begränsad</span><span class="sxs-lookup"><span data-stu-id="066ef-199">Gibson Innovations Limited</span></span>
* <span data-ttu-id="066ef-200">Haier Information Applicantion S.R.L</span><span class="sxs-lookup"><span data-stu-id="066ef-200">Haier Information Applicantion S.R.L</span></span>
* <span data-ttu-id="066ef-201">HANDAN BroadInfoCom Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-201">HANDAN BroadInfoCom Co., Ltd.</span></span>
* <span data-ttu-id="066ef-202">Hisense internationella Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-202">Hisense International Co., Ltd.</span></span> 
* <span data-ttu-id="066ef-203">Homecast Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="066ef-203">Homecast Co.,Ltd</span></span>
* <span data-ttu-id="066ef-204">Hon Hai Precision branschen Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-204">Hon Hai Precision Industry Co., Ltd.</span></span>
* <span data-ttu-id="066ef-205">Infomir GMBH</span><span class="sxs-lookup"><span data-stu-id="066ef-205">Infomir GMBH</span></span>
* <span data-ttu-id="066ef-206">Kaonmedia Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-206">Kaonmedia Co., Ltd.</span></span>
* <span data-ttu-id="066ef-207">KDDI Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-207">KDDI Corporation</span></span>
* <span data-ttu-id="066ef-208">NINTENDO Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-208">Nintendo Co., Ltd.</span></span>
* <span data-ttu-id="066ef-209">Orange SA</span><span class="sxs-lookup"><span data-stu-id="066ef-209">Orange SA</span></span>
* <span data-ttu-id="066ef-210">T.ex Digital begränsad</span><span class="sxs-lookup"><span data-stu-id="066ef-210">Saffron Digital Limited</span></span>
* <span data-ttu-id="066ef-211">Sagemcom bredband SAS</span><span class="sxs-lookup"><span data-stu-id="066ef-211">Sagemcom Broadband SAS</span></span>
* <span data-ttu-id="066ef-212">Shenzhen Coship Electronics CO., LTD</span><span class="sxs-lookup"><span data-stu-id="066ef-212">Shenzhen Coship Electronics CO., LTD</span></span>
* <span data-ttu-id="066ef-213">Shenzhen Jiuzhou elektriska Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="066ef-213">Shenzhen Jiuzhou Electric Co.,Ltd</span></span>
* <span data-ttu-id="066ef-214">Shenzhen Skyworth Digital teknik Co., Ltd</span><span class="sxs-lookup"><span data-stu-id="066ef-214">Shenzhen Skyworth Digital Technology Co., Ltd</span></span>
* <span data-ttu-id="066ef-215">Av Changhong elektriska Companys</span><span class="sxs-lookup"><span data-stu-id="066ef-215">Sichuan Changhong Electric Co., Ltd.</span></span>
* <span data-ttu-id="066ef-216">Skardin industriell Corp.</span><span class="sxs-lookup"><span data-stu-id="066ef-216">Skardin Industrial Corp.</span></span>
* <span data-ttu-id="066ef-217">Sky Deutschland Fernsehen GmbH & Co. KG</span><span class="sxs-lookup"><span data-stu-id="066ef-217">Sky Deutschland Fernsehen GmbH & Co. KG</span></span>
* <span data-ttu-id="066ef-218">SmarDTV S.A.</span><span class="sxs-lookup"><span data-stu-id="066ef-218">SmarDTV S.A.</span></span>
* <span data-ttu-id="066ef-219">SoftAtHome</span><span class="sxs-lookup"><span data-stu-id="066ef-219">SoftAtHome</span></span>
* <span data-ttu-id="066ef-220">Sony Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-220">Sony Corporation</span></span>
* <span data-ttu-id="066ef-221">Begränsad TCL utomeuropeiska marknadsföring (Offshore Macao kommersiell)</span><span class="sxs-lookup"><span data-stu-id="066ef-221">TCL Overseas Marketing (Macao Commercial Offshore) Limited</span></span>
* <span data-ttu-id="066ef-222">SAS-Technicolor leverans tekniker</span><span class="sxs-lookup"><span data-stu-id="066ef-222">Technicolor Delivery Technologies, SAS</span></span>
* <span data-ttu-id="066ef-223">Tongfang globala Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-223">Tongfang Global Ltd.</span></span>
* <span data-ttu-id="066ef-224">TOP Victory investeringar, Ltd.</span><span class="sxs-lookup"><span data-stu-id="066ef-224">Top Victory Investments, Ltd.</span></span>
* <span data-ttu-id="066ef-225">Toshiba Lifestyle produkter och tjänster Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-225">Toshiba Lifestyle Products & Services Corporation</span></span>
* <span data-ttu-id="066ef-226">Universal Media Corporation /Slovakia/ s.r.o.</span><span class="sxs-lookup"><span data-stu-id="066ef-226">Universal Media Corporation /Slovakia/ s.r.o.</span></span>
* <span data-ttu-id="066ef-227">VIZIO, Inc.</span><span class="sxs-lookup"><span data-stu-id="066ef-227">VIZIO, Inc.</span></span>
* <span data-ttu-id="066ef-228">Wistron Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-228">Wistron Corporation</span></span>
* <span data-ttu-id="066ef-229">ZTE Corporation</span><span class="sxs-lookup"><span data-stu-id="066ef-229">ZTE Corporation</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="066ef-230">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="066ef-230">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="066ef-231">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="066ef-231">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

