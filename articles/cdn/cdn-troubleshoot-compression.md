---
title: aaaTroubleshooting filkomprimering i Azure CDN | Microsoft Docs
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
ms.openlocfilehash: f00b98beaf6b3b3cd30108ece65a8191edc06ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-cdn-file-compression"></a><span data-ttu-id="22940-103">Felsöka CDN-filkomprimering</span><span class="sxs-lookup"><span data-stu-id="22940-103">Troubleshooting CDN file compression</span></span>
<span data-ttu-id="22940-104">Den här artikeln hjälper dig att felsöka problem med [CDN filkomprimering](cdn-improve-performance.md).</span><span class="sxs-lookup"><span data-stu-id="22940-104">This article helps you troubleshoot issues with [CDN file compression](cdn-improve-performance.md).</span></span>

<span data-ttu-id="22940-105">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="22940-105">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="22940-106">Alternativt kan du även filen en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="22940-106">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="22940-107">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och på **Get Support**.</span><span class="sxs-lookup"><span data-stu-id="22940-107">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span>

## <a name="symptom"></a><span data-ttu-id="22940-108">Symtom</span><span class="sxs-lookup"><span data-stu-id="22940-108">Symptom</span></span>
<span data-ttu-id="22940-109">Komprimering för din slutpunkt har aktiverats men filer returneras okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="22940-109">Compression for your endpoint is enabled, but files are being returned uncompressed.</span></span>

> [!TIP]
> <span data-ttu-id="22940-110">toocheck om filerna returneras komprimerade du behöver ett verktyg som toouse [Fiddler](http://www.telerik.com/fiddler) eller din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="22940-110">toocheck whether your files are being returned compressed, you need toouse a tool like [Fiddler](http://www.telerik.com/fiddler) or your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>  <span data-ttu-id="22940-111">Kontrollera hello HTTP-svarshuvuden returneras med den cachelagrade CDN innehåll.</span><span class="sxs-lookup"><span data-stu-id="22940-111">Check hello HTTP response headers returned with your cached CDN content.</span></span>  <span data-ttu-id="22940-112">Om det finns en rubrik med namnet `Content-Encoding` med värdet **gzip**, **bzip2**, eller **deflate**, ditt innehåll komprimeras.</span><span class="sxs-lookup"><span data-stu-id="22940-112">If there is a header named `Content-Encoding` with a value of **gzip**, **bzip2**, or **deflate**, your content is compressed.</span></span>
> 
> ![Innehållskodning sidhuvud](./media/cdn-troubleshoot-compression/cdn-content-header.png)
> 
> 

## <a name="cause"></a><span data-ttu-id="22940-114">Orsak</span><span class="sxs-lookup"><span data-stu-id="22940-114">Cause</span></span>
<span data-ttu-id="22940-115">Det finns flera möjliga orsaker, bland annat:</span><span class="sxs-lookup"><span data-stu-id="22940-115">There are several possible causes, including:</span></span>

* <span data-ttu-id="22940-116">hello begärt innehåll inte är tillgänglig för komprimering.</span><span class="sxs-lookup"><span data-stu-id="22940-116">hello requested content is not eligible for compression.</span></span>
* <span data-ttu-id="22940-117">Komprimering är inte aktiverad för hello begärt filtyp.</span><span class="sxs-lookup"><span data-stu-id="22940-117">Compression is not enabled for hello requested file type.</span></span>
* <span data-ttu-id="22940-118">hello HTTP-begäran innehöll inte en rubrik som begär en giltig Komprimeringstypen.</span><span class="sxs-lookup"><span data-stu-id="22940-118">hello HTTP request did not include a header requesting a valid compression type.</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="22940-119">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="22940-119">Troubleshooting steps</span></span>
> [!TIP]
> <span data-ttu-id="22940-120">Som distribuerar nya slutpunkter, tar CDN konfigurationsändringar vissa tid toopropagate via hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="22940-120">As with deploying new endpoints, CDN configuration changes take some time toopropagate through hello network.</span></span>  <span data-ttu-id="22940-121">Vanligtvis tillämpas ändringar inom 90 minuter.</span><span class="sxs-lookup"><span data-stu-id="22940-121">Usually, changes are applied within 90 minutes.</span></span>  <span data-ttu-id="22940-122">Om detta är hello första gången som du har konfigurerat komprimering för CDN-slutpunkt, bör du vänta 1 – 2 timmar toobe att hello komprimeringsinställningar har spridits toohello POP.</span><span class="sxs-lookup"><span data-stu-id="22940-122">If this is hello first time you've set up compression for your CDN endpoint, you should consider waiting 1-2 hours toobe sure hello compression settings have propagated toohello POPs.</span></span> 
> 
> 

### <a name="verify-hello-request"></a><span data-ttu-id="22940-123">Kontrollera hello begäran</span><span class="sxs-lookup"><span data-stu-id="22940-123">Verify hello request</span></span>
<span data-ttu-id="22940-124">Först ska vi göra en snabb förstånd kontroll på hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="22940-124">First, we should do a quick sanity check on hello request.</span></span>  <span data-ttu-id="22940-125">Du kan använda din webbläsare [utvecklingsverktyg](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello begäranden.</span><span class="sxs-lookup"><span data-stu-id="22940-125">You can use your browser's [developer tools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) tooview hello requests being made.</span></span>

* <span data-ttu-id="22940-126">Kontrollera hello-begäran skickas tooyour slutpunkts-URL, `<endpointname>.azureedge.net`, och inte din ursprung.</span><span class="sxs-lookup"><span data-stu-id="22940-126">Verify hello request is being sent tooyour endpoint URL, `<endpointname>.azureedge.net`, and not your origin.</span></span>
* <span data-ttu-id="22940-127">Kontrollera hello-begäran innehåller en **Accept-Encoding** sidhuvud och hello värde att huvudet innehåller **gzip**, **deflate**, eller **bzip2** .</span><span class="sxs-lookup"><span data-stu-id="22940-127">Verify hello request contains an **Accept-Encoding** header, and hello value for that header contains **gzip**, **deflate**, or **bzip2**.</span></span>

> [!NOTE]
> <span data-ttu-id="22940-128">**Azure CDN från Akamai** profiler endast stöder **gzip** kodning.</span><span class="sxs-lookup"><span data-stu-id="22940-128">**Azure CDN from Akamai** profiles only support **gzip** encoding.</span></span>
> 
> 

![CDN-huvuden för begäran](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### <a name="verify-compression-settings-standard-cdn-profile"></a><span data-ttu-id="22940-130">Kontrollera inställningarna för komprimering (Standard CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="22940-130">Verify compression settings (Standard CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="22940-131">Det här steget gäller endast om din CDN-profilen är en **Azure CDN Standard från Verizon** eller **Azure CDN Standard från Akamai** profil.</span><span class="sxs-lookup"><span data-stu-id="22940-131">This step only applies if your CDN profile is an **Azure CDN Standard from Verizon** or **Azure CDN Standard from Akamai** profile.</span></span> 
> 
> 

<span data-ttu-id="22940-132">Navigera tooyour slutpunkt i hello [Azure-portalen](https://portal.azure.com) och klicka på hello **konfigurera** knappen.</span><span class="sxs-lookup"><span data-stu-id="22940-132">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Configure** button.</span></span>

* <span data-ttu-id="22940-133">Kontrollera komprimering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="22940-133">Verify compression is enabled.</span></span>
* <span data-ttu-id="22940-134">Kontrollera hello MIME-typ för hello innehåll komprimeras toobe ingår i hello lista över komprimerat format.</span><span class="sxs-lookup"><span data-stu-id="22940-134">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![CDN-inställningarna för komprimering](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### <a name="verify-compression-settings-premium-cdn-profile"></a><span data-ttu-id="22940-136">Kontrollera inställningarna för komprimering (Premium CDN-profil)</span><span class="sxs-lookup"><span data-stu-id="22940-136">Verify compression settings (Premium CDN profile)</span></span>
> [!NOTE]
> <span data-ttu-id="22940-137">Det här steget gäller endast om din CDN-profilen är en **Azure CDN Premium från Verizon** profil.</span><span class="sxs-lookup"><span data-stu-id="22940-137">This step only applies if your CDN profile is an **Azure CDN Premium from Verizon** profile.</span></span>
> 
> 

<span data-ttu-id="22940-138">Navigera tooyour slutpunkt i hello [Azure-portalen](https://portal.azure.com) och klicka på hello **hantera** knappen.</span><span class="sxs-lookup"><span data-stu-id="22940-138">Navigate tooyour endpoint in hello [Azure portal](https://portal.azure.com) and click hello **Manage** button.</span></span>  <span data-ttu-id="22940-139">hello kompletterande portalen öppnas.</span><span class="sxs-lookup"><span data-stu-id="22940-139">hello supplemental portal will open.</span></span>  <span data-ttu-id="22940-140">Hovra över hello **HTTP stora** fliken och sedan hovra över hello **cacheinställningarna** utfällbar.</span><span class="sxs-lookup"><span data-stu-id="22940-140">Hover over hello **HTTP Large** tab, then hover over hello **Cache Settings** flyout.</span></span>  <span data-ttu-id="22940-141">Klicka på **komprimering**.</span><span class="sxs-lookup"><span data-stu-id="22940-141">Click **Compression**.</span></span> 

* <span data-ttu-id="22940-142">Kontrollera komprimering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="22940-142">Verify compression is enabled.</span></span>
* <span data-ttu-id="22940-143">Kontrollera hello **filtyper** listan innehåller en kommaavgränsad lista (inga blanksteg) över MIME-typer.</span><span class="sxs-lookup"><span data-stu-id="22940-143">Verify hello **File Types** list contains a comma-separated list (no spaces) of MIME types.</span></span>
* <span data-ttu-id="22940-144">Kontrollera hello MIME-typ för hello innehåll komprimeras toobe ingår i hello lista över komprimerat format.</span><span class="sxs-lookup"><span data-stu-id="22940-144">Verify hello MIME type for hello content toobe compressed is included in hello list of compressed formats.</span></span>

![CDN premium komprimeringsinställningar](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)

### <a name="verify-hello-content-is-cached"></a><span data-ttu-id="22940-146">Kontrollera hello innehåll cachelagras</span><span class="sxs-lookup"><span data-stu-id="22940-146">Verify hello content is cached</span></span>
> [!NOTE]
> <span data-ttu-id="22940-147">Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).</span><span class="sxs-lookup"><span data-stu-id="22940-147">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="22940-148">Kontrollera med din webbläsare utvecklingsverktyg hello huvuden tooensure hello svarsfilen cachelagras i hello region där den begärs.</span><span class="sxs-lookup"><span data-stu-id="22940-148">Using your browser's developer tools, check hello response headers tooensure hello file is cached in hello region where it is being requested.</span></span>

* <span data-ttu-id="22940-149">Kontrollera hello **Server** Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="22940-149">Check hello **Server** response header.</span></span>  <span data-ttu-id="22940-150">hello huvudet ska ha formatet hello **plattform (POP-Server-ID)**, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="22940-150">hello header should have hello format **Platform (POP/Server ID)**, as seen in hello following example.</span></span>
* <span data-ttu-id="22940-151">Kontrollera hello **X-Cache** Svarsrubrik.</span><span class="sxs-lookup"><span data-stu-id="22940-151">Check hello **X-Cache** response header.</span></span>  <span data-ttu-id="22940-152">hello sidhuvud bör du läsa **NÅDDE**.</span><span class="sxs-lookup"><span data-stu-id="22940-152">hello header should read **HIT**.</span></span>  

![CDN-svarshuvuden](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### <a name="verify-hello-file-meets-hello-size-requirements"></a><span data-ttu-id="22940-154">Kontrollera hello filen uppfyller hello storlek</span><span class="sxs-lookup"><span data-stu-id="22940-154">Verify hello file meets hello size requirements</span></span>
> [!NOTE]
> <span data-ttu-id="22940-155">Det här steget gäller endast om din CDN-profilen är en **Azure CDN från Verizon** profilen (Standard eller Premium).</span><span class="sxs-lookup"><span data-stu-id="22940-155">This step only applies if your CDN profile is an **Azure CDN from Verizon** profile (Standard or Premium).</span></span>
> 
> 

<span data-ttu-id="22940-156">toobe berättigad för komprimering, en fil måste uppfylla följande storlekskraven hello:</span><span class="sxs-lookup"><span data-stu-id="22940-156">toobe eligible for compression, a file must meet hello following size requirements:</span></span>

* <span data-ttu-id="22940-157">Större än 128 tecken.</span><span class="sxs-lookup"><span data-stu-id="22940-157">Larger than 128 bytes.</span></span>
* <span data-ttu-id="22940-158">Mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="22940-158">Smaller than 1 MB.</span></span>

### <a name="check-hello-request-at-hello-origin-server-for-a-via-header"></a><span data-ttu-id="22940-159">Kontrollera hello begäran vid hello ursprungsservern för en **Via** sidhuvud</span><span class="sxs-lookup"><span data-stu-id="22940-159">Check hello request at hello origin server for a **Via** header</span></span>
<span data-ttu-id="22940-160">Hej **Via** HTTP-huvudet anger toohello webbserver som hello begäran skickas via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="22940-160">hello **Via** HTTP header indicates toohello web server that hello request is being passed by a proxy server.</span></span>  <span data-ttu-id="22940-161">Microsoft IIS-webbservrar som standard komprimera svar när hello-begäran innehåller en **Via** huvud.</span><span class="sxs-lookup"><span data-stu-id="22940-161">Microsoft IIS web servers by default do not compress responses when hello request contains a **Via** header.</span></span>  <span data-ttu-id="22940-162">toooverride problemet, utföra hello följande:</span><span class="sxs-lookup"><span data-stu-id="22940-162">toooverride this behavior, perform hello following:</span></span>

* <span data-ttu-id="22940-163">**IIS 6**: [ange egenskaperna HcNoCompressionForProxies = ”FALSE” i egenskaperna för hello IIS-metabas](https://msdn.microsoft.com/library/ms525390.aspx)</span><span class="sxs-lookup"><span data-stu-id="22940-163">**IIS 6**: [Set HcNoCompressionForProxies="FALSE" in hello IIS Metabase properties](https://msdn.microsoft.com/library/ms525390.aspx)</span></span>
* <span data-ttu-id="22940-164">**IIS 7 och senare**: [anger både **noCompressionForHttp10** och **noCompressionForProxies** tooFalse i hello serverkonfiguration](http://www.iis.net/configreference/system.webserver/httpcompression)</span><span class="sxs-lookup"><span data-stu-id="22940-164">**IIS 7 and up**: [Set both **noCompressionForHttp10** and **noCompressionForProxies** tooFalse in hello server configuration](http://www.iis.net/configreference/system.webserver/httpcompression)</span></span>

