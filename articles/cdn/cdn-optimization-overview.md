---
title: "Optimera Azure content delivery för ditt scenario"
description: "Hur du optimerar leverans av ditt innehåll för specifika scenarier"
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
ms.openlocfilehash: 98941c49b057380b3ef9164515bcc2a63ccb56ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-azure-content-delivery-for-your-scenario"></a><span data-ttu-id="924f6-103">Optimera Azure content delivery för ditt scenario</span><span class="sxs-lookup"><span data-stu-id="924f6-103">Optimize Azure content delivery for your scenario</span></span>

<span data-ttu-id="924f6-104">När du levererar innehåll till en stor global målgrupp är det viktigt att kontrollera optimerade leveransen av ditt innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-104">When you deliver content to a large global audience, it's critical to ensure the optimized delivery of your content.</span></span> <span data-ttu-id="924f6-105">Azure innehåll Delivery Network kan optimera leverans upplevelsen baserat på typ av innehåll som du har.</span><span class="sxs-lookup"><span data-stu-id="924f6-105">The Azure Content Delivery Network can optimize the delivery experience based on the type of content you have.</span></span> <span data-ttu-id="924f6-106">Innehållet kan vara en webbplats, en direktsänd dataström, en video eller en stor fil för hämtning.</span><span class="sxs-lookup"><span data-stu-id="924f6-106">Content can be a website, a live stream, a video, or a large file for download.</span></span> <span data-ttu-id="924f6-107">När du skapar en slutpunkt för content delivery network (CDN) måste du ange ett scenario i den **optimerade för** alternativet.</span><span class="sxs-lookup"><span data-stu-id="924f6-107">When you create a content delivery network (CDN) endpoint, you specify a scenario in the **Optimized for** option.</span></span> <span data-ttu-id="924f6-108">Ditt val avgör vilka optimering tillämpas på innehåll från CDN-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="924f6-108">Your choice determines which optimization is applied to the content delivered from the CDN endpoint.</span></span>

<span data-ttu-id="924f6-109">Optimering alternativ är utformade för att använda metodtips beteenden för att förbättra prestanda för leverans av innehåll och bättre ursprung-avlastning.</span><span class="sxs-lookup"><span data-stu-id="924f6-109">Optimization choices are designed to use best-practice behaviors to improve content delivery performance and better origin offload.</span></span> <span data-ttu-id="924f6-110">Dina val för scenariot påverkar prestanda genom att ändra konfigurationer för partiellt cachelagring, objektet högoptimerat och försök felprincipen ursprung.</span><span class="sxs-lookup"><span data-stu-id="924f6-110">Your scenario choices affect performance by modifying configurations for partial caching, object chunking, and the origin failure retry policy.</span></span> 

<span data-ttu-id="924f6-111">Den här artikeln innehåller en översikt över olika optimering funktioner och när du ska använda dem.</span><span class="sxs-lookup"><span data-stu-id="924f6-111">This article provides an overview of various optimization features and when you should use them.</span></span> <span data-ttu-id="924f6-112">Mer information om funktioner och begränsningar finns i respektive artiklar på varje typ av enskilda optimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-112">For more information on features and limitations, see the respective articles on each individual optimization type.</span></span>

> [!NOTE]
> <span data-ttu-id="924f6-113">Din **optimerade för** alternativen kan variera beroende på den provider du väljer.</span><span class="sxs-lookup"><span data-stu-id="924f6-113">Your **Optimized for** options can vary based on the provider you select.</span></span> <span data-ttu-id="924f6-114">CDN-providers gäller förbättring på olika sätt beroende på scenario.</span><span class="sxs-lookup"><span data-stu-id="924f6-114">CDN providers apply enhancement in different ways, depending on the scenario.</span></span> 

## <a name="provider-options"></a><span data-ttu-id="924f6-115">Providern alternativ</span><span class="sxs-lookup"><span data-stu-id="924f6-115">Provider options</span></span>

<span data-ttu-id="924f6-116">Azure Content Delivery Network från Akamai stöder:</span><span class="sxs-lookup"><span data-stu-id="924f6-116">The Azure Content Delivery Network from Akamai supports:</span></span>

* <span data-ttu-id="924f6-117">Allmän web leverans</span><span class="sxs-lookup"><span data-stu-id="924f6-117">General web delivery</span></span> 

* <span data-ttu-id="924f6-118">Allmän direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="924f6-118">General media streaming</span></span>

* <span data-ttu-id="924f6-119">Video-on-demand-direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="924f6-119">Video-on-demand media streaming</span></span>

* <span data-ttu-id="924f6-120">Hämtning av stora filer</span><span class="sxs-lookup"><span data-stu-id="924f6-120">Large file download</span></span>

* <span data-ttu-id="924f6-121">Dynamiska acceleration</span><span class="sxs-lookup"><span data-stu-id="924f6-121">Dynamic site acceleration</span></span> 

<span data-ttu-id="924f6-122">Azure Content Delivery Network från Verizon stöder endast Internet-leverans.</span><span class="sxs-lookup"><span data-stu-id="924f6-122">The Azure Content Delivery Network from Verizon supports general web delivery only.</span></span> <span data-ttu-id="924f6-123">Det kan användas för video på begäran och hämtning av stora filer.</span><span class="sxs-lookup"><span data-stu-id="924f6-123">It can be used for video on demand and large file download.</span></span> <span data-ttu-id="924f6-124">Du behöver inte ange optimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-124">You don't have to select an optimization type.</span></span>

<span data-ttu-id="924f6-125">Vi rekommenderar starkt att du testar variationer mellan olika providrar för att välja den optimala providern för leveransen.</span><span class="sxs-lookup"><span data-stu-id="924f6-125">We highly recommend that you test performance variations between different providers to select the optimal provider for your delivery.</span></span>

## <a name="select-and-configure-optimization-types"></a><span data-ttu-id="924f6-126">Välj och konfigurera optimering typer</span><span class="sxs-lookup"><span data-stu-id="924f6-126">Select and configure optimization types</span></span>

<span data-ttu-id="924f6-127">Välj en typ av optimering som bäst passar scenariot och typ av innehåll som du vill att leverera slutpunkten för att skapa en ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="924f6-127">To create a new endpoint, select an optimization type that best matches the scenario and type of content that you want the endpoint to deliver.</span></span> <span data-ttu-id="924f6-128">**Allmän webben** är standardvalet.</span><span class="sxs-lookup"><span data-stu-id="924f6-128">**General web delivery** is the default selection.</span></span> <span data-ttu-id="924f6-129">Du kan uppdatera alternativet optimering för alla befintliga Akamai slutpunkten när som helst.</span><span class="sxs-lookup"><span data-stu-id="924f6-129">You can update the optimization option for any existing Akamai endpoint at any time.</span></span> <span data-ttu-id="924f6-130">Den här ändringen avbryta inte leverans från CDN.</span><span class="sxs-lookup"><span data-stu-id="924f6-130">This change doesn't interrupt delivery from the CDN.</span></span> 

1. <span data-ttu-id="924f6-131">Välj en slutpunkt i en profil för Standard Akamai.</span><span class="sxs-lookup"><span data-stu-id="924f6-131">Select an endpoint within a Standard Akamai profile.</span></span>

    ![<span data-ttu-id="924f6-132">Val av slutpunkten</span><span class="sxs-lookup"><span data-stu-id="924f6-132">Endpoint selection</span></span> ](./media/cdn-optimization-overview/01_Akamai.png)

2. <span data-ttu-id="924f6-133">Under **inställningar**väljer **optimering**.</span><span class="sxs-lookup"><span data-stu-id="924f6-133">Under **SETTINGS**, select **Optimization**.</span></span> <span data-ttu-id="924f6-134">Välj en typ från den **optimerade för** listrutan.</span><span class="sxs-lookup"><span data-stu-id="924f6-134">Then select a type from the **Optimized for** drop-down list.</span></span>

    ![Optimering och typ](./media/cdn-optimization-overview/02_Select.png)

## <a name="optimization-for-specific-scenarios"></a><span data-ttu-id="924f6-136">Optimering för specifika scenarier</span><span class="sxs-lookup"><span data-stu-id="924f6-136">Optimization for specific scenarios</span></span>

<span data-ttu-id="924f6-137">Du kan optimera CDN-slutpunkten för en av följande scenarier.</span><span class="sxs-lookup"><span data-stu-id="924f6-137">You can optimize the CDN endpoint for one of the following scenarios.</span></span> 

### <a name="general-web-delivery"></a><span data-ttu-id="924f6-138">Allmän web leverans</span><span class="sxs-lookup"><span data-stu-id="924f6-138">General web delivery</span></span>

<span data-ttu-id="924f6-139">Allmän webben är alternativet vanligaste optimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-139">General web delivery is the most common optimization option.</span></span> <span data-ttu-id="924f6-140">Den är avsedd för allmän web content optimering, till exempel webbplatser och webbprogram.</span><span class="sxs-lookup"><span data-stu-id="924f6-140">It's designed for general web content optimization, such as webpages and web applications.</span></span> <span data-ttu-id="924f6-141">Denna optimering kan också användas för filen och hämtar video.</span><span class="sxs-lookup"><span data-stu-id="924f6-141">This optimization also can be used for file and video downloads.</span></span>

<span data-ttu-id="924f6-142">En typisk webbplats innehåller statiska och dynamiska innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-142">A typical website contains static and dynamic content.</span></span> <span data-ttu-id="924f6-143">Statiskt innehåll innehåller bilder, JavaScript-bibliotek och formatmallar som kan cachelagras och levereras till olika användare.</span><span class="sxs-lookup"><span data-stu-id="924f6-143">Static content includes images, JavaScript libraries, and style sheets that can be cached and delivered to different users.</span></span> <span data-ttu-id="924f6-144">Dynamiskt innehåll anpassas för en enskild användare, till exempel artiklar som skräddarsys efter en användarprofil.</span><span class="sxs-lookup"><span data-stu-id="924f6-144">Dynamic content is personalized for an individual user, such as news items that are tailored to a user profile.</span></span> <span data-ttu-id="924f6-145">Dynamiskt innehåll är inte cachelagras på grund av det är unikt för varje användare, till exempel i kundvagn innehållet.</span><span class="sxs-lookup"><span data-stu-id="924f6-145">Dynamic content isn't cached because it's unique to each user, such as shopping cart contents.</span></span> <span data-ttu-id="924f6-146">Allmän web leverans kan optimera hela webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="924f6-146">General web delivery can optimize your entire website.</span></span> 

> [!NOTE]
> <span data-ttu-id="924f6-147">Om du använder Azure Content Delivery Network från Akamai kanske du vill använda denna optimering om genomsnittliga filstorleken är mindre än 10 MB.</span><span class="sxs-lookup"><span data-stu-id="924f6-147">If you use the Azure Content Delivery Network from Akamai, you might want to use this optimization if your average file size is smaller than 10 MB.</span></span> <span data-ttu-id="924f6-148">Om din Genomsnittlig filstorlek är större än 10 MB väljer **stora Filhämtning** från den **optimerade för** listrutan.</span><span class="sxs-lookup"><span data-stu-id="924f6-148">If your average file size is larger than 10 MB, select **Large file download** from the **Optimized for** drop-down list.</span></span>

### <a name="general-media-streaming"></a><span data-ttu-id="924f6-149">Allmän direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="924f6-149">General media streaming</span></span>

<span data-ttu-id="924f6-150">Om du behöver använda slutpunkten för direktsänd strömning och video-on-demand strömning rekommenderar vi allmänna direktuppspelning optimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-150">If you need to use the endpoint for live streaming and video-on-demand streaming, we recommend general media streaming optimization.</span></span>

<span data-ttu-id="924f6-151">Direktuppspelning är skiftlägeskänslig, tid eftersom paket som anländer sent på klienten kan orsaka en försämrad visa upplevelse, till exempel ofta buffring av videoinnehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-151">Media streaming is time sensitive, because packets that arrive late on the client can cause a degraded viewing experience, such as frequent buffering of video content.</span></span> <span data-ttu-id="924f6-152">Direktuppspelning optimering minskar svarstiden för leverans av innehåll för medier och ger en smooth streaming-upplevelse för användare.</span><span class="sxs-lookup"><span data-stu-id="924f6-152">Media streaming optimization reduces the latency of media content delivery and provides a smooth streaming experience for users.</span></span> 

<span data-ttu-id="924f6-153">Det här scenariot är gemensamt för Azure media-kunder.</span><span class="sxs-lookup"><span data-stu-id="924f6-153">This scenario is common for Azure media service customers.</span></span> <span data-ttu-id="924f6-154">När du använder Azure media services kan få du en strömmande slutpunkt som kan användas för strömning live och på begäran.</span><span class="sxs-lookup"><span data-stu-id="924f6-154">When you use Azure media services, you get one streaming endpoint that can be used for both live and on-demand streaming.</span></span> <span data-ttu-id="924f6-155">Med det här scenariot behöver kunder inte växla till en annan slutpunkt när de ändras från aktiv till strömning på begäran.</span><span class="sxs-lookup"><span data-stu-id="924f6-155">With this scenario, customers don't need to switch to another endpoint when they change from live to on-demand streaming.</span></span> <span data-ttu-id="924f6-156">Allmän media strömmande optimering har stöd för den här typen av scenario.</span><span class="sxs-lookup"><span data-stu-id="924f6-156">General media streaming optimization supports this type of scenario.</span></span>

<span data-ttu-id="924f6-157">Azure innehåll Delivery Network från Verizon använder Internet optimering Leveranstyp för att leverera strömmande media-innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-157">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="924f6-158">Läs mer om direktuppspelning optimering i [direktuppspelning optimering](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="924f6-158">To learn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

### <a name="video-on-demand-media-streaming"></a><span data-ttu-id="924f6-159">Video-on-demand-direktuppspelning</span><span class="sxs-lookup"><span data-stu-id="924f6-159">Video-on-demand media streaming</span></span>

<span data-ttu-id="924f6-160">Video-on-demand media strömmande optimering förbättrar video-on-demand liveströmmat innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-160">Video-on-demand media streaming optimization improves video-on-demand streaming content.</span></span> <span data-ttu-id="924f6-161">Om du använder en slutpunkt för video-on-demand strömning kanske du vill använda det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="924f6-161">If you use an endpoint for video-on-demand streaming, you might want to use this option.</span></span>

<span data-ttu-id="924f6-162">Azure innehåll Delivery Network från Verizon använder Internet optimering Leveranstyp för att leverera strömmande media-innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-162">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="924f6-163">Läs mer om direktuppspelning optimering i [direktuppspelning optimering](cdn-media-streaming-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="924f6-163">To learn more about media streaming optimization, see [Media streaming optimization](cdn-media-streaming-optimization.md).</span></span>

> [!NOTE]
> <span data-ttu-id="924f6-164">Om slutpunkten används främst för video-on-demand-innehåll, kan du använda den här typen av optimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-164">If the endpoint primarily serves video-on-demand content, use this optimization type.</span></span> <span data-ttu-id="924f6-165">Den största skillnaden mellan denna optimering och allmänna optimering för direktuppspelning är gör timeout.</span><span class="sxs-lookup"><span data-stu-id="924f6-165">The major difference between this optimization and the general media streaming optimization is the connection retry time-out.</span></span> <span data-ttu-id="924f6-166">Tidsgränsen är mycket kortare du arbetar med direktsänd strömning scenarier.</span><span class="sxs-lookup"><span data-stu-id="924f6-166">The time-out is much shorter to work with live streaming scenarios.</span></span>

### <a name="large-file-download"></a><span data-ttu-id="924f6-167">Hämtning av stora filer</span><span class="sxs-lookup"><span data-stu-id="924f6-167">Large file download</span></span>

<span data-ttu-id="924f6-168">Om du använder Azure Content Delivery Network från Akamai, måste du använda stora Filhämtning för att leverera filer som är större än 1,8 GB.</span><span class="sxs-lookup"><span data-stu-id="924f6-168">If you use the Azure Content Delivery Network from Akamai, you must use large file download to deliver files larger than 1.8 GB.</span></span> <span data-ttu-id="924f6-169">Azure Content Delivery Network från Verizon har inte en begränsning på filen filstorlek i dess allmänna web leveransoptimering.</span><span class="sxs-lookup"><span data-stu-id="924f6-169">The Azure Content Delivery Network from Verizon doesn't have a limitation on file download size in its general web delivery optimization.</span></span>

<span data-ttu-id="924f6-170">Om du använder Azure innehåll Delivery Network från Akamai är hämtning av stora filer optimerade för innehåll som är större än 10 MB.</span><span class="sxs-lookup"><span data-stu-id="924f6-170">If you use the Azure Content Delivery Network from Akamai, large file downloads are optimized for content larger than 10 MB.</span></span> <span data-ttu-id="924f6-171">Om din genomsnittlig storlek är mindre än 10 MB kanske du vill använda allmänna web leverans.</span><span class="sxs-lookup"><span data-stu-id="924f6-171">If your average file size is smaller than 10 MB, you might want to use general web delivery.</span></span> <span data-ttu-id="924f6-172">Om din Genomsnittlig filstorlek är konsekvent större än 10 MB, kan det vara mer effektivt att skapa en separat slutpunkt för stora filer.</span><span class="sxs-lookup"><span data-stu-id="924f6-172">If your average files sizes are consistently larger than 10 MB, it might be more efficient to create a separate endpoint for large files.</span></span> <span data-ttu-id="924f6-173">Inbyggd programvara eller programuppdateringar normalt exempelvis stora filer.</span><span class="sxs-lookup"><span data-stu-id="924f6-173">For example, firmware or software updates typically are large files.</span></span>

<span data-ttu-id="924f6-174">Azure innehåll Delivery Network från Verizon använder Internet optimering Leveranstyp för att leverera strömmande media-innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-174">The Azure Content Delivery Network from Verizon uses the general web delivery optimization type to deliver streaming media content.</span></span>

<span data-ttu-id="924f6-175">Läs mer om optimering för stora filer i [optimering för stora filer](cdn-large-file-optimization.md).</span><span class="sxs-lookup"><span data-stu-id="924f6-175">To learn more about large file optimization, see [Large file optimization](cdn-large-file-optimization.md).</span></span>

### <a name="dynamic-site-acceleration"></a><span data-ttu-id="924f6-176">Dynamiska acceleration</span><span class="sxs-lookup"><span data-stu-id="924f6-176">Dynamic site acceleration</span></span>

 <span data-ttu-id="924f6-177">Dynamiska acceleration är tillgänglig från både Akamai och Verizon innehållsleveransnätverk profiler.</span><span class="sxs-lookup"><span data-stu-id="924f6-177">Dynamic site acceleration is available from both Akamai and Verizon Content Delivery Network profiles.</span></span> <span data-ttu-id="924f6-178">Denna optimering innebär en avgift som ska användas.</span><span class="sxs-lookup"><span data-stu-id="924f6-178">This optimization involves an additional fee to use.</span></span> <span data-ttu-id="924f6-179">Se prissättningssidan för mer information.</span><span class="sxs-lookup"><span data-stu-id="924f6-179">For more information, see the pricing page.</span></span>

<span data-ttu-id="924f6-180">Dynamiska acceleration innehåller olika tekniker som omfattas svarstid och prestanda för dynamiskt innehåll.</span><span class="sxs-lookup"><span data-stu-id="924f6-180">Dynamic site acceleration includes various techniques that benefit the latency and performance of dynamic content.</span></span> <span data-ttu-id="924f6-181">Tekniken omfattar flödes- och optimering, optimering för TCP med mera.</span><span class="sxs-lookup"><span data-stu-id="924f6-181">Techniques include route and network optimization, TCP optimization, and more.</span></span> 

<span data-ttu-id="924f6-182">Du kan använda denna optimering för att påskynda en webbapp som innehåller flera svar som inte är Cacheable ställs.</span><span class="sxs-lookup"><span data-stu-id="924f6-182">You can use this optimization to accelerate a web app that includes numerous responses that aren't cacheable.</span></span> <span data-ttu-id="924f6-183">Exempel är sökresultat, utcheckningen transaktioner eller realtidsdata.</span><span class="sxs-lookup"><span data-stu-id="924f6-183">Examples are search results, checkout transactions, or real-time data.</span></span> <span data-ttu-id="924f6-184">Du kan fortsätta att använda cache grundfunktionerna för CDN för statiska data.</span><span class="sxs-lookup"><span data-stu-id="924f6-184">You can continue to use core CDN caching capabilities for static data.</span></span> 



