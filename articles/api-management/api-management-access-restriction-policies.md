---
title: "Azure API Management begränsning åtkomstprinciper | Microsoft Docs"
description: "Läs mer om de begränsning åtkomstprinciper kan användas i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 4c9991baf3fbcf3b8ea01f8dd573e2336db88b68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="30cab-103">Principer för begränsning av API Management-åtkomst</span><span class="sxs-lookup"><span data-stu-id="30cab-103">API Management access restriction policies</span></span>
<span data-ttu-id="30cab-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="30cab-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="30cab-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="30cab-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="30cab-106"><a name="AccessRestrictionPolicies"></a>Principer för begränsning av åtkomst</span><span class="sxs-lookup"><span data-stu-id="30cab-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="30cab-107">[Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="30cab-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="30cab-108">[Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="30cab-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="30cab-109">[Gränsen anropet frekvensen av nyckeln](#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="30cab-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="30cab-110">[Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="30cab-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="30cab-111">[Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -du kan tillämpa en förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="30cab-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="30cab-112">[Kvot för uppsättningen av nyckeln](#SetUsageQuotaByKey) -du kan tillämpa en förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="30cab-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="30cab-113">[Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="30cab-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="30cab-114"><a name="CheckHTTPHeader"></a>Kontrollera HTTP-huvud</span><span class="sxs-lookup"><span data-stu-id="30cab-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="30cab-115">Använd den `check-header` princip för att genomdriva att en begäran har en angivna HTTP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="30cab-115">Use the `check-header` policy to enforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="30cab-116">Du kan du kan också kontrollera om sidhuvudet har ett specifikt värde eller Sök efter en tillåtna värdeintervallet.</span><span class="sxs-lookup"><span data-stu-id="30cab-116">You can optionally check to see if the header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="30cab-117">Om den inte principen avslutar bearbeta begäran och returnerar det HTTP-kod och fel statusmeddelande som anges i principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-117">If the check fails, the policy terminates request processing and returns the HTTP status code and error message specified by the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-118">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-119">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-120">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-120">Elements</span></span>  
  
|<span data-ttu-id="30cab-121">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-121">Name</span></span>|<span data-ttu-id="30cab-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-122">Description</span></span>|<span data-ttu-id="30cab-123">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-124">Kontrollera-huvud</span><span class="sxs-lookup"><span data-stu-id="30cab-124">check-header</span></span>|<span data-ttu-id="30cab-125">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-125">Root element.</span></span>|<span data-ttu-id="30cab-126">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-126">Yes</span></span>|  
|<span data-ttu-id="30cab-127">värde</span><span class="sxs-lookup"><span data-stu-id="30cab-127">value</span></span>|<span data-ttu-id="30cab-128">Tillåtet värde för HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="30cab-128">Allowed HTTP header value.</span></span> <span data-ttu-id="30cab-129">När flera element med värdet har angetts, betraktas lyckas kontrollen om något av värdena finns en matchning.</span><span class="sxs-lookup"><span data-stu-id="30cab-129">When multiple value elements are specified, the check is considered a success if any one of the values is a match.</span></span>|<span data-ttu-id="30cab-130">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-131">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-131">Attributes</span></span>  
  
|<span data-ttu-id="30cab-132">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-132">Name</span></span>|<span data-ttu-id="30cab-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-133">Description</span></span>|<span data-ttu-id="30cab-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-134">Required</span></span>|<span data-ttu-id="30cab-135">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-136">misslyckades-check--felmeddelande</span><span class="sxs-lookup"><span data-stu-id="30cab-136">failed-check-error-message</span></span>|<span data-ttu-id="30cab-137">Felmeddelande om att returnera i texten för HTTP-svar om huvudet finns inte eller har ett ogiltigt värde.</span><span class="sxs-lookup"><span data-stu-id="30cab-137">Error message to return in the HTTP response body if the header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="30cab-138">Det här meddelandet måste ha specialtecken (escaped) korrekt.</span><span class="sxs-lookup"><span data-stu-id="30cab-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="30cab-139">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-139">Yes</span></span>|<span data-ttu-id="30cab-140">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-140">N/A</span></span>|  
|<span data-ttu-id="30cab-141">misslyckades-check-httpcode</span><span class="sxs-lookup"><span data-stu-id="30cab-141">failed-check-httpcode</span></span>|<span data-ttu-id="30cab-142">HTTP-statuskoden ska returneras om huvudet finns inte eller har ett ogiltigt värde.</span><span class="sxs-lookup"><span data-stu-id="30cab-142">HTTP Status code to return if the header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="30cab-143">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-143">Yes</span></span>|<span data-ttu-id="30cab-144">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-144">N/A</span></span>|  
|<span data-ttu-id="30cab-145">huvudets namn</span><span class="sxs-lookup"><span data-stu-id="30cab-145">header-name</span></span>|<span data-ttu-id="30cab-146">Namnet på det HTTP-huvud för att kontrollera.</span><span class="sxs-lookup"><span data-stu-id="30cab-146">The name of the HTTP Header to check.</span></span>|<span data-ttu-id="30cab-147">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-147">Yes</span></span>|<span data-ttu-id="30cab-148">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-148">N/A</span></span>|  
|<span data-ttu-id="30cab-149">Ignorera skiftläge</span><span class="sxs-lookup"><span data-stu-id="30cab-149">ignore-case</span></span>|<span data-ttu-id="30cab-150">Kan anges till True eller False.</span><span class="sxs-lookup"><span data-stu-id="30cab-150">Can be set to True or False.</span></span> <span data-ttu-id="30cab-151">Om värdet är SANT om ignoreras när huvudets värde jämförs uppsättningen av godkända värden.</span><span class="sxs-lookup"><span data-stu-id="30cab-151">If set to True case is ignored when the header value is compared against the set of acceptable values.</span></span>|<span data-ttu-id="30cab-152">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-152">Yes</span></span>|<span data-ttu-id="30cab-153">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-154">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-154">Usage</span></span>  
 <span data-ttu-id="30cab-155">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-155">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-156">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="30cab-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="30cab-157">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="30cab-158"><a name="LimitCallRate"></a>Gränsen anropet frekvensen av prenumerationen</span><span class="sxs-lookup"><span data-stu-id="30cab-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="30cab-159">Den `rate-limit` princip förhindrar API användningstoppar på grundval av per prenumeration genom att begränsa anropet frekvensen till ett angivet antal per angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="30cab-159">The `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="30cab-160">När den här principen utlöses anroparen tar emot en `429 Too Many Requests` Svarets statuskod.</span><span class="sxs-lookup"><span data-stu-id="30cab-160">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="30cab-161">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="30cab-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="30cab-162">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i något av attribut för principen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-163">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-164">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-164">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-165">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-165">Elements</span></span>  
  
|<span data-ttu-id="30cab-166">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-166">Name</span></span>|<span data-ttu-id="30cab-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-167">Description</span></span>|<span data-ttu-id="30cab-168">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-169">Ställ in gräns</span><span class="sxs-lookup"><span data-stu-id="30cab-169">set-limit</span></span>|<span data-ttu-id="30cab-170">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-170">Root element.</span></span>|<span data-ttu-id="30cab-171">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-171">Yes</span></span>|  
|<span data-ttu-id="30cab-172">api</span><span class="sxs-lookup"><span data-stu-id="30cab-172">api</span></span>|<span data-ttu-id="30cab-173">Lägg till en eller flera av de här elementen att införa en anropet hastighetsbegränsning på API: er inom produkten.</span><span class="sxs-lookup"><span data-stu-id="30cab-173">Add one  or more of these elements to impose a call rate limit on APIs within the product.</span></span> <span data-ttu-id="30cab-174">Produkt- och API anropa räntesats gränser som är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="30cab-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="30cab-175">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-175">No</span></span>|  
|<span data-ttu-id="30cab-176">åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-176">operation</span></span>|<span data-ttu-id="30cab-177">Lägg till en eller flera av de här elementen att införa en anropet hastighetsbegränsning på åtgärder i en API.</span><span class="sxs-lookup"><span data-stu-id="30cab-177">Add one  or more of these elements to impose a call rate limit on operations within an API.</span></span> <span data-ttu-id="30cab-178">Produkt-, API- och åtgärden anropa räntesats gränser som är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="30cab-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="30cab-179">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-180">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-180">Attributes</span></span>  
  
|<span data-ttu-id="30cab-181">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-181">Name</span></span>|<span data-ttu-id="30cab-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-182">Description</span></span>|<span data-ttu-id="30cab-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-183">Required</span></span>|<span data-ttu-id="30cab-184">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-185">namn</span><span class="sxs-lookup"><span data-stu-id="30cab-185">name</span></span>|<span data-ttu-id="30cab-186">Namnet på API som gräns för överföringshastigheten.</span><span class="sxs-lookup"><span data-stu-id="30cab-186">The name of the API for which to apply the rate limit.</span></span>|<span data-ttu-id="30cab-187">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-187">Yes</span></span>|<span data-ttu-id="30cab-188">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-188">N/A</span></span>|  
|<span data-ttu-id="30cab-189">anrop</span><span class="sxs-lookup"><span data-stu-id="30cab-189">calls</span></span>|<span data-ttu-id="30cab-190">Det maximala totalt antalet anrop tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-190">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-191">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-191">Yes</span></span>|<span data-ttu-id="30cab-192">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-192">N/A</span></span>|  
|<span data-ttu-id="30cab-193">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="30cab-193">renewal-period</span></span>|<span data-ttu-id="30cab-194">Hur lång tid i sekunder, efter vilken kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="30cab-194">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="30cab-195">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-195">Yes</span></span>|<span data-ttu-id="30cab-196">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-197">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-197">Usage</span></span>  
 <span data-ttu-id="30cab-198">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-198">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-199">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-200">**Princip för scope:** produkt</span><span class="sxs-lookup"><span data-stu-id="30cab-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="30cab-201"><a name="LimitCallRateByKey"></a>Gränsen anropet frekvensen av nyckel</span><span class="sxs-lookup"><span data-stu-id="30cab-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="30cab-202">Den `rate-limit-by-key` princip förhindrar API användningstoppar på grundval av per nyckel genom att begränsa anropet frekvensen till ett angivet antal per angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="30cab-202">The `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting the call rate to a specified number per a specified time period.</span></span> <span data-ttu-id="30cab-203">Nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="30cab-203">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="30cab-204">Valfritt steg villkor kan läggas till ange vilka begäranden som ska räknas till gränsen.</span><span class="sxs-lookup"><span data-stu-id="30cab-204">Optional increment condition can be added to specify which requests should be counted towards the limit.</span></span> <span data-ttu-id="30cab-205">När den här principen utlöses anroparen tar emot en `429 Too Many Requests` Svarets statuskod.</span><span class="sxs-lookup"><span data-stu-id="30cab-205">When this policy is triggered the caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="30cab-206">Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="30cab-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="30cab-207">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="30cab-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-208">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-209">Example</span></span>  
 <span data-ttu-id="30cab-210">I följande exempel aktiveras gräns för överföringshastigheten i anroparen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="30cab-210">In the following example, the rate limit is keyed by the caller IP address.</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-211">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-211">Elements</span></span>  
  
|<span data-ttu-id="30cab-212">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-212">Name</span></span>|<span data-ttu-id="30cab-213">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-213">Description</span></span>|<span data-ttu-id="30cab-214">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-215">Ställ in gräns</span><span class="sxs-lookup"><span data-stu-id="30cab-215">set-limit</span></span>|<span data-ttu-id="30cab-216">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-216">Root element.</span></span>|<span data-ttu-id="30cab-217">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-218">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-218">Attributes</span></span>  
  
|<span data-ttu-id="30cab-219">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-219">Name</span></span>|<span data-ttu-id="30cab-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-220">Description</span></span>|<span data-ttu-id="30cab-221">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-221">Required</span></span>|<span data-ttu-id="30cab-222">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-223">anrop</span><span class="sxs-lookup"><span data-stu-id="30cab-223">calls</span></span>|<span data-ttu-id="30cab-224">Det maximala totalt antalet anrop tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-224">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-225">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-225">Yes</span></span>|<span data-ttu-id="30cab-226">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-226">N/A</span></span>|  
|<span data-ttu-id="30cab-227">motparterna nyckel</span><span class="sxs-lookup"><span data-stu-id="30cab-227">counter-key</span></span>|<span data-ttu-id="30cab-228">Nyckeln som ska användas för hastighet gränsen principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-228">The key to use for the rate limit policy.</span></span>|<span data-ttu-id="30cab-229">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-229">Yes</span></span>|<span data-ttu-id="30cab-230">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-230">N/A</span></span>|  
|<span data-ttu-id="30cab-231">öka villkor</span><span class="sxs-lookup"><span data-stu-id="30cab-231">increment-condition</span></span>|<span data-ttu-id="30cab-232">Den booleska uttryck som anger om begäran ska räknas mot kvoten (`true`).</span><span class="sxs-lookup"><span data-stu-id="30cab-232">The boolean expression specifying if the request should be counted towards the quota (`true`).</span></span>|<span data-ttu-id="30cab-233">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-233">No</span></span>|<span data-ttu-id="30cab-234">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-234">N/A</span></span>|  
|<span data-ttu-id="30cab-235">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="30cab-235">renewal-period</span></span>|<span data-ttu-id="30cab-236">Hur lång tid i sekunder, efter vilken kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="30cab-236">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="30cab-237">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-237">Yes</span></span>|<span data-ttu-id="30cab-238">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-239">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-239">Usage</span></span>  
 <span data-ttu-id="30cab-240">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-240">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-241">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-242">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="30cab-243"><a name="RestrictCallerIPs"></a>Begränsa anroparen IP-adresser</span><span class="sxs-lookup"><span data-stu-id="30cab-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="30cab-244">Den `ip-filter` princip filtrerar (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="30cab-244">The `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-245">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-246">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-247">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-247">Elements</span></span>  
  
|<span data-ttu-id="30cab-248">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-248">Name</span></span>|<span data-ttu-id="30cab-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-249">Description</span></span>|<span data-ttu-id="30cab-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-251">IP-filter</span><span class="sxs-lookup"><span data-stu-id="30cab-251">ip-filter</span></span>|<span data-ttu-id="30cab-252">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-252">Root element.</span></span>|<span data-ttu-id="30cab-253">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-253">Yes</span></span>|  
|<span data-ttu-id="30cab-254">Adress</span><span class="sxs-lookup"><span data-stu-id="30cab-254">address</span></span>|<span data-ttu-id="30cab-255">Anger en IP-adress som ska filtreras.</span><span class="sxs-lookup"><span data-stu-id="30cab-255">Specifies a single IP address on which to filter.</span></span>|<span data-ttu-id="30cab-256">Minst en `address` eller `address-range` -element krävs.</span><span class="sxs-lookup"><span data-stu-id="30cab-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="30cab-257">adressintervallet från = ”adress” till = ”adress”</span><span class="sxs-lookup"><span data-stu-id="30cab-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="30cab-258">Anger ett intervall av IP-adresser som ska filtreras.</span><span class="sxs-lookup"><span data-stu-id="30cab-258">Specifies a range of IP address on which to filter.</span></span>|<span data-ttu-id="30cab-259">Minst en `address` eller `address-range` -element krävs.</span><span class="sxs-lookup"><span data-stu-id="30cab-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-260">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-260">Attributes</span></span>  
  
|<span data-ttu-id="30cab-261">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-261">Name</span></span>|<span data-ttu-id="30cab-262">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-262">Description</span></span>|<span data-ttu-id="30cab-263">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-263">Required</span></span>|<span data-ttu-id="30cab-264">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-265">adressintervallet från = ”adress” till = ”adress”</span><span class="sxs-lookup"><span data-stu-id="30cab-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="30cab-266">Ett IP-adressintervall om du vill tillåta eller neka åtkomst för.</span><span class="sxs-lookup"><span data-stu-id="30cab-266">A range of IP addresses to allow or deny access for.</span></span>|<span data-ttu-id="30cab-267">Krävs när den `address-range` elementet används.</span><span class="sxs-lookup"><span data-stu-id="30cab-267">Required when the `address-range` element is used.</span></span>|<span data-ttu-id="30cab-268">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-268">N/A</span></span>|  
|<span data-ttu-id="30cab-269">IP-filteråtgärd = ”Tillåt &#124; förbjuda ”</span><span class="sxs-lookup"><span data-stu-id="30cab-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="30cab-270">Anger om anrop ska tillåtas eller inte för den angivna IP-adresser och intervall.</span><span class="sxs-lookup"><span data-stu-id="30cab-270">Specifies whether calls should be allowed or not for the specified IP addresses and ranges.</span></span>|<span data-ttu-id="30cab-271">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-271">Yes</span></span>|<span data-ttu-id="30cab-272">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-273">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-273">Usage</span></span>  
 <span data-ttu-id="30cab-274">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-274">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-275">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-276">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="30cab-277"><a name="SetUsageQuota"></a>Ange kvot för prenumeration</span><span class="sxs-lookup"><span data-stu-id="30cab-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="30cab-278">Den `quota` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="30cab-278">The `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="30cab-279">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="30cab-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="30cab-280">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i något av attribut för principen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-281">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-282">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-282">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-283">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-283">Elements</span></span>  
  
|<span data-ttu-id="30cab-284">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-284">Name</span></span>|<span data-ttu-id="30cab-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-285">Description</span></span>|<span data-ttu-id="30cab-286">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-287">kvot</span><span class="sxs-lookup"><span data-stu-id="30cab-287">quota</span></span>|<span data-ttu-id="30cab-288">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-288">Root element.</span></span>|<span data-ttu-id="30cab-289">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-289">Yes</span></span>|  
|<span data-ttu-id="30cab-290">api</span><span class="sxs-lookup"><span data-stu-id="30cab-290">api</span></span>|<span data-ttu-id="30cab-291">Lägg till en eller flera av de här elementen att införa en kvot på API: er inom produkten.</span><span class="sxs-lookup"><span data-stu-id="30cab-291">Add one  or more of these elements to impose a quota on APIs within the product.</span></span> <span data-ttu-id="30cab-292">Produkt- och API-kvoter tillämpas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="30cab-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="30cab-293">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-293">No</span></span>|  
|<span data-ttu-id="30cab-294">åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-294">operation</span></span>|<span data-ttu-id="30cab-295">Lägg till en eller flera av de här elementen att införa en kvot på åtgärder i en API.</span><span class="sxs-lookup"><span data-stu-id="30cab-295">Add one  or more of these elements to impose a quota on operations within an API.</span></span> <span data-ttu-id="30cab-296">Produkt-, API- och åtgärden kvoter tillämpas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="30cab-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="30cab-297">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-298">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-298">Attributes</span></span>  
  
|<span data-ttu-id="30cab-299">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-299">Name</span></span>|<span data-ttu-id="30cab-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-300">Description</span></span>|<span data-ttu-id="30cab-301">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-301">Required</span></span>|<span data-ttu-id="30cab-302">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-303">namn</span><span class="sxs-lookup"><span data-stu-id="30cab-303">name</span></span>|<span data-ttu-id="30cab-304">Namnet på API: et eller åtgärden som kvoten gäller.</span><span class="sxs-lookup"><span data-stu-id="30cab-304">The name of the API or operation for which the quota applies.</span></span>|<span data-ttu-id="30cab-305">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-305">Yes</span></span>|<span data-ttu-id="30cab-306">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-306">N/A</span></span>|  
|<span data-ttu-id="30cab-307">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="30cab-307">bandwidth</span></span>|<span data-ttu-id="30cab-308">Det maximala totala antalet kilobyte tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-308">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-309">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="30cab-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="30cab-310">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-310">N/A</span></span>|  
|<span data-ttu-id="30cab-311">anrop</span><span class="sxs-lookup"><span data-stu-id="30cab-311">calls</span></span>|<span data-ttu-id="30cab-312">Det maximala totalt antalet anrop tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-312">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-313">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="30cab-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="30cab-314">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-314">N/A</span></span>|  
|<span data-ttu-id="30cab-315">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="30cab-315">renewal-period</span></span>|<span data-ttu-id="30cab-316">Hur lång tid i sekunder, efter vilken kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="30cab-316">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="30cab-317">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-317">Yes</span></span>|<span data-ttu-id="30cab-318">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-319">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-319">Usage</span></span>  
 <span data-ttu-id="30cab-320">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-320">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-321">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-322">**Princip för scope:** produkt</span><span class="sxs-lookup"><span data-stu-id="30cab-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="30cab-323"><a name="SetUsageQuotaByKey"></a>Kvot för uppsättningen av nyckel</span><span class="sxs-lookup"><span data-stu-id="30cab-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="30cab-324">Den `quota-by-key` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="30cab-324">The `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="30cab-325">Nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="30cab-325">The key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="30cab-326">Valfritt steg villkor kan läggas till ange vilka begäranden som ska räknas mot kvoten.</span><span class="sxs-lookup"><span data-stu-id="30cab-326">Optional increment condition can be added to specify which requests should be counted towards the quota.</span></span>  
  
 <span data-ttu-id="30cab-327">Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="30cab-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="30cab-328">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="30cab-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="30cab-329">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i något av attribut för principen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of the policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-330">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="30cab-331">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-331">Example</span></span>  
 <span data-ttu-id="30cab-332">I följande exempel aktiveras kvoten av anroparen IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="30cab-332">In the following example, the quota is keyed by the caller IP address.</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-333">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-333">Elements</span></span>  
  
|<span data-ttu-id="30cab-334">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-334">Name</span></span>|<span data-ttu-id="30cab-335">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-335">Description</span></span>|<span data-ttu-id="30cab-336">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="30cab-337">kvot</span><span class="sxs-lookup"><span data-stu-id="30cab-337">quota</span></span>|<span data-ttu-id="30cab-338">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-338">Root element.</span></span>|<span data-ttu-id="30cab-339">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-340">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-340">Attributes</span></span>  
  
|<span data-ttu-id="30cab-341">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-341">Name</span></span>|<span data-ttu-id="30cab-342">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-342">Description</span></span>|<span data-ttu-id="30cab-343">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-343">Required</span></span>|<span data-ttu-id="30cab-344">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-345">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="30cab-345">bandwidth</span></span>|<span data-ttu-id="30cab-346">Det maximala totala antalet kilobyte tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-346">The maximum total number of kilobytes allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-347">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="30cab-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="30cab-348">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-348">N/A</span></span>|  
|<span data-ttu-id="30cab-349">anrop</span><span class="sxs-lookup"><span data-stu-id="30cab-349">calls</span></span>|<span data-ttu-id="30cab-350">Det maximala totalt antalet anrop tillåts under en tidsperiod som angetts i den `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="30cab-350">The maximum total number of calls allowed during the time interval specified in the `renewal-period`.</span></span>|<span data-ttu-id="30cab-351">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="30cab-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="30cab-352">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-352">N/A</span></span>|  
|<span data-ttu-id="30cab-353">motparterna nyckel</span><span class="sxs-lookup"><span data-stu-id="30cab-353">counter-key</span></span>|<span data-ttu-id="30cab-354">Nyckeln som ska användas för kvot principen.</span><span class="sxs-lookup"><span data-stu-id="30cab-354">The key to use for the quota policy.</span></span>|<span data-ttu-id="30cab-355">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-355">Yes</span></span>|<span data-ttu-id="30cab-356">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-356">N/A</span></span>|  
|<span data-ttu-id="30cab-357">öka villkor</span><span class="sxs-lookup"><span data-stu-id="30cab-357">increment-condition</span></span>|<span data-ttu-id="30cab-358">Den booleska uttryck som anger om begäran ska räknas mot kvoten (`true`)</span><span class="sxs-lookup"><span data-stu-id="30cab-358">The boolean expression specifying if the request should be counted towards the quota (`true`)</span></span>|<span data-ttu-id="30cab-359">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-359">No</span></span>|<span data-ttu-id="30cab-360">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-360">N/A</span></span>|  
|<span data-ttu-id="30cab-361">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="30cab-361">renewal-period</span></span>|<span data-ttu-id="30cab-362">Hur lång tid i sekunder, efter vilken kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="30cab-362">The time period in seconds after which the quota resets.</span></span>|<span data-ttu-id="30cab-363">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-363">Yes</span></span>|<span data-ttu-id="30cab-364">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-365">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-365">Usage</span></span>  
 <span data-ttu-id="30cab-366">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-366">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-367">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-368">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="30cab-369"><a name="ValidateJWT"></a>Validera JWT</span><span class="sxs-lookup"><span data-stu-id="30cab-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="30cab-370">Den `validate-jwt` princip tillämpar existens och giltigheten för en JWT extraheras från antingen angivna HTTP-huvudet eller angivna frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="30cab-370">The `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="30cab-371">Den `validate-jwt` principen kräver att den `exp` registrerade anspråk är som ingår i JWT-token om `require-expiration-time` attribut anges och värdet `false`.</span><span class="sxs-lookup"><span data-stu-id="30cab-371">The `validate-jwt` policy requires that the `exp` registered claim is inlcuded in the JWT token, unless `require-expiration-time` attribute is specified and set to `false`.</span></span>  
> <span data-ttu-id="30cab-372">Den `validate-jwt` stöder HS256 och RS256 signering algoritmer.</span><span class="sxs-lookup"><span data-stu-id="30cab-372">The `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="30cab-373">Nyckeln måste anges infogad i princip i base64-kodade formatet för för HS256.</span><span class="sxs-lookup"><span data-stu-id="30cab-373">For HS256 the key must be provided inline within the policy in the base64 encoded form.</span></span> <span data-ttu-id="30cab-374">RS256 har nyckeln att tillhandahålla via en öppen ID configuration slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="30cab-374">For RS256 the key has to be provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="30cab-375">Principframställning</span><span class="sxs-lookup"><span data-stu-id="30cab-375">Policy statement</span></span>  
  
```xml  
<validate-jwt   
    header-name="name of http header containing the token (use query-parameter-name attribute if the token is passed in the URL)"   
    failed-validation-httpcode="http status code to return on failure"   
    failed-validation-error-message="error message to return on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of the claim as it appears in the token" match="all|any">  
      <value>claim value as it is expected to appear in the token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of the configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="30cab-376">Exempel</span><span class="sxs-lookup"><span data-stu-id="30cab-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="30cab-377">Azure Mobile Services token verifiering</span><span class="sxs-lookup"><span data-stu-id="30cab-377">Azure Mobile Services token validation</span></span>  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="30cab-378">Azure Active Directory token verifiering</span><span class="sxs-lookup"><span data-stu-id="30cab-378">Azure Active Directory token validation</span></span>  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="30cab-379">Azure Active Directory B2C-token verifiering</span><span class="sxs-lookup"><span data-stu-id="30cab-379">Azure Active Directory B2C token validation</span></span>  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-to-operations-based-on-token-claims"></a><span data-ttu-id="30cab-380">Auktorisera åtkomst till åtgärder baserat på token anspråk</span><span class="sxs-lookup"><span data-stu-id="30cab-380">Authorize access to operations based on token claims</span></span>  
 <span data-ttu-id="30cab-381">Det här exemplet visar hur du använder den [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) för att bevilja åtkomst till åtgärder före baserat på token anspråk.</span><span class="sxs-lookup"><span data-stu-id="30cab-381">This example shows how to use the [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy to pre-authorize access to operations based on token claims.</span></span> <span data-ttu-id="30cab-382">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 13:50.</span><span class="sxs-lookup"><span data-stu-id="30cab-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 13:50.</span></span> <span data-ttu-id="30cab-383">Spola fram till 15:00 för att se de principer som konfigurerats i Redigeraren för grupprinciper och sedan till 18:50 för en demonstration av anropa en funktion från utvecklarportal både med och utan autentiseringstoken som krävs.</span><span class="sxs-lookup"><span data-stu-id="30cab-383">Fast forward to 15:00 to see the policies configured in the policy editor and then to 18:50 for a demonstration of calling an operation from the developer portal both with and without the required authorization token.</span></span>  
  
```xml  
<!-- Copy the following snippet into the inbound section at the api (or higher) level to pre-authorize access to operations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a><span data-ttu-id="30cab-384">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-384">Elements</span></span>  
  
|<span data-ttu-id="30cab-385">Element</span><span class="sxs-lookup"><span data-stu-id="30cab-385">Element</span></span>|<span data-ttu-id="30cab-386">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-386">Description</span></span>|<span data-ttu-id="30cab-387">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="30cab-388">Validera jwt</span><span class="sxs-lookup"><span data-stu-id="30cab-388">validate-jwt</span></span>|<span data-ttu-id="30cab-389">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="30cab-389">Root element.</span></span>|<span data-ttu-id="30cab-390">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-390">Yes</span></span>|  
|<span data-ttu-id="30cab-391">målgrupper</span><span class="sxs-lookup"><span data-stu-id="30cab-391">audiences</span></span>|<span data-ttu-id="30cab-392">Innehåller en lista över godkända målgruppen anspråk som kan ingå i token.</span><span class="sxs-lookup"><span data-stu-id="30cab-392">Contains a list of acceptable audience claims that can be present on the token.</span></span> <span data-ttu-id="30cab-393">Om flera målgruppen värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="30cab-394">Du måste ange minst en målgrupp.</span><span class="sxs-lookup"><span data-stu-id="30cab-394">At least one audience must be specified.</span></span>|<span data-ttu-id="30cab-395">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-395">No</span></span>|  
|<span data-ttu-id="30cab-396">utfärdaren signering nycklar</span><span class="sxs-lookup"><span data-stu-id="30cab-396">issuer-signing-keys</span></span>|<span data-ttu-id="30cab-397">En lista över Base64-kodad säkerhetsnycklar som används för att verifiera signerade token.</span><span class="sxs-lookup"><span data-stu-id="30cab-397">A list of Base64-encoded security keys used to validate signed tokens.</span></span> <span data-ttu-id="30cab-398">Om det finns flera säkerhetsnycklar och sedan varje nyckel testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas (användbart för token förnyelse).</span><span class="sxs-lookup"><span data-stu-id="30cab-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="30cab-399">Viktiga delar har en valfri `id` attribut som används för matchning mot `kid` anspråk.</span><span class="sxs-lookup"><span data-stu-id="30cab-399">Key elements have an optional `id` attribute used to match against `kid` claim.</span></span>|<span data-ttu-id="30cab-400">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-400">No</span></span>|  
|<span data-ttu-id="30cab-401">certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="30cab-401">issuers</span></span>|<span data-ttu-id="30cab-402">En lista över godkända säkerhetsobjekt som utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="30cab-402">A list of acceptable principals that issued the token.</span></span> <span data-ttu-id="30cab-403">Om flera utfärdaren värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="30cab-404">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-404">No</span></span>|  
|<span data-ttu-id="30cab-405">openid-config</span><span class="sxs-lookup"><span data-stu-id="30cab-405">openid-config</span></span>|<span data-ttu-id="30cab-406">Det element som används för att ange en kompatibel öppna ID configuration-slutpunkt som signering nycklar och utfärdaren kan hämtas.</span><span class="sxs-lookup"><span data-stu-id="30cab-406">The element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="30cab-407">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-407">No</span></span>|  
|<span data-ttu-id="30cab-408">krävs anspråk</span><span class="sxs-lookup"><span data-stu-id="30cab-408">required-claims</span></span>|<span data-ttu-id="30cab-409">Innehåller en lista över anspråkstyper som förväntas finnas på token att anses vara giltiga.</span><span class="sxs-lookup"><span data-stu-id="30cab-409">Contains a list of claims expected to be present on the token for it to be considered valid.</span></span> <span data-ttu-id="30cab-410">När den `match` -attributet är inställt på `all` varje anspråksvärde i principen måste finnas i token för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-410">When the `match` attribute is set to `all` every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="30cab-411">När den `match` -attributet är inställt på `any` måste finnas minst ett anspråk i token för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-411">When the `match` attribute is set to `any` at least one claim must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="30cab-412">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-412">No</span></span>|  
|<span data-ttu-id="30cab-413">zumo huvudnyckeln</span><span class="sxs-lookup"><span data-stu-id="30cab-413">zumo-master-key</span></span>|<span data-ttu-id="30cab-414">Huvudnyckeln för token som utfärdats av Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="30cab-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="30cab-415">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="30cab-416">Attribut</span><span class="sxs-lookup"><span data-stu-id="30cab-416">Attributes</span></span>  
  
|<span data-ttu-id="30cab-417">Namn</span><span class="sxs-lookup"><span data-stu-id="30cab-417">Name</span></span>|<span data-ttu-id="30cab-418">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="30cab-418">Description</span></span>|<span data-ttu-id="30cab-419">Krävs</span><span class="sxs-lookup"><span data-stu-id="30cab-419">Required</span></span>|<span data-ttu-id="30cab-420">Standard</span><span class="sxs-lookup"><span data-stu-id="30cab-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="30cab-421">förskjutning av klockan</span><span class="sxs-lookup"><span data-stu-id="30cab-421">clock-skew</span></span>|<span data-ttu-id="30cab-422">TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="30cab-422">Timespan.</span></span> <span data-ttu-id="30cab-423">Innehåller vissa små handtag om token upphör att gälla anspråk finns i token och har passerat aktuellt datum / tid.</span><span class="sxs-lookup"><span data-stu-id="30cab-423">Provides some small leeway in case the token's expiration claim is present in the token and is past the current date / time.</span></span>|<span data-ttu-id="30cab-424">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-424">No</span></span>|<span data-ttu-id="30cab-425">0 sekunder</span><span class="sxs-lookup"><span data-stu-id="30cab-425">0 seconds</span></span>|  
|<span data-ttu-id="30cab-426">misslyckades---valideringsfelmeddelande</span><span class="sxs-lookup"><span data-stu-id="30cab-426">failed-validation-error-message</span></span>|<span data-ttu-id="30cab-427">Felmeddelande om att returnera i texten för HTTP-svar om JWT inte klarar valideringen.</span><span class="sxs-lookup"><span data-stu-id="30cab-427">Error message to return in the HTTP response body if the JWT does not pass validation.</span></span> <span data-ttu-id="30cab-428">Det här meddelandet måste ha specialtecken (escaped) korrekt.</span><span class="sxs-lookup"><span data-stu-id="30cab-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="30cab-429">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-429">No</span></span>|<span data-ttu-id="30cab-430">Standardfelmeddelandet beror på verifieringsfel, till exempel ”JWT finns inte”.</span><span class="sxs-lookup"><span data-stu-id="30cab-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="30cab-431">misslyckades-validering-httpcode</span><span class="sxs-lookup"><span data-stu-id="30cab-431">failed-validation-httpcode</span></span>|<span data-ttu-id="30cab-432">HTTP-statuskoden ska returneras om JWT inte valideras.</span><span class="sxs-lookup"><span data-stu-id="30cab-432">HTTP Status code to return if the JWT doesn't pass validation.</span></span>|<span data-ttu-id="30cab-433">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-433">No</span></span>|<span data-ttu-id="30cab-434">401</span><span class="sxs-lookup"><span data-stu-id="30cab-434">401</span></span>|  
|<span data-ttu-id="30cab-435">huvudets namn</span><span class="sxs-lookup"><span data-stu-id="30cab-435">header-name</span></span>|<span data-ttu-id="30cab-436">Namnet på det HTTP-huvud som denna token.</span><span class="sxs-lookup"><span data-stu-id="30cab-436">The name of the HTTP header holding the token.</span></span>|<span data-ttu-id="30cab-437">Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="30cab-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="30cab-438">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-438">N/A</span></span>|  
|<span data-ttu-id="30cab-439">id</span><span class="sxs-lookup"><span data-stu-id="30cab-439">id</span></span>|<span data-ttu-id="30cab-440">Den `id` attributet för den `key` element kan du ange den sträng som matchas mot `kid` anspråk i token (om sådan finns) att ta reda på lämplig nyckel ska användas för signaturverifiering.</span><span class="sxs-lookup"><span data-stu-id="30cab-440">The `id` attribute on the `key` element allows you to specify the string that will be matched against `kid` claim in the token (if present) to find out the appropriate key to use for signature validation.</span></span>|<span data-ttu-id="30cab-441">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-441">No</span></span>|<span data-ttu-id="30cab-442">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-442">N/A</span></span>|  
|<span data-ttu-id="30cab-443">Matcha</span><span class="sxs-lookup"><span data-stu-id="30cab-443">match</span></span>|<span data-ttu-id="30cab-444">Den `match` attributet för den `claim` element anger om varje anspråksvärde i principen måste finnas i token för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-444">The `match` attribute on the `claim` element specifies whether every claim value in the policy must be present in the token for validation to succeed.</span></span> <span data-ttu-id="30cab-445">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="30cab-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="30cab-446">-                          `all`-värde för alla anspråk i principen måste finnas i token för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-446">-                          `all` - every claim value in the policy must be present in the token for validation to succeed.</span></span><br /><br /> <span data-ttu-id="30cab-447">-                          `any`-minst en anspråksvärdet måste finnas i token för verifiering ska lyckas.</span><span class="sxs-lookup"><span data-stu-id="30cab-447">-                          `any` - at least one claim value must be present in the token for validation to succeed.</span></span>|<span data-ttu-id="30cab-448">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-448">No</span></span>|<span data-ttu-id="30cab-449">Alla</span><span class="sxs-lookup"><span data-stu-id="30cab-449">all</span></span>|  
|<span data-ttu-id="30cab-450">paremeter frågenamnet</span><span class="sxs-lookup"><span data-stu-id="30cab-450">query-paremeter-name</span></span>|<span data-ttu-id="30cab-451">Namnet på den Frågeparametern denna token.</span><span class="sxs-lookup"><span data-stu-id="30cab-451">The name of the the query parameter holding the token.</span></span>|<span data-ttu-id="30cab-452">Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="30cab-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="30cab-453">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-453">N/A</span></span>|  
|<span data-ttu-id="30cab-454">kräver förfallotid</span><span class="sxs-lookup"><span data-stu-id="30cab-454">require-expiration-time</span></span>|<span data-ttu-id="30cab-455">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="30cab-455">Boolean.</span></span> <span data-ttu-id="30cab-456">Anger om ett förfallodatum anspråk krävs i token.</span><span class="sxs-lookup"><span data-stu-id="30cab-456">Specifies whether an expiration claim is required in the token.</span></span>|<span data-ttu-id="30cab-457">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-457">No</span></span>|<span data-ttu-id="30cab-458">SANT</span><span class="sxs-lookup"><span data-stu-id="30cab-458">true</span></span>|
|<span data-ttu-id="30cab-459">kräver schema</span><span class="sxs-lookup"><span data-stu-id="30cab-459">require-scheme</span></span>|<span data-ttu-id="30cab-460">Namnet på token schemat, t.ex. ”Ägar”.</span><span class="sxs-lookup"><span data-stu-id="30cab-460">The name of the token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="30cab-461">När det här attributet anges säkerställer principen för det angivna schemat finns i huvudvärde auktorisering.</span><span class="sxs-lookup"><span data-stu-id="30cab-461">When this attribute is set, the policy will ensure that specified scheme is present in the Authorization header value.</span></span>|<span data-ttu-id="30cab-462">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-462">No</span></span>|<span data-ttu-id="30cab-463">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-463">N/A</span></span>|
|<span data-ttu-id="30cab-464">Kräv-signerad-token</span><span class="sxs-lookup"><span data-stu-id="30cab-464">require-signed-tokens</span></span>|<span data-ttu-id="30cab-465">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="30cab-465">Boolean.</span></span> <span data-ttu-id="30cab-466">Anger om det krävs en token som ska signeras.</span><span class="sxs-lookup"><span data-stu-id="30cab-466">Specifies whether a token is required to be signed.</span></span>|<span data-ttu-id="30cab-467">Nej</span><span class="sxs-lookup"><span data-stu-id="30cab-467">No</span></span>|<span data-ttu-id="30cab-468">SANT</span><span class="sxs-lookup"><span data-stu-id="30cab-468">true</span></span>|  
|<span data-ttu-id="30cab-469">URL: en</span><span class="sxs-lookup"><span data-stu-id="30cab-469">url</span></span>|<span data-ttu-id="30cab-470">Öppna ID configuration slutpunkts-URL från där du kan hämta öppna ID configuration metadata.</span><span class="sxs-lookup"><span data-stu-id="30cab-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="30cab-471">Använd följande URL för Azure Active Directory: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` ersätter din klient katalognamn, t.ex. `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="30cab-471">For Azure Active Directory use the following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="30cab-472">Ja</span><span class="sxs-lookup"><span data-stu-id="30cab-472">Yes</span></span>|<span data-ttu-id="30cab-473">Saknas</span><span class="sxs-lookup"><span data-stu-id="30cab-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="30cab-474">Användning</span><span class="sxs-lookup"><span data-stu-id="30cab-474">Usage</span></span>  
 <span data-ttu-id="30cab-475">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="30cab-475">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="30cab-476">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="30cab-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="30cab-477">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="30cab-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="30cab-478">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30cab-478">Next steps</span></span>
<span data-ttu-id="30cab-479">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="30cab-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
