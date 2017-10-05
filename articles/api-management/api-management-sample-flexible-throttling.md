---
title: "Avancerade begäran begränsning med Azure API Management"
description: "Lär dig hur du skapar och använder flexibla kvoten och begränsa principer med Azure API Management hastighet."
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 35375e599891a9443a91c4c3a8657e8c9c48c7b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="0a278-103">Avancerade begäran begränsning med Azure API Management</span><span class="sxs-lookup"><span data-stu-id="0a278-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="0a278-104">Att kunna begränsa inkommande begäranden är en viktig roll i Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="0a278-104">Being able to throttle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="0a278-105">Antingen genom att kontrollera antalet förfrågningar eller totalt antal begäranden/överförda data kan API Management-API-leverantörer att skydda sina API: er från missbruk och skapa värde för olika nivåer för API-produkten.</span><span class="sxs-lookup"><span data-stu-id="0a278-105">Either by controlling the rate of requests or the total requests/data transferred, API Management allows API providers to protect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="0a278-106">Produkten baserat begränsning</span><span class="sxs-lookup"><span data-stu-id="0a278-106">Product based throttling</span></span>
<span data-ttu-id="0a278-107">Hittills, hastighet begränsningen har funktioner begränsats till är begränsade till en viss produkt prenumeration (i praktiken en nyckel), definieras i API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="0a278-107">To date, the rate throttling capabilities have been limited to being scoped to a particular Product subscription (essentially a key), defined in the API Management publisher portal.</span></span> <span data-ttu-id="0a278-108">Detta är användbart för API-providern kan tillämpa begränsningar på utvecklare som har registrerat dig att använda sitt API, men det hjälper inte, till exempel i begränsning enskilda slutanvändare API.</span><span class="sxs-lookup"><span data-stu-id="0a278-108">This is useful for the API provider to apply limits on the developers who have signed up to use their API, however, it does not help, for example, in throttling individual end-users of the API.</span></span> <span data-ttu-id="0a278-109">Det är möjligt att för enkel användaren av utvecklarens program att använda hela kvoten och sedan förhindra andra kunder som utvecklare kan använda programmet.</span><span class="sxs-lookup"><span data-stu-id="0a278-109">It is possible that for single user of the developer's application to consume the entire quota and then prevent other customers of the developer from being able to use the application.</span></span> <span data-ttu-id="0a278-110">Flera kunder som kan generera en stor mängd begäranden kan också begränsa åtkomsten till tillfällig användare.</span><span class="sxs-lookup"><span data-stu-id="0a278-110">Also, several customers who might generate a high volume of requests may limit access to occasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="0a278-111">Anpassade nyckelbaserad begränsning</span><span class="sxs-lookup"><span data-stu-id="0a278-111">Custom key based throttling</span></span>
<span data-ttu-id="0a278-112">Den nya [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principerna förser dig med en betydligt mer flexibel lösning för att kontrollera trafik.</span><span class="sxs-lookup"><span data-stu-id="0a278-112">The new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution to traffic control.</span></span> <span data-ttu-id="0a278-113">Dessa nya principer kan du definiera uttryck för att identifiera de nycklar som används för att spåra användningen av trafik.</span><span class="sxs-lookup"><span data-stu-id="0a278-113">These new policies allow you to define expressions to identify the keys that will be used to track traffic usage.</span></span> <span data-ttu-id="0a278-114">Hur detta fungerar illustreras easiest med ett exempel.</span><span class="sxs-lookup"><span data-stu-id="0a278-114">The way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="0a278-115">Begränsning av IP-adress</span><span class="sxs-lookup"><span data-stu-id="0a278-115">IP Address throttling</span></span>
<span data-ttu-id="0a278-116">Följande principer begränsa en enskild klient-IP-adress till endast 10 anrop i minuten, med totalt 1 000 000 anrop och 10 000 kB bandbredd per månad.</span><span class="sxs-lookup"><span data-stu-id="0a278-116">The following policies restrict a single client IP address to only 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="0a278-117">Om en unik IP-adress används för alla klienter på Internet, kan det vara ett effektivt sätt för att begränsa användningen av användaren.</span><span class="sxs-lookup"><span data-stu-id="0a278-117">If all clients on the Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="0a278-118">Det är mycket troligt att flera användare att dela en offentlig IP-adress på grund av dem åtkomst till Internet via en NAT-enhet.</span><span class="sxs-lookup"><span data-stu-id="0a278-118">However, it is quite likely that multiple users will sharing a single public IP address due to them accessing the Internet via a NAT device.</span></span> <span data-ttu-id="0a278-119">Trots detta för API: er som tillåter obehörig åtkomst av `IpAddress` kan vara det bästa alternativet.</span><span class="sxs-lookup"><span data-stu-id="0a278-119">Despite this, for APIs that allow unauthenticated access the `IpAddress` might be the best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="0a278-120">Begränsning av identitet</span><span class="sxs-lookup"><span data-stu-id="0a278-120">User identity throttling</span></span>
<span data-ttu-id="0a278-121">Om en användare autentiseras och sedan en bandbreddsbegränsning nyckel kan genereras baserat på information som unikt identifierar en som användaren.</span><span class="sxs-lookup"><span data-stu-id="0a278-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="0a278-122">I det här exemplet vi extrahera Authorization-huvud, konvertera den till `JWT` objekt och använda föremål för token för att identifiera användaren och använda det som den hastighet som begränsar nyckel.</span><span class="sxs-lookup"><span data-stu-id="0a278-122">In this example we extract the Authorization header, convert it to `JWT` object and use the subject of the token to identify the user and use that as the rate limiting key.</span></span> <span data-ttu-id="0a278-123">Om användarens identitet lagras i den `JWT` som en av de andra anspråk sedan att värdet kan användas i dess ställe.</span><span class="sxs-lookup"><span data-stu-id="0a278-123">If the user identity is stored in the `JWT` as one of the other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="0a278-124">Kombinerade principer</span><span class="sxs-lookup"><span data-stu-id="0a278-124">Combined policies</span></span>
<span data-ttu-id="0a278-125">Även om de nya bandbreddsbegränsning principerna ger mer kontroll än de befintliga bandbreddsbegränsning principerna, finns det fortfarande värde kombinera båda funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a278-125">Although the new throttling policies provide more control than the existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="0a278-126">Begränsning av prenumeration produktnyckel ([gränsen anropet frekvensen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [kvot för uppsättningen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) är ett bra sätt att aktivera monetizing för en API genom att dra baserat på användning nivåer.</span><span class="sxs-lookup"><span data-stu-id="0a278-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way to enable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="0a278-127">Avancerat hjälpmedel metataggkontroll kontroll av att kunna begränsa av användare kompletterar och förhindrar att en användarbeteende försämring upplevelse av en annan.</span><span class="sxs-lookup"><span data-stu-id="0a278-127">The finer grained control of being able to throttle by user is complementary and prevents one user's behavior from degrading the experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="0a278-128">Klienten drivs begränsning</span><span class="sxs-lookup"><span data-stu-id="0a278-128">Client driven throttling</span></span>
<span data-ttu-id="0a278-129">När nyckeln bandbreddsbegränsning definieras med hjälp av en [principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx), så är det den API-provider som väljer hur begränsningen är begränsad.</span><span class="sxs-lookup"><span data-stu-id="0a278-129">When the throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is the API provider that is choosing how the throttling is scoped.</span></span> <span data-ttu-id="0a278-130">Men en utvecklare kanske vill styra hur de hastighetsbegränsning sina egna kunder.</span><span class="sxs-lookup"><span data-stu-id="0a278-130">However, a developer might want to control how they rate limit their own customers.</span></span> <span data-ttu-id="0a278-131">Detta kan aktiveras av API-providern genom att introducera en anpassad rubrik så att utvecklarens klientprogram att kommunicera nyckeln till API: et.</span><span class="sxs-lookup"><span data-stu-id="0a278-131">This could be enabled by the API provider by introducing a custom header to allow the developer's client application to communicate the key to the API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="0a278-132">Detta gör att utvecklarens klientprogram att välja hur de vill skapa den hastighet som begränsar nyckel.</span><span class="sxs-lookup"><span data-stu-id="0a278-132">This enables the developer's client application to choose how they want to create the rate limiting key.</span></span> <span data-ttu-id="0a278-133">Med lite uppfinningsrikedom kunde klienten utvecklare skapa sina egna hastighet nivåer genom att tilldela användare uppsättningar av nycklar och rotera nyckelanvändningen.</span><span class="sxs-lookup"><span data-stu-id="0a278-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys to users and rotating the key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="0a278-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0a278-134">Summary</span></span>
<span data-ttu-id="0a278-135">Azure API Management ger hastighet och citattecken begränsning för både skydd och Lägg till värde i API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0a278-135">Azure API Management provides rate and quote throttling to both protect and add value to your API service.</span></span> <span data-ttu-id="0a278-136">Nya bandbreddsbegränsning principer med anpassade reglerna Tillåt avancerat hjälpmedel metataggkontroll kontroll över dessa principer för att aktivera dina kunder att bygga program ännu bättre.</span><span class="sxs-lookup"><span data-stu-id="0a278-136">The new throttling policies with custom scoping rules allow you finer grained control over those policies to enable your customers to build even better applications.</span></span> <span data-ttu-id="0a278-137">Exemplen i den här artikeln visar användningen av dessa nya principer med tillverkning som begränsar nycklar med klienternas IP-adresser, användar-ID och klienten genererade värden.</span><span class="sxs-lookup"><span data-stu-id="0a278-137">The examples in this article demonstrate the use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="0a278-138">Det finns emellertid många andra delar av meddelandet som kan användas som användaragent, URL-sökväg fragment, meddelandestorlek.</span><span class="sxs-lookup"><span data-stu-id="0a278-138">However, there are many other parts of the message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a278-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a278-139">Next steps</span></span>
<span data-ttu-id="0a278-140">Ge oss din feedback i Disqus-tråden för det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a278-140">Please give us your feedback in the Disqus thread for this topic.</span></span> <span data-ttu-id="0a278-141">Det är bra att läsa om andra potentiella nyckelvärden som har ett logiskt val i dina scenarier.</span><span class="sxs-lookup"><span data-stu-id="0a278-141">It would be great to hear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="0a278-142">Titta på en videoöversikt över dessa principer</span><span class="sxs-lookup"><span data-stu-id="0a278-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="0a278-143">Mer information om den [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principer som beskrivs i den här artikeln får du titta på följande videon.</span><span class="sxs-lookup"><span data-stu-id="0a278-143">For more information on the [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch the following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

