---
title: "Förbättra prestanda genom att komprimera filerna i Azure CDN | Microsoft Docs"
description: "Lär dig att förbättra hastighet för överföring av filen och ökar belastningen prestanda genom att komprimera filerna i Azure CDN."
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
ms.openlocfilehash: 7546650e6096a880f4fb4d0c94dd4ecc00b70160
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="improve-performance-by-compressing-files-in-azure-cdn"></a><span data-ttu-id="a3b0f-103">Förbättra prestanda genom att komprimera filerna i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="a3b0f-103">Improve performance by compressing files in Azure CDN</span></span>
<span data-ttu-id="a3b0f-104">Komprimering är ett enkelt och effektivt sätt att förbättra hastighet för överföring av filen och öka belastningen prestanda genom att minska filstorleken innan den skickas från servern.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-104">Compression is a simple and effective method to improve file transfer speed and increase page load performance by reducing file size before it is sent from the server.</span></span> <span data-ttu-id="a3b0f-105">Det minskar kostnader för bandbredd och ger en mer effektiv upplevelse för användarna.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-105">It reduces bandwidth costs and provides a more responsive experience for your users.</span></span>

<span data-ttu-id="a3b0f-106">Det finns två sätt att aktivera komprimering:</span><span class="sxs-lookup"><span data-stu-id="a3b0f-106">There are two ways to enable compression:</span></span>

* <span data-ttu-id="a3b0f-107">Du kan aktivera komprimering på ursprungsservern, i vilket fall CDN passerar genom komprimerade filer och leverera komprimerade filer till klienter som begär dem.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-107">You can enable compression on your origin server, in which case the CDN passes through the compressed files and deliver compressed files to clients that request them.</span></span>
* <span data-ttu-id="a3b0f-108">Du kan aktivera komprimering direkt på CDN edge-servrar, i vilket fall CDN komprimerar filerna och hantera dem till användare, även om de inte komprimeras i den ursprungliga servern.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-108">You can enable compression directly on CDN edge servers, in which case the CDN compresses the files and serve them to end users, even if they are not compressed by the origin server.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3b0f-109">CDN-konfigurationsändringar ta lite tid att spridas via nätverket.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-109">CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="a3b0f-110">För <b>Azure CDN från Akamai</b> profiler, spridningen vanligtvis har slutförts under en minut.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-110">For <b>Azure CDN from Akamai</b> profiles, propagation usually completes in under one minute.</span></span>  <span data-ttu-id="a3b0f-111">För <b>Azure CDN från Verizon</b> profiler, vanligtvis visas ändringarna gäller inom 90 minuter.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-111">For <b>Azure CDN from Verizon</b> profiles, you will usually see your changes apply within 90 minutes.</span></span>  <span data-ttu-id="a3b0f-112">Om du har konfigurerat komprimering för CDN-slutpunkten bör du väntar på 1 – 2 timmar att komprimering inställningar har spridits till POP före felsökning</span><span class="sxs-lookup"><span data-stu-id="a3b0f-112">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs before troubleshooting</span></span>
> 
> 

## <a name="enabling-compression"></a><span data-ttu-id="a3b0f-113">Aktivera komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-113">Enabling compression</span></span>
> [!NOTE]
> <span data-ttu-id="a3b0f-114">Standard- och Premium CDN nivåerna ge samma funktioner för komprimering, men användargränssnittet skiljer sig åt.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-114">The Standard and Premium CDN tiers provide the same compression functionality, but the user interface differs.</span></span>  <span data-ttu-id="a3b0f-115">Mer information om skillnaderna mellan Standard och Premium CDN nivåer finns [översikt över Azure CDN](cdn-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a3b0f-115">For more information about the differences between Standard and Premium CDN tiers, see [Azure CDN Overview](cdn-overview.md).</span></span>
> 
> 

### <a name="standard-tier"></a><span data-ttu-id="a3b0f-116">Standard-nivå</span><span class="sxs-lookup"><span data-stu-id="a3b0f-116">Standard tier</span></span>
> [!NOTE]
> <span data-ttu-id="a3b0f-117">Det här avsnittet gäller **Azure CDN Standard från Verizon** och **Azure CDN Standard från Akamai** profiler.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-117">This section applies to **Azure CDN Standard from Verizon** and **Azure CDN Standard from Akamai** profiles.</span></span>
> 
> 

1. <span data-ttu-id="a3b0f-118">Klicka på CDN-slutpunkt som du vill hantera från sidan CDN-profil.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-118">From the CDN profile page, click the CDN endpoint you wish to manage.</span></span>
   
    ![CDN-profilen sidan slutpunkter](./media/cdn-file-compression/cdn-endpoints.png)
   
    <span data-ttu-id="a3b0f-120">CDN-slutpunkten sidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-120">The CDN endpoint page opens.</span></span>
2. <span data-ttu-id="a3b0f-121">Klicka på den **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-121">Click the **Configure** button.</span></span>
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-config-btn.png)
   
    <span data-ttu-id="a3b0f-123">CDN-konfigurationssidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-123">The CDN Configuration page opens.</span></span>
3. <span data-ttu-id="a3b0f-124">Aktivera **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-124">Turn on **Compression**.</span></span>
   
    ![CDN-komprimeringsalternativ](./media/cdn-file-compression/cdn-compress-standard.png)
4. <span data-ttu-id="a3b0f-126">Använd standardtyperna eller ändra listan genom att ta bort eller lägga till filtyper.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-126">Use the default types, or modify the list by removing or adding file types.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="a3b0f-127">Även möjligt, rekommenderas inte att tillämpa komprimering komprimerat format, till exempel ZIP, MP3, MP4, JPG och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-127">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span>
   > 
   > 
5. <span data-ttu-id="a3b0f-128">När du har gjort ändringarna klickar du på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-128">After making your changes, click the **Save** button.</span></span>

### <a name="premium-tier"></a><span data-ttu-id="a3b0f-129">Premiumnivå</span><span class="sxs-lookup"><span data-stu-id="a3b0f-129">Premium tier</span></span>
> [!NOTE]
> <span data-ttu-id="a3b0f-130">Det här avsnittet gäller **Azure CDN Premium från Verizon** profiler.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-130">This section applies to **Azure CDN Premium from Verizon** profiles.</span></span>
> 
> 

1. <span data-ttu-id="a3b0f-131">Från sidan CDN-profil klickar du på den **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-131">From the CDN profile page, click the **Manage** button.</span></span>
   
    ![CDN-profilsidan hantera knappen](./media/cdn-file-compression/cdn-manage-btn.png)
   
    <span data-ttu-id="a3b0f-133">CDN-hanteringsportalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-133">The CDN management portal opens.</span></span>
2. <span data-ttu-id="a3b0f-134">Hovra över den **HTTP stora** och klicka sedan hovra över den **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-134">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="a3b0f-135">Klicka på **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-135">Click on **Compression**.</span></span>

    ![Komprimering filval](./media/cdn-file-compression/cdn-compress-select.png)
   
    <span data-ttu-id="a3b0f-137">Komprimeringsalternativ visas.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-137">Compression options are displayed.</span></span>
   
    ![Komprimering](./media/cdn-file-compression/cdn-compress-files.png)
3. <span data-ttu-id="a3b0f-139">Aktivera komprimering genom att klicka på den **komprimering aktiverat** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-139">Enable compression by clicking the **Compression Enabled** radio button.</span></span>  <span data-ttu-id="a3b0f-140">Ange MIME-typer som du vill komprimera som en kommaavgränsad lista (inga blanksteg) i den **filtyper** textruta.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-140">Enter the MIME types you wish to compress as a comma-delimited list (no spaces) in the **File Types** textbox.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="a3b0f-141">Även möjligt, rekommenderas inte att tillämpa komprimering komprimerat format, till exempel ZIP, MP3, MP4, JPG och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-141">While possible, it is not recommended to apply compression to compressed formats, such as ZIP, MP3, MP4, JPG, etc.</span></span> 
   > 
   > 
4. <span data-ttu-id="a3b0f-142">När du har gjort ändringarna klickar du på den **uppdatering** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-142">After making your changes, click the **Update** button.</span></span>

## <a name="compression-rules"></a><span data-ttu-id="a3b0f-143">Regler för komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-143">Compression rules</span></span>
<span data-ttu-id="a3b0f-144">Dessa tabeller beskrivs Azure CDN komprimering beteendet för varje scenario.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-144">These tables describe Azure CDN compression behavior for every scenario.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3b0f-145">För **Azure CDN från Verizon** (Standard och Premium) endast filer komprimeras.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-145">For **Azure CDN from Verizon** (Standard and Premium), only eligible files are compressed.</span></span>  <span data-ttu-id="a3b0f-146">En fil måste vara uppfyllda för komprimering:</span><span class="sxs-lookup"><span data-stu-id="a3b0f-146">To be eligible for compression, a file must:</span></span>
> 
> * <span data-ttu-id="a3b0f-147">Är större än 128 byte.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-147">Be larger than 128 bytes.</span></span>
> * <span data-ttu-id="a3b0f-148">Vara mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-148">Be smaller than 1 MB.</span></span>
> 
> <span data-ttu-id="a3b0f-149">För **Azure CDN från Akamai**, alla filer är tillgängliga för komprimering.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-149">For **Azure CDN from Akamai**, all files are eligible for compression.</span></span>
> 
> <span data-ttu-id="a3b0f-150">För alla Azure CDN-produkter, en fil måste vara en MIME-typ som har varit [konfigurerats för komprimering](#enabling-compression).</span><span class="sxs-lookup"><span data-stu-id="a3b0f-150">For all Azure CDN products, a file must be a MIME type that has been [configured for compression](#enabling-compression).</span></span>
> 
> <span data-ttu-id="a3b0f-151">**Azure CDN från Verizon** profiler (Standard och Premium) stöd **gzip** (GNU zip) **deflate**, **bzip2**, eller **br**(Brotli) kodning.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-151">**Azure CDN from Verizon** profiles (Standard and Premium) support **gzip** (GNU zip), **deflate**, **bzip2**, or  **br** (Brotli) encoding.</span></span> <span data-ttu-id="a3b0f-152">Komprimeringen görs endast i utkanten för Brotli kodning.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-152">For Brotli encoding, the compression is done only at the edge.</span></span> <span data-ttu-id="a3b0f-153">Klientwebbläsaren måste skicka förfrågan Brotli kodning och komprimerade tillgången måste komprimerats på ursprung sida först.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-153">The client/browser must send the request for Brotli encoding and the compressed asset must have been compressed on the origin side first.</span></span> 
>
><span data-ttu-id="a3b0f-154">**Azure CDN från Akamai** profiler stöder bara **gzip** kodning.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-154">**Azure CDN from Akamai** profiles support only **gzip** encoding.</span></span>
> 
> <span data-ttu-id="a3b0f-155">**Azure CDN från Akamai** slutpunkter begär alltid **gzip** kodade filer från ursprung, oavsett klientbegäran.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-155">**Azure CDN from Akamai** endpoints always request **gzip** encoded files from the origin, regardless of the client request.</span></span> 

### <a name="compression-disabled-or-file-is-ineligible-for-compression"></a><span data-ttu-id="a3b0f-156">Komprimering inaktiveras eller filen är inte tillgänglig för komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-156">Compression disabled or file is ineligible for compression</span></span>
| <span data-ttu-id="a3b0f-157">Klienten begärde format (kopplingshuvud Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="a3b0f-157">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="a3b0f-158">Cachelagrade filformat</span><span class="sxs-lookup"><span data-stu-id="a3b0f-158">Cached file format</span></span> | <span data-ttu-id="a3b0f-159">CDN-svar till klienten</span><span class="sxs-lookup"><span data-stu-id="a3b0f-159">CDN response to the client</span></span> | <span data-ttu-id="a3b0f-160">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a3b0f-160">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3b0f-161">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-161">Compressed</span></span> |<span data-ttu-id="a3b0f-162">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-162">Compressed</span></span> |<span data-ttu-id="a3b0f-163">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-163">Compressed</span></span> | |
| <span data-ttu-id="a3b0f-164">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-164">Compressed</span></span> |<span data-ttu-id="a3b0f-165">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-165">Uncompressed</span></span> |<span data-ttu-id="a3b0f-166">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-166">Uncompressed</span></span> | |
| <span data-ttu-id="a3b0f-167">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-167">Compressed</span></span> |<span data-ttu-id="a3b0f-168">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="a3b0f-168">Not cached</span></span> |<span data-ttu-id="a3b0f-169">Komprimerad eller okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-169">Compressed or Uncompressed</span></span> |<span data-ttu-id="a3b0f-170">Beror på ursprung svar</span><span class="sxs-lookup"><span data-stu-id="a3b0f-170">Depends on origin response</span></span> |
| <span data-ttu-id="a3b0f-171">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-171">Uncompressed</span></span> |<span data-ttu-id="a3b0f-172">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-172">Compressed</span></span> |<span data-ttu-id="a3b0f-173">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-173">Uncompressed</span></span> | |
| <span data-ttu-id="a3b0f-174">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-174">Uncompressed</span></span> |<span data-ttu-id="a3b0f-175">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-175">Uncompressed</span></span> |<span data-ttu-id="a3b0f-176">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-176">Uncompressed</span></span> | |
| <span data-ttu-id="a3b0f-177">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-177">Uncompressed</span></span> |<span data-ttu-id="a3b0f-178">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="a3b0f-178">Not cached</span></span> |<span data-ttu-id="a3b0f-179">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-179">Uncompressed</span></span> | |

### <a name="compression-enabled-and-file-is-eligible-for-compression"></a><span data-ttu-id="a3b0f-180">Aktiverad komprimering och filnamnet är berättigad för komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-180">Compression enabled and file is eligible for compression</span></span>
| <span data-ttu-id="a3b0f-181">Klienten begärde format (kopplingshuvud Accept-Encoding)</span><span class="sxs-lookup"><span data-stu-id="a3b0f-181">Client requested format (via Accept-Encoding header)</span></span> | <span data-ttu-id="a3b0f-182">Cachelagrade filformat</span><span class="sxs-lookup"><span data-stu-id="a3b0f-182">Cached file format</span></span> | <span data-ttu-id="a3b0f-183">CDN-svar till klienten</span><span class="sxs-lookup"><span data-stu-id="a3b0f-183">CDN response to the client</span></span> | <span data-ttu-id="a3b0f-184">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a3b0f-184">Notes</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a3b0f-185">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-185">Compressed</span></span> |<span data-ttu-id="a3b0f-186">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-186">Compressed</span></span> |<span data-ttu-id="a3b0f-187">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-187">Compressed</span></span> |<span data-ttu-id="a3b0f-188">CDN-transcodes mellan de format som stöds</span><span class="sxs-lookup"><span data-stu-id="a3b0f-188">CDN transcodes between supported formats</span></span> |
| <span data-ttu-id="a3b0f-189">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-189">Compressed</span></span> |<span data-ttu-id="a3b0f-190">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-190">Uncompressed</span></span> |<span data-ttu-id="a3b0f-191">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-191">Compressed</span></span> |<span data-ttu-id="a3b0f-192">CDN utför komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-192">CDN performs compression</span></span> |
| <span data-ttu-id="a3b0f-193">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-193">Compressed</span></span> |<span data-ttu-id="a3b0f-194">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="a3b0f-194">Not cached</span></span> |<span data-ttu-id="a3b0f-195">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-195">Compressed</span></span> |<span data-ttu-id="a3b0f-196">CDN utför komprimering om ursprung returnerar okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-196">CDN performs compression if origin returns uncompressed.</span></span>  <span data-ttu-id="a3b0f-197">**Azure CDN från Verizon** skickar okomprimerad fil på den första begäranden och sedan komprimeras och cachelagrar filen för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-197">**Azure CDN from Verizon** passes the uncompressed file on the first request and then compresses and caches the file for subsequent requests.</span></span>  <span data-ttu-id="a3b0f-198">Filer med `Cache-Control: no-cache` kommer aldrig att komprimera huvudet.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-198">Files with `Cache-Control: no-cache` header will never be compressed.</span></span> |
| <span data-ttu-id="a3b0f-199">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-199">Uncompressed</span></span> |<span data-ttu-id="a3b0f-200">Komprimerade</span><span class="sxs-lookup"><span data-stu-id="a3b0f-200">Compressed</span></span> |<span data-ttu-id="a3b0f-201">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-201">Uncompressed</span></span> |<span data-ttu-id="a3b0f-202">CDN utför dekomprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-202">CDN performs decompression</span></span> |
| <span data-ttu-id="a3b0f-203">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-203">Uncompressed</span></span> |<span data-ttu-id="a3b0f-204">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-204">Uncompressed</span></span> |<span data-ttu-id="a3b0f-205">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-205">Uncompressed</span></span> | |
| <span data-ttu-id="a3b0f-206">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-206">Uncompressed</span></span> |<span data-ttu-id="a3b0f-207">Inte cachelagras</span><span class="sxs-lookup"><span data-stu-id="a3b0f-207">Not cached</span></span> |<span data-ttu-id="a3b0f-208">Okomprimerad</span><span class="sxs-lookup"><span data-stu-id="a3b0f-208">Uncompressed</span></span> | |

## <a name="media-services-cdn-compression"></a><span data-ttu-id="a3b0f-209">Media Services CDN komprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-209">Media Services CDN Compression</span></span>
<span data-ttu-id="a3b0f-210">För Media Services CDN aktiverat strömningsslutpunkter komprimering är aktiverat som standard för följande typer av innehåll: application/vnd.ms-sstr + xml, application/dash+xml,application/vnd.apple.mpegurl, program/f4m + xml.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-210">For Media Services CDN enabled streaming endpoints, compression is enabled by default for the following content types: application/vnd.ms-sstr+xml, application/dash+xml,application/vnd.apple.mpegurl, application/f4m+xml.</span></span> <span data-ttu-id="a3b0f-211">Du kan inte aktivera/inaktivera komprimering för nämnda typer med Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a3b0f-211">You cannot enable/disable compression for the mentioned types using the Azure portal.</span></span>  

## <a name="see-also"></a><span data-ttu-id="a3b0f-212">Se även</span><span class="sxs-lookup"><span data-stu-id="a3b0f-212">See also</span></span>
* [<span data-ttu-id="a3b0f-213">Felsöka CDN-filkomprimering</span><span class="sxs-lookup"><span data-stu-id="a3b0f-213">Troubleshooting CDN file compression</span></span>](cdn-troubleshoot-compression.md)    

