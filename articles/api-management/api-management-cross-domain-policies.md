---
title: "Azure API Management korsdomänprinciper | Microsoft Docs"
description: "Läs mer om principerna över domäner kan användas i Azure API Management."
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
ms.openlocfilehash: ddca9e35b44a21294abbb5eaa4418bcdb85494cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-cross-domain-policies"></a><span data-ttu-id="d5cf9-103">Korsdomänprinciper för API Management</span><span class="sxs-lookup"><span data-stu-id="d5cf9-103">API Management cross domain policies</span></span>
<span data-ttu-id="d5cf9-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="d5cf9-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="d5cf9-106"><a name="CrossDomainPolicies"></a>Mellan domänprinciper</span><span class="sxs-lookup"><span data-stu-id="d5cf9-106"><a name="CrossDomainPolicies"></a> Cross domain policies</span></span>  
  
-   <span data-ttu-id="d5cf9-107">[Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör API: et åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-107">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
-   <span data-ttu-id="d5cf9-108">[CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöd till en åtgärd eller en API för att tillåta domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-108">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
-   <span data-ttu-id="d5cf9-109">[Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) -lägger till JSON med stöd för utfyllnad (hanteras JSONP) till en åtgärd eller en API för att tillåta domänerna anrop från JavaScript-webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-109">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
##  <span data-ttu-id="d5cf9-110"><a name="AllowCrossDomainCalls"></a>Tillåta anrop mellan domäner</span><span class="sxs-lookup"><span data-stu-id="d5cf9-110"><a name="AllowCrossDomainCalls"></a> Allow cross-domain calls</span></span>  
 <span data-ttu-id="d5cf9-111">Använd den `cross-domain` princip för att nå API: et från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-111">Use the `cross-domain` policy to make the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="d5cf9-112">Principframställning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-112">Policy statement</span></span>  
  
```xml  
<cross-domain>  
   <!-Policy configuration is in the Adobe cross-domain policy file format,   
      see http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html-->  
</cross-domain>  
```  
  
### <a name="example"></a><span data-ttu-id="d5cf9-113">Exempel</span><span class="sxs-lookup"><span data-stu-id="d5cf9-113">Example</span></span>  
  
```xml  
<cross-domain>  
    <cross-domain-policy>  
        <allow-http-request-headers-from domain='*' headers='*' />  
    </cross-domain-policy>  
</cross-domain>  
```  
  
### <a name="elements"></a><span data-ttu-id="d5cf9-114">Element</span><span class="sxs-lookup"><span data-stu-id="d5cf9-114">Elements</span></span>  
  
|<span data-ttu-id="d5cf9-115">Namn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-115">Name</span></span>|<span data-ttu-id="d5cf9-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-116">Description</span></span>|<span data-ttu-id="d5cf9-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="d5cf9-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="d5cf9-118">mellan domäner</span><span class="sxs-lookup"><span data-stu-id="d5cf9-118">cross-domain</span></span>|<span data-ttu-id="d5cf9-119">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-119">Root element.</span></span> <span data-ttu-id="d5cf9-120">Underordnade element måste motsvara den [Adobe domänerna filen principspecifikationen](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-120">Child elements must conform to the [Adobe cross-domain policy file specification](http://www.adobe.com/devnet/articles/crossdomain_policy_file_spec.html).</span></span>|<span data-ttu-id="d5cf9-121">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-121">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="d5cf9-122">Användning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-122">Usage</span></span>  
 <span data-ttu-id="d5cf9-123">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-123">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="d5cf9-124">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="d5cf9-124">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="d5cf9-125">**Princip för scope:** globala</span><span class="sxs-lookup"><span data-stu-id="d5cf9-125">**Policy scopes:** global</span></span>  
  
##  <span data-ttu-id="d5cf9-126"><a name="CORS"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="d5cf9-126"><a name="CORS"></a> CORS</span></span>  
 <span data-ttu-id="d5cf9-127">Den `cors` principen lägger till resursdelning för korsande ursprung (CORS) stöd till en åtgärd eller en API för att tillåta domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-127">The `cors` policy adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
 <span data-ttu-id="d5cf9-128">CORS kan en webbläsare och en server för att interagera och avgöra om att tillåta specifika cross-origin-begäranden (d.v.s. XMLHttpRequests anrop från JavaScript på en webbsida till andra domäner) eller inte.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-128">CORS allows a browser and a server to interact and determine whether or not to allow specific cross-origin requests (i.e. XMLHttpRequests calls made from JavaScript on a web page to other domains).</span></span> <span data-ttu-id="d5cf9-129">Detta ger bättre flexibilitet än att bara tillåta samma origin-begäranden, men det är säkrare än att tillåta alla cross-origin-begäranden.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-129">This allows for more flexibility than only allowing same-origin requests, but is more secure than allowing all cross-origin requests.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="d5cf9-130">Principframställning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-130">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="d5cf9-131">Exempel</span><span class="sxs-lookup"><span data-stu-id="d5cf9-131">Example</span></span>  
 <span data-ttu-id="d5cf9-132">Det här exemplet visar hur du stöder före svarta begäranden, till exempel de som har anpassade huvuden eller andra metoder än GET och POST.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-132">This example demonstrates how to support pre-flight requests, such as those with custom headers or methods other than GET and POST.</span></span> <span data-ttu-id="d5cf9-133">För att stödja anpassade sidhuvuden och ytterligare HTTP-verb, Använd den `allowed-methods` och `allowed-headers` avsnitten som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-133">To support custom headers and additional HTTP verbs, use the `allowed-methods` and `allowed-headers` sections as shown in the following example.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="d5cf9-134">Element</span><span class="sxs-lookup"><span data-stu-id="d5cf9-134">Elements</span></span>  
  
|<span data-ttu-id="d5cf9-135">Namn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-135">Name</span></span>|<span data-ttu-id="d5cf9-136">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-136">Description</span></span>|<span data-ttu-id="d5cf9-137">Krävs</span><span class="sxs-lookup"><span data-stu-id="d5cf9-137">Required</span></span>|<span data-ttu-id="d5cf9-138">Standard</span><span class="sxs-lookup"><span data-stu-id="d5cf9-138">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="d5cf9-139">cors</span><span class="sxs-lookup"><span data-stu-id="d5cf9-139">cors</span></span>|<span data-ttu-id="d5cf9-140">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-140">Root element.</span></span>|<span data-ttu-id="d5cf9-141">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-141">Yes</span></span>|<span data-ttu-id="d5cf9-142">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-142">N/A</span></span>|  
|<span data-ttu-id="d5cf9-143">tillåtna ursprung</span><span class="sxs-lookup"><span data-stu-id="d5cf9-143">allowed-origins</span></span>|<span data-ttu-id="d5cf9-144">Innehåller `origin` element som beskriver de tillåtna ursprung för domäner begäranden.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-144">Contains `origin` elements that describe the allowed origins for cross-domain requests.</span></span> <span data-ttu-id="d5cf9-145">`allowed-origins`kan innehålla antingen en enskild `origin` element som anger `*` så att alla ursprung, eller en eller flera `origin` element som innehåller en URI.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-145">`allowed-origins` can contain either a single `origin` element that specifies `*` to allow any origin, or one or more `origin` elements that contain a URI.</span></span>|<span data-ttu-id="d5cf9-146">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-146">Yes</span></span>|<span data-ttu-id="d5cf9-147">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-147">N/A</span></span>|  
|<span data-ttu-id="d5cf9-148">ursprung</span><span class="sxs-lookup"><span data-stu-id="d5cf9-148">origin</span></span>|<span data-ttu-id="d5cf9-149">Värdet kan vara antingen `*` så att alla ursprung eller en URI som anger ett enda ursprung.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-149">The value can be either `*` to allow all origins, or a URI that specifies a single origin.</span></span> <span data-ttu-id="d5cf9-150">URI måste innehålla ett schema, värd och port.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-150">The URI must include a scheme, host, and port.</span></span>|<span data-ttu-id="d5cf9-151">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-151">Yes</span></span>|<span data-ttu-id="d5cf9-152">Om porten anges i en URI: N används port 80 för HTTP och port 443 används för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-152">If the port is omitted in a URI, port 80 is used for HTTP and port 443 is used for HTTPS.</span></span>|  
|<span data-ttu-id="d5cf9-153">tillåtna metoder</span><span class="sxs-lookup"><span data-stu-id="d5cf9-153">allowed-methods</span></span>|<span data-ttu-id="d5cf9-154">Det här elementet krävs om metoder än hämta eller POST tillåts.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-154">This element is required if methods other than GET or POST are allowed.</span></span> <span data-ttu-id="d5cf9-155">Innehåller `method` element som anger HTTP-verb som stöds.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-155">Contains `method` elements that specify the supported HTTP verbs.</span></span>|<span data-ttu-id="d5cf9-156">Nej</span><span class="sxs-lookup"><span data-stu-id="d5cf9-156">No</span></span>|<span data-ttu-id="d5cf9-157">Om det här avsnittet inte finns, stöds GET och POST.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-157">If this section is not present, GET and POST are supported.</span></span>|  
|<span data-ttu-id="d5cf9-158">Metoden</span><span class="sxs-lookup"><span data-stu-id="d5cf9-158">method</span></span>|<span data-ttu-id="d5cf9-159">Anger ett HTTP-verb.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-159">Specifies an HTTP verb.</span></span>|<span data-ttu-id="d5cf9-160">Minst en `method` -element krävs om de `allowed-methods` avsnittet finns.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-160">At least one `method` element is required if the `allowed-methods` section is present.</span></span>|<span data-ttu-id="d5cf9-161">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-161">N/A</span></span>|  
|<span data-ttu-id="d5cf9-162">tillåtna rubriker</span><span class="sxs-lookup"><span data-stu-id="d5cf9-162">allowed-headers</span></span>|<span data-ttu-id="d5cf9-163">Det här elementet innehåller `header` element anger namn på rubriker som kan ingå i begäran.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-163">This element contains `header` elements specifying names of the headers that can be included in the request.</span></span>|<span data-ttu-id="d5cf9-164">Nej</span><span class="sxs-lookup"><span data-stu-id="d5cf9-164">No</span></span>|<span data-ttu-id="d5cf9-165">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-165">N/A</span></span>|  
|<span data-ttu-id="d5cf9-166">exponera rubriker</span><span class="sxs-lookup"><span data-stu-id="d5cf9-166">expose-headers</span></span>|<span data-ttu-id="d5cf9-167">Det här elementet innehåller `header` element anger namn på rubriker som ska vara tillgängliga för klienten.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-167">This element contains `header` elements specifying names of the headers that will be accessible by the client.</span></span>|<span data-ttu-id="d5cf9-168">Nej</span><span class="sxs-lookup"><span data-stu-id="d5cf9-168">No</span></span>|<span data-ttu-id="d5cf9-169">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-169">N/A</span></span>|  
|<span data-ttu-id="d5cf9-170">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="d5cf9-170">header</span></span>|<span data-ttu-id="d5cf9-171">Anger ett namn för sidhuvudet.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-171">Specifies a header name.</span></span>|<span data-ttu-id="d5cf9-172">Minst en `header` -element krävs i `allowed-headers` eller `expose-headers` om avsnittet finns.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-172">At least one `header` element is required in `allowed-headers` or `expose-headers` if the section is present.</span></span>|<span data-ttu-id="d5cf9-173">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-173">N/A</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="d5cf9-174">Attribut</span><span class="sxs-lookup"><span data-stu-id="d5cf9-174">Attributes</span></span>  
  
|<span data-ttu-id="d5cf9-175">Namn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-175">Name</span></span>|<span data-ttu-id="d5cf9-176">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-176">Description</span></span>|<span data-ttu-id="d5cf9-177">Krävs</span><span class="sxs-lookup"><span data-stu-id="d5cf9-177">Required</span></span>|<span data-ttu-id="d5cf9-178">Standard</span><span class="sxs-lookup"><span data-stu-id="d5cf9-178">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="d5cf9-179">Tillåt autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d5cf9-179">allow-credentials</span></span>|<span data-ttu-id="d5cf9-180">Den `Access-Control-Allow-Credentials` sidhuvud preflight-svar anges till värdet för det här attributet och påverka klientens möjlighet att skicka autentiseringsuppgifter i domänerna begäranden.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-180">The `Access-Control-Allow-Credentials` header in the preflight response will be set to the value of this attribute and affect the client’s ability to submit credentials in cross-domain requests.</span></span>|<span data-ttu-id="d5cf9-181">Nej</span><span class="sxs-lookup"><span data-stu-id="d5cf9-181">No</span></span>|<span data-ttu-id="d5cf9-182">FALSKT</span><span class="sxs-lookup"><span data-stu-id="d5cf9-182">false</span></span>|  
|<span data-ttu-id="d5cf9-183">preflight-resultat-max-ålder</span><span class="sxs-lookup"><span data-stu-id="d5cf9-183">preflight-result-max-age</span></span>|<span data-ttu-id="d5cf9-184">Den `Access-Control-Max-Age` sidhuvud preflight-svar anges till värdet för det här attributet och påverka användaragenten möjlighet att före svarta cachesvar.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-184">The `Access-Control-Max-Age` header in the preflight response will be set to the value of this attribute and affect the user agent’s ability to cache pre-flight response.</span></span>|<span data-ttu-id="d5cf9-185">Nej</span><span class="sxs-lookup"><span data-stu-id="d5cf9-185">No</span></span>|<span data-ttu-id="d5cf9-186">0</span><span class="sxs-lookup"><span data-stu-id="d5cf9-186">0</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="d5cf9-187">Användning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-187">Usage</span></span>  
 <span data-ttu-id="d5cf9-188">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-188">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="d5cf9-189">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="d5cf9-189">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="d5cf9-190">**Princip för scope:** API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="d5cf9-190">**Policy scopes:** API, operation</span></span>  
  
##  <span data-ttu-id="d5cf9-191"><a name="JSONP"></a>HANTERAS JSONP</span><span class="sxs-lookup"><span data-stu-id="d5cf9-191"><a name="JSONP"></a> JSONP</span></span>  
 <span data-ttu-id="d5cf9-192">Den `jsonp` principen lägger till JSON med stöd för utfyllnad (hanteras JSONP) i en åtgärd eller en API för att tillåta domänerna anrop från JavaScript-webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-192">The `jsonp` policy adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span> <span data-ttu-id="d5cf9-193">Hanteras JSONP är en metod som används i JavaScript-program för att begäran om data från en server i en annan domän.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-193">JSONP is a method used in JavaScript programs to request data from a server in a different domain.</span></span> <span data-ttu-id="d5cf9-194">Hanteras JSONP kringgår den begränsning som tillämpas av de flesta webbläsare där åtkomst till webbsidor måste vara i samma domän.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-194">JSONP bypasses the limitation enforced by most web browsers where access to web pages must be in the same domain.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="d5cf9-195">Principframställning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-195">Policy statement</span></span>  
  
```xml  
<jsonp callback-parameter-name="callback function name" />  
```  
  
### <a name="example"></a><span data-ttu-id="d5cf9-196">Exempel</span><span class="sxs-lookup"><span data-stu-id="d5cf9-196">Example</span></span>  
  
```xml  
<jsonp callback-parameter-name="cb" />  
```  
  
 <span data-ttu-id="d5cf9-197">Om du anropar metoden utan parametern motringning? cb = XXX returneras oformaterade JSON (utan en funktionen anropet wrapper).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-197">If you call the method without the callback parameter ?cb=XXX it will return plain JSON (without a function call wrapper).</span></span>  
  
 <span data-ttu-id="d5cf9-198">Om du lägger till parametern återanrop `?cb=XXX` returneras ett hanteras JSONP-resultat, wrapping ursprungliga JSON resultat runt Återanropsfunktionen som`XYZ('<json result goes here>');`</span><span class="sxs-lookup"><span data-stu-id="d5cf9-198">If you add the callback parameter `?cb=XXX` it will return a JSONP result, wrapping the original JSON results around the callback function like `XYZ('<json result goes here>');`</span></span>  
  
### <a name="elements"></a><span data-ttu-id="d5cf9-199">Element</span><span class="sxs-lookup"><span data-stu-id="d5cf9-199">Elements</span></span>  
  
|<span data-ttu-id="d5cf9-200">Namn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-200">Name</span></span>|<span data-ttu-id="d5cf9-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-201">Description</span></span>|<span data-ttu-id="d5cf9-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="d5cf9-202">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="d5cf9-203">hanteras jsonp</span><span class="sxs-lookup"><span data-stu-id="d5cf9-203">jsonp</span></span>|<span data-ttu-id="d5cf9-204">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-204">Root element.</span></span>|<span data-ttu-id="d5cf9-205">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-205">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="d5cf9-206">Attribut</span><span class="sxs-lookup"><span data-stu-id="d5cf9-206">Attributes</span></span>  
  
|<span data-ttu-id="d5cf9-207">Namn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-207">Name</span></span>|<span data-ttu-id="d5cf9-208">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-208">Description</span></span>|<span data-ttu-id="d5cf9-209">Krävs</span><span class="sxs-lookup"><span data-stu-id="d5cf9-209">Required</span></span>|<span data-ttu-id="d5cf9-210">Standard</span><span class="sxs-lookup"><span data-stu-id="d5cf9-210">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="d5cf9-211">motringning parameternamn</span><span class="sxs-lookup"><span data-stu-id="d5cf9-211">callback-parameter-name</span></span>|<span data-ttu-id="d5cf9-212">De domäner JavaScript-anrop med det fullständigt kvalificerade domännamnet där funktionen finns prefixet.</span><span class="sxs-lookup"><span data-stu-id="d5cf9-212">The cross-domain JavaScript function call prefixed with the fully qualified domain name where the function resides.</span></span>|<span data-ttu-id="d5cf9-213">Ja</span><span class="sxs-lookup"><span data-stu-id="d5cf9-213">Yes</span></span>|<span data-ttu-id="d5cf9-214">Saknas</span><span class="sxs-lookup"><span data-stu-id="d5cf9-214">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="d5cf9-215">Användning</span><span class="sxs-lookup"><span data-stu-id="d5cf9-215">Usage</span></span>  
 <span data-ttu-id="d5cf9-216">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-216">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="d5cf9-217">**Avsnitt i princip:** utgående</span><span class="sxs-lookup"><span data-stu-id="d5cf9-217">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="d5cf9-218">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="d5cf9-218">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="d5cf9-219">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5cf9-219">Next steps</span></span>
<span data-ttu-id="d5cf9-220">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="d5cf9-220">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  