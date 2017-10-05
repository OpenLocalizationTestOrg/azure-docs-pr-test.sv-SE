---
title: "HTTP-2-stöd i Azure CDN | Microsoft Docs"
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
ms.openlocfilehash: 4f8dd685c3ae89535217d7a17a01c5129ca7e6e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="http2-support-in-azure-cdn"></a><span data-ttu-id="499c6-103">HTTP-2-stöd i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="499c6-103">HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="499c6-104">HTTP-2 är en större ändring till HTTP/1.1\.</span><span class="sxs-lookup"><span data-stu-id="499c6-104">HTTP/2 is a major revision to HTTP/1.1\.</span></span> <span data-ttu-id="499c6-105">Det ger snabbare webbprestanda, minskade svarstid och ger en förbättrad upplevelse samtidigt bekant HTTP-metoderna, statuskoder och semantik.</span><span class="sxs-lookup"><span data-stu-id="499c6-105">It provides faster web performance, reduced response time, and improved user experience, while maintaining the familiar HTTP methods, status codes, and semantics.</span></span> <span data-ttu-id="499c6-106">Även om HTTP-2 är utformat för att arbeta med HTTP och HTTPS, stöder endast HTTP/2 via TLS många klientens webbläsare.</span><span class="sxs-lookup"><span data-stu-id="499c6-106">Though HTTP/2 is designed to work with HTTP and HTTPS, many client web browsers only support HTTP/2 over TLS.</span></span>

###<a name="http2-benefits"></a><span data-ttu-id="499c6-107">Fördelar med HTTP-2</span><span class="sxs-lookup"><span data-stu-id="499c6-107">HTTP/2 Benefits</span></span>

<span data-ttu-id="499c6-108">Fördelarna med HTTP/2 inkluderar:</span><span class="sxs-lookup"><span data-stu-id="499c6-108">The benefits of HTTP/2 include:</span></span>

*   <span data-ttu-id="499c6-109">**Multiplexering och samtidighet**</span><span class="sxs-lookup"><span data-stu-id="499c6-109">**Multiplexing and concurrency**</span></span>

    <span data-ttu-id="499c6-110">Med HTTP 1.1 flera gör flera förfrågningar för resursen kräver flera TCP-anslutningar och varje anslutning har prestanda försämras som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="499c6-110">Using HTTP 1.1, multiple making multiple resource requests requires multiple TCP connections, and each connection has performance overhead associated with it.</span></span> <span data-ttu-id="499c6-111">HTTP-2 kan flera resurser som krävs för en TCP-anslutning.</span><span class="sxs-lookup"><span data-stu-id="499c6-111">HTTP/2 allows multiple resources to be requested on a single TCP connection.</span></span>

*   <span data-ttu-id="499c6-112">**Komprimering av huvud**</span><span class="sxs-lookup"><span data-stu-id="499c6-112">**Header compression**</span></span>

    <span data-ttu-id="499c6-113">Genom att komprimera HTTP-huvuden för hanteras resurser minskas tiden på kabeln avsevärt.</span><span class="sxs-lookup"><span data-stu-id="499c6-113">By compressing the HTTP headers for served resources, time on the wire is reduced significantly.</span></span>

*   <span data-ttu-id="499c6-114">**Stream-beroenden**</span><span class="sxs-lookup"><span data-stu-id="499c6-114">**Stream dependencies**</span></span>

    <span data-ttu-id="499c6-115">Dataströmmen beroenden kan klienten för att ange att servern där resurser har prioritet.</span><span class="sxs-lookup"><span data-stu-id="499c6-115">Stream dependencies allow the client to indicate to the server which of resources have priority.</span></span>


##<a name="http2-browser-support"></a><span data-ttu-id="499c6-116">Stöd för HTTP-2-webbläsare</span><span class="sxs-lookup"><span data-stu-id="499c6-116">HTTP/2 Browser Support</span></span>

<span data-ttu-id="499c6-117">Alla större webbläsare har implementerat HTTP/2-stöd i aktuell version.</span><span class="sxs-lookup"><span data-stu-id="499c6-117">All of the major browsers have implemented HTTP/2 support in their current versions.</span></span> <span data-ttu-id="499c6-118">Webbläsare som inte stöds kommer den automatiskt reserv vid HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="499c6-118">Non-supported browsers will automatically fallback to HTTP/1.1.</span></span>

|<span data-ttu-id="499c6-119">Webbläsare</span><span class="sxs-lookup"><span data-stu-id="499c6-119">Browser</span></span>|<span data-ttu-id="499c6-120">Lägsta Version</span><span class="sxs-lookup"><span data-stu-id="499c6-120">Minimum Version</span></span>|
|-------------|------------|
|<span data-ttu-id="499c6-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="499c6-121">Microsoft Edge</span></span>| <span data-ttu-id="499c6-122">12</span><span class="sxs-lookup"><span data-stu-id="499c6-122">12</span></span>|
|<span data-ttu-id="499c6-123">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="499c6-123">Google Chrome</span></span>| <span data-ttu-id="499c6-124">43</span><span class="sxs-lookup"><span data-stu-id="499c6-124">43</span></span>|
|<span data-ttu-id="499c6-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="499c6-125">Mozilla Firefox</span></span>| <span data-ttu-id="499c6-126">38</span><span class="sxs-lookup"><span data-stu-id="499c6-126">38</span></span>|
|<span data-ttu-id="499c6-127">Opera</span><span class="sxs-lookup"><span data-stu-id="499c6-127">Opera</span></span>| <span data-ttu-id="499c6-128">32</span><span class="sxs-lookup"><span data-stu-id="499c6-128">32</span></span>|
|<span data-ttu-id="499c6-129">Safari</span><span class="sxs-lookup"><span data-stu-id="499c6-129">Safari</span></span>| <span data-ttu-id="499c6-130">9</span><span class="sxs-lookup"><span data-stu-id="499c6-130">9</span></span>|

##<a name="enabling-http2-support-in-azure-cdn"></a><span data-ttu-id="499c6-131">Aktivera HTTP-2-stöd i Azure CDN</span><span class="sxs-lookup"><span data-stu-id="499c6-131">Enabling HTTP/2 Support in Azure CDN</span></span>

<span data-ttu-id="499c6-132">För närvarande stöd för HTTP-2 är aktiv för **Azure CDN från Akamai** och **Azure CDN från Verizon** profiler.</span><span class="sxs-lookup"><span data-stu-id="499c6-132">Currently HTTP/2 support is active for **Azure CDN from Akamai** and **Azure CDN from Verizon** profiles.</span></span> <span data-ttu-id="499c6-133">Ingen ytterligare åtgärd krävs från kunder.</span><span class="sxs-lookup"><span data-stu-id="499c6-133">No further action is required from customers.</span></span>

##<a name="next-steps"></a><span data-ttu-id="499c6-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="499c6-134">Next Steps</span></span>

<span data-ttu-id="499c6-135">Fördelarna med HTTP/2 i praktiken finns [i den här demon från Akamai](https://http2.akamai.com/demo).</span><span class="sxs-lookup"><span data-stu-id="499c6-135">To see the benefits of HTTP/2 in action, see [this demo from Akamai](https://http2.akamai.com/demo).</span></span>

<span data-ttu-id="499c6-136">Mer information om HTTP-2 finns i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="499c6-136">To learn more about HTTP/2, visit the following resources:</span></span>

*   [<span data-ttu-id="499c6-137">HTTP-2-specifikationen webbsida</span><span class="sxs-lookup"><span data-stu-id="499c6-137">HTTP/2 specification homepage</span></span>](https://http2.github.io/)
*   [<span data-ttu-id="499c6-138">Officiell HTTP/2 vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="499c6-138">Official HTTP/2 FAQ</span></span>](https://http2.github.io/faq/)
*   [<span data-ttu-id="499c6-139">Akamai HTTP/2-information</span><span class="sxs-lookup"><span data-stu-id="499c6-139">Akamai HTTP/2 information</span></span>](https://http2.akamai.com/)

<span data-ttu-id="499c6-140">Mer information om tillgängliga funktioner i Azure CDN finns i [översikt över Azure CDN](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span><span class="sxs-lookup"><span data-stu-id="499c6-140">To learn more about Azure CDN's available features, see the [Azure CDN Overview](https://azure.microsoft.com/documentation/articles/cdn-overview/).</span></span>