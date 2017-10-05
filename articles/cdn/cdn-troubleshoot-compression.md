---
title: "Felsöka filkomprimering i Azure CDN | Microsoft Docs"
description: "Felsöka problem med Azure CDN komprimering."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: a6624e65-1a77-4486-b473-8d720ce28f8b
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 5ef8a8262eb40aa827161764f03a63d031e43273
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="54946-103">Felsöka CDN-filkomprimering</span><span class="sxs-lookup"><span data-stu-id="54946-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="54946-104">Den här artikeln hjälper dig att felsöka problem med [CDN filkomprimering](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="54946-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="54946-105">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta Azure-experter på [MSDN Azure och Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="54946-105">If you need more help at any point in this article, you can contact the Azure experts on [the MSDN Azure and the Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="54946-106">Alternativt kan du även filen en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="54946-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="54946-107">Gå till den [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och på **Get Support**.</span><span class="sxs-lookup"><span data-stu-id="54946-107">Go to the [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="54946-108">Symtom</span><span class="sxs-lookup"><span data-stu-id="54946-108">Symptom</span></span>
<span data-ttu-id="54946-109">Komprimering för din slutpunkt har aktiverats men filer returneras okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="54946-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="54946-110">Om du vill kontrollera om filerna returneras komprimerade, måste du använda ett verktyg som [Fiddler](http://www.telerik.com/fiddler) eller din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="54946-110">To check whether your files are being returned compressed, you need to use a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="54946-111">Kontrollera HTTP-svarshuvuden returneras med den cachelagrade CDN innehåll.</span><span class="sxs-lookup"><span data-stu-id="54946-111">Check the HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="54946-112">Om det finns en rubrik med namnet `Content-Encoding` med värdet **gzip**, **bzip2**, eller **deflate**, ditt innehåll komprimeras.</span><span class="sxs-lookup"><span data-stu-id="54946-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Innehållskodning sidhuvud](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="54946-114">Orsak</span><span class="sxs-lookup"><span data-stu-id="54946-114">Cause</span></span>
<span data-ttu-id="54946-115">Det finns flera möjliga orsaker, bland annat:</span><span class="sxs-lookup"><span data-stu-id="54946-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="54946-116">Det begärda innehållet är inte tillgänglig för komprimering.</span><span class="sxs-lookup"><span data-stu-id="54946-116">The requested content is not eligible for compression.</span></span>
* <span data-ttu-id="54946-117">Komprimering är inte aktiverat för den begärda filen.</span><span class="sxs-lookup"><span data-stu-id="54946-117">Compression is not enabled for the requested file type.</span></span>
* <span data-ttu-id="54946-118">HTTP-begäran innehöll inte en rubrik som begär en giltig Komprimeringstypen.</span><span class="sxs-lookup"><span data-stu-id="54946-118">The HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="54946-119">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="54946-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="54946-120">Som distribuerar nya slutpunkter, ta CDN konfigurationsändringar lite tid att spridas via nätverket.</span><span class="sxs-lookup"><span data-stu-id="54946-120">As with deploying new endpoints, CDN configuration changes take some time to propagate through the network.</span></span>  <span data-ttu-id="54946-121">Vanligtvis tillämpas ändringar inom 90 minuter.</span><span class="sxs-lookup"><span data-stu-id="54946-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="54946-122">Om du har konfigurerat komprimering för CDN-slutpunkten bör du vänta 1 – 2 timmar att komprimering inställningar har spridits till POP.</span><span class="sxs-lookup"><span data-stu-id="54946-122">If this is the first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours to be sure the compression settings have propagated to the POPs.</span></span> 
> 
> 

### <a name="verify-the-request"></a><span data-ttu-id="54946-123">Kontrollera begäran</span><span class="sxs-lookup"><span data-stu-id="54946-123">Verify the request</span></span>
<span data-ttu-id="54946-124">Först ska vi göra en snabb förstånd kontroll på begäran.</span><span class="sxs-lookup"><span data-stu-id="54946-124">First, we should do a quick sanity check on the request.</span></span>  <span data-ttu-id="54946-125">Du kan använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) att se begäranden som gjorts.</span><span class="sxs-lookup"><span data-stu-id="54946-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) to view the requests being made.</span></span>

* <span data-ttu-id="54946-126">Kontrollera begäran skickas till slutpunkts-URL `<endpointname>.azureedge.net`, och inte din ursprung.</span><span class="sxs-lookup"><span data-stu-id="54946-126">Verify the request is being sent to your endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="54946-127">Kontrollera begäran innehåller en **Accept-Encoding** sidhuvud och värdet för det huvudet innehåller **gzip**, **deflate**, eller **bzip2**.</span><span class="sxs-lookup"><span data-stu-id="54946-127">Verify the request contains an **Accept-Encoding** header, and the value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="54946-128">**Azure CDN från Akamai** profiler endast stöder **gzip** kodning.</span><span class="sxs-lookup"><span data-stu-id="54946-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![CDN-huvuden för begäran](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="54946-130">Kontrollera inställningarna för komprimering (Standard CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="54946-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="54946-131">Det här steget gäller endast om din CDN-profilen är en **Azure CDN Standard från Verizon** eller **Azure CDN Standard från Akamai** profil.</span><span class="sxs-lookup"><span data-stu-id="54946-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="54946-132">Navigera till din slutpunkt i den [Azure-portalen](https://portal.azure.com) och klicka på den **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="54946-132">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Configure** button.</span></span>

* <span data-ttu-id="54946-133">Kontrollera komprimering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="54946-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="54946-134">Kontrollera att MIME-typ för innehållet som kan komprimeras ingår i listan i komprimerat format.</span><span class="sxs-lookup"><span data-stu-id="54946-134">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![CDN-inställningarna för komprimering](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="54946-136">Kontrollera inställningarna för komprimering (Premium CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="54946-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="54946-137">Det här steget gäller endast om din CDN-profilen är en **Azure CDN Premium från Verizon** profil.</span><span class="sxs-lookup"><span data-stu-id="54946-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="54946-138">Navigera till din slutpunkt i den [Azure-portalen](https://portal.azure.com) och klicka på den **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="54946-138">Navigate to your endpoint in the [Azure portal](https://portal.azure.com) and click the **Manage** button.</span></span>  <span data-ttu-id="54946-139">Den kompletterande portalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="54946-139">The supplemental portal will open.</span></span>  <span data-ttu-id="54946-140">Hovra över den **HTTP stora** och klicka sedan hovra över den **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="54946-140">Hover over the **HTTP Large** tab, then hover over the **Cache Settings** flyout.</span></span>  <span data-ttu-id="54946-141">Klicka på **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="54946-141">Click **Compression**.</span></span> 

* <span data-ttu-id="54946-142">Kontrollera komprimering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="54946-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="54946-143">Kontrollera den **filtyper** listan innehåller en kommaavgränsad lista (inga blanksteg) över MIME-typer.</span><span class="sxs-lookup"><span data-stu-id="54946-143">Verify the **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="54946-144">Kontrollera att MIME-typ för innehållet som kan komprimeras ingår i listan i komprimerat format.</span><span class="sxs-lookup"><span data-stu-id="54946-144">Verify the MIME type for the content to be compressed is included in the list of compressed formats.</span></span>

![CDN premium komprimeringsinställningar](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-the-content-is-cached"></a><span data-ttu-id="54946-146">Kontrollera innehållet cachelagras</span><span class="sxs-lookup"><span data-stu-id="54946-146">Verify the content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="54946-147">Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).</span><span class="sxs-lookup"><span data-stu-id="54946-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="54946-148">Kontrollera med din webbläsare utvecklingsverktyg svarshuvuden för att se till att filen lagras i den region där den begärs.</span><span class="sxs-lookup"><span data-stu-id="54946-148">Using your browser's developer tools, check the response headers to ensure the file is cached in the region where it is being requested.</span></span>

* <span data-ttu-id="54946-149">Kontrollera den **Server** Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="54946-149">Check the **Server** response header.</span></span>  <span data-ttu-id="54946-150">Huvudet ska ha formatet **plattform (POP-Server-ID)**, som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="54946-150">The header should have the format **Platform (POP/Server ID)**, as seen in the following example.</span></span>
* <span data-ttu-id="54946-151">Kontrollera den **X-Cache** Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="54946-151">Check the **X-Cache** response header.</span></span>  <span data-ttu-id="54946-152">Huvudet bör du läsa **NÅDDE**.</span><span class="sxs-lookup"><span data-stu-id="54946-152">The header should read **HIT**.</span></span>  

![CDN-svarshuvuden](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-the-file-meets-the-size-requirements"></a><span data-ttu-id="54946-154">Kontrollera att filen uppfyller storlek</span><span class="sxs-lookup"><span data-stu-id="54946-154">Verify the file meets the size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="54946-155">Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).</span><span class="sxs-lookup"><span data-stu-id="54946-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="54946-156">För att få komprimering, måste en fil uppfylla följande krav för storlek:</span><span class="sxs-lookup"><span data-stu-id="54946-156">To be eligible for compression, a file must meet the following size requirements:</span></span>

* <span data-ttu-id="54946-157">Större än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="54946-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="54946-158">Mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="54946-158">Smaller than 1 MB.</span></span>

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a><span data-ttu-id="54946-159">Kontrollera begäran på den ursprungliga servern en **Via** sidhuvud</span><span class="sxs-lookup"><span data-stu-id="54946-159">Check the request at the origin server for a **Via** header</span></span>
<span data-ttu-id="54946-160">Den **Via** HTTP-huvud som anger att webbservern att begäran som skickas via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="54946-160">The **Via** HTTP header indicates to the web server that the request is being passed by a proxy server.</span></span>  <span data-ttu-id="54946-161">Microsoft IIS-webbservrar som standard komprimera svar när begäran innehåller en **Via** huvud.</span><span class="sxs-lookup"><span data-stu-id="54946-161">Microsoft IIS web servers by default do not compress responses when the request contains a **Via** header.</span></span>  <span data-ttu-id="54946-162">Om du vill åsidosätta detta genom att utföra följande:</span><span class="sxs-lookup"><span data-stu-id="54946-162">To override this behavior, perform the following:</span></span>

* <span data-ttu-id="54946-163">**IIS 6**: [ange egenskaperna HcNoCompressionForProxies = ”FALSE” i egenskaperna för IIS-metabasen](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="54946-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in the IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="54946-164">**IIS 7 och senare**: [anger både **noCompressionForHttp10** och **noCompressionForProxies** till False i serverkonfigurationen](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="54946-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** to False in the server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

