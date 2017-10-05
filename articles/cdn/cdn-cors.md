---
title: "Med hjälp av Azure CDN med CORS | Microsoft Docs"
description: "Lär dig hur du använder i Azure Content innehållsleveransnätverk (CDN) till med Cross-Origin Resource Sharing (CORS)."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 86740a96-4269-4060-aba3-a69f00e6f14e
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 7070397f6e69b21add75bad8220f0b8ebe36d266
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-azure-cdn-with-cors"></a><span data-ttu-id="cb5c5-103">Med hjälp av Azure CDN med CORS</span><span class="sxs-lookup"><span data-stu-id="cb5c5-103">Using Azure CDN with CORS</span></span>
## <a name="what-is-cors"></a><span data-ttu-id="cb5c5-104">Vad är CORS?</span><span class="sxs-lookup"><span data-stu-id="cb5c5-104">What is CORS?</span></span>
<span data-ttu-id="cb5c5-105">CORS (Cross Origin Resource Sharing) är en HTTP-funktion som gör att ett webbprogram som körs i en domän kan komma åt resurser i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-105">CORS (Cross Origin Resource Sharing) is an HTTP feature that enables a web application running under one domain to access resources in another domain.</span></span> <span data-ttu-id="cb5c5-106">För att minska risken för angrepp för webbplatser scripting alla moderna webbläsare för att implementera säkerhetsbegränsning kallas [samma ursprung princip](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span><span class="sxs-lookup"><span data-stu-id="cb5c5-106">In order to reduce the possibility of cross-site scripting attacks, all modern web browsers implement a security restriction known as [same-origin policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span></span>  <span data-ttu-id="cb5c5-107">Detta förhindrar att en webbsida från anropa API: er i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-107">This prevents a web page from calling APIs in a different domain.</span></span>  <span data-ttu-id="cb5c5-108">CORS ger ett säkert sätt så att ett ursprung (ursprungsdomän) att anropa API: er i ett ursprung.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-108">CORS provides a secure way to allow one origin (the origin domain) to call APIs in another origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="cb5c5-109">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="cb5c5-109">How it works</span></span>
<span data-ttu-id="cb5c5-110">Det finns två typer av CORS-förfrågningar, *enkla begäranden* och *komplexa begäranden.*</span><span class="sxs-lookup"><span data-stu-id="cb5c5-110">There are two types of CORS requests, *simple requests* and *complex requests.*</span></span>

### <a name="for-simple-requests"></a><span data-ttu-id="cb5c5-111">För enkla begäranden:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-111">For simple requests:</span></span>

1. <span data-ttu-id="cb5c5-112">Webbläsaren skickar en begäran för CORS med en ytterligare **ursprung** HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-112">The browser sends the CORS request with an additional **Origin** HTTP request header.</span></span> <span data-ttu-id="cb5c5-113">Värdet för det här sidhuvudet är ursprung som hanteras av den överordnade sidan som har definierats som en kombination av *protokoll,* *domän,* och *port.*</span><span class="sxs-lookup"><span data-stu-id="cb5c5-113">The value of this header is the origin that served the parent page, which is defined as the combination of *protocol,* *domain,* and *port.*</span></span>  <span data-ttu-id="cb5c5-114">När en sida från https://www.contoso.com försöker få åtkomst till användardata i fabrikam.com ursprung, skulle begärandehuvudet följande skickas till fabrikam.com:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-114">When a page from https://www.contoso.com attempts to access a user's data in the fabrikam.com origin, the following request header would be sent to fabrikam.com:</span></span>

   `Origin: https://www.contoso.com`

2. <span data-ttu-id="cb5c5-115">Servern svarar med något av följande:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-115">The server may respond with any of the following:</span></span>

   * <span data-ttu-id="cb5c5-116">En **Access Control-Tillåt-ursprung** rubrik i sitt svar som anger vilken plats ursprung är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-116">An **Access-Control-Allow-Origin** header in its response indicating which origin site is allowed.</span></span> <span data-ttu-id="cb5c5-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-117">For example:</span></span>

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * <span data-ttu-id="cb5c5-118">En HTTP-felkod, till exempel 403 om servern inte tillåter cross-origin-begäran när du har kontrollerat rubriken ursprung</span><span class="sxs-lookup"><span data-stu-id="cb5c5-118">An HTTP error code such as 403 if the server does not allow the cross-origin request after checking the Origin header</span></span>

   * <span data-ttu-id="cb5c5-119">En **Access Control-Tillåt-ursprung** huvud med jokertecken som gör att alla ursprung:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-119">An **Access-Control-Allow-Origin** header with a wildcard that allows all origins:</span></span>

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a><span data-ttu-id="cb5c5-120">För komplexa begäranden:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-120">For complex requests:</span></span>

<span data-ttu-id="cb5c5-121">En komplex begäran är en CORS-begäran där webbläsaren krävs för att skicka en *preflight-begäran* (d.v.s. en preliminär avsökning) innan den faktiska CORS-begäranden skickas.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-121">A complex request is a CORS request where the browser is required to send a *preflight request* (i.e. a preliminary probe) before sending the actual CORS request.</span></span> <span data-ttu-id="cb5c5-122">Preflight-begäran frågar behörigheten server om de ursprungliga CORS begäran kan fortsätta och är en `OPTIONS` begäran till samma URL.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-122">The preflight request asks the server permission if the original CORS request can proceed and is an `OPTIONS` request to the same URL.</span></span>

> [!TIP]
> <span data-ttu-id="cb5c5-123">Mer information om CORS-flöden och vanliga fallgropar, visa den [Guide till CORS för REST API: er](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span><span class="sxs-lookup"><span data-stu-id="cb5c5-123">For more details on CORS flows and common pitfalls, view the [Guide to CORS for REST APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span></span>
>
>

## <a name="wildcard-or-single-origin-scenarios"></a><span data-ttu-id="cb5c5-124">Jokertecken eller enskild ursprung scenarier</span><span class="sxs-lookup"><span data-stu-id="cb5c5-124">Wildcard or single origin scenarios</span></span>
<span data-ttu-id="cb5c5-125">CORS i Azure CDN fungerar automatiskt med ingen ytterligare konfiguration när den **Access Control-Tillåt-ursprung** huvud är inställt på jokertecken (*) eller ett enda ursprung.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-125">CORS on Azure CDN will work automatically with no additional configuration when the **Access-Control-Allow-Origin** header is set to wildcard (*) or a single origin.</span></span>  <span data-ttu-id="cb5c5-126">CDN cachelagrar första svaret och efterföljande begäranden använder samma huvud.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-126">The CDN will cache the first response and subsequent requests will use the same header.</span></span>

<span data-ttu-id="cb5c5-127">Om begäran redan har gjorts till CDN innan CORS har angetts på den din ursprung måste du rensa innehållet för slutpunkten innehållet att läsa in innehåll med den **Access Control-Tillåt-ursprung** huvud.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-127">If requests have already been made to the CDN prior to CORS being set on the your origin, you will need to purge content on your endpoint content to reload the content with the **Access-Control-Allow-Origin** header.</span></span>

## <a name="multiple-origin-scenarios"></a><span data-ttu-id="cb5c5-128">Scenarier med flera ursprung</span><span class="sxs-lookup"><span data-stu-id="cb5c5-128">Multiple origin scenarios</span></span>
<span data-ttu-id="cb5c5-129">Om du vill tillåta att en viss lista över ursprung som ska tillåtas för CORS saker lite mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-129">If you need to allow a specific list of origins to be allowed for CORS, things get a little more complicated.</span></span> <span data-ttu-id="cb5c5-130">Problemet uppstår när CDN cachelagrar den **Access Control-Tillåt-ursprung** sidhuvud för det första CORS-ursprunget.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-130">The problem occurs when the CDN caches the **Access-Control-Allow-Origin** header for the first CORS origin.</span></span>  <span data-ttu-id="cb5c5-131">När en annan CORS-ursprunget gör en efterföljande begäran, CDN fungerar det cachelagrade **Access Control-Tillåt-ursprung** rubriken, som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-131">When a different CORS origin makes a subsequent request, the CDN will serve the cached **Access-Control-Allow-Origin** header, which won't match.</span></span>  <span data-ttu-id="cb5c5-132">Det finns flera sätt att åtgärda detta.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-132">There are several ways to correct this.</span></span>

### <a name="azure-cdn-premium-from-verizon"></a><span data-ttu-id="cb5c5-133">Azure CDN Premium från Verizon</span><span class="sxs-lookup"><span data-stu-id="cb5c5-133">Azure CDN Premium from Verizon</span></span>
<span data-ttu-id="cb5c5-134">Det bästa sättet att göra detta är att använda **Azure CDN Premium från Verizon**, vilket visar att vissa avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-134">The best way to enable this is to use **Azure CDN Premium from Verizon**, which exposes some advanced functionality.</span></span> 

<span data-ttu-id="cb5c5-135">Du behöver [skapa en regel](cdn-rules-engine.md) att kontrollera den **ursprung** huvud i begäran.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-135">You'll need to [create a rule](cdn-rules-engine.md) to check the **Origin** header on the request.</span></span>  <span data-ttu-id="cb5c5-136">Om det är ett giltigt ursprung din regel kommer att ange den **Access Control-Tillåt-ursprung** huvud med ursprung i begäran.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-136">If it's a valid origin, your rule will set the **Access-Control-Allow-Origin** header with the origin provided in the request.</span></span>  <span data-ttu-id="cb5c5-137">Om ursprung har angetts i den **ursprung** huvud är inte tillåtet, regeln bör utelämna den **Access Control-Tillåt-ursprung** huvud vilket leder till webbläsaren om du vill avvisa begäran.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-137">If the origin specified in the **Origin** header is not allowed, your rule should omit the **Access-Control-Allow-Origin** header which will cause the browser to reject the request.</span></span> 

<span data-ttu-id="cb5c5-138">Det finns två sätt att göra detta med regler-motorn.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-138">There are two ways to do this with the rules engine.</span></span>  <span data-ttu-id="cb5c5-139">I båda fallen den **Access Control-Tillåt-ursprung** huvudet från filservern ursprung ignoreras helt, den CDN regelmotor hanterar helt tillåtna CORS ursprung.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-139">In both cases, the **Access-Control-Allow-Origin** header from the file's origin server is completely ignored, the CDN's rules engine completely manages the allowed CORS origins.</span></span>

#### <a name="one-regular-expression-with-all-valid-origins"></a><span data-ttu-id="cb5c5-140">Ett reguljärt uttryck med alla giltiga ursprung</span><span class="sxs-lookup"><span data-stu-id="cb5c5-140">One regular expression with all valid origins</span></span>
<span data-ttu-id="cb5c5-141">I det här fallet skapar du ett reguljärt uttryck som innehåller alla ursprung som du vill tillåta:</span><span class="sxs-lookup"><span data-stu-id="cb5c5-141">In this case, you'll create a regular expression that includes all of the origins you want to allow:</span></span> 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> <span data-ttu-id="cb5c5-142">**Azure CDN från Verizon** använder [Perl kompatibel reguljära uttryck](http://pcre.org/) som motorn för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-142">**Azure CDN from Verizon** uses [Perl Compatible Regular Expressions](http://pcre.org/) as its engine for regular expressions.</span></span>  <span data-ttu-id="cb5c5-143">Du kan använda ett verktyg som [reguljära uttryck 101](https://regex101.com/) att verifiera det reguljära uttrycket.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-143">You can use a tool like [Regular Expressions 101](https://regex101.com/) to validate your regular expression.</span></span>  <span data-ttu-id="cb5c5-144">Observera att tecknet ”/” är giltig i reguljära uttryck och behöver inte hoppas, men undantagstecken tecknet anses vara bästa praxis och förväntas av vissa regex verifierare.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-144">Note that the "/" character is valid in regular expressions and doesn't need to be escaped, however, escaping that character is considered a best practice and is expected by some regex validators.</span></span>
> 
> 

<span data-ttu-id="cb5c5-145">Om det reguljära uttrycket matchar regeln ersätter den **Access Control-Tillåt-ursprung** huvud (eventuella) från ursprunget med ursprung som skickade begäran.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-145">If the regular expression matches, your rule will replace the **Access-Control-Allow-Origin** header (if any) from the origin with the origin that sent the request.</span></span>  <span data-ttu-id="cb5c5-146">Du kan också lägga till ytterligare CORS-huvuden som **-kontroll-Tillåt-åtkomstmetoder**.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-146">You can also add additional CORS headers, such as **Access-Control-Allow-Methods**.</span></span>

![Exempel på regler med reguljära uttryck](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a><span data-ttu-id="cb5c5-148">Begäran huvud-regel för varje ursprung.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-148">Request header rule for each origin.</span></span>
<span data-ttu-id="cb5c5-149">I stället för reguljära uttryck kan du i stället skapa en separat regel för varje ursprung som du vill tillåta med hjälp av den **begära huvud med jokertecken** [matchar villkoret](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="cb5c5-149">Rather than regular expressions, you can instead create a separate rule for each origin you wish to allow using the **Request Header Wildcard** [match condition](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span></span> <span data-ttu-id="cb5c5-150">Precis som med metoden reguljärt uttryck anger enbart regler motorn CORS-huvuden.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-150">As with the regular expression method, the rules engine alone sets the CORS headers.</span></span> 

![Regler som exempel utan reguljärt uttryck](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> <span data-ttu-id="cb5c5-152">I exemplet ovan med jokertecknet * anger regelmotor att matcha både HTTP och HTTPS.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-152">In the example above, the use of the wildcard character * tells the rules engine to match both HTTP and HTTPS.</span></span>
> 
> 

### <a name="azure-cdn-standard"></a><span data-ttu-id="cb5c5-153">Standard Azure CDN</span><span class="sxs-lookup"><span data-stu-id="cb5c5-153">Azure CDN Standard</span></span>
<span data-ttu-id="cb5c5-154">På Azure CDN Standard profiler endast mekanism för att tillåta flera ursprung utan att använda jokertecken ursprung är att använda [fråga cachelagring av frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="cb5c5-154">On Azure CDN Standard profiles, the only mechanism to allow for multiple origins without the use of the wildcard origin is to use [query string caching](cdn-query-string.md).</span></span>  <span data-ttu-id="cb5c5-155">Du måste aktivera inställningen för frågan anslutningssträngen för CDN-slutpunkten och sedan använda en unik frågesträng för begäranden från varje tillåtna domän.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-155">You need to enable query string setting for the CDN endpoint and then use a unique query string for requests from each allowed domain.</span></span> <span data-ttu-id="cb5c5-156">Detta resulterar i CDN cachelagring ett separat objekt för varje unik frågesträng.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-156">Doing this will result in the CDN caching a separate object for each unique query string.</span></span> <span data-ttu-id="cb5c5-157">Den här metoden är perfekt, dock inte eftersom den resulterar i flera kopior av samma fil cachelagras på CDN.</span><span class="sxs-lookup"><span data-stu-id="cb5c5-157">This approach is not ideal, however, as it will result in multiple copies of the same file cached on the CDN.</span></span>  

