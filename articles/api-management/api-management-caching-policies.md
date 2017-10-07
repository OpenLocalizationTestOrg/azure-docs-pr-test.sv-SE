---
title: cachelagring aaaAzure API Management-principer | Microsoft Docs
description: "Läs mer om hello cachelagring principer som är tillgängliga för användning i Azure API Management."
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
ms.openlocfilehash: bd6b0721945609b28dbf6e7ef0631979c08c8c65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-caching-policies"></a><span data-ttu-id="235d0-103">Cachelagring API Management-principer</span><span class="sxs-lookup"><span data-stu-id="235d0-103">API Management caching policies</span></span>
<span data-ttu-id="235d0-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="235d0-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="235d0-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="235d0-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="235d0-106"><a name="CachingPolicies"></a>Principer för cachelagring</span><span class="sxs-lookup"><span data-stu-id="235d0-106"><a name="CachingPolicies"></a> Caching policies</span></span>  
  
-   <span data-ttu-id="235d0-107">Svaret cachelagring principer</span><span class="sxs-lookup"><span data-stu-id="235d0-107">Response caching policies</span></span>  
  
    -   <span data-ttu-id="235d0-108">[Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="235d0-108">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached responses when available.</span></span>  
  
    -   <span data-ttu-id="235d0-109">[Lagra toocache](api-management-caching-policies.md#StoreToCache) - cacheminnen svar enligt toohello angivna konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="235d0-109">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches responses according toohello specified cache control configuration.</span></span>  
  
-   <span data-ttu-id="235d0-110">Värdet cachelagring principer</span><span class="sxs-lookup"><span data-stu-id="235d0-110">Value caching policies</span></span>  
  
    -   <span data-ttu-id="235d0-111">[Hämta ett värde från cache](#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.</span><span class="sxs-lookup"><span data-stu-id="235d0-111">[Get value from cache](#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="235d0-112">[Lagrar värdet i cache](#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="235d0-112">[Store value in cache](#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="235d0-113">[Ta bort värdet från cache](#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="235d0-113">[Remove value from cache](#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
##  <span data-ttu-id="235d0-114"><a name="GetFromCache"></a>Hämta från cache</span><span class="sxs-lookup"><span data-stu-id="235d0-114"><a name="GetFromCache"></a> Get from cache</span></span>  
 <span data-ttu-id="235d0-115">Använd hello `cache-lookup` tooperform principcachen Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="235d0-115">Use hello `cache-lookup` policy tooperform cache look up and return a valid cached response when available.</span></span> <span data-ttu-id="235d0-116">Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="235d0-116">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="235d0-117">Svaret cachelagring minskar bandbredden och bearbetningskrav införts på hello backend web server och driftmiljön svarstid, uppfattas av API-konsumenter.</span><span class="sxs-lookup"><span data-stu-id="235d0-117">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="235d0-118">Den här principen måste ha en motsvarande [Store toocache](api-management-caching-policies.md#StoreToCache) princip.</span><span class="sxs-lookup"><span data-stu-id="235d0-118">This policy must have a corresponding [Store toocache](api-management-caching-policies.md#StoreToCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="235d0-119">Principframställning</span><span class="sxs-lookup"><span data-stu-id="235d0-119">Policy statement</span></span>  
  
```xml  
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression tooevaluate)">  
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
  
### <a name="examples"></a><span data-ttu-id="235d0-120">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-120">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="235d0-121">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-121">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="235d0-122">Exempel på användning av princip uttryck</span><span class="sxs-lookup"><span data-stu-id="235d0-122">Example using policy expressions</span></span>  
 <span data-ttu-id="235d0-123">Det här exemplet illustrerar hur tootooconfigure API Management svaret som matchar hello svar cachelagring av hello serverdelstjänst som anges av hello cachevaraktighet säkerhetskopieras tjänstens `Cache-Control` direktiv.</span><span class="sxs-lookup"><span data-stu-id="235d0-123">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="235d0-124">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too25:25.</span><span class="sxs-lookup"><span data-stu-id="235d0-124">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="235d0-125">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="235d0-125">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="235d0-126">Element</span><span class="sxs-lookup"><span data-stu-id="235d0-126">Elements</span></span>  
  
|<span data-ttu-id="235d0-127">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-127">Name</span></span>|<span data-ttu-id="235d0-128">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-128">Description</span></span>|<span data-ttu-id="235d0-129">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-129">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="235d0-130">cache-sökning</span><span class="sxs-lookup"><span data-stu-id="235d0-130">cache-lookup</span></span>|<span data-ttu-id="235d0-131">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="235d0-131">Root element.</span></span>|<span data-ttu-id="235d0-132">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-132">Yes</span></span>|  
|<span data-ttu-id="235d0-133">variera av huvud</span><span class="sxs-lookup"><span data-stu-id="235d0-133">vary-by-header</span></span>|<span data-ttu-id="235d0-134">Börjar cachelagra svar per värde för angivna huvudet, till exempel acceptera Accept-Charset Accept-Encoding, acceptera-språk, auktorisering, förväntat, från värden, If-Match.</span><span class="sxs-lookup"><span data-stu-id="235d0-134">Start caching responses per value of specified header, such as Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.</span></span>|<span data-ttu-id="235d0-135">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-135">No</span></span>|  
|<span data-ttu-id="235d0-136">variera-av--frågeparameter</span><span class="sxs-lookup"><span data-stu-id="235d0-136">vary-by-query-parameter</span></span>|<span data-ttu-id="235d0-137">Starta cachelagring svar per värde för angivna Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="235d0-137">Start caching responses per value of specified query parameters.</span></span> <span data-ttu-id="235d0-138">Ange en eller flera parametrar.</span><span class="sxs-lookup"><span data-stu-id="235d0-138">Enter a single or multiple parameters.</span></span> <span data-ttu-id="235d0-139">Använd semikolon som avgränsare.</span><span class="sxs-lookup"><span data-stu-id="235d0-139">Use semicolon as a separator.</span></span> <span data-ttu-id="235d0-140">Om inget anges används alla Frågeparametrar.</span><span class="sxs-lookup"><span data-stu-id="235d0-140">If none are specified, all query parameters are used.</span></span>|<span data-ttu-id="235d0-141">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-141">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="235d0-142">Attribut</span><span class="sxs-lookup"><span data-stu-id="235d0-142">Attributes</span></span>  
  
|<span data-ttu-id="235d0-143">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-143">Name</span></span>|<span data-ttu-id="235d0-144">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-144">Description</span></span>|<span data-ttu-id="235d0-145">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-145">Required</span></span>|<span data-ttu-id="235d0-146">Standard</span><span class="sxs-lookup"><span data-stu-id="235d0-146">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="235d0-147">Tillåt privat-svar-cachelagring</span><span class="sxs-lookup"><span data-stu-id="235d0-147">allow-private-response-caching</span></span>|<span data-ttu-id="235d0-148">När värdet för`true`, tillåter cachelagring av begäranden som innehåller ett Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="235d0-148">When set too`true`, allows caching of requests that contain an Authorization header.</span></span>|<span data-ttu-id="235d0-149">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-149">No</span></span>|<span data-ttu-id="235d0-150">FALSKT</span><span class="sxs-lookup"><span data-stu-id="235d0-150">false</span></span>|  
|<span data-ttu-id="235d0-151">underordnade-cachelagring-typ</span><span class="sxs-lookup"><span data-stu-id="235d0-151">downstream-caching-type</span></span>|<span data-ttu-id="235d0-152">Det här attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="235d0-152">This attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="235d0-153">-Ingen - underordnade cachelagring tillåts inte.</span><span class="sxs-lookup"><span data-stu-id="235d0-153">-   none - downstream caching is not allowed.</span></span><br /><span data-ttu-id="235d0-154">-privat - cachelagring underordnade privata.</span><span class="sxs-lookup"><span data-stu-id="235d0-154">-   private - downstream private caching is allowed.</span></span><br /><span data-ttu-id="235d0-155">-offentlig - cachelagring privata och delade underordnade.</span><span class="sxs-lookup"><span data-stu-id="235d0-155">-   public - private and shared downstream caching is allowed.</span></span>|<span data-ttu-id="235d0-156">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-156">No</span></span>|<span data-ttu-id="235d0-157">Ingen</span><span class="sxs-lookup"><span data-stu-id="235d0-157">none</span></span>|  
|<span data-ttu-id="235d0-158">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="235d0-158">must-revalidate</span></span>|<span data-ttu-id="235d0-159">När efterföljande cachelagring av det här attributet aktiverar eller inaktiverar hello `must-revalidate` cache-control-direktiv i gateway-svar.</span><span class="sxs-lookup"><span data-stu-id="235d0-159">When downstream caching is enabled this attribute turns on or off  hello `must-revalidate` cache control directive in gateway responses.</span></span>|<span data-ttu-id="235d0-160">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-160">No</span></span>|<span data-ttu-id="235d0-161">SANT</span><span class="sxs-lookup"><span data-stu-id="235d0-161">true</span></span>|  
|<span data-ttu-id="235d0-162">variera av utvecklare</span><span class="sxs-lookup"><span data-stu-id="235d0-162">vary-by-developer</span></span>|<span data-ttu-id="235d0-163">Ställa in också`true` toocache svar per developer nyckel.</span><span class="sxs-lookup"><span data-stu-id="235d0-163">Set too`true` toocache responses per developer key.</span></span>|<span data-ttu-id="235d0-164">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-164">No</span></span>|<span data-ttu-id="235d0-165">FALSKT</span><span class="sxs-lookup"><span data-stu-id="235d0-165">false</span></span>|  
|<span data-ttu-id="235d0-166">variera-av-utvecklare-grupper</span><span class="sxs-lookup"><span data-stu-id="235d0-166">vary-by-developer-groups</span></span>|<span data-ttu-id="235d0-167">Ställa in också`true` toocache svar per användarroll.</span><span class="sxs-lookup"><span data-stu-id="235d0-167">Set too`true` toocache responses per user role.</span></span>|<span data-ttu-id="235d0-168">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-168">No</span></span>|<span data-ttu-id="235d0-169">FALSKT</span><span class="sxs-lookup"><span data-stu-id="235d0-169">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="235d0-170">Användning</span><span class="sxs-lookup"><span data-stu-id="235d0-170">Usage</span></span>  
 <span data-ttu-id="235d0-171">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="235d0-171">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="235d0-172">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="235d0-172">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="235d0-173">**Princip för scope:** API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="235d0-173">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="235d0-174"><a name="StoreToCache"></a>Store toocache</span><span class="sxs-lookup"><span data-stu-id="235d0-174"><a name="StoreToCache"></a> Store toocache</span></span>  
 <span data-ttu-id="235d0-175">Hej `cache-store` princip cacheminnen svar enligt toohello angetts cacheinställningarna.</span><span class="sxs-lookup"><span data-stu-id="235d0-175">hello `cache-store` policy caches responses according toohello specified cache settings.</span></span> <span data-ttu-id="235d0-176">Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="235d0-176">This policy can be applied in cases where response content remains static over a period of time.</span></span> <span data-ttu-id="235d0-177">Svaret cachelagring minskar bandbredden och bearbetningskrav införts på hello backend web server och driftmiljön svarstid, uppfattas av API-konsumenter.</span><span class="sxs-lookup"><span data-stu-id="235d0-177">Response caching reduces bandwidth and processing requirements imposed on hello backend web server and lowers latency perceived by API consumers.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="235d0-178">Den här principen måste ha en motsvarande [hämta från cache](api-management-caching-policies.md#GetFromCache) princip.</span><span class="sxs-lookup"><span data-stu-id="235d0-178">This policy must have a corresponding [Get from cache](api-management-caching-policies.md#GetFromCache) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="235d0-179">Principframställning</span><span class="sxs-lookup"><span data-stu-id="235d0-179">Policy statement</span></span>  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a><span data-ttu-id="235d0-180">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-180">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="235d0-181">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-181">Example</span></span>  
  
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
  
#### <a name="example-using-policy-expressions"></a><span data-ttu-id="235d0-182">Exempel på användning av princip uttryck</span><span class="sxs-lookup"><span data-stu-id="235d0-182">Example using policy expressions</span></span>  
 <span data-ttu-id="235d0-183">Det här exemplet illustrerar hur tootooconfigure API Management svaret som matchar hello svar cachelagring av hello serverdelstjänst som anges av hello cachevaraktighet säkerhetskopieras tjänstens `Cache-Control` direktiv.</span><span class="sxs-lookup"><span data-stu-id="235d0-183">This example shows how tootooconfigure API Management response caching duration that matches hello response caching of hello backend service as specified by hello backed service's `Cache-Control` directive.</span></span> <span data-ttu-id="235d0-184">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too25:25.</span><span class="sxs-lookup"><span data-stu-id="235d0-184">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too25:25.</span></span>  
  
```xml  
<!-- hello following cache policy snippets demonstrate how toocontrol API Management reponse cache duration with Cache-Control headers sent by hello backend service. -->  
  
<!-- Copy this snippet into hello inbound section -->  
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >  
  <vary-by-header>Accept</vary-by-header>  
  <vary-by-header>Accept-Charset</vary-by-header>  
</cache-lookup>  
  
<!-- Copy this snippet into hello outbound section. Note that cache duration is set toohello max-age value provided in hello Cache-Control header received from hello backend service or toohello deafult value of 5 min if none is found  -->  
<cache-store duration="@{  
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");  
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;  
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;  
  }"  
 />  
```  
  
 <span data-ttu-id="235d0-185">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="235d0-185">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="235d0-186">Element</span><span class="sxs-lookup"><span data-stu-id="235d0-186">Elements</span></span>  
  
|<span data-ttu-id="235d0-187">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-187">Name</span></span>|<span data-ttu-id="235d0-188">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-188">Description</span></span>|<span data-ttu-id="235d0-189">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-189">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="235d0-190">cache-lagra</span><span class="sxs-lookup"><span data-stu-id="235d0-190">cache-store</span></span>|<span data-ttu-id="235d0-191">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="235d0-191">Root element.</span></span>|<span data-ttu-id="235d0-192">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-192">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="235d0-193">Attribut</span><span class="sxs-lookup"><span data-stu-id="235d0-193">Attributes</span></span>  
  
|<span data-ttu-id="235d0-194">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-194">Name</span></span>|<span data-ttu-id="235d0-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-195">Description</span></span>|<span data-ttu-id="235d0-196">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-196">Required</span></span>|<span data-ttu-id="235d0-197">Standard</span><span class="sxs-lookup"><span data-stu-id="235d0-197">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="235d0-198">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="235d0-198">duration</span></span>|<span data-ttu-id="235d0-199">Time-to-live av hello cachelagras poster som har angetts i sekunder.</span><span class="sxs-lookup"><span data-stu-id="235d0-199">Time-to-live of hello cached entries, specified in seconds.</span></span>|<span data-ttu-id="235d0-200">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-200">Yes</span></span>|<span data-ttu-id="235d0-201">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-201">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="235d0-202">Användning</span><span class="sxs-lookup"><span data-stu-id="235d0-202">Usage</span></span>  
 <span data-ttu-id="235d0-203">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="235d0-203">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="235d0-204">**Avsnitt i princip:** utgående</span><span class="sxs-lookup"><span data-stu-id="235d0-204">**Policy sections:** outbound</span></span>  
  
-   <span data-ttu-id="235d0-205">**Princip för scope:** API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="235d0-205">**Policy scopes:** API, operation, product</span></span>  
  
##  <span data-ttu-id="235d0-206"><a name="GetFromCacheByKey"></a>Hämta ett värde från cache</span><span class="sxs-lookup"><span data-stu-id="235d0-206"><a name="GetFromCacheByKey"></a> Get value from cache</span></span>  
 <span data-ttu-id="235d0-207">Använd hello `cache-lookup-value` princip tooperform cachelagra sökning av nyckeln och cachelagrade returvärde.</span><span class="sxs-lookup"><span data-stu-id="235d0-207">Use hello `cache-lookup-value` policy tooperform cache lookup by key and return a cached value.</span></span> <span data-ttu-id="235d0-208">hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="235d0-208">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="235d0-209">Den här principen måste ha en motsvarande [lagrar värdet i cache](#StoreToCacheByKey) princip.</span><span class="sxs-lookup"><span data-stu-id="235d0-209">This policy must have a corresponding [Store value in cache](#StoreToCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="235d0-210">Principframställning</span><span class="sxs-lookup"><span data-stu-id="235d0-210">Policy statement</span></span>  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a><span data-ttu-id="235d0-211">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-211">Example</span></span>  
 <span data-ttu-id="235d0-212">Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="235d0-212">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="235d0-213">Element</span><span class="sxs-lookup"><span data-stu-id="235d0-213">Elements</span></span>  
  
|<span data-ttu-id="235d0-214">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-214">Name</span></span>|<span data-ttu-id="235d0-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-215">Description</span></span>|<span data-ttu-id="235d0-216">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-216">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="235d0-217">cache-sökning-värde</span><span class="sxs-lookup"><span data-stu-id="235d0-217">cache-lookup-value</span></span>|<span data-ttu-id="235d0-218">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="235d0-218">Root element.</span></span>|<span data-ttu-id="235d0-219">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="235d0-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="235d0-220">Attributes</span></span>  
  
|<span data-ttu-id="235d0-221">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-221">Name</span></span>|<span data-ttu-id="235d0-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-222">Description</span></span>|<span data-ttu-id="235d0-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-223">Required</span></span>|<span data-ttu-id="235d0-224">Standard</span><span class="sxs-lookup"><span data-stu-id="235d0-224">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="235d0-225">standard-värde</span><span class="sxs-lookup"><span data-stu-id="235d0-225">default-value</span></span>|<span data-ttu-id="235d0-226">Ett värde som ska tilldelas toohello variabeln om hello cache nyckelsökningen resulterade i en miss.</span><span class="sxs-lookup"><span data-stu-id="235d0-226">A value that will be assigned toohello variable if hello cache key lookup resulted in a miss.</span></span> <span data-ttu-id="235d0-227">Om det här attributet inte anges `null` är tilldelad.</span><span class="sxs-lookup"><span data-stu-id="235d0-227">If this attribute is not specified, `null` is assigned.</span></span>|<span data-ttu-id="235d0-228">Nej</span><span class="sxs-lookup"><span data-stu-id="235d0-228">No</span></span>|`null`|  
|<span data-ttu-id="235d0-229">key</span><span class="sxs-lookup"><span data-stu-id="235d0-229">key</span></span>|<span data-ttu-id="235d0-230">Cachelagra nyckelvärdet toouse i hello sökning.</span><span class="sxs-lookup"><span data-stu-id="235d0-230">Cache key value toouse in hello lookup.</span></span>|<span data-ttu-id="235d0-231">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-231">Yes</span></span>|<span data-ttu-id="235d0-232">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-232">N/A</span></span>|  
|<span data-ttu-id="235d0-233">variabeln namn</span><span class="sxs-lookup"><span data-stu-id="235d0-233">variable-name</span></span>|<span data-ttu-id="235d0-234">Namnet på hello [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) hello slås upp värdet kommer att tilldelas, om sökning lyckas.</span><span class="sxs-lookup"><span data-stu-id="235d0-234">Name of hello [context variable](api-management-policy-expressions.md#ContextVariables) hello looked up value will be assigned to, if lookup is successful.</span></span> <span data-ttu-id="235d0-235">Om sökning resulterar i en miss, hello variabeln tilldelas hello värdet för hello `default-value` attribut eller `null`om hello `default-value` attributet utelämnas.</span><span class="sxs-lookup"><span data-stu-id="235d0-235">If lookup results in a miss, hello variable will be assigned hello value of hello `default-value` attribute or `null`, if hello `default-value` attribute is omitted.</span></span>|<span data-ttu-id="235d0-236">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-236">Yes</span></span>|<span data-ttu-id="235d0-237">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-237">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="235d0-238">Användning</span><span class="sxs-lookup"><span data-stu-id="235d0-238">Usage</span></span>  
 <span data-ttu-id="235d0-239">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="235d0-239">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="235d0-240">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="235d0-240">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="235d0-241">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="235d0-241">**Policy scopes:** global, API, operation, product</span></span>  
  
##  <span data-ttu-id="235d0-242"><a name="StoreToCacheByKey"></a>Lagrar värdet i cache</span><span class="sxs-lookup"><span data-stu-id="235d0-242"><a name="StoreToCacheByKey"></a> Store value in cache</span></span>  
 <span data-ttu-id="235d0-243">Hej `cache-store-value` utför cachelagring av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="235d0-243">hello `cache-store-value` performs cache storage by key.</span></span> <span data-ttu-id="235d0-244">hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="235d0-244">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="235d0-245">Den här principen måste ha en motsvarande [hämta värdet från cache](#GetFromCacheByKey) princip.</span><span class="sxs-lookup"><span data-stu-id="235d0-245">This policy must have a corresponding [Get value from cache](#GetFromCacheByKey) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="235d0-246">Principframställning</span><span class="sxs-lookup"><span data-stu-id="235d0-246">Policy statement</span></span>  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a><span data-ttu-id="235d0-247">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-247">Example</span></span>  
 <span data-ttu-id="235d0-248">Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span><span class="sxs-lookup"><span data-stu-id="235d0-248">For more information and examples of this policy, see [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).</span></span>  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a><span data-ttu-id="235d0-249">Element</span><span class="sxs-lookup"><span data-stu-id="235d0-249">Elements</span></span>  
  
|<span data-ttu-id="235d0-250">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-250">Name</span></span>|<span data-ttu-id="235d0-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-251">Description</span></span>|<span data-ttu-id="235d0-252">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-252">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="235d0-253">cache-lagra-värde</span><span class="sxs-lookup"><span data-stu-id="235d0-253">cache-store-value</span></span>|<span data-ttu-id="235d0-254">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="235d0-254">Root element.</span></span>|<span data-ttu-id="235d0-255">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-255">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="235d0-256">Attribut</span><span class="sxs-lookup"><span data-stu-id="235d0-256">Attributes</span></span>  
  
|<span data-ttu-id="235d0-257">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-257">Name</span></span>|<span data-ttu-id="235d0-258">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-258">Description</span></span>|<span data-ttu-id="235d0-259">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-259">Required</span></span>|<span data-ttu-id="235d0-260">Standard</span><span class="sxs-lookup"><span data-stu-id="235d0-260">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="235d0-261">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="235d0-261">duration</span></span>|<span data-ttu-id="235d0-262">Värdet cachelagras för hello tillhandahålls duration-värde som anges i sekunder.</span><span class="sxs-lookup"><span data-stu-id="235d0-262">Value will be cached for hello provided duration value, specified in seconds.</span></span>|<span data-ttu-id="235d0-263">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-263">Yes</span></span>|<span data-ttu-id="235d0-264">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-264">N/A</span></span>|  
|<span data-ttu-id="235d0-265">key</span><span class="sxs-lookup"><span data-stu-id="235d0-265">key</span></span>|<span data-ttu-id="235d0-266">Cache key hello värdet lagras.</span><span class="sxs-lookup"><span data-stu-id="235d0-266">Cache key hello value will be stored under.</span></span>|<span data-ttu-id="235d0-267">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-267">Yes</span></span>|<span data-ttu-id="235d0-268">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-268">N/A</span></span>|  
|<span data-ttu-id="235d0-269">värde</span><span class="sxs-lookup"><span data-stu-id="235d0-269">value</span></span>|<span data-ttu-id="235d0-270">hello värdet toobe cachelagras.</span><span class="sxs-lookup"><span data-stu-id="235d0-270">hello value toobe cached.</span></span>|<span data-ttu-id="235d0-271">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-271">Yes</span></span>|<span data-ttu-id="235d0-272">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="235d0-273">Användning</span><span class="sxs-lookup"><span data-stu-id="235d0-273">Usage</span></span>  
 <span data-ttu-id="235d0-274">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="235d0-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="235d0-275">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="235d0-275">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="235d0-276">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="235d0-276">**Policy scopes:** global, API, operation, product</span></span>  
  
###  <span data-ttu-id="235d0-277"><a name="RemoveCacheByKey"></a>Ta bort värdet från cache</span><span class="sxs-lookup"><span data-stu-id="235d0-277"><a name="RemoveCacheByKey"></a> Remove value from cache</span></span>  
 <span data-ttu-id="235d0-278">Hej `cache-remove-value` tar bort en cachelagrade objekt som identifieras av dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="235d0-278">hello             `cache-remove-value` deletes a cached item identified by its key.</span></span> <span data-ttu-id="235d0-279">hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="235d0-279">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span>  
  
#### <a name="policy-statement"></a><span data-ttu-id="235d0-280">Principframställning</span><span class="sxs-lookup"><span data-stu-id="235d0-280">Policy statement</span></span>  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="235d0-281">Exempel</span><span class="sxs-lookup"><span data-stu-id="235d0-281">Example</span></span>  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a><span data-ttu-id="235d0-282">Element</span><span class="sxs-lookup"><span data-stu-id="235d0-282">Elements</span></span>  
  
|<span data-ttu-id="235d0-283">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-283">Name</span></span>|<span data-ttu-id="235d0-284">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-284">Description</span></span>|<span data-ttu-id="235d0-285">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-285">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="235d0-286">cache-remove-värde</span><span class="sxs-lookup"><span data-stu-id="235d0-286">cache-remove-value</span></span>|<span data-ttu-id="235d0-287">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="235d0-287">Root element.</span></span>|<span data-ttu-id="235d0-288">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-288">Yes</span></span>|  
  
#### <a name="attributes"></a><span data-ttu-id="235d0-289">Attribut</span><span class="sxs-lookup"><span data-stu-id="235d0-289">Attributes</span></span>  
  
|<span data-ttu-id="235d0-290">Namn</span><span class="sxs-lookup"><span data-stu-id="235d0-290">Name</span></span>|<span data-ttu-id="235d0-291">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="235d0-291">Description</span></span>|<span data-ttu-id="235d0-292">Krävs</span><span class="sxs-lookup"><span data-stu-id="235d0-292">Required</span></span>|<span data-ttu-id="235d0-293">Standard</span><span class="sxs-lookup"><span data-stu-id="235d0-293">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="235d0-294">key</span><span class="sxs-lookup"><span data-stu-id="235d0-294">key</span></span>|<span data-ttu-id="235d0-295">hello nyckeln för hello tidigare cachelagrats värdet toobe bort från cachen i hello.</span><span class="sxs-lookup"><span data-stu-id="235d0-295">hello key of hello previously cached value toobe removed from hello cache.</span></span>|<span data-ttu-id="235d0-296">Ja</span><span class="sxs-lookup"><span data-stu-id="235d0-296">Yes</span></span>|<span data-ttu-id="235d0-297">Saknas</span><span class="sxs-lookup"><span data-stu-id="235d0-297">N/A</span></span>|  
  
#### <a name="usage"></a><span data-ttu-id="235d0-298">Användning</span><span class="sxs-lookup"><span data-stu-id="235d0-298">Usage</span></span>  
 <span data-ttu-id="235d0-299">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="235d0-299">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="235d0-300">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="235d0-300">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="235d0-301">**Princip för scope:** globala API, åtgärden, produkt</span><span class="sxs-lookup"><span data-stu-id="235d0-301">**Policy scopes:** global, API, operation, product</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="235d0-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="235d0-302">Next steps</span></span>
<span data-ttu-id="235d0-303">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="235d0-303">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  