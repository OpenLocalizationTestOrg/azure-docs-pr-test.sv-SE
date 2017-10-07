---
title: "aaaOptimize Azure content delivery för ditt scenario"
description: "Hur toooptimize leverans av ditt innehåll för specifika scenarier"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: 922a92fdbf7e6e21f2b5ae9a2fb4ac32735fc15a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a><span data-ttu-id="1a597-103">Optimera Azure content delivery för ditt scenario</span><span class="sxs-lookup"><span data-stu-id="1a597-103">Optimize Azure content delivery for your scenario</span></span>

<span data-ttu-id="1a597-104">När du levererar innehåll tooa stor global målgrupp är kritiska tooensure hello optimerade överföringen av innehållet.</span><span class="sxs-lookup"><span data-stu-id="1a597-104">When you deliver content tooa large global audience, it's critical tooensure hello optimized delivery of your content.</span></span> <span data-ttu-id="1a597-105">hello Azure innehåll Delivery Network kan optimera hello leverans miljö baserat på hello typ av innehåll som du har.</span><span class="sxs-lookup"><span data-stu-id="1a597-105">hello Azure Content Delivery Network can optimize hello delivery experience based on hello type of content you have.</span></span> <span data-ttu-id="1a597-106">Innehållet kan vara en webbplats, en direktsänd dataström, en video eller en stor fil för hämtning.</span><span class="sxs-lookup"><span data-stu-id="1a597-106">Content can be a website, a live stream, a video, or a large file for download.</span></span> <span data-ttu-id="1a597-107">När du skapar en slutpunkt för content delivery network (CDN) måste du ange ett scenario i hello **optimerade för** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1a597-107">When you create a content delivery network (CDN) endpoint, you specify a scenario in hello **Optimized for** option.</span></span> <span data-ttu-id="1a597-108">Ditt val avgör vilka optimering är tillämpade toohello innehåll från hello CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="1a597-108">Your choice determines which optimization is applied toohello content delivered from hello CDN endpoint.</span></span>

<span data-ttu-id="1a597-109">Optimering alternativ är utformad toouse metodtips beteenden tooimprove innehållsleverans prestanda och bättre ursprung-avlastning.</span><span class="sxs-lookup"><span data-stu-id="1a597-109">Optimization choices are designed toouse best-practice behaviors tooimprove content delivery performance and better origin offload.</span></span> <span data-ttu-id="1a597-110">Dina val för scenariot påverkar prestanda genom att ändra konfigurationer för partiellt cachelagring, objektet högoptimerat och hello ursprung fel i principen.</span><span class="sxs-lookup"><span data-stu-id="1a597-110">Your scenario choices affect performance by modifying configurations for partial caching, object chunking, and hello origin failure retry policy.</span></span> 

<span data-ttu-id="1a597-111">Den här artikeln innehåller en översikt över olika optimering funktioner och när du ska använda dem.</span><span class="sxs-lookup"><span data-stu-id="1a597-111">This article provides an overview of various optimization features and when you should use them.</span></span> <span data-ttu-id="1a597-112">Mer information om funktioner och begränsningar finns i hello respektive artiklar på varje typ av enskilda optimering.</span><span class="sxs-lookup"><span data-stu-id="1a597-112">For more information on features and limitations, see hello respective articles on each individual optimization type.</span></span>

> [!NOTE]
> <span data-ttu-id="1a597-113">Din **optimerade för** alternativen kan variera beroende på hello-provider som du väljer.</span><span class="sxs-lookup"><span data-stu-id="1a597-113">Your **Optimized for** options can vary based on hello provider you select.</span></span> <span data-ttu-id="1a597-114">CDN-providers gäller förbättring på olika sätt beroende på hello scenario.</span><span class="sxs-lookup"><span data-stu-id="1a597-114">CDN providers apply enhancement in different ways, depending on hello scenario.</span></span> 

## <a name="provider-options"></a><span data-ttu-id="1a597-115">Providern alternativ</span><span class="sxs-lookup"><span data-stu-id="1a597-115">Provider options</span></span>

<span data-ttu-id="1a597-116">hello Azure Content Delivery Network från Akamai stöder:</span><span class="sxs-lookup"><span data-stu-id="1a597-116">hello Azure Content Delivery Network from Akamai supports:</span></span>

* <span data-ttu-id="1a597-117">Allmän web leverans</span><span class="sxs-lookup"><span data-stu-id="1a597-117">General web delivery</span></span> 

* <span data-ttu-id="1a597-118">Allmän direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="1a597-118">General media streaming</span></span>

* <span data-ttu-id="1a597-119">Video-on-demand-direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="1a597-119">Video-on-demand media streaming</span></span>

* <span data-ttu-id="1a597-120">Hämtning av stora filer</span><span class="sxs-lookup"><span data-stu-id="1a597-120">Large file download</span></span>

* <span data-ttu-id="1a597-121">Dynamiska acceleration</span><span class="sxs-lookup"><span data-stu-id="1a597-121">Dynamic site acceleration</span></span> 

<span data-ttu-id="1a597-122">hello Azure Content Delivery Network från Verizon stöder endast Internet-leverans.</span><span class="sxs-lookup"><span data-stu-id="1a597-122">hello Azure Content Delivery Network from Verizon supports general web delivery only.</span></span> <span data-ttu-id="1a597-123">Det kan användas för video på begäran och hämtning av stora filer.</span><span class="sxs-lookup"><span data-stu-id="1a597-123">It can be used for video on demand and large file download.</span></span> <span data-ttu-id="1a597-124">Du har inte tooselect en typ av optimering.</span><span class="sxs-lookup"><span data-stu-id="1a597-124">You don't have tooselect an optimization type.</span></span>

<span data-ttu-id="1a597-125">Vi rekommenderar starkt att du testar variationer mellan olika providrar tooselect hello optimala provider för din leverans.</span><span class="sxs-lookup"><span data-stu-id="1a597-125">We highly recommend that you test performance variations between different providers tooselect hello optimal provider for your delivery.</span></span>

## <a name="select-and-configure-optimization-types"></a><span data-ttu-id="1a597-126">Välj och konfigurera optimering typer</span><span class="sxs-lookup"><span data-stu-id="1a597-126">Select and configure optimization types</span></span>

<span data-ttu-id="1a597-127">toocreate en ny slutpunkt, Välj en optimering-typ som bäst passar hello scenario och typ av innehåll som du vill hello endpoint toodeliver.</span><span class="sxs-lookup"><span data-stu-id="1a597-127">toocreate a new endpoint, select an optimization type that best matches hello scenario and type of content that you want hello endpoint toodeliver.</span></span> <span data-ttu-id="1a597-128">**Allmän webben** är hello standardvalet.</span><span class="sxs-lookup"><span data-stu-id="1a597-128">**General web delivery** is hello default selection.</span></span> <span data-ttu-id="1a597-129">Du kan uppdatera hello optimeringsalternativ för alla befintliga Akamai slutpunkten när som helst.</span><span class="sxs-lookup"><span data-stu-id="1a597-129">You can update hello optimization option for any existing Akamai endpoint at any time.</span></span> <span data-ttu-id="1a597-130">Den här ändringen avbryta inte leverans från hello CDN.</span><span class="sxs-lookup"><span data-stu-id="1a597-130">This change doesn't interrupt delivery from hello CDN.</span></span> 

1. <span data-ttu-id="1a597-131">Välj en slutpunkt i en profil för Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="1a597-131">Select an endpoint within a Standard Akamai profile.</span></span>

    ![<span data-ttu-id="1a597-132">Val av slutpunkten</span><span class="sxs-lookup"><span data-stu-id="1a597-132">Endpoint selection</span></span> ](./media/cdn-optimization-overview/01_Akamai.png)

2. <span data-ttu-id="1a597-133">Under **inställningar**väljer **optimering**.</span><span class="sxs-lookup"><span data-stu-id="1a597-133">Under **SETTINGS**, select **Optimization**.</span></span> <span data-ttu-id="1a597-134">Välj en typ från hello **optimerade för** listrutan.</span><span class="sxs-lookup"><span data-stu-id="1a597-134">Then select a type from hello **Optimized for** drop-down list.</span></span>

    ![Optimering och typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a><span data-ttu-id="1a597-136">Optimering för specifika scenarier</span><span class="sxs-lookup"><span data-stu-id="1a597-136">Optimization for specific scenarios</span></span>

<span data-ttu-id="1a597-137">Du kan optimera hello CDN-slutpunkten för en av följande scenarier hello.</span><span class="sxs-lookup"><span data-stu-id="1a597-137">You can optimize hello CDN endpoint for one of hello following scenarios.</span></span> 

### <a name="general-web-delivery"></a><span data-ttu-id="1a597-138">Allmän web leverans</span><span class="sxs-lookup"><span data-stu-id="1a597-138">General web delivery</span></span>

<span data-ttu-id="1a597-139">Allmän webben är hello vanligaste optimeringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="1a597-139">General web delivery is hello most common optimization option.</span></span> <span data-ttu-id="1a597-140">Den är avsedd för allmän web content optimering, till exempel webbplatser och webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1a597-140">It's designed for general web content optimization, such as webpages and web applications.</span></span> <span data-ttu-id="1a597-141">Denna optimering kan också användas för filen och hämtar video.</span><span class="sxs-lookup"><span data-stu-id="1a597-141">This optimization also can be used for file and video downloads.</span></span>

<span data-ttu-id="1a597-142">En typisk webbplats innehåller statiska och dynamiska innehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-142">A typical website contains static and dynamic content.</span></span> <span data-ttu-id="1a597-143">Statiskt innehåll innehåller bilder, JavaScript-bibliotek och formatmallar som kan cachelagras och leverera toodifferent användare.</span><span class="sxs-lookup"><span data-stu-id="1a597-143">Static content includes images, JavaScript libraries, and style sheets that can be cached and delivered toodifferent users.</span></span> <span data-ttu-id="1a597-144">Dynamiskt innehåll anpassas för en enskild användare, t.ex. artiklar som är skräddarsydda tooa användarprofil.</span><span class="sxs-lookup"><span data-stu-id="1a597-144">Dynamic content is personalized for an individual user, such as news items that are tailored tooa user profile.</span></span> <span data-ttu-id="1a597-145">Dynamiskt innehåll cachelagras inte eftersom den är unik tooeach användare, till exempel handla kundvagn innehållet.</span><span class="sxs-lookup"><span data-stu-id="1a597-145">Dynamic content isn't cached because it's unique tooeach user, such as shopping cart contents.</span></span> <span data-ttu-id="1a597-146">Allmän web leverans kan optimera hela webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="1a597-146">General web delivery can optimize your entire website.</span></span> 

> [!NOTE]
> <span data-ttu-id="1a597-147">Om du använder hello Azure Content Delivery Network från Akamai kanske du vill toouse denna optimering om genomsnittliga filstorleken är mindre än 10 MB.</span><span class="sxs-lookup"><span data-stu-id="1a597-147">If you use hello Azure Content Delivery Network from Akamai, you might want toouse this optimization if your average file size is smaller than 10 MB.</span></span> <span data-ttu-id="1a597-148">Om din Genomsnittlig filstorlek är större än 10 MB väljer **stora Filhämtning** från hello **optimerade för** listrutan.</span><span class="sxs-lookup"><span data-stu-id="1a597-148">If your average file size is larger than 10 MB, select **Large file download** from hello **Optimized for** drop-down list.</span></span>

### <a name="general-media-streaming"></a><span data-ttu-id="1a597-149">Allmän direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="1a597-149">General media streaming</span></span>

<span data-ttu-id="1a597-150">Om du behöver toouse hello slutpunkten för direktsänd strömning och video-on-demand strömning rekommenderar vi Allmänt direktuppspelning optimering.</span><span class="sxs-lookup"><span data-stu-id="1a597-150">If you need toouse hello endpoint for live streaming and video-on-demand streaming, we recommend general media streaming optimization.</span></span>

<span data-ttu-id="1a597-151">Direktuppspelning är skiftlägeskänslig, tid eftersom paket som anländer sent på hello klient kan orsaka en försämrad visa upplevelse, till exempel ofta buffring av videoinnehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-151">Media streaming is time sensitive, because packets that arrive late on hello client can cause a degraded viewing experience, such as frequent buffering of video content.</span></span> <span data-ttu-id="1a597-152">Direktuppspelning optimering minskar hello svarstiden för leverans av innehåll för medier och ger en smooth streaming-upplevelse för användare.</span><span class="sxs-lookup"><span data-stu-id="1a597-152">Media streaming optimization reduces hello latency of media content delivery and provides a smooth streaming experience for users.</span></span> 

<span data-ttu-id="1a597-153">Det här scenariot är gemensamt för Azure media-kunder.</span><span class="sxs-lookup"><span data-stu-id="1a597-153">This scenario is common for Azure media service customers.</span></span> <span data-ttu-id="1a597-154">När du använder Azure media services kan få du en strömmande slutpunkt som kan användas för strömning live och på begäran.</span><span class="sxs-lookup"><span data-stu-id="1a597-154">When you use Azure media services, you get one streaming endpoint that can be used for both live and on-demand streaming.</span></span> <span data-ttu-id="1a597-155">Med det här scenariot behöver kunder inte tooswitch tooanother slutpunkt när de ändrar från strömning live tooon-begäran.</span><span class="sxs-lookup"><span data-stu-id="1a597-155">With this scenario, customers don't need tooswitch tooanother endpoint when they change from live tooon-demand streaming.</span></span> <span data-ttu-id="1a597-156">Allmän media strömmande optimering har stöd för den här typen av scenario.</span><span class="sxs-lookup"><span data-stu-id="1a597-156">General media streaming optimization supports this type of scenario.</span></span>

<span data-ttu-id="1a597-157">hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-157">hello Azure Content Delivery Network from Verizon uses hello general web delivery optimization type toodeliver streaming media content.</span></span>

<span data-ttu-id="1a597-158">toolearn mer om direktuppspelning optimering, se [direktuppspelning optimering](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="1a597-158">toolearn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

### <a name="video-on-demand-media-streaming"></a><span data-ttu-id="1a597-159">Video-on-demand-direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="1a597-159">Video-on-demand media streaming</span></span>

<span data-ttu-id="1a597-160">Video-on-demand media strömmande optimering förbättrar video-on-demand liveströmmat innehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-160">Video-on-demand media streaming optimization improves video-on-demand streaming content.</span></span> <span data-ttu-id="1a597-161">Om du använder en slutpunkt för video-on-demand strömning kanske du vill toouse det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="1a597-161">If you use an endpoint for video-on-demand streaming, you might want toouse this option.</span></span>

<span data-ttu-id="1a597-162">hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-162">hello Azure Content Delivery Network from Verizon uses hello general web delivery optimization type toodeliver streaming media content.</span></span>

<span data-ttu-id="1a597-163">toolearn mer om direktuppspelning optimering, se [direktuppspelning optimering](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="1a597-163">toolearn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

> [!NOTE]
> <span data-ttu-id="1a597-164">Om hello slutpunkt har främst video-on-demand-innehåll, Använd den här typen av optimering.</span><span class="sxs-lookup"><span data-stu-id="1a597-164">If hello endpoint primarily serves video-on-demand content, use this optimization type.</span></span> <span data-ttu-id="1a597-165">hello största skillnaden mellan denna optimering och hello allmänna media strömmande optimering är hello gör timeout-värde. hello timeout är mycket kortare toowork med direktsänd strömning scenarier.</span><span class="sxs-lookup"><span data-stu-id="1a597-165">hello major difference between this optimization and hello general media streaming optimization is hello connection retry time-out. hello time-out is much shorter toowork with live streaming scenarios.</span></span>

### <a name="large-file-download"></a><span data-ttu-id="1a597-166">Hämtning av stora filer</span><span class="sxs-lookup"><span data-stu-id="1a597-166">Large file download</span></span>

<span data-ttu-id="1a597-167">Om du använder hello Azure Content Delivery Network från Akamai, måste du använda en stor fil download toodeliver filer som är större än 1,8 GB.</span><span class="sxs-lookup"><span data-stu-id="1a597-167">If you use hello Azure Content Delivery Network from Akamai, you must use large file download toodeliver files larger than 1.8 GB.</span></span> <span data-ttu-id="1a597-168">hello Azure Content Delivery Network från Verizon har inte en begränsning på filen filstorlek i dess allmänna web leveransoptimering.</span><span class="sxs-lookup"><span data-stu-id="1a597-168">hello Azure Content Delivery Network from Verizon doesn't have a limitation on file download size in its general web delivery optimization.</span></span>

<span data-ttu-id="1a597-169">Om du använder hello Azure innehåll Delivery Network från Akamai är hämtning av stora filer optimerade för innehåll som är större än 10 MB.</span><span class="sxs-lookup"><span data-stu-id="1a597-169">If you use hello Azure Content Delivery Network from Akamai, large file downloads are optimized for content larger than 10 MB.</span></span> <span data-ttu-id="1a597-170">Om din genomsnittlig storlek är mindre än 10 MB, kanske du vill toouse allmänna web leverans.</span><span class="sxs-lookup"><span data-stu-id="1a597-170">If your average file size is smaller than 10 MB, you might want toouse general web delivery.</span></span> <span data-ttu-id="1a597-171">Om din Genomsnittlig filstorlek är konsekvent större än 10 MB, kan det vara effektivare toocreate en separat slutpunkt för stora filer.</span><span class="sxs-lookup"><span data-stu-id="1a597-171">If your average files sizes are consistently larger than 10 MB, it might be more efficient toocreate a separate endpoint for large files.</span></span> <span data-ttu-id="1a597-172">Inbyggd programvara eller programuppdateringar normalt exempelvis stora filer.</span><span class="sxs-lookup"><span data-stu-id="1a597-172">For example, firmware or software updates typically are large files.</span></span>

<span data-ttu-id="1a597-173">hello Azure innehåll Delivery Network från Verizon använder hello allmänna leverans optimering typen toodeliver strömmande media webbinnehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-173">hello Azure Content Delivery Network from Verizon uses hello general web delivery optimization type toodeliver streaming media content.</span></span>

<span data-ttu-id="1a597-174">toolearn mer om optimering av stora filer, se [optimering för stora filer](cdn-large-file-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="1a597-174">toolearn more about large file optimization, see [Large file optimization](cdn-large-file-optimization.md).</span></span>

### <a name="dynamic-site-acceleration"></a><span data-ttu-id="1a597-175">Dynamiska acceleration</span><span class="sxs-lookup"><span data-stu-id="1a597-175">Dynamic site acceleration</span></span>

 <span data-ttu-id="1a597-176">Dynamiska acceleration är tillgänglig från både Akamai och Verizon innehållsleveransnätverk profiler.</span><span class="sxs-lookup"><span data-stu-id="1a597-176">Dynamic site acceleration is available from both Akamai and Verizon Content Delivery Network profiles.</span></span> <span data-ttu-id="1a597-177">Denna optimering innebär en avgift toouse.</span><span class="sxs-lookup"><span data-stu-id="1a597-177">This optimization involves an additional fee toouse.</span></span> <span data-ttu-id="1a597-178">Mer information finns i hello sida med priser.</span><span class="sxs-lookup"><span data-stu-id="1a597-178">For more information, see hello pricing page.</span></span>

<span data-ttu-id="1a597-179">Dynamiska acceleration innehåller olika tekniker som omfattas hello svarstid och prestanda för dynamiskt innehåll.</span><span class="sxs-lookup"><span data-stu-id="1a597-179">Dynamic site acceleration includes various techniques that benefit hello latency and performance of dynamic content.</span></span> <span data-ttu-id="1a597-180">Tekniken omfattar flödes- och optimering, optimering för TCP med mera.</span><span class="sxs-lookup"><span data-stu-id="1a597-180">Techniques include route and network optimization, TCP optimization, and more.</span></span> 

<span data-ttu-id="1a597-181">Du kan använda den här optimering tooaccelerate en webbapp som innehåller flera svar som inte är Cacheable ställs.</span><span class="sxs-lookup"><span data-stu-id="1a597-181">You can use this optimization tooaccelerate a web app that includes numerous responses that aren't cacheable.</span></span> <span data-ttu-id="1a597-182">Exempel är sökresultat, utcheckningen transaktioner eller realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="1a597-182">Examples are search results, checkout transactions, or real-time data.</span></span> <span data-ttu-id="1a597-183">Du kan fortsätta toouse CDN cache grundfunktionerna för statiska data.</span><span class="sxs-lookup"><span data-stu-id="1a597-183">You can continue toouse core CDN caching capabilities for static data.</span></span> 



