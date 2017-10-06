---
title: "aaaAzure API Management korsdomänprinciper | Microsoft Docs"
description: "Läs mer om hello korsdomänprinciper tillgängligt för användning i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7689d277-8abe-472a-a78c-e6d4bd43455d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: dd5ebfd65b92ebd0c1f589a2bac669a3928d40b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="b99a1-103">Korsdomänprinciper för API Management</span><span class="sxs-lookup"><span data-stu-id="b99a1-103">API Management cross domain policies</span></span>
<span data-ttu-id="b99a1-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="b99a1-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="b99a1-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="b99a1-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="b99a1-106"><a name="CrossDomainPolicies"></a>Mellan domänprinciper</span><span class="sxs-lookup"><span data-stu-id="b99a1-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="b99a1-107">[Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="b99a1-108">[CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="b99a1-109">[Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="b99a1-110"><a name="AllowCrossDomainCalls"></a>Tillåta anrop mellan domäner</span><span class="sxs-lookup"><span data-stu-id="b99a1-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="b99a1-111">Använd hello `cross-domain` princip toomake hello API som är tillgängliga från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-111">Use hello `cross-domain` policy toomake hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b99a1-112">Principframställning</span><span class="sxs-lookup"><span data-stu-id="b99a1-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in hello Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="b99a1-113">Exempel</span><span class="sxs-lookup"><span data-stu-id="b99a1-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="b99a1-114">Element</span><span class="sxs-lookup"><span data-stu-id="b99a1-114">Elements</span></span>  
  
|<span data-ttu-id="b99a1-115">Namn</span><span class="sxs-lookup"><span data-stu-id="b99a1-115">Name</span></span>|<span data-ttu-id="b99a1-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b99a1-116">Description</span></span>|<span data-ttu-id="b99a1-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="b99a1-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b99a1-118">mellan domäner</span><span class="sxs-lookup"><span data-stu-id="b99a1-118">cross-domain</span></span>|<span data-ttu-id="b99a1-119">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="b99a1-119">Root element.</span></span> <span data-ttu-id="b99a1-120">Underordnade element måste uppfylla toohello [Adobe domänerna filen principspecifikationen](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="b99a1-120">Child elements must conform toohello [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="b99a1-121">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b99a1-122">Användning</span><span class="sxs-lookup"><span data-stu-id="b99a1-122">Usage</span></span>  
 <span data-ttu-id="b99a1-123">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b99a1-123">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b99a1-124">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="b99a1-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="b99a1-125">**Princip för scope:** globala</span><span class="sxs-lookup"><span data-stu-id="b99a1-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="b99a1-126"><a name="CORS"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="b99a1-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="b99a1-127">Hej `cors` princip lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-127">hello `cors` policy adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="b99a1-128">CORS kan en webbläsare och en server toointeract och avgöra huruvida tooallow specifika cross-origin-begäranden (d.v.s. XMLHttpRequests anrop från JavaScript på en webbsida tooother domäner).</span><span class="sxs-lookup"><span data-stu-id="b99a1-128">CORS allows a browser and a server toointeract and determine whether or not tooallow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page tooother domains).</span></span> <span data-ttu-id="b99a1-129">Detta ger bättre flexibilitet än att bara tillåta samma origin-begäranden, men det är säkrare än att tillåta alla cross-origin-begäranden.</span><span class="sxs-lookup"><span data-stu-id="b99a1-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b99a1-130">Principframställning</span><span class="sxs-lookup"><span data-stu-id="b99a1-130">Policy statement</span></span>  
  
```xml  
<cors allow-credentials="false|true">  
    <allowed-origins>  
        <origin>origin uri</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="number of seconds">  
        <method>http verb</method>  
    </allowed-methods>  
    <allowed-headers>  
        <header>header name</header>  
    </allowed-headers>  
    <expose-headers>  
        <header>header name</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="example"></a><span data-ttu-id="b99a1-131">Exempel</span><span class="sxs-lookup"><span data-stu-id="b99a1-131">Example</span></span>  
 <span data-ttu-id="b99a1-132">Det här exemplet visar hur toosupport före svarta begär, till exempel de som har anpassade huvuden eller andra metoder än GET och POST.</span><span class="sxs-lookup"><span data-stu-id="b99a1-132">This example demonstrates how toosupport pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="b99a1-133">toosupport anpassade sidhuvuden och ytterligare HTTP-verb, Använd hello `allowed-methods` och `allowed-headers` avsnitten som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="b99a1-133">toosupport custom headers and additional HTTP verbs, use hello `allowed-methods` and `allowed-headers` sections as shown in hello following example.</span></span>  
  
```xml  
<cors allow-credentials="true">  
    <allowed-origins>  
        <!-- Localhost useful for development -->  
        <origin>http://localhost:8080/</origin>  
        <origin>http://example.com/</origin>  
    </allowed-origins>  
    <allowed-methods preflight-result-max-age="300">  
        <method>GET</method>  
        <method>POST</method>  
        <method>PATCH</method>  
        <method>DELETE</method>  
    </allowed-methods>  
    <allowed-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
        <header>x-zumo-version</header>  
        <header>x-zumo-auth</header>  
        <header>content-type</header>  
        <header>accept</header>  
    </allowed-headers>  
    <expose-headers>  
        <!-- Examples below show Azure Mobile Services headers -->  
        <header>x-zumo-installation-id</header>  
        <header>x-zumo-application</header>  
    </expose-headers>  
</cors>  
```  
  
### <a name="elements"></a><span data-ttu-id="b99a1-134">Element</span><span class="sxs-lookup"><span data-stu-id="b99a1-134">Elements</span></span>  
  
|<span data-ttu-id="b99a1-135">Namn</span><span class="sxs-lookup"><span data-stu-id="b99a1-135">Name</span></span>|<span data-ttu-id="b99a1-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b99a1-136">Description</span></span>|<span data-ttu-id="b99a1-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="b99a1-137">Required</span></span>|<span data-ttu-id="b99a1-138">Standard</span><span class="sxs-lookup"><span data-stu-id="b99a1-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b99a1-139">cors</span><span class="sxs-lookup"><span data-stu-id="b99a1-139">cors</span></span>|<span data-ttu-id="b99a1-140">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="b99a1-140">Root element.</span></span>|<span data-ttu-id="b99a1-141">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-141">Yes</span></span>|<span data-ttu-id="b99a1-142">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-142">N/A</span></span>|  
|<span data-ttu-id="b99a1-143">tillåtna ursprung</span><span class="sxs-lookup"><span data-stu-id="b99a1-143">allowed-origins</span></span>|<span data-ttu-id="b99a1-144">Innehåller `origin` element som beskriver hello tillåtna ursprung för domäner begäranden.</span><span class="sxs-lookup"><span data-stu-id="b99a1-144">Contains `origin` elements that describe hello allowed origins for cross-domain requests.</span></span> <span data-ttu-id="b99a1-145">`allowed-origins`kan innehålla antingen en enskild `origin` element som anger `*` tooallow alla ursprung, eller en eller flera `origin` element som innehåller en URI.</span><span class="sxs-lookup"><span data-stu-id="b99a1-145">`allowed-origins` can contain either a single `origin` element that specifies `*` tooallow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="b99a1-146">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-146">Yes</span></span>|<span data-ttu-id="b99a1-147">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-147">N/A</span></span>|  
|<span data-ttu-id="b99a1-148">ursprung</span><span class="sxs-lookup"><span data-stu-id="b99a1-148">origin</span></span>|<span data-ttu-id="b99a1-149">hello värdet kan vara antingen `*` tooallow alla ursprung eller en URI som anger ett enda ursprung.</span><span class="sxs-lookup"><span data-stu-id="b99a1-149">hello value can be either `*` tooallow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="b99a1-150">hello URI måste innehålla ett schema, värd och port.</span><span class="sxs-lookup"><span data-stu-id="b99a1-150">hello URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="b99a1-151">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-151">Yes</span></span>|<span data-ttu-id="b99a1-152">Om hello port utelämnas i en URI används port 80 för HTTP och port 443 används för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b99a1-152">If hello port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="b99a1-153">tillåtna metoder</span><span class="sxs-lookup"><span data-stu-id="b99a1-153">allowed-methods</span></span>|<span data-ttu-id="b99a1-154">Det här elementet krävs om metoder än hämta eller POST tillåts.</span><span class="sxs-lookup"><span data-stu-id="b99a1-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="b99a1-155">Innehåller `method` element som anger hello stöds HTTP-verb.</span><span class="sxs-lookup"><span data-stu-id="b99a1-155">Contains `method` elements that specify hello supported HTTP verbs.</span></span>|<span data-ttu-id="b99a1-156">Nej</span><span class="sxs-lookup"><span data-stu-id="b99a1-156">No</span></span>|<span data-ttu-id="b99a1-157">Om det här avsnittet inte finns, stöds GET och POST.</span><span class="sxs-lookup"><span data-stu-id="b99a1-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="b99a1-158">Metoden</span><span class="sxs-lookup"><span data-stu-id="b99a1-158">method</span></span>|<span data-ttu-id="b99a1-159">Anger ett HTTP-verb.</span><span class="sxs-lookup"><span data-stu-id="b99a1-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="b99a1-160">Minst en `method` -element krävs om hello `allowed-methods` avsnittet finns.</span><span class="sxs-lookup"><span data-stu-id="b99a1-160">At least one `method` element is required if hello `allowed-methods` section is present.</span></span>|<span data-ttu-id="b99a1-161">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-161">N/A</span></span>|  
|<span data-ttu-id="b99a1-162">tillåtna rubriker</span><span class="sxs-lookup"><span data-stu-id="b99a1-162">allowed-headers</span></span>|<span data-ttu-id="b99a1-163">Det här elementet innehåller `header` element att ange namnen på hello-huvuden som kan ingå i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="b99a1-163">This element contains `header` elements specifying names of hello headers that can be included in hello request.</span></span>|<span data-ttu-id="b99a1-164">Nej</span><span class="sxs-lookup"><span data-stu-id="b99a1-164">No</span></span>|<span data-ttu-id="b99a1-165">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-165">N/A</span></span>|  
|<span data-ttu-id="b99a1-166">exponera rubriker</span><span class="sxs-lookup"><span data-stu-id="b99a1-166">expose-headers</span></span>|<span data-ttu-id="b99a1-167">Det här elementet innehåller `header` element att ange namnen på hello-huvuden som ska vara tillgängligt av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="b99a1-167">This element contains `header` elements specifying names of hello headers that will be accessible by hello client.</span></span>|<span data-ttu-id="b99a1-168">Nej</span><span class="sxs-lookup"><span data-stu-id="b99a1-168">No</span></span>|<span data-ttu-id="b99a1-169">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-169">N/A</span></span>|  
|<span data-ttu-id="b99a1-170">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="b99a1-170">header</span></span>|<span data-ttu-id="b99a1-171">Anger ett namn för sidhuvudet.</span><span class="sxs-lookup"><span data-stu-id="b99a1-171">Specifies a header name.</span></span>|<span data-ttu-id="b99a1-172">Minst en `header` -element krävs i `allowed-headers` eller `expose-headers` om hello avsnittet finns.</span><span class="sxs-lookup"><span data-stu-id="b99a1-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if hello section is present.</span></span>|<span data-ttu-id="b99a1-173">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b99a1-174">Attribut</span><span class="sxs-lookup"><span data-stu-id="b99a1-174">Attributes</span></span>  
  
|<span data-ttu-id="b99a1-175">Namn</span><span class="sxs-lookup"><span data-stu-id="b99a1-175">Name</span></span>|<span data-ttu-id="b99a1-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b99a1-176">Description</span></span>|<span data-ttu-id="b99a1-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="b99a1-177">Required</span></span>|<span data-ttu-id="b99a1-178">Standard</span><span class="sxs-lookup"><span data-stu-id="b99a1-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b99a1-179">Tillåt autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b99a1-179">allow-credentials</span></span>|<span data-ttu-id="b99a1-180">Hej `Access-Control-Allow-Credentials` huvud hello preflight-svar kommer att ange toohello värdet för det här attributet och påverkar hello klientens möjlighet toosubmit autentiseringsuppgifter i domänerna begäranden.</span><span class="sxs-lookup"><span data-stu-id="b99a1-180">hello `Access-Control-Allow-Credentials` header in hello preflight response will be set toohello value of this attribute and affect hello client’s ability toosubmit credentials in cross-domain requests.</span></span>|<span data-ttu-id="b99a1-181">Nej</span><span class="sxs-lookup"><span data-stu-id="b99a1-181">No</span></span>|<span data-ttu-id="b99a1-182">FALSKT</span><span class="sxs-lookup"><span data-stu-id="b99a1-182">false</span></span>|  
|<span data-ttu-id="b99a1-183">preflight-resultat-max-ålder</span><span class="sxs-lookup"><span data-stu-id="b99a1-183">preflight-result-max-age</span></span>|<span data-ttu-id="b99a1-184">Hej `Access-Control-Max-Age` huvud hello preflight-svar kommer att ange toohello värdet för det här attributet och påverkar hello användaragenten möjlighet toocache före svarta svar.</span><span class="sxs-lookup"><span data-stu-id="b99a1-184">hello `Access-Control-Max-Age` header in hello preflight response will be set toohello value of this attribute and affect hello user agent’s ability toocache pre-flight response.</span></span>|<span data-ttu-id="b99a1-185">Nej</span><span class="sxs-lookup"><span data-stu-id="b99a1-185">No</span></span>|<span data-ttu-id="b99a1-186">0</span><span class="sxs-lookup"><span data-stu-id="b99a1-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b99a1-187">Användning</span><span class="sxs-lookup"><span data-stu-id="b99a1-187">Usage</span></span>  
 <span data-ttu-id="b99a1-188">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b99a1-188">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b99a1-189">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="b99a1-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="b99a1-190">**Princip för scope:** API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="b99a1-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="b99a1-191"><a name="JSONP"></a>HANTERAS JSONP</span><span class="sxs-lookup"><span data-stu-id="b99a1-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="b99a1-192">Hej `jsonp` principen lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow mellan domäner-anrop från JavaScript-webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="b99a1-192">hello `jsonp` policy adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="b99a1-193">Hanteras JSONP är en metod som används i JavaScript program toorequest data från en server i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="b99a1-193">JSONP is a method used in JavaScript programs toorequest data from a server in a different domain.</span></span> <span data-ttu-id="b99a1-194">Hanteras JSONP kringgår hello begränsning som tillämpas av de flesta webbläsare där tooweb sidor måste vara i hello samma domän.</span><span class="sxs-lookup"><span data-stu-id="b99a1-194">JSONP bypasses hello limitation enforced by most web browsers where access tooweb pages must be in hello same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="b99a1-195">Principframställning</span><span class="sxs-lookup"><span data-stu-id="b99a1-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="b99a1-196">Exempel</span><span class="sxs-lookup"><span data-stu-id="b99a1-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="b99a1-197">Om du anropar hello metoden utan hello återanrop parametern? cb = XXX returneras oformaterade JSON (utan en funktionen anropet wrapper).</span><span class="sxs-lookup"><span data-stu-id="b99a1-197">If you call hello method without hello callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="b99a1-198">Om du lägger till hello återanrop parametern `?cb=XXX` hanteras JSONP resulterar i att wrapping hello ursprungliga JSON-resultaten runt hello Återanropsfunktionen som returneras`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="b99a1-198">If you add hello callback parameter `?cb=XXX` it will return a JSONP result, wrapping hello original JSON results around hello callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="b99a1-199">Element</span><span class="sxs-lookup"><span data-stu-id="b99a1-199">Elements</span></span>  
  
|<span data-ttu-id="b99a1-200">Namn</span><span class="sxs-lookup"><span data-stu-id="b99a1-200">Name</span></span>|<span data-ttu-id="b99a1-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b99a1-201">Description</span></span>|<span data-ttu-id="b99a1-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="b99a1-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="b99a1-203">hanteras jsonp</span><span class="sxs-lookup"><span data-stu-id="b99a1-203">jsonp</span></span>|<span data-ttu-id="b99a1-204">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="b99a1-204">Root element.</span></span>|<span data-ttu-id="b99a1-205">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="b99a1-206">Attribut</span><span class="sxs-lookup"><span data-stu-id="b99a1-206">Attributes</span></span>  
  
|<span data-ttu-id="b99a1-207">Namn</span><span class="sxs-lookup"><span data-stu-id="b99a1-207">Name</span></span>|<span data-ttu-id="b99a1-208">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b99a1-208">Description</span></span>|<span data-ttu-id="b99a1-209">Krävs</span><span class="sxs-lookup"><span data-stu-id="b99a1-209">Required</span></span>|<span data-ttu-id="b99a1-210">Standard</span><span class="sxs-lookup"><span data-stu-id="b99a1-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="b99a1-211">motringning parameternamn</span><span class="sxs-lookup"><span data-stu-id="b99a1-211">callback-parameter-name</span></span>|<span data-ttu-id="b99a1-212">hello domänerna JavaScript-anrop med prefixet hello fullständigt kvalificerade domännamnet där hello funktionen finns.</span><span class="sxs-lookup"><span data-stu-id="b99a1-212">hello cross-domain JavaScript function call prefixed with hello fully qualified domain name where hello function resides.</span></span>|<span data-ttu-id="b99a1-213">Ja</span><span class="sxs-lookup"><span data-stu-id="b99a1-213">Yes</span></span>|<span data-ttu-id="b99a1-214">Saknas</span><span class="sxs-lookup"><span data-stu-id="b99a1-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="b99a1-215">Användning</span><span class="sxs-lookup"><span data-stu-id="b99a1-215">Usage</span></span>  
 <span data-ttu-id="b99a1-216">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="b99a1-216">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="b99a1-217">**Avsnitt i princip:** utgående</span><span class="sxs-lookup"><span data-stu-id="b99a1-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="b99a1-218">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="b99a1-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="b99a1-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b99a1-219">Next steps</span></span>
<span data-ttu-id="b99a1-220">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="b99a1-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  