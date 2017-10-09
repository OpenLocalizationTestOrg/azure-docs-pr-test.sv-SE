---
title: aaaUsing Azure CDN med CORS | Microsoft Docs
description: "Lär dig hur toouse hello Azure Content Delivery Network (CDN) toowith Cross-Origin Resource Sharing (CORS)."
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
ms.openlocfilehash: 6c743b56c32a2d3aacc9a77094cb87d61b95d2f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-cdn-with-cors"></a><span data-ttu-id="c6d23-103">Med hjälp av Azure CDN med CORS</span><span class="sxs-lookup"><span data-stu-id="c6d23-103">Using Azure CDN with CORS</span></span>
## <a name="what-is-cors"></a><span data-ttu-id="c6d23-104">Vad är CORS?</span><span class="sxs-lookup"><span data-stu-id="c6d23-104">What is CORS?</span></span>
<span data-ttu-id="c6d23-105">CORS (Cross Origin Resource Sharing) är en HTTP-funktion som gör att ett webbprogram som körs i en domän tooaccess resurser i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="c6d23-105">CORS (Cross Origin Resource Sharing) is an HTTP feature that enables a web application running under one domain tooaccess resources in another domain.</span></span> <span data-ttu-id="c6d23-106">I ordning tooreduce hello risken för webbplatser scripting angrepp, alla moderna webbläsare för att implementera säkerhetsbegränsning kallas [samma ursprung princip](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span><span class="sxs-lookup"><span data-stu-id="c6d23-106">In order tooreduce hello possibility of cross-site scripting attacks, all modern web browsers implement a security restriction known as [same-origin policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).</span></span>  <span data-ttu-id="c6d23-107">Detta förhindrar att en webbsida från anropa API: er i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="c6d23-107">This prevents a web page from calling APIs in a different domain.</span></span>  <span data-ttu-id="c6d23-108">CORS ger ett säkert sätt tooallow ett ursprung (hello ursprungsdomän) toocall API: er i ett ursprung.</span><span class="sxs-lookup"><span data-stu-id="c6d23-108">CORS provides a secure way tooallow one origin (hello origin domain) toocall APIs in another origin.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="c6d23-109">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="c6d23-109">How it works</span></span>
<span data-ttu-id="c6d23-110">Det finns två typer av CORS-förfrågningar, *enkla begäranden* och *komplexa begäranden.*</span><span class="sxs-lookup"><span data-stu-id="c6d23-110">There are two types of CORS requests, *simple requests* and *complex requests.*</span></span>

### <a name="for-simple-requests"></a><span data-ttu-id="c6d23-111">För enkla begäranden:</span><span class="sxs-lookup"><span data-stu-id="c6d23-111">For simple requests:</span></span>

1. <span data-ttu-id="c6d23-112">hello webbläsaren skickar hello CORS-begäran med en ytterligare **ursprung** HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="c6d23-112">hello browser sends hello CORS request with an additional **Origin** HTTP request header.</span></span> <span data-ttu-id="c6d23-113">hello-värdet för det här sidhuvudet är hello ursprung som betjänats hello överordnad sida som har definierats som hello kombination av *protokoll,* *domän,* och *port.*</span><span class="sxs-lookup"><span data-stu-id="c6d23-113">hello value of this header is hello origin that served hello parent page, which is defined as hello combination of *protocol,* *domain,* and *port.*</span></span>  <span data-ttu-id="c6d23-114">En sida från https://www.contoso.com försöker tooaccess användardata i hello fabrikam.com ursprung, skickas hello följande huvudet i begäran toofabrikam.com:</span><span class="sxs-lookup"><span data-stu-id="c6d23-114">When a page from https://www.contoso.com attempts tooaccess a user's data in hello fabrikam.com origin, hello following request header would be sent toofabrikam.com:</span></span>

   `Origin: https://www.contoso.com`

2. <span data-ttu-id="c6d23-115">hello servern svarar med hello följande:</span><span class="sxs-lookup"><span data-stu-id="c6d23-115">hello server may respond with any of hello following:</span></span>

   * <span data-ttu-id="c6d23-116">En **Access Control-Tillåt-ursprung** rubrik i sitt svar som anger vilken plats ursprung är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="c6d23-116">An **Access-Control-Allow-Origin** header in its response indicating which origin site is allowed.</span></span> <span data-ttu-id="c6d23-117">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c6d23-117">For example:</span></span>

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * <span data-ttu-id="c6d23-118">En HTTP-felkod: till exempel 403 om hello server inte tillåter hello cross-origin begäran när du har kontrollerat hello ursprung sidhuvud</span><span class="sxs-lookup"><span data-stu-id="c6d23-118">An HTTP error code such as 403 if hello server does not allow hello cross-origin request after checking hello Origin header</span></span>

   * <span data-ttu-id="c6d23-119">En **Access Control-Tillåt-ursprung** huvud med jokertecken som gör att alla ursprung:</span><span class="sxs-lookup"><span data-stu-id="c6d23-119">An **Access-Control-Allow-Origin** header with a wildcard that allows all origins:</span></span>

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a><span data-ttu-id="c6d23-120">För komplexa begäranden:</span><span class="sxs-lookup"><span data-stu-id="c6d23-120">For complex requests:</span></span>

<span data-ttu-id="c6d23-121">En komplex begäran är en begäran om CORS där hello webbläsare krävs toosend en *preflight-begäran* (d.v.s. en preliminär avsökning) innan du skickar hello faktiska CORS-begäran.</span><span class="sxs-lookup"><span data-stu-id="c6d23-121">A complex request is a CORS request where hello browser is required toosend a *preflight request* (i.e. a preliminary probe) before sending hello actual CORS request.</span></span> <span data-ttu-id="c6d23-122">hello preflight-begäran frågar hello server behörighet om hello ursprungliga CORS-begäran kan fortsätta och är en `OPTIONS` begära toohello samma URL.</span><span class="sxs-lookup"><span data-stu-id="c6d23-122">hello preflight request asks hello server permission if hello original CORS request can proceed and is an `OPTIONS` request toohello same URL.</span></span>

> [!TIP]
> <span data-ttu-id="c6d23-123">Visa mer information om CORS-flöden och vanliga fallgropar hello [Guide tooCORS för REST API: er](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span><span class="sxs-lookup"><span data-stu-id="c6d23-123">For more details on CORS flows and common pitfalls, view hello [Guide tooCORS for REST APIs](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).</span></span>
>
>

## <a name="wildcard-or-single-origin-scenarios"></a><span data-ttu-id="c6d23-124">Jokertecken eller enskild ursprung scenarier</span><span class="sxs-lookup"><span data-stu-id="c6d23-124">Wildcard or single origin scenarios</span></span>
<span data-ttu-id="c6d23-125">CORS i Azure CDN fungerar automatiskt med ingen ytterligare konfiguration när hello **Access Control-Tillåt-ursprung** huvud är inställt toowildcard (*) eller ett enda ursprung.</span><span class="sxs-lookup"><span data-stu-id="c6d23-125">CORS on Azure CDN will work automatically with no additional configuration when hello **Access-Control-Allow-Origin** header is set toowildcard (*) or a single origin.</span></span>  <span data-ttu-id="c6d23-126">hello CDN cachelagrar hello första svar och efterföljande begäranden använder hello samma sidhuvud.</span><span class="sxs-lookup"><span data-stu-id="c6d23-126">hello CDN will cache hello first response and subsequent requests will use hello same header.</span></span>

<span data-ttu-id="c6d23-127">Om begäran har redan gjorts toohello CDN tidigare tooCORS anges på hello din ursprung, behöver du toopurge innehåll på din slutpunkt innehåll tooreload hello innehåll med hello **Access Control-Tillåt-ursprung** huvud.</span><span class="sxs-lookup"><span data-stu-id="c6d23-127">If requests have already been made toohello CDN prior tooCORS being set on hello your origin, you will need toopurge content on your endpoint content tooreload hello content with hello **Access-Control-Allow-Origin** header.</span></span>

## <a name="multiple-origin-scenarios"></a><span data-ttu-id="c6d23-128">Scenarier med flera ursprung</span><span class="sxs-lookup"><span data-stu-id="c6d23-128">Multiple origin scenarios</span></span>
<span data-ttu-id="c6d23-129">Om du behöver tooallow en specifik lista över ursprung toobe tillåts för CORS saker lite mer komplicerad.</span><span class="sxs-lookup"><span data-stu-id="c6d23-129">If you need tooallow a specific list of origins toobe allowed for CORS, things get a little more complicated.</span></span> <span data-ttu-id="c6d23-130">hello problemet uppstår när hello CDN cachelagrar hello **Access Control-Tillåt-ursprung** huvud för hello första CORS-ursprunget.</span><span class="sxs-lookup"><span data-stu-id="c6d23-130">hello problem occurs when hello CDN caches hello **Access-Control-Allow-Origin** header for hello first CORS origin.</span></span>  <span data-ttu-id="c6d23-131">När en annan CORS-ursprunget gör en efterföljande begäran, hello CDN fungerar hello cachelagrade **Access Control-Tillåt-ursprung** rubriken, som inte matchar.</span><span class="sxs-lookup"><span data-stu-id="c6d23-131">When a different CORS origin makes a subsequent request, hello CDN will serve hello cached **Access-Control-Allow-Origin** header, which won't match.</span></span>  <span data-ttu-id="c6d23-132">Det finns flera sätt toocorrect detta.</span><span class="sxs-lookup"><span data-stu-id="c6d23-132">There are several ways toocorrect this.</span></span>

### <a name="azure-cdn-premium-from-verizon"></a><span data-ttu-id="c6d23-133">Azure CDN Premium från Verizon</span><span class="sxs-lookup"><span data-stu-id="c6d23-133">Azure CDN Premium from Verizon</span></span>
<span data-ttu-id="c6d23-134">Hej bästa sätt tooenable är toouse **Azure CDN Premium från Verizon**, vilket visar att vissa avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="c6d23-134">hello best way tooenable this is toouse **Azure CDN Premium from Verizon**, which exposes some advanced functionality.</span></span> 

<span data-ttu-id="c6d23-135">Du behöver för[skapa en regel](cdn-rules-engine.md) toocheck hello **ursprung** sidhuvud på hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="c6d23-135">You'll need too[create a rule](cdn-rules-engine.md) toocheck hello **Origin** header on hello request.</span></span>  <span data-ttu-id="c6d23-136">Om det är ett giltigt ursprung din regel kommer att ange hello **Access Control-Tillåt-ursprung** huvud med hello ursprung i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="c6d23-136">If it's a valid origin, your rule will set hello **Access-Control-Allow-Origin** header with hello origin provided in hello request.</span></span>  <span data-ttu-id="c6d23-137">Om hello ursprung har angetts i hello **ursprung** huvud är inte tillåtet, regeln bör utelämna hello **Access Control-Tillåt-ursprung** huvudet som gör att hello webbläsarbegäran tooreject hello.</span><span class="sxs-lookup"><span data-stu-id="c6d23-137">If hello origin specified in hello **Origin** header is not allowed, your rule should omit hello **Access-Control-Allow-Origin** header which will cause hello browser tooreject hello request.</span></span> 

<span data-ttu-id="c6d23-138">Det finns två sätt toodo detta med hello regelmotor.</span><span class="sxs-lookup"><span data-stu-id="c6d23-138">There are two ways toodo this with hello rules engine.</span></span>  <span data-ttu-id="c6d23-139">I båda fallen hello **Access Control-Tillåt-ursprung** huvudet från hello filens ursprungsserver ignoreras helt, hello CDN regelmotor helt hanterar hello tillåtna CORS ursprung.</span><span class="sxs-lookup"><span data-stu-id="c6d23-139">In both cases, hello **Access-Control-Allow-Origin** header from hello file's origin server is completely ignored, hello CDN's rules engine completely manages hello allowed CORS origins.</span></span>

#### <a name="one-regular-expression-with-all-valid-origins"></a><span data-ttu-id="c6d23-140">Ett reguljärt uttryck med alla giltiga ursprung</span><span class="sxs-lookup"><span data-stu-id="c6d23-140">One regular expression with all valid origins</span></span>
<span data-ttu-id="c6d23-141">I det här fallet ska du skapa ett reguljärt uttryck som innehåller alla hello ursprung önskade tooallow:</span><span class="sxs-lookup"><span data-stu-id="c6d23-141">In this case, you'll create a regular expression that includes all of hello origins you want tooallow:</span></span> 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$

> [!TIP]
> <span data-ttu-id="c6d23-142">**Azure CDN från Verizon** använder [Perl kompatibel reguljära uttryck](http://pcre.org/) som motorn för reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="c6d23-142">**Azure CDN from Verizon** uses [Perl Compatible Regular Expressions](http://pcre.org/) as its engine for regular expressions.</span></span>  <span data-ttu-id="c6d23-143">Du kan använda ett verktyg som [reguljära uttryck 101](https://regex101.com/) toovalidate reguljära uttryck.</span><span class="sxs-lookup"><span data-stu-id="c6d23-143">You can use a tool like [Regular Expressions 101](https://regex101.com/) toovalidate your regular expression.</span></span>  <span data-ttu-id="c6d23-144">Observera att hello ”/” tecken är giltig i reguljära uttryck och behöver inte toobe undantaget, men undantagstecken tecknet anses vara bästa praxis och förväntas av vissa regex verifierare.</span><span class="sxs-lookup"><span data-stu-id="c6d23-144">Note that hello "/" character is valid in regular expressions and doesn't need toobe escaped, however, escaping that character is considered a best practice and is expected by some regex validators.</span></span>
> 
> 

<span data-ttu-id="c6d23-145">Om hello reguljära uttryck matchar, ersätts regeln hello **Access Control-Tillåt-ursprung** huvud (eventuella) från hello ursprung med hello ursprung som skickade hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="c6d23-145">If hello regular expression matches, your rule will replace hello **Access-Control-Allow-Origin** header (if any) from hello origin with hello origin that sent hello request.</span></span>  <span data-ttu-id="c6d23-146">Du kan också lägga till ytterligare CORS-huvuden som **-kontroll-Tillåt-åtkomstmetoder**.</span><span class="sxs-lookup"><span data-stu-id="c6d23-146">You can also add additional CORS headers, such as **Access-Control-Allow-Methods**.</span></span>

![Exempel på regler med reguljära uttryck](./media/cdn-cors/cdn-cors-regex.png)

#### <a name="request-header-rule-for-each-origin"></a><span data-ttu-id="c6d23-148">Begäran huvud-regel för varje ursprung.</span><span class="sxs-lookup"><span data-stu-id="c6d23-148">Request header rule for each origin.</span></span>
<span data-ttu-id="c6d23-149">I stället för reguljära uttryck kan du istället skapa en separat regel för varje ursprung du vill använda hello tooallow **begära huvud med jokertecken** [matchar villkoret](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span><span class="sxs-lookup"><span data-stu-id="c6d23-149">Rather than regular expressions, you can instead create a separate rule for each origin you wish tooallow using hello **Request Header Wildcard** [match condition](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).</span></span> <span data-ttu-id="c6d23-150">Som med hello reguljärt uttryck metoden motorn hello regler fristående anger hello CORS huvuden.</span><span class="sxs-lookup"><span data-stu-id="c6d23-150">As with hello regular expression method, hello rules engine alone sets hello CORS headers.</span></span> 

![Regler som exempel utan reguljärt uttryck](./media/cdn-cors/cdn-cors-no-regex.png)

> [!TIP]
> <span data-ttu-id="c6d23-152">I hello-exemplet ovan, hello användning av jokertecken hello * visar hello regler motorn toomatch både HTTP och HTTPS.</span><span class="sxs-lookup"><span data-stu-id="c6d23-152">In hello example above, hello use of hello wildcard character * tells hello rules engine toomatch both HTTP and HTTPS.</span></span>
> 
> 

### <a name="azure-cdn-standard"></a><span data-ttu-id="c6d23-153">Standard Azure CDN</span><span class="sxs-lookup"><span data-stu-id="c6d23-153">Azure CDN Standard</span></span>
<span data-ttu-id="c6d23-154">På Azure CDN Standard profiler hello mekanism tooallow för flera ursprung utan hello hello jokertecken ursprung är toouse [fråga cachelagring av frågesträngar](cdn-query-string.md).</span><span class="sxs-lookup"><span data-stu-id="c6d23-154">On Azure CDN Standard profiles, hello only mechanism tooallow for multiple origins without hello use of hello wildcard origin is toouse [query string caching](cdn-query-string.md).</span></span>  <span data-ttu-id="c6d23-155">Du måste ha tooenable frågan sträng inställningen för hello CDN-slutpunkten och sedan använda en unik frågesträng för begäranden från alla domäner som är tillåtna.</span><span class="sxs-lookup"><span data-stu-id="c6d23-155">You need tooenable query string setting for hello CDN endpoint and then use a unique query string for requests from each allowed domain.</span></span> <span data-ttu-id="c6d23-156">Detta resulterar i hello CDN cachelagring ett separat objekt för varje unik frågesträng.</span><span class="sxs-lookup"><span data-stu-id="c6d23-156">Doing this will result in hello CDN caching a separate object for each unique query string.</span></span> <span data-ttu-id="c6d23-157">Den här metoden är inte idealiskt, men eftersom den resulterar i flera kopior av hello samma fil cachelagrade på hello CDN.</span><span class="sxs-lookup"><span data-stu-id="c6d23-157">This approach is not ideal, however, as it will result in multiple copies of hello same file cached on hello CDN.</span></span>  

