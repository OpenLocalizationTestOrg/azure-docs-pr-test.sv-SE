---
title: "aaaAdvanced begäran begränsning med Azure API Management"
description: "Lär dig hur toocreate och tillämpa flexibla kvoten och begränsa principer med Azure API Management hastighet."
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
ms.openlocfilehash: ac87f83118a37bd587fddf044e5c2d6fc2af9031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a><span data-ttu-id="fa108-103">Avancerade begäran begränsning med Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fa108-103">Advanced request throttling with Azure API Management</span></span>
<span data-ttu-id="fa108-104">Att kunna toothrottle inkommande begäranden är en viktig roll i Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="fa108-104">Being able toothrottle incoming requests is a key role of Azure API Management.</span></span> <span data-ttu-id="fa108-105">Antingen genom att kontrollera hello andel begäranden eller hello Totalt antal begäranden/data överförs API Management gör API providers tooprotect sina API: er från missbruk och skapa värde för olika nivåer för API-produkten.</span><span class="sxs-lookup"><span data-stu-id="fa108-105">Either by controlling hello rate of requests or hello total requests/data transferred, API Management allows API providers tooprotect their APIs from abuse and create value for different API product tiers.</span></span>

## <a name="product-based-throttling"></a><span data-ttu-id="fa108-106">Produkten baserat begränsning</span><span class="sxs-lookup"><span data-stu-id="fa108-106">Product based throttling</span></span>
<span data-ttu-id="fa108-107">toodate hello hastighet kapacitet för begränsning har begränsad toobeing omfång tooa viss produkt prenumeration (i praktiken en nyckel), definieras i hello API Management publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa108-107">toodate, hello rate throttling capabilities have been limited toobeing scoped tooa particular Product subscription (essentially a key), defined in hello API Management publisher portal.</span></span> <span data-ttu-id="fa108-108">Detta är användbart för hello API providern tooapply gränser för hello utvecklare som har registrerat dig toouse sitt API, men det hjälper inte, till exempel i begränsning enskilda slutanvändare av hello API.</span><span class="sxs-lookup"><span data-stu-id="fa108-108">This is useful for hello API provider tooapply limits on hello developers who have signed up toouse their API, however, it does not help, for example, in throttling individual end-users of hello API.</span></span> <span data-ttu-id="fa108-109">Det är möjligt att för enskild användare av hello developer program tooconsume hello hela kvoten och sedan förhindra andra hello developer-kunder kan toouse hello program.</span><span class="sxs-lookup"><span data-stu-id="fa108-109">It is possible that for single user of hello developer's application tooconsume hello entire quota and then prevent other customers of hello developer from being able toouse hello application.</span></span> <span data-ttu-id="fa108-110">Flera kunder som kan generera en stor mängd begäranden kan också begränsa åtkomst toooccasional användare.</span><span class="sxs-lookup"><span data-stu-id="fa108-110">Also, several customers who might generate a high volume of requests may limit access toooccasional users.</span></span>

## <a name="custom-key-based-throttling"></a><span data-ttu-id="fa108-111">Anpassade nyckelbaserad begränsning</span><span class="sxs-lookup"><span data-stu-id="fa108-111">Custom key based throttling</span></span>
<span data-ttu-id="fa108-112">hello nya [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principerna ger ett betydligt mer flexibel lösning tootraffic kontroll.</span><span class="sxs-lookup"><span data-stu-id="fa108-112">hello new [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies provide a significantly more flexible solution tootraffic control.</span></span> <span data-ttu-id="fa108-113">Dessa nya principer gör toodefine uttryck tooidentify hello nycklar som kommer att använda tootrack trafik användning.</span><span class="sxs-lookup"><span data-stu-id="fa108-113">These new policies allow you toodefine expressions tooidentify hello keys that will be used tootrack traffic usage.</span></span> <span data-ttu-id="fa108-114">Hej hur detta fungerar illustreras easiest med ett exempel.</span><span class="sxs-lookup"><span data-stu-id="fa108-114">hello way this works is easiest illustrated with an example.</span></span> 

## <a name="ip-address-throttling"></a><span data-ttu-id="fa108-115">Begränsning av IP-adress</span><span class="sxs-lookup"><span data-stu-id="fa108-115">IP Address throttling</span></span>
<span data-ttu-id="fa108-116">hello följande principer begränsa en enskild klient IP-adress tooonly 10 anrop i minuten, med totalt 1 000 000 samtal och 10 000 kB bandbredd per månad.</span><span class="sxs-lookup"><span data-stu-id="fa108-116">hello following policies restrict a single client IP address tooonly 10 calls every minute, with a total of 1,000,000 calls and 10,000 kilobytes of bandwidth per month.</span></span> 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

<span data-ttu-id="fa108-117">Om alla klienter i hello Internet används en unik IP-adress, kan det vara ett effektivt sätt för att begränsa användningen av användaren.</span><span class="sxs-lookup"><span data-stu-id="fa108-117">If all clients on hello Internet used a unique IP address, this might be an effective way of limiting usage by user.</span></span> <span data-ttu-id="fa108-118">Det är mycket troligt att flera användare att dela en offentlig IP-adress på grund av toothem använder hello Internet via en NAT-enhet.</span><span class="sxs-lookup"><span data-stu-id="fa108-118">However, it is quite likely that multiple users will sharing a single public IP address due toothem accessing hello Internet via a NAT device.</span></span> <span data-ttu-id="fa108-119">Trots detta för API: er som tillåter obehörig åtkomst hello `IpAddress` kanske hello bästa alternativet.</span><span class="sxs-lookup"><span data-stu-id="fa108-119">Despite this, for APIs that allow unauthenticated access hello `IpAddress` might be hello best option.</span></span>

## <a name="user-identity-throttling"></a><span data-ttu-id="fa108-120">Begränsning av identitet</span><span class="sxs-lookup"><span data-stu-id="fa108-120">User identity throttling</span></span>
<span data-ttu-id="fa108-121">Om en användare autentiseras och sedan en bandbreddsbegränsning nyckel kan genereras baserat på information som unikt identifierar en som användaren.</span><span class="sxs-lookup"><span data-stu-id="fa108-121">If an end user is authenticated then a throttling key can be generated based on information that uniquely identifies an that user.</span></span>

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

<span data-ttu-id="fa108-122">I det här exemplet vi extrahera hello Authorization-huvud, konvertera den för`JWT` objekt och använda hello ämnet för hello token tooidentify hello användare och använda det som hello hastighet begränsa nyckel.</span><span class="sxs-lookup"><span data-stu-id="fa108-122">In this example we extract hello Authorization header, convert it too`JWT` object and use hello subject of hello token tooidentify hello user and use that as hello rate limiting key.</span></span> <span data-ttu-id="fa108-123">Om hello användaridentitet lagras i hello `JWT` som en hello andra anspråk sedan att värdet kan användas i dess ställe.</span><span class="sxs-lookup"><span data-stu-id="fa108-123">If hello user identity is stored in hello `JWT` as one of hello other claims then that value could be used in its place.</span></span>

## <a name="combined-policies"></a><span data-ttu-id="fa108-124">Kombinerade principer</span><span class="sxs-lookup"><span data-stu-id="fa108-124">Combined policies</span></span>
<span data-ttu-id="fa108-125">Hello nya principer för begränsning av ger mer kontroll än hello befintliga bandbreddsbegränsning principer, finns det fortfarande värde kombinera båda funktioner.</span><span class="sxs-lookup"><span data-stu-id="fa108-125">Although hello new throttling policies provide more control than hello existing throttling policies, there is still value combining both capabilities.</span></span> <span data-ttu-id="fa108-126">Begränsning av prenumeration produktnyckel ([gränsen anropet frekvensen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [kvot för uppsättningen av prenumerationen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) är ett bra sätt tooenable monetizing för en API genom att dra baserat på användning nivåer.</span><span class="sxs-lookup"><span data-stu-id="fa108-126">Throttling by product subscription key ([Limit call rate by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota by subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is a great way tooenable monetizing of an API by charging based on usage levels.</span></span> <span data-ttu-id="fa108-127">hello avancerat hjälpmedel metataggkontroll kontroll av att kunna toothrottle av användare kompletterar och förhindrar att en användarbeteende försämring hello upplevelse av en annan.</span><span class="sxs-lookup"><span data-stu-id="fa108-127">hello finer grained control of being able toothrottle by user is complementary and prevents one user's behavior from degrading hello experience of another.</span></span> 

## <a name="client-driven-throttling"></a><span data-ttu-id="fa108-128">Klienten drivs begränsning</span><span class="sxs-lookup"><span data-stu-id="fa108-128">Client driven throttling</span></span>
<span data-ttu-id="fa108-129">När hello begränsning nyckel definieras med hjälp av en [principuttryck](https://msdn.microsoft.com/library/azure/dn910913.aspx), så är det hello API-leverantör som väljer hur hello begränsning är begränsad.</span><span class="sxs-lookup"><span data-stu-id="fa108-129">When hello throttling key is defined using a [policy expression](https://msdn.microsoft.com/library/azure/dn910913.aspx), then it is hello API provider that is choosing how hello throttling is scoped.</span></span> <span data-ttu-id="fa108-130">Dock vill kanske en utvecklare toocontrol hur de betygsätta begränsa sina kunder.</span><span class="sxs-lookup"><span data-stu-id="fa108-130">However, a developer might want toocontrol how they rate limit their own customers.</span></span> <span data-ttu-id="fa108-131">Detta kan aktiveras av hello API-providern genom att introducera en anpassad rubrik tooallow hello utvecklarens klienten programmet toocommunicate hello viktiga toohello API.</span><span class="sxs-lookup"><span data-stu-id="fa108-131">This could be enabled by hello API provider by introducing a custom header tooallow hello developer's client application toocommunicate hello key toohello API.</span></span>

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

<span data-ttu-id="fa108-132">Detta gör att hello developer klienten programmet toochoose hur de vill toocreate hello hastighet begränsa nyckel.</span><span class="sxs-lookup"><span data-stu-id="fa108-132">This enables hello developer's client application toochoose how they want toocreate hello rate limiting key.</span></span> <span data-ttu-id="fa108-133">Med lite uppfinningsrikedom kunde klienten utvecklare skapa sina egna hastighet nivåer genom att allokera uppsättningar av nycklar toousers och rotera hello nyckelanvändning.</span><span class="sxs-lookup"><span data-stu-id="fa108-133">With a little bit of ingenuity a client developer could create their own rate tiers by allocating sets of keys toousers and rotating hello key usage.</span></span>

## <a name="summary"></a><span data-ttu-id="fa108-134">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="fa108-134">Summary</span></span>
<span data-ttu-id="fa108-135">Azure API Management ger hastighet och citattecken begränsning tooboth skydda och lägga till värdet tooyour API-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fa108-135">Azure API Management provides rate and quote throttling tooboth protect and add value tooyour API service.</span></span> <span data-ttu-id="fa108-136">hello begränsning nya principer med anpassade reglerna kan du bättre kornat kontroll över dessa principer tooenable dina kunder toobuild ännu bättre program.</span><span class="sxs-lookup"><span data-stu-id="fa108-136">hello new throttling policies with custom scoping rules allow you finer grained control over those policies tooenable your customers toobuild even better applications.</span></span> <span data-ttu-id="fa108-137">hello exemplen i den här artikeln visar hello användningen av dessa nya principer med tillverkning som begränsar nycklar med klienternas IP-adresser, användar-ID och klienten genererade värden.</span><span class="sxs-lookup"><span data-stu-id="fa108-137">hello examples in this article demonstrate hello use of these new policies by manufacturing rate limiting keys with client IP addresses, user identity, and client generated values.</span></span> <span data-ttu-id="fa108-138">Det finns emellertid många andra delar av hello-meddelande som kan användas som användaragent, URL-sökväg fragment, meddelandestorlek.</span><span class="sxs-lookup"><span data-stu-id="fa108-138">However, there are many other parts of hello message that could be used such as user agent, URL path fragments, message size.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa108-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa108-139">Next steps</span></span>
<span data-ttu-id="fa108-140">Ge oss din feedback i Disqus-tråden hello för det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fa108-140">Please give us your feedback in hello Disqus thread for this topic.</span></span> <span data-ttu-id="fa108-141">Det är bra toohear om andra potentiella nyckelvärden som har ett logiskt val i dina scenarier.</span><span class="sxs-lookup"><span data-stu-id="fa108-141">It would be great toohear about other potential key values that have been a logical choice in your scenarios.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="fa108-142">Titta på en videoöversikt över dessa principer</span><span class="sxs-lookup"><span data-stu-id="fa108-142">Watch a video overview of these policies</span></span>
<span data-ttu-id="fa108-143">Mer information om hello [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principer som beskrivs i den här artikeln får du titta på hello följande video.</span><span class="sxs-lookup"><span data-stu-id="fa108-143">For more information on hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

