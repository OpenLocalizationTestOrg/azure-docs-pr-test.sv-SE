---
title: aaaImprove prestanda genom att komprimera filerna i Azure CDN | Microsoft Docs
description: "Lär dig hur tooimprove filöverföring hastighet och ökar belastningen prestanda genom att komprimera filerna i Azure CDN."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: af1cddff-78d8-476b-a9d0-8c2164e4de5d
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9a1698b6c29233c2e2e6fb17cdd8e919ae8bfa1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="b096d-103">Förbättra prestanda genom att komprimera filerna i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="b096d-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="b096d-104">Komprimering är ett enkelt och effektivt metoden tooimprove filen överföringshastighet och öka belastningen prestanda genom att minska filstorleken innan den skickas från hello-servern.</span><span class="sxs-lookup"><span data-stu-id="b096d-104">Compression is a simple and effective method tooimprove file transfer speed and increase page load performance by reducing file size before it is sent from hello server.</span></span> <span data-ttu-id="b096d-105">Det minskar kostnader för bandbredd och ger en mer effektiv upplevelse för användarna.</span><span class="sxs-lookup"><span data-stu-id="b096d-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="b096d-106">Det finns två sätt tooenable komprimering:</span><span class="sxs-lookup"><span data-stu-id="b096d-106">There are two ways tooenable compression:</span></span>

* <span data-ttu-id="b096d-107">Du kan aktivera komprimering på ursprungsservern, i vilket fall hello CDN passerar genom hello komprimerade filer och leverera komprimerade filer tooclients som begär dem..</span><span class="sxs-lookup"><span data-stu-id="b096d-107">You can enable compression on your origin server, in which case hello CDN passes through hello compressed files and deliver compressed files tooclients that request them.</span></span>
* <span data-ttu-id="b096d-108">Du kan aktivera komprimering direkt på CDN edge servrar, där case hello CDN komprimerar hello filer och hantera dem tooend användare, även om de inte komprimeras i hello ursprungsservern.</span><span class="sxs-lookup"><span data-stu-id="b096d-108">You can enable compression directly on CDN edge servers, in which case hello CDN compresses hello files and serve them tooend users, even if they are not compressed by hello origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b096d-109">CDN konfigurationsändringar ta viss tid toopropagate via hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="b096d-109">CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="b096d-110">För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis har slutförts under en minut.</span><span class="sxs-lookup"><span data-stu-id="b096d-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="b096d-111">För <b>Azure CDN från Verizon</b> profiler, vanligtvis visas ändringarna gäller inom 90 minuter.</span><span class="sxs-lookup"><span data-stu-id="b096d-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="b096d-112">Om detta är hello första gången som du har konfigurerat komprimering för CDN-slutpunkt, bör du vänta 1 – 2 timmar toobe att hello komprimeringsinställningar har spridits toohello POP före felsökning</span><span class="sxs-lookup"><span data-stu-id="b096d-112">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="b096d-113">Aktivera komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="b096d-114">hello Standard och Premium CDN nivåer ange hello samma komprimering funktioner, men hello användargränssnittet skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="b096d-114">hello Standard and Premium CDN tiers provide hello same compression functionality, but hello user interface differs.</span></span>  <span data-ttu-id="b096d-115">Läs mer om hello skillnader mellan Standard och Premium CDN nivåer [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b096d-115">For more information about hello differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="b096d-116">Standard-nivå</span><span class="sxs-lookup"><span data-stu-id="b096d-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="b096d-117">Det här avsnittet gäller för**Azure CDN Standard från Verizon** och **Azure CDN Standard från Akamai** profiler.</span><span class="sxs-lookup"><span data-stu-id="b096d-117">This section applies too**Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="b096d-118">Klicka på hello CDN-slutpunkt som du vill toomanage från hello CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="b096d-118">From hello CDN profile page, click hello CDN endpoint you wish toomanage.</span></span>
   
    ![CDN-profilen sidan slutpunkter](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="b096d-120">hello CDN-slutpunkten sidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="b096d-120">hello CDN endpoint page opens.</span></span>
2. <span data-ttu-id="b096d-121">Klicka på hello **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="b096d-121">Click hello **Configure** button.</span></span>
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="b096d-123">hello CDN konfigurationssidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="b096d-123">hello CDN Configuration page opens.</span></span>
3. <span data-ttu-id="b096d-124">Aktivera **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="b096d-124">Turn on **Compression**.</span></span>
   
    ![CDN-komprimeringsalternativ](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="b096d-126">Använd standardtyperna hello eller ändra hello listan genom att ta bort eller lägga till filtyper.</span><span class="sxs-lookup"><span data-stu-id="b096d-126">Use hello default types, or modify hello list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="b096d-127">Även möjligt, inte rekommenderas tooapply komprimering toocompressed format, till exempel ZIP, MP3, MP4, JPG och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b096d-127">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="b096d-128">När du har gjort ändringarna klickar du på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b096d-128">After making your changes, click hello **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="b096d-129">Premiumnivå</span><span class="sxs-lookup"><span data-stu-id="b096d-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="b096d-130">Det här avsnittet gäller för**Azure CDN Premium från Verizon** profiler.</span><span class="sxs-lookup"><span data-stu-id="b096d-130">This section applies too**Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="b096d-131">Klicka på hello från hello CDN-profilen **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="b096d-131">From hello CDN profile page, click hello **Manage** button.</span></span>
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="b096d-133">hello CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="b096d-133">hello CDN management portal opens.</span></span>
2. <span data-ttu-id="b096d-134">Hovra över hello **HTTP stora** fliken och sedan hovra över hello **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="b096d-134">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="b096d-135">Klicka på **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="b096d-135">Click on **Compression**.</span></span>

    ![Komprimering filval](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="b096d-137">Komprimeringsalternativ visas.</span><span class="sxs-lookup"><span data-stu-id="b096d-137">Compression options are displayed.</span></span>
   
    ![Komprimering](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="b096d-139">Aktivera komprimering genom att klicka på hello **komprimering aktiverat** knappen.</span><span class="sxs-lookup"><span data-stu-id="b096d-139">Enable compression by clicking hello **Compression Enabled** radio button.</span></span>  <span data-ttu-id="b096d-140">Ange hello MIME-typer gärna toocompress som en kommaavgränsad lista (inga blanksteg) i hello **filtyper** textruta.</span><span class="sxs-lookup"><span data-stu-id="b096d-140">Enter hello MIME types you wish toocompress as a comma-delimited list (no spaces) in hello **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="b096d-141">Även möjligt, inte rekommenderas tooapply komprimering toocompressed format, till exempel ZIP, MP3, MP4, JPG och så vidare.</span><span class="sxs-lookup"><span data-stu-id="b096d-141">While possible, it is not recommended tooapply compression toocompressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="b096d-142">När du har gjort ändringarna klickar du på hello **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="b096d-142">After making your changes, click hello **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="b096d-143">Regler för komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-143">Compression rules</span></span>
<span data-ttu-id="b096d-144">Dessa tabeller beskrivs Azure CDN komprimering beteendet för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="b096d-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b096d-145">För **Azure CDN från Verizon** (Standard och Premium) endast filer komprimeras.</span><span class="sxs-lookup"><span data-stu-id="b096d-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="b096d-146">toobe berättigad för komprimering, en fil måste:</span><span class="sxs-lookup"><span data-stu-id="b096d-146">toobe eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="b096d-147">Är större än 128 byte.</span><span class="sxs-lookup"><span data-stu-id="b096d-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="b096d-148">Vara mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="b096d-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="b096d-149">För **Azure CDN från Akamai**, alla filer är tillgängliga för komprimering.</span><span class="sxs-lookup"><span data-stu-id="b096d-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="b096d-150">För alla Azure CDN-produkter, en fil måste vara en MIME-typ som har varit [konfigurerats för komprimering](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="b096d-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="b096d-151">**Azure CDN från Verizon** profiler (Standard och Premium) stöd **gzip** (GNU zip) **deflate**, **bzip2**, eller **br**(Brotli) kodning.</span><span class="sxs-lookup"><span data-stu-id="b096d-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="b096d-152">För Brotli kodning görs hello komprimering endast vid hello kant.</span><span class="sxs-lookup"><span data-stu-id="b096d-152">For Brotli encoding, hello compression is done only at hello edge.</span></span> <span data-ttu-id="b096d-153">hello klientwebbläsaren skicka begäran om hello Brotli kodning och hello komprimerade tillgångsinformation måste komprimerats på hello ursprung sida först.</span><span class="sxs-lookup"><span data-stu-id="b096d-153">hello client/browser must send hello request for Brotli encoding and hello compressed asset must have been compressed on hello origin side first.</span></span> 
>
><span data-ttu-id="b096d-154">**Azure CDN från Akamai** profiler stöder bara **gzip** kodning.</span><span class="sxs-lookup"><span data-stu-id="b096d-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="b096d-155">**Azure CDN från Akamai** slutpunkter begär alltid **gzip** kodade filer från hello ursprung, oavsett hello klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="b096d-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from hello origin, regardless of hello client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="b096d-156">Komprimering inaktiveras eller filen är inte tillgänglig för komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="b096d-157">Klienten begärde format (kopplingshuvud Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="b096d-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="b096d-158">Cachelagrade filformat</span><span class="sxs-lookup"><span data-stu-id="b096d-158">Cached file format</span></span> | <span data-ttu-id="b096d-159">CDN-svar toohello klienten</span><span class="sxs-lookup"><span data-stu-id="b096d-159">CDN response toohello client</span></span> | <span data-ttu-id="b096d-160">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b096d-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b096d-161">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-161">Compressed</span></span> |<span data-ttu-id="b096d-162">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-162">Compressed</span></span> |<span data-ttu-id="b096d-163">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-163">Compressed</span></span> | |
| <span data-ttu-id="b096d-164">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-164">Compressed</span></span> |<span data-ttu-id="b096d-165">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-165">Uncompressed</span></span> |<span data-ttu-id="b096d-166">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-166">Uncompressed</span></span> | |
| <span data-ttu-id="b096d-167">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-167">Compressed</span></span> |<span data-ttu-id="b096d-168">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="b096d-168">Not cached</span></span> |<span data-ttu-id="b096d-169">Komprimerad eller okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="b096d-170">Beror på ursprung svar</span><span class="sxs-lookup"><span data-stu-id="b096d-170">Depends on origin response</span></span> |
| <span data-ttu-id="b096d-171">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-171">Uncompressed</span></span> |<span data-ttu-id="b096d-172">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-172">Compressed</span></span> |<span data-ttu-id="b096d-173">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-173">Uncompressed</span></span> | |
| <span data-ttu-id="b096d-174">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-174">Uncompressed</span></span> |<span data-ttu-id="b096d-175">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-175">Uncompressed</span></span> |<span data-ttu-id="b096d-176">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-176">Uncompressed</span></span> | |
| <span data-ttu-id="b096d-177">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-177">Uncompressed</span></span> |<span data-ttu-id="b096d-178">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="b096d-178">Not cached</span></span> |<span data-ttu-id="b096d-179">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="b096d-180">Aktiverad komprimering och filnamnet är berättigad för komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="b096d-181">Klienten begärde format (kopplingshuvud Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="b096d-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="b096d-182">Cachelagrade filformat</span><span class="sxs-lookup"><span data-stu-id="b096d-182">Cached file format</span></span> | <span data-ttu-id="b096d-183">CDN-svar toohello klienten</span><span class="sxs-lookup"><span data-stu-id="b096d-183">CDN response toohello client</span></span> | <span data-ttu-id="b096d-184">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b096d-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b096d-185">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-185">Compressed</span></span> |<span data-ttu-id="b096d-186">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-186">Compressed</span></span> |<span data-ttu-id="b096d-187">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-187">Compressed</span></span> |<span data-ttu-id="b096d-188">CDN-transcodes mellan de format som stöds</span><span class="sxs-lookup"><span data-stu-id="b096d-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="b096d-189">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-189">Compressed</span></span> |<span data-ttu-id="b096d-190">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-190">Uncompressed</span></span> |<span data-ttu-id="b096d-191">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-191">Compressed</span></span> |<span data-ttu-id="b096d-192">CDN utför komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-192">CDN performs compression</span></span> |
| <span data-ttu-id="b096d-193">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-193">Compressed</span></span> |<span data-ttu-id="b096d-194">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="b096d-194">Not cached</span></span> |<span data-ttu-id="b096d-195">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-195">Compressed</span></span> |<span data-ttu-id="b096d-196">CDN utför komprimering om ursprung returnerar okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="b096d-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="b096d-197">**Azure CDN från Verizon** överför hello okomprimerad fil på hello första begäran och sedan komprimerar och cacheminnen hello-filen för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="b096d-197">**Azure CDN from Verizon** passes hello uncompressed file on hello first request and then compresses and caches hello file for subsequent requests.</span></span>  <span data-ttu-id="b096d-198">Filer med `Cache-Control: no-cache` kommer aldrig att komprimera huvudet.</span><span class="sxs-lookup"><span data-stu-id="b096d-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="b096d-199">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-199">Uncompressed</span></span> |<span data-ttu-id="b096d-200">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="b096d-200">Compressed</span></span> |<span data-ttu-id="b096d-201">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-201">Uncompressed</span></span> |<span data-ttu-id="b096d-202">CDN utför dekomprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-202">CDN performs decompression</span></span> |
| <span data-ttu-id="b096d-203">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-203">Uncompressed</span></span> |<span data-ttu-id="b096d-204">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-204">Uncompressed</span></span> |<span data-ttu-id="b096d-205">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-205">Uncompressed</span></span> | |
| <span data-ttu-id="b096d-206">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-206">Uncompressed</span></span> |<span data-ttu-id="b096d-207">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="b096d-207">Not cached</span></span> |<span data-ttu-id="b096d-208">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="b096d-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="b096d-209">Media Services CDN komprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-209">Media Services CDN Compression</span></span>
<span data-ttu-id="b096d-210">För Media Services CDN aktiverat strömningsslutpunkter komprimering är aktiverat som standard för hello följande typer av innehåll: application/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, program/f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="b096d-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for hello following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="b096d-211">Du kan inte aktivera/inaktivera komprimering för hello anges med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b096d-211">You cannot enable/disable compression for hello mentioned types using hello Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="b096d-212">Se även</span><span class="sxs-lookup"><span data-stu-id="b096d-212">See also</span></span>
* [<span data-ttu-id="b096d-213">Felsöka CDN-filkomprimering</span><span class="sxs-lookup"><span data-stu-id="b096d-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

