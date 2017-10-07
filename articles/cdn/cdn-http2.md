---
title: "aaaHTTP/2-stöd i Azure CDN | Microsoft Docs"
description: "Läs mer om stöd för HTTP-2 och CDN."
services: cdn
documentationcenter: 
author: lichard
manager: erikre
editor: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/04/2017
ms.author: rli
ms.openlocfilehash: 2e5e5345e8cf5c40e080ebf18b4f13a239a5aac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="998d4-103">HTTP-2-stöd i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="998d4-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="998d4-104">HTTP-2 är en större ändring tooHTTP/1.1\.</span><span class="sxs-lookup"><span data-stu-id="998d4-104">HTTP/2 is a major revision tooHTTP/1.1\.</span></span> <span data-ttu-id="998d4-105">Det ger snabbare webbprestanda, minskade svarstid och ger en förbättrad upplevelse samtidigt hello bekant HTTP-metoderna och statuskoder semantik.</span><span class="sxs-lookup"><span data-stu-id="998d4-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining hello familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="998d4-106">Även om HTTP-2 är utformad toowork med HTTP och HTTPS, stöder endast HTTP/2 via TLS många klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="998d4-106">Though HTTP/2 is designed toowork with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="998d4-107">Fördelar med HTTP-2</span><span class="sxs-lookup"><span data-stu-id="998d4-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="998d4-108">hello fördelar HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="998d4-108">hello benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="998d4-109">**Multiplexering och samtidighet**</span><span class="sxs-lookup"><span data-stu-id="998d4-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="998d4-110">Med HTTP 1.1 flera gör flera förfrågningar för resursen kräver flera TCP-anslutningar och varje anslutning har prestanda försämras som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="998d4-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="998d4-111">HTTP-2 kan flera resurser toobe efterfrågas på en enda TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="998d4-111">HTTP/2 allows multiple resources toobe requested on a single TCP connection.</span></span>

*   <span data-ttu-id="998d4-112">**Komprimering av huvud**</span><span class="sxs-lookup"><span data-stu-id="998d4-112">**Header compression**</span></span>

    <span data-ttu-id="998d4-113">Genom att komprimera hello HTTP-huvuden för hanteras resurser minskas tid hello överföring avsevärt.</span><span class="sxs-lookup"><span data-stu-id="998d4-113">By compressing hello HTTP headers for served resources, time on hello wire is reduced significantly.</span></span>

*   <span data-ttu-id="998d4-114">**Stream-beroenden**</span><span class="sxs-lookup"><span data-stu-id="998d4-114">**Stream dependencies**</span></span>

    <span data-ttu-id="998d4-115">Dataströmmen beroenden kan hello klienten tooindicate toohello server där resurser som har prioritet.</span><span class="sxs-lookup"><span data-stu-id="998d4-115">Stream dependencies allow hello client tooindicate toohello server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="998d4-116">Stöd för HTTP-2-webbläsare</span><span class="sxs-lookup"><span data-stu-id="998d4-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="998d4-117">Alla större webbläsare för hello har implementerat HTTP/2-stöd i aktuell version.</span><span class="sxs-lookup"><span data-stu-id="998d4-117">All of hello major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="998d4-118">Webbläsare som inte stöds kommer automatiskt återställningsplats tooHTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="998d4-118">Non-supported browsers will automatically fallback tooHTTP/1.1.</span></span>

|<span data-ttu-id="998d4-119">Webbläsare</span><span class="sxs-lookup"><span data-stu-id="998d4-119">Browser</span></span>|<span data-ttu-id="998d4-120">Lägsta Version</span><span class="sxs-lookup"><span data-stu-id="998d4-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="998d4-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="998d4-121">Microsoft Edge</span></span>| <span data-ttu-id="998d4-122">12</span><span class="sxs-lookup"><span data-stu-id="998d4-122">12</span></span>|
|<span data-ttu-id="998d4-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="998d4-123">Google Chrome</span></span>| <span data-ttu-id="998d4-124">43</span><span class="sxs-lookup"><span data-stu-id="998d4-124">43</span></span>|
|<span data-ttu-id="998d4-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="998d4-125">Mozilla Firefox</span></span>| <span data-ttu-id="998d4-126">38</span><span class="sxs-lookup"><span data-stu-id="998d4-126">38</span></span>|
|<span data-ttu-id="998d4-127">Opera</span><span class="sxs-lookup"><span data-stu-id="998d4-127">Opera</span></span>| <span data-ttu-id="998d4-128">32</span><span class="sxs-lookup"><span data-stu-id="998d4-128">32</span></span>|
|<span data-ttu-id="998d4-129">Safari</span><span class="sxs-lookup"><span data-stu-id="998d4-129">Safari</span></span>| <span data-ttu-id="998d4-130">9</span><span class="sxs-lookup"><span data-stu-id="998d4-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="998d4-131">Aktivera HTTP-2-stöd i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="998d4-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="998d4-132">För närvarande stöd för HTTP-2 är aktiv för **Azure CDN från Akamai** och **Azure CDN från Verizon** profiler.</span><span class="sxs-lookup"><span data-stu-id="998d4-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="998d4-133">Ingen ytterligare åtgärd krävs från kunder.</span><span class="sxs-lookup"><span data-stu-id="998d4-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="998d4-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="998d4-134">Next Steps</span></span>

<span data-ttu-id="998d4-135">toosee hello fördelarna med HTTP/2 i åtgärden, se [i den här demon från Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="998d4-135">toosee hello benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="998d4-136">toolearn mer information om HTTP-2 finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="998d4-136">toolearn more about HTTP/2, visit hello following resources:</span></span>

*   [<span data-ttu-id="998d4-137">HTTP-2-specifikationen webbsida</span><span class="sxs-lookup"><span data-stu-id="998d4-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="998d4-138">Officiell HTTP/2 vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="998d4-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="998d4-139">Akamai HTTP/2-information</span><span class="sxs-lookup"><span data-stu-id="998d4-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="998d4-140">toolearn mer om Azure CDN tillgängliga funktioner, se hello [översikt över Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="998d4-140">toolearn more about Azure CDN's available features, see hello [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>
