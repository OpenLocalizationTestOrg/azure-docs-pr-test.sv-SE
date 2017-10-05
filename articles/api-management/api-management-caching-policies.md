---
title: Azure API Management cachelagring principer | Microsoft Docs
description: "Läs mer om cachelagring principer som är tillgängliga för användning i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2a8f012e7e223ef5c1683c8a6c5ecf2f3e96bed8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-caching-policies"></a><span data-ttu-id="e5d0c-103">Cachelagring API Management-principer</span><span class="sxs-lookup"><span data-stu-id="e5d0c-103">API Management caching policies</span></span>
<span data-ttu-id="e5d0c-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="e5d0c-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="e5d0c-106"><a name="CachingPolicies"></a>Principer för cachelagring</span><span class="sxs-lookup"><span data-stu-id="e5d0c-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="e5d0c-107">Svaret cachelagring principer</span><span class="sxs-lookup"><span data-stu-id="e5d0c-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="e5d0c-108">[Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="e5d0c-109">[Arkivet ska cachelagras](api-management-caching-policies.md#StoreToCache) -cachelagrar svar enligt den angivna konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-109">[Store to cache](api-management-caching-policies.md#StoreToCache) - Caches responses according to the specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="e5d0c-110">Värdet cachelagring principer</span><span class="sxs-lookup"><span data-stu-id="e5d0c-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="e5d0c-111">[Hämta ett värde från cache](#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="e5d0c-112">[Lagrar värdet i cache](#StoreToCacheByKey) -lagra ett objekt i cacheminnet av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-112">[Store value in cache](#StoreToCacheByKey) - Store an item in the cache by key.</span></span>  
  
    -   <span data-ttu-id="e5d0c-113">[Ta bort värdet från cache](#RemoveCacheByKey) -ta bort ett objekt i cacheminnet av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>  
  
##  <span data-ttu-id="e5d0c-114"><a name="GetFromCache"></a>Hämta från cache</span><span class="sxs-lookup"><span data-stu-id="e5d0c-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="e5d0c-115">Använd den `cache-lookup` Grupprincip för att utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-115">Use the `cache-lookup` policy to perform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="e5d0c-116">Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="e5d0c-117">Svaret cachelagring minskar bandbredden och bearbetningskrav införts på backend web server och driftmiljön svarstiden uppfattas av API-konsumenter.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-117">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="e5d0c-118">Den här principen måste ha en motsvarande [Store cacheminne](api-management-caching-policies.md#StoreToCache) princip.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-118">This policy must have a corresponding [Store to cache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="e5d0c-119">Principframställning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-119">Policy statement</span></span>  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">  
  <vary-by-header>Accept</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Accept-Charset</vary-by-header>  
  <!-- should be present in most cases -->  
  <vary-by-header>Authorization</vary-by-header>  
  <!-- should be present when allow-authorized-response-caching is "true"-->  
  <vary-by-header>header name</vary-by-header>  
  <!-- optional, can repeated several times -->  
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>  
  <!-- optional, can repeated several times -->  
</cache-lookup>  
```  
  
### <a name="examples"></a><span data-ttu-id="e5d0c-120">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="e5d0c-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-121">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true">  
            <vary-by-query-parameter>version</vary-by-query-parameter>  
        </cache-lookup>           
    </inbound>  
    <outbound>  
        <cache-store duration="seconds" />  
        <base />          
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="e5d0c-122">Exempel på användning av princip uttryck</span><span class="sxs-lookup"><span data-stu-id="e5d0c-122">Example using policy expressions</span></span>  
 <span data-ttu-id="e5d0c-123">Det här exemplet illustrerar hur till att konfigurera API Management svar cachevaraktighet som matchar svar cachelagring av serverdelstjänsten som anges av tjänsten säkerhetskopierade `Cache-Control` direktiv.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-123">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="e5d0c-124">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 25:25.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="e5d0c-125">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="e5d0c-126">Element</span><span class="sxs-lookup"><span data-stu-id="e5d0c-126">Elements</span></span>  
  
|<span data-ttu-id="e5d0c-127">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-127">Name</span></span>|<span data-ttu-id="e5d0c-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-128">Description</span></span>|<span data-ttu-id="e5d0c-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="e5d0c-130">cache-sökning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-130">cache-lookup</span></span>|<span data-ttu-id="e5d0c-131">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-131">Root element.</span></span>|<span data-ttu-id="e5d0c-132">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-132">Yes</span></span>|  
|<span data-ttu-id="e5d0c-133">variera av huvud</span><span class="sxs-lookup"><span data-stu-id="e5d0c-133">vary-by-header</span></span>|<span data-ttu-id="e5d0c-134">Börjar cachelagra svar per värde för angivna huvudet, till exempel acceptera Accept-Charset Accept-Encoding, acceptera-språk, auktorisering, förväntat, från värden, If-Match.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="e5d0c-135">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-135">No</span></span>|  
|<span data-ttu-id="e5d0c-136">variera-av--frågeparameter</span><span class="sxs-lookup"><span data-stu-id="e5d0c-136">vary-by-query-parameter</span></span>|<span data-ttu-id="e5d0c-137">Starta cachelagring svar per värde för angivna Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="e5d0c-138">Ange en eller flera parametrar.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="e5d0c-139">Använd semikolon som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-139">Use semicolon as a separator.</span></span> <span data-ttu-id="e5d0c-140">Om inget anges används alla Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="e5d0c-141">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="e5d0c-142">Attribut</span><span class="sxs-lookup"><span data-stu-id="e5d0c-142">Attributes</span></span>  
  
|<span data-ttu-id="e5d0c-143">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-143">Name</span></span>|<span data-ttu-id="e5d0c-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-144">Description</span></span>|<span data-ttu-id="e5d0c-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-145">Required</span></span>|<span data-ttu-id="e5d0c-146">Standard</span><span class="sxs-lookup"><span data-stu-id="e5d0c-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="e5d0c-147">Tillåt privat-svar-cachelagring</span><span class="sxs-lookup"><span data-stu-id="e5d0c-147">allow-private-response-caching</span></span>|<span data-ttu-id="e5d0c-148">Om värdet är `true`, tillåter cachelagring av begäranden som innehåller ett Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-148">When set to `true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="e5d0c-149">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-149">No</span></span>|<span data-ttu-id="e5d0c-150">FALSKT</span><span class="sxs-lookup"><span data-stu-id="e5d0c-150">false</span></span>|  
|<span data-ttu-id="e5d0c-151">underordnade-cachelagring-typ</span><span class="sxs-lookup"><span data-stu-id="e5d0c-151">downstream-caching-type</span></span>|<span data-ttu-id="e5d0c-152">Det här attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-152">This attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="e5d0c-153">-Ingen - underordnade cachelagring tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="e5d0c-154">-privat - cachelagring underordnade privata.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="e5d0c-155">-offentlig - cachelagring privata och delade underordnade.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="e5d0c-156">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-156">No</span></span>|<span data-ttu-id="e5d0c-157">Ingen</span><span class="sxs-lookup"><span data-stu-id="e5d0c-157">none</span></span>|  
|<span data-ttu-id="e5d0c-158">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="e5d0c-158">must-revalidate</span></span>|<span data-ttu-id="e5d0c-159">När efterföljande cachelagring av det här attributet aktiverar eller inaktiverar det `must-revalidate` cache-control-direktiv i gateway-svar.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-159">When downstream caching is enabled this attribute turns on or off  the `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="e5d0c-160">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-160">No</span></span>|<span data-ttu-id="e5d0c-161">SANT</span><span class="sxs-lookup"><span data-stu-id="e5d0c-161">true</span></span>|  
|<span data-ttu-id="e5d0c-162">variera av utvecklare</span><span class="sxs-lookup"><span data-stu-id="e5d0c-162">vary-by-developer</span></span>|<span data-ttu-id="e5d0c-163">Ange till `true` till cache svar per developer nyckel.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-163">Set to `true` to cache responses per developer key.</span></span>|<span data-ttu-id="e5d0c-164">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-164">No</span></span>|<span data-ttu-id="e5d0c-165">FALSKT</span><span class="sxs-lookup"><span data-stu-id="e5d0c-165">false</span></span>|  
|<span data-ttu-id="e5d0c-166">variera-av-utvecklare-grupper</span><span class="sxs-lookup"><span data-stu-id="e5d0c-166">vary-by-developer-groups</span></span>|<span data-ttu-id="e5d0c-167">Ange till `true` till cache svar per användarroll.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-167">Set to `true` to cache responses per user role.</span></span>|<span data-ttu-id="e5d0c-168">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-168">No</span></span>|<span data-ttu-id="e5d0c-169">FALSKT</span><span class="sxs-lookup"><span data-stu-id="e5d0c-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="e5d0c-170">Användning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-170">Usage</span></span>  
 <span data-ttu-id="e5d0c-171">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-171">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="e5d0c-172">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="e5d0c-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="e5d0c-173">**Princip för scope:** API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="e5d0c-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="e5d0c-174"><a name="StoreToCache"></a>Lagra cacheminne</span><span class="sxs-lookup"><span data-stu-id="e5d0c-174"><a name="StoreToCache"></a> Store to cache</span></span>  
 <span data-ttu-id="e5d0c-175">Den `cache-store` princip cachelagrar svar enligt de angivna inställningarna.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-175">The `cache-store` policy caches responses according to the specified cache settings.</span></span> <span data-ttu-id="e5d0c-176">Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="e5d0c-177">Svaret cachelagring minskar bandbredden och bearbetningskrav införts på backend web server och driftmiljön svarstiden uppfattas av API-konsumenter.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-177">Response caching reduces bandwidth and processing requirements imposed on the backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="e5d0c-178">Den här principen måste ha en motsvarande [hämta från cache](api-management-caching-policies.md#GetFromCache) princip.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="e5d0c-179">Principframställning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="e5d0c-180">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="e5d0c-181">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-181">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
          <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">  
                <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->  
        </cache-lookup>  
    </inbound>  
    <outbound>  
        <base />  
            <cache-store duration="3600" />  
    </outbound>  
</policies>  
```  
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="e5d0c-182">Exempel på användning av princip uttryck</span><span class="sxs-lookup"><span data-stu-id="e5d0c-182">Example using policy expressions</span></span>  
 <span data-ttu-id="e5d0c-183">Det här exemplet illustrerar hur till att konfigurera API Management svar cachevaraktighet som matchar svar cachelagring av serverdelstjänsten som anges av tjänsten säkerhetskopierade `Cache-Control` direktiv.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-183">This example shows how to to configure API Management response caching duration that matches the response caching of the backend service as specified by the backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="e5d0c-184">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 25:25.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 25:25.</span></span>  
  
```xml  
<!-- The following cache policy snippets demonstrate how to control API Management reponse cache duration with Cache-Control headers sent by the backend service. -->  
  
<!-- Copy this snippet into the inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="e5d0c-185">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="e5d0c-186">Element</span><span class="sxs-lookup"><span data-stu-id="e5d0c-186">Elements</span></span>  
  
|<span data-ttu-id="e5d0c-187">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-187">Name</span></span>|<span data-ttu-id="e5d0c-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-188">Description</span></span>|<span data-ttu-id="e5d0c-189">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="e5d0c-190">cache-lagra</span><span class="sxs-lookup"><span data-stu-id="e5d0c-190">cache-store</span></span>|<span data-ttu-id="e5d0c-191">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-191">Root element.</span></span>|<span data-ttu-id="e5d0c-192">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="e5d0c-193">Attribut</span><span class="sxs-lookup"><span data-stu-id="e5d0c-193">Attributes</span></span>  
  
|<span data-ttu-id="e5d0c-194">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-194">Name</span></span>|<span data-ttu-id="e5d0c-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-195">Description</span></span>|<span data-ttu-id="e5d0c-196">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-196">Required</span></span>|<span data-ttu-id="e5d0c-197">Standard</span><span class="sxs-lookup"><span data-stu-id="e5d0c-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="e5d0c-198">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="e5d0c-198">duration</span></span>|<span data-ttu-id="e5d0c-199">Time-to-live cachelagrade poster som anges i sekunder.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-199">Time-to-live of the cached entries, specified in seconds.</span></span>|<span data-ttu-id="e5d0c-200">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-200">Yes</span></span>|<span data-ttu-id="e5d0c-201">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="e5d0c-202">Användning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-202">Usage</span></span>  
 <span data-ttu-id="e5d0c-203">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-203">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="e5d0c-204">**Avsnitt i princip:** utgående</span><span class="sxs-lookup"><span data-stu-id="e5d0c-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="e5d0c-205">**Princip för scope:** API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="e5d0c-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="e5d0c-206"><a name="GetFromCacheByKey"></a>Hämta ett värde från cache</span><span class="sxs-lookup"><span data-stu-id="e5d0c-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="e5d0c-207">Använd den `cache-lookup-value` princip för att utföra cache-sökning av nyckeln och cachelagrade returvärde.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-207">Use the `cache-lookup-value` policy to perform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="e5d0c-208">Nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-208">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="e5d0c-209">Den här principen måste ha en motsvarande [lagrar värdet i cache](#StoreToCacheByKey) princip.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="e5d0c-210">Principframställning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value to use if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="e5d0c-211">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-211">Example</span></span>  
 <span data-ttu-id="e5d0c-212">Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="e5d0c-213">Element</span><span class="sxs-lookup"><span data-stu-id="e5d0c-213">Elements</span></span>  
  
|<span data-ttu-id="e5d0c-214">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-214">Name</span></span>|<span data-ttu-id="e5d0c-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-215">Description</span></span>|<span data-ttu-id="e5d0c-216">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="e5d0c-217">cache-sökning-värde</span><span class="sxs-lookup"><span data-stu-id="e5d0c-217">cache-lookup-value</span></span>|<span data-ttu-id="e5d0c-218">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-218">Root element.</span></span>|<span data-ttu-id="e5d0c-219">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="e5d0c-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="e5d0c-220">Attributes</span></span>  
  
|<span data-ttu-id="e5d0c-221">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-221">Name</span></span>|<span data-ttu-id="e5d0c-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-222">Description</span></span>|<span data-ttu-id="e5d0c-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-223">Required</span></span>|<span data-ttu-id="e5d0c-224">Standard</span><span class="sxs-lookup"><span data-stu-id="e5d0c-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="e5d0c-225">standard-värde</span><span class="sxs-lookup"><span data-stu-id="e5d0c-225">default-value</span></span>|<span data-ttu-id="e5d0c-226">Ett värde som ska tilldelas variabeln om cache nyckelsökningen resulterade i en miss.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-226">A value that will be assigned to the variable if the cache key lookup resulted in a miss.</span></span> <span data-ttu-id="e5d0c-227">Om det här attributet inte anges `null` är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="e5d0c-228">Nej</span><span class="sxs-lookup"><span data-stu-id="e5d0c-228">No</span></span>|`null`|  
|<span data-ttu-id="e5d0c-229">key</span><span class="sxs-lookup"><span data-stu-id="e5d0c-229">key</span></span>|<span data-ttu-id="e5d0c-230">Cachelagra värdet för nyckeln ska användas i sökningen.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-230">Cache key value to use in the lookup.</span></span>|<span data-ttu-id="e5d0c-231">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-231">Yes</span></span>|<span data-ttu-id="e5d0c-232">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-232">N/A</span></span>|  
|<span data-ttu-id="e5d0c-233">variabeln namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-233">variable-name</span></span>|<span data-ttu-id="e5d0c-234">Namnet på den [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) looked in värdet kommer att tilldelas, om sökning lyckas.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-234">Name of the [context variable](api-management-policy-expressions.md#ContextVariables) the looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="e5d0c-235">Om sökning resulterar i en miss, variabeln tilldelas värdet för den `default-value` attribut eller `null`, om den `default-value` attributet utelämnas.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-235">If lookup results in a miss, the variable will be assigned the value of the `default-value` attribute or `null`, if the `default-value` attribute is omitted.</span></span>|<span data-ttu-id="e5d0c-236">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-236">Yes</span></span>|<span data-ttu-id="e5d0c-237">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="e5d0c-238">Användning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-238">Usage</span></span>  
 <span data-ttu-id="e5d0c-239">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-239">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="e5d0c-240">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="e5d0c-241">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="e5d0c-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="e5d0c-242"><a name="StoreToCacheByKey"></a>Lagrar värdet i cache</span><span class="sxs-lookup"><span data-stu-id="e5d0c-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="e5d0c-243">Den `cache-store-value` utför cachelagring av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-243">The `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="e5d0c-244">Nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-244">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="e5d0c-245">Den här principen måste ha en motsvarande [hämta värdet från cache](#GetFromCacheByKey) princip.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="e5d0c-246">Principframställning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value to cache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="e5d0c-247">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-247">Example</span></span>  
 <span data-ttu-id="e5d0c-248">Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="e5d0c-249">Element</span><span class="sxs-lookup"><span data-stu-id="e5d0c-249">Elements</span></span>  
  
|<span data-ttu-id="e5d0c-250">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-250">Name</span></span>|<span data-ttu-id="e5d0c-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-251">Description</span></span>|<span data-ttu-id="e5d0c-252">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="e5d0c-253">cache-lagra-värde</span><span class="sxs-lookup"><span data-stu-id="e5d0c-253">cache-store-value</span></span>|<span data-ttu-id="e5d0c-254">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-254">Root element.</span></span>|<span data-ttu-id="e5d0c-255">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="e5d0c-256">Attribut</span><span class="sxs-lookup"><span data-stu-id="e5d0c-256">Attributes</span></span>  
  
|<span data-ttu-id="e5d0c-257">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-257">Name</span></span>|<span data-ttu-id="e5d0c-258">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-258">Description</span></span>|<span data-ttu-id="e5d0c-259">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-259">Required</span></span>|<span data-ttu-id="e5d0c-260">Standard</span><span class="sxs-lookup"><span data-stu-id="e5d0c-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="e5d0c-261">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="e5d0c-261">duration</span></span>|<span data-ttu-id="e5d0c-262">Värdet cachelagras för angivna duration-värdet som anges i sekunder.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-262">Value will be cached for the provided duration value, specified in seconds.</span></span>|<span data-ttu-id="e5d0c-263">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-263">Yes</span></span>|<span data-ttu-id="e5d0c-264">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-264">N/A</span></span>|  
|<span data-ttu-id="e5d0c-265">key</span><span class="sxs-lookup"><span data-stu-id="e5d0c-265">key</span></span>|<span data-ttu-id="e5d0c-266">Cachenyckel värdet lagras.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-266">Cache key the value will be stored under.</span></span>|<span data-ttu-id="e5d0c-267">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-267">Yes</span></span>|<span data-ttu-id="e5d0c-268">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-268">N/A</span></span>|  
|<span data-ttu-id="e5d0c-269">värde</span><span class="sxs-lookup"><span data-stu-id="e5d0c-269">value</span></span>|<span data-ttu-id="e5d0c-270">Värdet som ska cachelagras.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-270">The value to be cached.</span></span>|<span data-ttu-id="e5d0c-271">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-271">Yes</span></span>|<span data-ttu-id="e5d0c-272">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="e5d0c-273">Användning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-273">Usage</span></span>  
 <span data-ttu-id="e5d0c-274">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="e5d0c-275">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="e5d0c-276">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="e5d0c-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="e5d0c-277"><a name="RemoveCacheByKey"></a>Ta bort värdet från cache</span><span class="sxs-lookup"><span data-stu-id="e5d0c-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="e5d0c-278">Den `cache-remove-value` tar bort en cachelagrade objekt som identifieras av dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-278">The             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="e5d0c-279">Nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-279">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="e5d0c-280">Principframställning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="e5d0c-281">Exempel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="e5d0c-282">Element</span><span class="sxs-lookup"><span data-stu-id="e5d0c-282">Elements</span></span>  
  
|<span data-ttu-id="e5d0c-283">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-283">Name</span></span>|<span data-ttu-id="e5d0c-284">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-284">Description</span></span>|<span data-ttu-id="e5d0c-285">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="e5d0c-286">cache-remove-värde</span><span class="sxs-lookup"><span data-stu-id="e5d0c-286">cache-remove-value</span></span>|<span data-ttu-id="e5d0c-287">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-287">Root element.</span></span>|<span data-ttu-id="e5d0c-288">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="e5d0c-289">Attribut</span><span class="sxs-lookup"><span data-stu-id="e5d0c-289">Attributes</span></span>  
  
|<span data-ttu-id="e5d0c-290">Namn</span><span class="sxs-lookup"><span data-stu-id="e5d0c-290">Name</span></span>|<span data-ttu-id="e5d0c-291">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-291">Description</span></span>|<span data-ttu-id="e5d0c-292">Krävs</span><span class="sxs-lookup"><span data-stu-id="e5d0c-292">Required</span></span>|<span data-ttu-id="e5d0c-293">Standard</span><span class="sxs-lookup"><span data-stu-id="e5d0c-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="e5d0c-294">key</span><span class="sxs-lookup"><span data-stu-id="e5d0c-294">key</span></span>|<span data-ttu-id="e5d0c-295">Nyckeln för cachelagrad värdet som ska tas bort från cachen.</span><span class="sxs-lookup"><span data-stu-id="e5d0c-295">The key of the previously cached value to be removed from the cache.</span></span>|<span data-ttu-id="e5d0c-296">Ja</span><span class="sxs-lookup"><span data-stu-id="e5d0c-296">Yes</span></span>|<span data-ttu-id="e5d0c-297">Saknas</span><span class="sxs-lookup"><span data-stu-id="e5d0c-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="e5d0c-298">Användning</span><span class="sxs-lookup"><span data-stu-id="e5d0c-298">Usage</span></span>  
 <span data-ttu-id="e5d0c-299">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="e5d0c-299">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="e5d0c-300">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="e5d0c-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="e5d0c-301">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="e5d0c-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="e5d0c-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5d0c-302">Next steps</span></span>
<span data-ttu-id="e5d0c-303">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="e5d0c-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  