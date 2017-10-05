---
ms.assetid: 
title: "Azure Key Vault begränsning vägledning | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: fe700e22c5323c2a0bdc315e349cd119798bcf40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="868a5-102">Azure Key Vault begränsning vägledning</span><span class="sxs-lookup"><span data-stu-id="868a5-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="868a5-103">Begränsning är en process som du har initierat som begränsar antalet samtidiga anrop till Azure-tjänsten för att förhindra felaktig användning av resurser.</span><span class="sxs-lookup"><span data-stu-id="868a5-103">Throttling is a process you initiate that limits the number of concurrent calls to the Azure service to prevent overuse of resources.</span></span> <span data-ttu-id="868a5-104">Azure Key Vault (AKV) är utformad för att hantera ett stort antal begäranden.</span><span class="sxs-lookup"><span data-stu-id="868a5-104">Azure Key Vault (AKV) is designed to handle a high volume of requests.</span></span> <span data-ttu-id="868a5-105">Om det inträffar ett överväldigande antal begäranden, upprätthåller begränsning din klientbegäranden optimala prestanda och tillförlitlighet för AKV-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="868a5-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of the AKV service.</span></span>

<span data-ttu-id="868a5-106">Bandbreddsbegränsning gränser variera beroende på scenario.</span><span class="sxs-lookup"><span data-stu-id="868a5-106">Throttling limits vary based on the scenario.</span></span> <span data-ttu-id="868a5-107">Om du utför ett stort antal skrivningar är till exempel möjligheten för begränsning av högre än om du bara utför läsningar.</span><span class="sxs-lookup"><span data-stu-id="868a5-107">For example, if you are performing a large volume of writes, the possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="868a5-108">Hur hanterar Key Vault gränsen?</span><span class="sxs-lookup"><span data-stu-id="868a5-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="868a5-109">Tjänstbegränsningarna i Key Vault finns det att förhindra missbruk av resurser och säkerställa Tjänstkvalitet för alla Key Vault-klienter.</span><span class="sxs-lookup"><span data-stu-id="868a5-109">Service limits in Key Vault are there to prevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="868a5-110">När ett tröskelvärde för tjänsten har överskridits begränsar Key Vault ytterligare förfrågningar från klienten för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="868a5-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="868a5-111">När det händer gör Key Vault returnerar HTTP-statuskod 429 (för många begäranden), och begäranden att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="868a5-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and the requests fail.</span></span> <span data-ttu-id="868a5-112">Dessutom misslyckade begäranden som 429 antal mot begränsningen gränserna spåras av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="868a5-112">Also, failed requests that return a 429 count towards the throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="868a5-113">Om du har ett giltigt företag ärende för högre begränsning gränser, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="868a5-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a><span data-ttu-id="868a5-114">Hur du begränsar din app som svar på tjänstbegränsningarna</span><span class="sxs-lookup"><span data-stu-id="868a5-114">How to throttle your app in response to service limits</span></span>

<span data-ttu-id="868a5-115">Följande är **metodtips** för begränsning av din app:</span><span class="sxs-lookup"><span data-stu-id="868a5-115">The following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="868a5-116">Minska antalet åtgärder per begäran.</span><span class="sxs-lookup"><span data-stu-id="868a5-116">Reduce the number of operations per request.</span></span>
- <span data-ttu-id="868a5-117">Minska frekvensen av begäranden.</span><span class="sxs-lookup"><span data-stu-id="868a5-117">Reduce the frequency of requests.</span></span>
- <span data-ttu-id="868a5-118">Undvik omedelbara försök.</span><span class="sxs-lookup"><span data-stu-id="868a5-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="868a5-119">Alla begäranden som görs mot gränserna för Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="868a5-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="868a5-120">När du implementerar din app felhantering, Använd HTTP-felkod 429 för att identifiera behovet av att klientens begränsning.</span><span class="sxs-lookup"><span data-stu-id="868a5-120">When you implement your app's error handling, use the HTTP error code 429 to detect the need for client-side throttling.</span></span> <span data-ttu-id="868a5-121">Om begäran misslyckas igen med en HTTP-429 felkod, det fortfarande uppstår en gräns för Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="868a5-121">If the request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="868a5-122">Fortsätta att använda den rekommenderade klientsidan begränsning metod, försök tills den lyckas.</span><span class="sxs-lookup"><span data-stu-id="868a5-122">Continue to use the recommended client-side throttling method, retrying the request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="868a5-123">Rekommenderad metod för klientsidan bandbreddsbegränsning</span><span class="sxs-lookup"><span data-stu-id="868a5-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="868a5-124">HTTP-felkod: 429 börja begränsning klienten med en exponentiell backoff-metoden:</span><span class="sxs-lookup"><span data-stu-id="868a5-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="868a5-125">Vänta 1 sekund, försök igen</span><span class="sxs-lookup"><span data-stu-id="868a5-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="868a5-126">Om du fortfarande begränsas vänta 2 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="868a5-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="868a5-127">Om du fortfarande begränsas vänta 4 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="868a5-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="868a5-128">Om du fortfarande begränsas vänta 8 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="868a5-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="868a5-129">Om du fortfarande begränsas vänta 16 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="868a5-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="868a5-130">Nu bör du inte att hämta HTTP 429 svarskoder.</span><span class="sxs-lookup"><span data-stu-id="868a5-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="868a5-131">Se även</span><span class="sxs-lookup"><span data-stu-id="868a5-131">See also</span></span>

<span data-ttu-id="868a5-132">En djupare orientering med begränsningen på Microsoft Cloud finns [begränsning mönster](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="868a5-132">For a deeper orientation of throttling on the Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

