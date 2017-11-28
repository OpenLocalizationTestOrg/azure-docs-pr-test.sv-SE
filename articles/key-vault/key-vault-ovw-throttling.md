---
ms.assetid: 
title: "aaaAzure Key Vault bandbreddsbegränsning vägledning | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 06/21/2017
ms.openlocfilehash: a75cf96bc6503e51f14378bee598bad57e85be82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-throttling-guidance"></a><span data-ttu-id="c4cac-102">Azure Key Vault begränsning vägledning</span><span class="sxs-lookup"><span data-stu-id="c4cac-102">Azure Key Vault throttling guidance</span></span>

<span data-ttu-id="c4cac-103">Begränsning är en process som du har initierat som begränsar hello antal samtidiga anrop toohello Azure-tjänsten tooprevent felaktig användning av resurser.</span><span class="sxs-lookup"><span data-stu-id="c4cac-103">Throttling is a process you initiate that limits hello number of concurrent calls toohello Azure service tooprevent overuse of resources.</span></span> <span data-ttu-id="c4cac-104">Azure Key Vault (AKV) är utformad toohandle en stor mängd begäranden.</span><span class="sxs-lookup"><span data-stu-id="c4cac-104">Azure Key Vault (AKV) is designed toohandle a high volume of requests.</span></span> <span data-ttu-id="c4cac-105">Om det inträffar ett överväldigande antal begäranden, upprätthåller begränsning din klientbegäranden optimala prestanda och tillförlitlighet för hello AKV-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4cac-105">If an overwhelming number of requests occurs, throttling your client's requests helps maintain optimal performance and reliability of hello AKV service.</span></span>

<span data-ttu-id="c4cac-106">Bandbreddsbegränsning gränser variera beroende på hello scenario.</span><span class="sxs-lookup"><span data-stu-id="c4cac-106">Throttling limits vary based on hello scenario.</span></span> <span data-ttu-id="c4cac-107">Till exempel om du utför ett stort antal skrivningar hello möjlighet för begränsning är högre än om du bara utför läsningar.</span><span class="sxs-lookup"><span data-stu-id="c4cac-107">For example, if you are performing a large volume of writes, hello possibility for throttling is higher than if you are only performing reads.</span></span>

## <a name="how-does-key-vault-handle-its-limits"></a><span data-ttu-id="c4cac-108">Hur hanterar Key Vault gränsen?</span><span class="sxs-lookup"><span data-stu-id="c4cac-108">How does Key Vault handle its limits?</span></span>

<span data-ttu-id="c4cac-109">Tjänstbegränsningarna i Key Vault finns det tooprevent missbruk av resurser och kontrollera Tjänstkvalitet för alla Key Vault-klienter.</span><span class="sxs-lookup"><span data-stu-id="c4cac-109">Service limits in Key Vault are there tooprevent misuse of resources and ensure quality of service for all of Key Vault’s clients.</span></span> <span data-ttu-id="c4cac-110">När ett tröskelvärde för tjänsten har överskridits begränsar Key Vault ytterligare förfrågningar från klienten för en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="c4cac-110">When a service threshold is exceeded, Key Vault limits any further requests from that client for a period of time.</span></span> <span data-ttu-id="c4cac-111">När det händer gör Key Vault returnerar HTTP-statuskod 429 (för många begäranden), och hello begär misslyckas.</span><span class="sxs-lookup"><span data-stu-id="c4cac-111">When this happens, Key Vault returns HTTP status code 429 (Too many requests), and hello requests fail.</span></span> <span data-ttu-id="c4cac-112">Dessutom misslyckade begäranden som 429 antal mot hello begränsning gränser spåras av Key Vault.</span><span class="sxs-lookup"><span data-stu-id="c4cac-112">Also, failed requests that return a 429 count towards hello throttle limits tracked by Key Vault.</span></span> 

<span data-ttu-id="c4cac-113">Om du har ett giltigt företag ärende för högre begränsning gränser, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="c4cac-113">If you have a valid business case for higher throttle limits, please contact us.</span></span>


## <a name="how-toothrottle-your-app-in-response-tooservice-limits"></a><span data-ttu-id="c4cac-114">Hur toothrottle din app i svaret tooservice begränsar</span><span class="sxs-lookup"><span data-stu-id="c4cac-114">How toothrottle your app in response tooservice limits</span></span>

<span data-ttu-id="c4cac-115">hello följande är **metodtips** för begränsning av din app:</span><span class="sxs-lookup"><span data-stu-id="c4cac-115">hello following are **best practices** for throttling your app:</span></span>
- <span data-ttu-id="c4cac-116">Minska hello antal åtgärder per begäran.</span><span class="sxs-lookup"><span data-stu-id="c4cac-116">Reduce hello number of operations per request.</span></span>
- <span data-ttu-id="c4cac-117">Minska hello frekvensen för begäranden.</span><span class="sxs-lookup"><span data-stu-id="c4cac-117">Reduce hello frequency of requests.</span></span>
- <span data-ttu-id="c4cac-118">Undvik omedelbara försök.</span><span class="sxs-lookup"><span data-stu-id="c4cac-118">Avoid immediate retries.</span></span> 
    - <span data-ttu-id="c4cac-119">Alla begäranden som görs mot gränserna för Resursanvändning.</span><span class="sxs-lookup"><span data-stu-id="c4cac-119">All requests accrue against your usage limits.</span></span>

<span data-ttu-id="c4cac-120">När du implementerar din app felhantering måste använda hello HTTP felkod 429 toodetect hello för klientsidan begränsning.</span><span class="sxs-lookup"><span data-stu-id="c4cac-120">When you implement your app's error handling, use hello HTTP error code 429 toodetect hello need for client-side throttling.</span></span> <span data-ttu-id="c4cac-121">Om hello begäran misslyckas igen med en HTTP-429 felkod, det fortfarande uppstår en gräns för Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c4cac-121">If hello request fails again with an HTTP 429 error code, you are still encountering an Azure service limit.</span></span> <span data-ttu-id="c4cac-122">Fortsätt toouse hello rekommenderad klientsidan bandbreddsbegränsning metod, som du försöker hello begäran tills den lyckas.</span><span class="sxs-lookup"><span data-stu-id="c4cac-122">Continue toouse hello recommended client-side throttling method, retrying hello request until it succeeds.</span></span>

### <a name="recommended-client-side-throttling-method"></a><span data-ttu-id="c4cac-123">Rekommenderad metod för klientsidan bandbreddsbegränsning</span><span class="sxs-lookup"><span data-stu-id="c4cac-123">Recommended client-side throttling method</span></span>

<span data-ttu-id="c4cac-124">HTTP-felkod: 429 börja begränsning klienten med en exponentiell backoff-metoden:</span><span class="sxs-lookup"><span data-stu-id="c4cac-124">On HTTP error code 429, begin throttling your client using an exponential backoff approach:</span></span>

1. <span data-ttu-id="c4cac-125">Vänta 1 sekund, försök igen</span><span class="sxs-lookup"><span data-stu-id="c4cac-125">Wait 1 second, retry request</span></span>
2. <span data-ttu-id="c4cac-126">Om du fortfarande begränsas vänta 2 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="c4cac-126">If still throttled wait 2 seconds, retry request</span></span>
3. <span data-ttu-id="c4cac-127">Om du fortfarande begränsas vänta 4 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="c4cac-127">If still throttled wait 4 seconds, retry request</span></span>
4. <span data-ttu-id="c4cac-128">Om du fortfarande begränsas vänta 8 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="c4cac-128">If still throttled wait 8 seconds, retry request</span></span>
5. <span data-ttu-id="c4cac-129">Om du fortfarande begränsas vänta 16 sekunder, försök igen med begäran</span><span class="sxs-lookup"><span data-stu-id="c4cac-129">If still throttled wait 16 seconds, retry request</span></span>

<span data-ttu-id="c4cac-130">Nu bör du inte att hämta HTTP 429 svarskoder.</span><span class="sxs-lookup"><span data-stu-id="c4cac-130">At this point, you should not be getting HTTP 429 response codes.</span></span>

## <a name="see-also"></a><span data-ttu-id="c4cac-131">Se även</span><span class="sxs-lookup"><span data-stu-id="c4cac-131">See also</span></span>

<span data-ttu-id="c4cac-132">En djupare orientering med begränsningen på hello Microsoft Cloud finns [begränsning mönster](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span><span class="sxs-lookup"><span data-stu-id="c4cac-132">For a deeper orientation of throttling on hello Microsoft Cloud, see [Throttling Pattern](https://docs.microsoft.com/azure/architecture/patterns/throttling).</span></span>

