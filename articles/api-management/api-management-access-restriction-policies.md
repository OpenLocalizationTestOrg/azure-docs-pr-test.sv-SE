---
title: "principer för begränsning av aaaAzure API Management åtkomst | Microsoft Docs"
description: "Läs mer om hello åtkomst för programvarubegränsning kan användas i Azure API Management."
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
ms.openlocfilehash: 0ef368c2781d9a5cf9eaaa41a47489c904ed3198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-access-restriction-policies"></a><span data-ttu-id="8e224-103">Principer för begränsning av API Management-åtkomst</span><span class="sxs-lookup"><span data-stu-id="8e224-103">API Management access restriction policies</span></span>
<span data-ttu-id="8e224-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="8e224-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="8e224-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="8e224-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="8e224-106"><a name="AccessRestrictionPolicies"></a>Principer för begränsning av åtkomst</span><span class="sxs-lookup"><span data-stu-id="8e224-106"><a name="AccessRestrictionPolicies"></a> Access restriction policies</span></span>  
  
-   <span data-ttu-id="8e224-107">[Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="8e224-107">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
-   <span data-ttu-id="8e224-108">[Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8e224-108">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8e224-109">[Gränsen anropet frekvensen av nyckeln](#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="8e224-109">[Limit call rate by key](#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8e224-110">[Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="8e224-110">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
-   <span data-ttu-id="8e224-111">[Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8e224-111">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
-   <span data-ttu-id="8e224-112">[Kvot för uppsättningen av nyckeln](#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="8e224-112">[Set usage quota by key](#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
-   <span data-ttu-id="8e224-113">[Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="8e224-113">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
##  <span data-ttu-id="8e224-114"><a name="CheckHTTPHeader"></a>Kontrollera HTTP-huvud</span><span class="sxs-lookup"><span data-stu-id="8e224-114"><a name="CheckHTTPHeader"></a> Check HTTP header</span></span>  
 <span data-ttu-id="8e224-115">Använd hello `check-header` princip tooenforce att en begäran har en angivna HTTP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="8e224-115">Use hello `check-header` policy tooenforce that a request has a specified HTTP header.</span></span> <span data-ttu-id="8e224-116">Alternativt kan du kontrollera toosee om hello-huvud har ett specifikt värde eller Sök efter en tillåtna värdeintervallet.</span><span class="sxs-lookup"><span data-stu-id="8e224-116">You can optionally check toosee if hello header has a specific value or check for a range of allowed values.</span></span> <span data-ttu-id="8e224-117">Om hello kontrollen misslyckas hello princip avslutar bearbeta begäran och returnerar HTTP-status kod och fel hälsningsmeddelande anges av hello princip.</span><span class="sxs-lookup"><span data-stu-id="8e224-117">If hello check fails, hello policy terminates request processing and returns hello HTTP status code and error message specified by hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-118">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-118">Policy statement</span></span>  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-119">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-119">Example</span></span>  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a><span data-ttu-id="8e224-120">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-120">Elements</span></span>  
  
|<span data-ttu-id="8e224-121">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-121">Name</span></span>|<span data-ttu-id="8e224-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-122">Description</span></span>|<span data-ttu-id="8e224-123">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-123">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-124">Kontrollera-huvud</span><span class="sxs-lookup"><span data-stu-id="8e224-124">check-header</span></span>|<span data-ttu-id="8e224-125">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-125">Root element.</span></span>|<span data-ttu-id="8e224-126">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-126">Yes</span></span>|  
|<span data-ttu-id="8e224-127">värde</span><span class="sxs-lookup"><span data-stu-id="8e224-127">value</span></span>|<span data-ttu-id="8e224-128">Tillåtet värde för HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="8e224-128">Allowed HTTP header value.</span></span> <span data-ttu-id="8e224-129">När flera element med värdet har angetts, betraktas lyckas hello kontrollera om någon av hello värden finns en matchning.</span><span class="sxs-lookup"><span data-stu-id="8e224-129">When multiple value elements are specified, hello check is considered a success if any one of hello values is a match.</span></span>|<span data-ttu-id="8e224-130">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-130">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-131">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-131">Attributes</span></span>  
  
|<span data-ttu-id="8e224-132">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-132">Name</span></span>|<span data-ttu-id="8e224-133">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-133">Description</span></span>|<span data-ttu-id="8e224-134">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-134">Required</span></span>|<span data-ttu-id="8e224-135">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-135">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-136">misslyckades-check--felmeddelande</span><span class="sxs-lookup"><span data-stu-id="8e224-136">failed-check-error-message</span></span>|<span data-ttu-id="8e224-137">Fel meddelande tooreturn i hello HTTP svarstexten om hello huvudet finns inte eller har ett ogiltigt värde.</span><span class="sxs-lookup"><span data-stu-id="8e224-137">Error message tooreturn in hello HTTP response body if hello header doesn't exist or has an invalid value.</span></span> <span data-ttu-id="8e224-138">Det här meddelandet måste ha specialtecken (escaped) korrekt.</span><span class="sxs-lookup"><span data-stu-id="8e224-138">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8e224-139">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-139">Yes</span></span>|<span data-ttu-id="8e224-140">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-140">N/A</span></span>|  
|<span data-ttu-id="8e224-141">misslyckades-check-httpcode</span><span class="sxs-lookup"><span data-stu-id="8e224-141">failed-check-httpcode</span></span>|<span data-ttu-id="8e224-142">HTTP-Status kod tooreturn om hello huvudet finns inte eller har ett ogiltigt värde.</span><span class="sxs-lookup"><span data-stu-id="8e224-142">HTTP Status code tooreturn if hello header doesn't exist or has an invalid value.</span></span>|<span data-ttu-id="8e224-143">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-143">Yes</span></span>|<span data-ttu-id="8e224-144">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-144">N/A</span></span>|  
|<span data-ttu-id="8e224-145">huvudets namn</span><span class="sxs-lookup"><span data-stu-id="8e224-145">header-name</span></span>|<span data-ttu-id="8e224-146">hello namnet på hello HTTP-huvudet toocheck.</span><span class="sxs-lookup"><span data-stu-id="8e224-146">hello name of hello HTTP Header toocheck.</span></span>|<span data-ttu-id="8e224-147">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-147">Yes</span></span>|<span data-ttu-id="8e224-148">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-148">N/A</span></span>|  
|<span data-ttu-id="8e224-149">Ignorera skiftläge</span><span class="sxs-lookup"><span data-stu-id="8e224-149">ignore-case</span></span>|<span data-ttu-id="8e224-150">Kan ställas in tooTrue eller False.</span><span class="sxs-lookup"><span data-stu-id="8e224-150">Can be set tooTrue or False.</span></span> <span data-ttu-id="8e224-151">Om set tooTrue fallet ignoreras när hello huvudvärde jämförs hello uppsättning godkända värden.</span><span class="sxs-lookup"><span data-stu-id="8e224-151">If set tooTrue case is ignored when hello header value is compared against hello set of acceptable values.</span></span>|<span data-ttu-id="8e224-152">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-152">Yes</span></span>|<span data-ttu-id="8e224-153">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-153">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-154">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-154">Usage</span></span>  
 <span data-ttu-id="8e224-155">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-155">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-156">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="8e224-156">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="8e224-157">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-157">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8e224-158"><a name="LimitCallRate"></a>Gränsen anropet frekvensen av prenumerationen</span><span class="sxs-lookup"><span data-stu-id="8e224-158"><a name="LimitCallRate"></a> Limit call rate by subscription</span></span>  
 <span data-ttu-id="8e224-159">Hej `rate-limit` princip förhindrar API användning toppar på grundval av per prenumeration genom att begränsa hello anropa hastighet tooa anges antalet per angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="8e224-159">hello `rate-limit` policy prevents API usage spikes on a per subscription basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8e224-160">När den här principen utlöses hello anroparen tar emot en `429 Too Many Requests` Svarets statuskod.</span><span class="sxs-lookup"><span data-stu-id="8e224-160">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8e224-161">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="8e224-161">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8e224-162">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="8e224-162">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-163">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-163">Policy statement</span></span>  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-164">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-164">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8e224-165">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-165">Elements</span></span>  
  
|<span data-ttu-id="8e224-166">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-166">Name</span></span>|<span data-ttu-id="8e224-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-167">Description</span></span>|<span data-ttu-id="8e224-168">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-168">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-169">Ställ in gräns</span><span class="sxs-lookup"><span data-stu-id="8e224-169">set-limit</span></span>|<span data-ttu-id="8e224-170">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-170">Root element.</span></span>|<span data-ttu-id="8e224-171">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-171">Yes</span></span>|  
|<span data-ttu-id="8e224-172">api</span><span class="sxs-lookup"><span data-stu-id="8e224-172">api</span></span>|<span data-ttu-id="8e224-173">Lägg till en eller flera av dessa element tooimpose en anropet hastighetsbegränsning på API: er i hello produkten.</span><span class="sxs-lookup"><span data-stu-id="8e224-173">Add one  or more of these elements tooimpose a call rate limit on APIs within hello product.</span></span> <span data-ttu-id="8e224-174">Produkt- och API anropa räntesats gränser som är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="8e224-174">Product and API call rate limits are applied independently.</span></span>|<span data-ttu-id="8e224-175">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-175">No</span></span>|  
|<span data-ttu-id="8e224-176">åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-176">operation</span></span>|<span data-ttu-id="8e224-177">Lägg till en eller flera av dessa element tooimpose en anropet hastighetsbegränsning på åtgärder i en API.</span><span class="sxs-lookup"><span data-stu-id="8e224-177">Add one  or more of these elements tooimpose a call rate limit on operations within an API.</span></span> <span data-ttu-id="8e224-178">Produkt-, API- och åtgärden anropa räntesats gränser som är oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="8e224-178">Product, API, and operation call rate limits are applied independently.</span></span>|<span data-ttu-id="8e224-179">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-179">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-180">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-180">Attributes</span></span>  
  
|<span data-ttu-id="8e224-181">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-181">Name</span></span>|<span data-ttu-id="8e224-182">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-182">Description</span></span>|<span data-ttu-id="8e224-183">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-183">Required</span></span>|<span data-ttu-id="8e224-184">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-184">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-185">namn</span><span class="sxs-lookup"><span data-stu-id="8e224-185">name</span></span>|<span data-ttu-id="8e224-186">hello namnet på hello API för vilka tooapply hello hastighetsbegränsning.</span><span class="sxs-lookup"><span data-stu-id="8e224-186">hello name of hello API for which tooapply hello rate limit.</span></span>|<span data-ttu-id="8e224-187">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-187">Yes</span></span>|<span data-ttu-id="8e224-188">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-188">N/A</span></span>|  
|<span data-ttu-id="8e224-189">anrop</span><span class="sxs-lookup"><span data-stu-id="8e224-189">calls</span></span>|<span data-ttu-id="8e224-190">Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-190">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-191">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-191">Yes</span></span>|<span data-ttu-id="8e224-192">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-192">N/A</span></span>|  
|<span data-ttu-id="8e224-193">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="8e224-193">renewal-period</span></span>|<span data-ttu-id="8e224-194">hello tidsperiod i sekunder, efter vilken hello kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="8e224-194">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8e224-195">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-195">Yes</span></span>|<span data-ttu-id="8e224-196">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-196">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-197">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-197">Usage</span></span>  
 <span data-ttu-id="8e224-198">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-198">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-199">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-199">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-200">**Princip för scope:** produkt</span><span class="sxs-lookup"><span data-stu-id="8e224-200">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8e224-201"><a name="LimitCallRateByKey"></a>Gränsen anropet frekvensen av nyckel</span><span class="sxs-lookup"><span data-stu-id="8e224-201"><a name="LimitCallRateByKey"></a> Limit call rate by key</span></span>  
 <span data-ttu-id="8e224-202">Hej `rate-limit-by-key` princip förhindrar API användning toppar på grundval av per nyckel genom att begränsa hello anropa hastighet tooa anges antalet per angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="8e224-202">hello `rate-limit-by-key` policy prevents API usage spikes on a per key basis by limiting hello call rate tooa specified number per a specified time period.</span></span> <span data-ttu-id="8e224-203">hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="8e224-203">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8e224-204">Valfria steg villkor kan läggas till toospecify vilka begäranden som ska räknas till hello gränsen.</span><span class="sxs-lookup"><span data-stu-id="8e224-204">Optional increment condition can be added toospecify which requests should be counted towards hello limit.</span></span> <span data-ttu-id="8e224-205">När den här principen utlöses hello anroparen tar emot en `429 Too Many Requests` Svarets statuskod.</span><span class="sxs-lookup"><span data-stu-id="8e224-205">When this policy is triggered hello caller receives a `429 Too Many Requests` response status code.</span></span>  
  
 <span data-ttu-id="8e224-206">Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8e224-206">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8e224-207">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="8e224-207">This policy can be used only once per policy document.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-208">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-208">Policy statement</span></span>  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-209">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-209">Example</span></span>  
 <span data-ttu-id="8e224-210">I följande exempel hello, aktiveras hello hastighetsbegränsning hello anroparen IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="8e224-210">In hello following example, hello rate limit is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8e224-211">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-211">Elements</span></span>  
  
|<span data-ttu-id="8e224-212">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-212">Name</span></span>|<span data-ttu-id="8e224-213">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-213">Description</span></span>|<span data-ttu-id="8e224-214">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-214">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-215">Ställ in gräns</span><span class="sxs-lookup"><span data-stu-id="8e224-215">set-limit</span></span>|<span data-ttu-id="8e224-216">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-216">Root element.</span></span>|<span data-ttu-id="8e224-217">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-217">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-218">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-218">Attributes</span></span>  
  
|<span data-ttu-id="8e224-219">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-219">Name</span></span>|<span data-ttu-id="8e224-220">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-220">Description</span></span>|<span data-ttu-id="8e224-221">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-221">Required</span></span>|<span data-ttu-id="8e224-222">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-222">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-223">anrop</span><span class="sxs-lookup"><span data-stu-id="8e224-223">calls</span></span>|<span data-ttu-id="8e224-224">Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-224">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-225">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-225">Yes</span></span>|<span data-ttu-id="8e224-226">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-226">N/A</span></span>|  
|<span data-ttu-id="8e224-227">motparterna nyckel</span><span class="sxs-lookup"><span data-stu-id="8e224-227">counter-key</span></span>|<span data-ttu-id="8e224-228">hello viktiga toouse för hello hastighet gränsen principen.</span><span class="sxs-lookup"><span data-stu-id="8e224-228">hello key toouse for hello rate limit policy.</span></span>|<span data-ttu-id="8e224-229">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-229">Yes</span></span>|<span data-ttu-id="8e224-230">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-230">N/A</span></span>|  
|<span data-ttu-id="8e224-231">öka villkor</span><span class="sxs-lookup"><span data-stu-id="8e224-231">increment-condition</span></span>|<span data-ttu-id="8e224-232">hello booleskt uttryck som anger om hello begäran ska räknas mot hello kvoten (`true`).</span><span class="sxs-lookup"><span data-stu-id="8e224-232">hello boolean expression specifying if hello request should be counted towards hello quota (`true`).</span></span>|<span data-ttu-id="8e224-233">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-233">No</span></span>|<span data-ttu-id="8e224-234">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-234">N/A</span></span>|  
|<span data-ttu-id="8e224-235">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="8e224-235">renewal-period</span></span>|<span data-ttu-id="8e224-236">hello tidsperiod i sekunder, efter vilken hello kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="8e224-236">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8e224-237">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-237">Yes</span></span>|<span data-ttu-id="8e224-238">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-238">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-239">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-239">Usage</span></span>  
 <span data-ttu-id="8e224-240">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-240">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-241">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-241">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-242">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-242">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8e224-243"><a name="RestrictCallerIPs"></a>Begränsa anroparen IP-adresser</span><span class="sxs-lookup"><span data-stu-id="8e224-243"><a name="RestrictCallerIPs"></a> Restrict caller IPs</span></span>  
 <span data-ttu-id="8e224-244">Hej `ip-filter` princip filtrerar (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="8e224-244">hello `ip-filter` policy filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-245">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-245">Policy statement</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-246">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-246">Example</span></span>  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a><span data-ttu-id="8e224-247">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-247">Elements</span></span>  
  
|<span data-ttu-id="8e224-248">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-248">Name</span></span>|<span data-ttu-id="8e224-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-249">Description</span></span>|<span data-ttu-id="8e224-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-250">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-251">IP-filter</span><span class="sxs-lookup"><span data-stu-id="8e224-251">ip-filter</span></span>|<span data-ttu-id="8e224-252">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-252">Root element.</span></span>|<span data-ttu-id="8e224-253">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-253">Yes</span></span>|  
|<span data-ttu-id="8e224-254">Adress</span><span class="sxs-lookup"><span data-stu-id="8e224-254">address</span></span>|<span data-ttu-id="8e224-255">Anger en IP-adress på vilken toofilter.</span><span class="sxs-lookup"><span data-stu-id="8e224-255">Specifies a single IP address on which toofilter.</span></span>|<span data-ttu-id="8e224-256">Minst en `address` eller `address-range` -element krävs.</span><span class="sxs-lookup"><span data-stu-id="8e224-256">At least one `address` or `address-range` element is required.</span></span>|  
|<span data-ttu-id="8e224-257">adressintervallet från = ”adress” till = ”adress”</span><span class="sxs-lookup"><span data-stu-id="8e224-257">address-range from="address" to="address"</span></span>|<span data-ttu-id="8e224-258">Anger ett intervall med IP-adress på vilken toofilter.</span><span class="sxs-lookup"><span data-stu-id="8e224-258">Specifies a range of IP address on which toofilter.</span></span>|<span data-ttu-id="8e224-259">Minst en `address` eller `address-range` -element krävs.</span><span class="sxs-lookup"><span data-stu-id="8e224-259">At least one `address` or `address-range` element is required.</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-260">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-260">Attributes</span></span>  
  
|<span data-ttu-id="8e224-261">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-261">Name</span></span>|<span data-ttu-id="8e224-262">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-262">Description</span></span>|<span data-ttu-id="8e224-263">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-263">Required</span></span>|<span data-ttu-id="8e224-264">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-264">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-265">adressintervallet från = ”adress” till = ”adress”</span><span class="sxs-lookup"><span data-stu-id="8e224-265">address-range from="address" to="address"</span></span>|<span data-ttu-id="8e224-266">Ett intervall med IP-adresser tooallow eller neka åtkomst för.</span><span class="sxs-lookup"><span data-stu-id="8e224-266">A range of IP addresses tooallow or deny access for.</span></span>|<span data-ttu-id="8e224-267">Krävs när hello `address-range` elementet används.</span><span class="sxs-lookup"><span data-stu-id="8e224-267">Required when hello `address-range` element is used.</span></span>|<span data-ttu-id="8e224-268">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-268">N/A</span></span>|  
|<span data-ttu-id="8e224-269">IP-filteråtgärd = ”Tillåt &#124; förbjuda ”</span><span class="sxs-lookup"><span data-stu-id="8e224-269">ip-filter action="allow &#124; forbid"</span></span>|<span data-ttu-id="8e224-270">Anger om anrop ska tillåtas eller inte för hello angivna IP-adresser och intervall.</span><span class="sxs-lookup"><span data-stu-id="8e224-270">Specifies whether calls should be allowed or not for hello specified IP addresses and ranges.</span></span>|<span data-ttu-id="8e224-271">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-271">Yes</span></span>|<span data-ttu-id="8e224-272">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-272">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-273">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-273">Usage</span></span>  
 <span data-ttu-id="8e224-274">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-274">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-275">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-275">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-276">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-276">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8e224-277"><a name="SetUsageQuota"></a>Ange kvot för prenumeration</span><span class="sxs-lookup"><span data-stu-id="8e224-277"><a name="SetUsageQuota"></a> Set usage quota by subscription</span></span>  
 <span data-ttu-id="8e224-278">Hej `quota` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8e224-278">hello `quota` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8e224-279">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="8e224-279">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8e224-280">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="8e224-280">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-281">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-281">Policy statement</span></span>  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-282">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-282">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8e224-283">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-283">Elements</span></span>  
  
|<span data-ttu-id="8e224-284">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-284">Name</span></span>|<span data-ttu-id="8e224-285">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-285">Description</span></span>|<span data-ttu-id="8e224-286">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-286">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-287">kvot</span><span class="sxs-lookup"><span data-stu-id="8e224-287">quota</span></span>|<span data-ttu-id="8e224-288">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-288">Root element.</span></span>|<span data-ttu-id="8e224-289">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-289">Yes</span></span>|  
|<span data-ttu-id="8e224-290">api</span><span class="sxs-lookup"><span data-stu-id="8e224-290">api</span></span>|<span data-ttu-id="8e224-291">Lägg till en eller flera av dessa element tooimpose en kvot på API: er i hello produkten.</span><span class="sxs-lookup"><span data-stu-id="8e224-291">Add one  or more of these elements tooimpose a quota on APIs within hello product.</span></span> <span data-ttu-id="8e224-292">Produkt- och API-kvoter tillämpas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="8e224-292">Product and API quotas are applied independently.</span></span>|<span data-ttu-id="8e224-293">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-293">No</span></span>|  
|<span data-ttu-id="8e224-294">åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-294">operation</span></span>|<span data-ttu-id="8e224-295">Lägg till en eller flera av dessa element tooimpose en kvot på åtgärder i en API.</span><span class="sxs-lookup"><span data-stu-id="8e224-295">Add one  or more of these elements tooimpose a quota on operations within an API.</span></span> <span data-ttu-id="8e224-296">Produkt-, API- och åtgärden kvoter tillämpas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="8e224-296">Product, API, and operation quotas are applied independently.</span></span>|<span data-ttu-id="8e224-297">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-297">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-298">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-298">Attributes</span></span>  
  
|<span data-ttu-id="8e224-299">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-299">Name</span></span>|<span data-ttu-id="8e224-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-300">Description</span></span>|<span data-ttu-id="8e224-301">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-301">Required</span></span>|<span data-ttu-id="8e224-302">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-302">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-303">namn</span><span class="sxs-lookup"><span data-stu-id="8e224-303">name</span></span>|<span data-ttu-id="8e224-304">hello namnet på hello API eller åtgärden som hello kvoten gäller.</span><span class="sxs-lookup"><span data-stu-id="8e224-304">hello name of hello API or operation for which hello quota applies.</span></span>|<span data-ttu-id="8e224-305">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-305">Yes</span></span>|<span data-ttu-id="8e224-306">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-306">N/A</span></span>|  
|<span data-ttu-id="8e224-307">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="8e224-307">bandwidth</span></span>|<span data-ttu-id="8e224-308">Hej maximalt antal kilobyte får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-308">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-309">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="8e224-309">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8e224-310">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-310">N/A</span></span>|  
|<span data-ttu-id="8e224-311">anrop</span><span class="sxs-lookup"><span data-stu-id="8e224-311">calls</span></span>|<span data-ttu-id="8e224-312">Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-312">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-313">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="8e224-313">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8e224-314">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-314">N/A</span></span>|  
|<span data-ttu-id="8e224-315">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="8e224-315">renewal-period</span></span>|<span data-ttu-id="8e224-316">hello tidsperiod i sekunder, efter vilken hello kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="8e224-316">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8e224-317">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-317">Yes</span></span>|<span data-ttu-id="8e224-318">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-318">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-319">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-319">Usage</span></span>  
 <span data-ttu-id="8e224-320">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-320">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-321">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-321">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-322">**Princip för scope:** produkt</span><span class="sxs-lookup"><span data-stu-id="8e224-322">**Policy scopes:** product</span></span>  
  
##  <span data-ttu-id="8e224-323"><a name="SetUsageQuotaByKey"></a>Kvot för uppsättningen av nyckel</span><span class="sxs-lookup"><span data-stu-id="8e224-323"><a name="SetUsageQuotaByKey"></a> Set usage quota by key</span></span>  
 <span data-ttu-id="8e224-324">Hej `quota-by-key` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="8e224-324">hello `quota-by-key` policy enforces a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span> <span data-ttu-id="8e224-325">hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.</span><span class="sxs-lookup"><span data-stu-id="8e224-325">hello key can have an arbitrary string value and is typically provided using a policy expression.</span></span> <span data-ttu-id="8e224-326">Valfria steg villkor kan läggas till toospecify vilka begäranden som ska räknas till hello kvoten.</span><span class="sxs-lookup"><span data-stu-id="8e224-326">Optional increment condition can be added toospecify which requests should be counted towards hello quota.</span></span>  
  
 <span data-ttu-id="8e224-327">Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span><span class="sxs-lookup"><span data-stu-id="8e224-327">For more information and examples of this policy, see [Advanced request throttling with Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8e224-328">Den här principen kan användas en gång per dokument.</span><span class="sxs-lookup"><span data-stu-id="8e224-328">This policy can be used only once per policy document.</span></span>  
>   
>  <span data-ttu-id="8e224-329">[Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.</span><span class="sxs-lookup"><span data-stu-id="8e224-329">[Policy expressions](api-management-policy-expressions.md) cannot be used in any of hello policy attributes for this policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-330">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-330">Policy statement</span></span>  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="8e224-331">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-331">Example</span></span>  
 <span data-ttu-id="8e224-332">I följande exempel hello, aktiveras hello kvoten av hello anroparen IP-adress.</span><span class="sxs-lookup"><span data-stu-id="8e224-332">In hello following example, hello quota is keyed by hello caller IP address.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="8e224-333">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-333">Elements</span></span>  
  
|<span data-ttu-id="8e224-334">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-334">Name</span></span>|<span data-ttu-id="8e224-335">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-335">Description</span></span>|<span data-ttu-id="8e224-336">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-336">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="8e224-337">kvot</span><span class="sxs-lookup"><span data-stu-id="8e224-337">quota</span></span>|<span data-ttu-id="8e224-338">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-338">Root element.</span></span>|<span data-ttu-id="8e224-339">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-339">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-340">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-340">Attributes</span></span>  
  
|<span data-ttu-id="8e224-341">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-341">Name</span></span>|<span data-ttu-id="8e224-342">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-342">Description</span></span>|<span data-ttu-id="8e224-343">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-343">Required</span></span>|<span data-ttu-id="8e224-344">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-344">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-345">Bandbredd</span><span class="sxs-lookup"><span data-stu-id="8e224-345">bandwidth</span></span>|<span data-ttu-id="8e224-346">Hej maximalt antal kilobyte får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-346">hello maximum total number of kilobytes allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-347">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="8e224-347">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8e224-348">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-348">N/A</span></span>|  
|<span data-ttu-id="8e224-349">anrop</span><span class="sxs-lookup"><span data-stu-id="8e224-349">calls</span></span>|<span data-ttu-id="8e224-350">Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.</span><span class="sxs-lookup"><span data-stu-id="8e224-350">hello maximum total number of calls allowed during hello time interval specified in hello `renewal-period`.</span></span>|<span data-ttu-id="8e224-351">Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.</span><span class="sxs-lookup"><span data-stu-id="8e224-351">Either `calls`, `bandwidth`, or both together must be specified.</span></span>|<span data-ttu-id="8e224-352">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-352">N/A</span></span>|  
|<span data-ttu-id="8e224-353">motparterna nyckel</span><span class="sxs-lookup"><span data-stu-id="8e224-353">counter-key</span></span>|<span data-ttu-id="8e224-354">hello viktiga toouse för hello kvoten principen.</span><span class="sxs-lookup"><span data-stu-id="8e224-354">hello key toouse for hello quota policy.</span></span>|<span data-ttu-id="8e224-355">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-355">Yes</span></span>|<span data-ttu-id="8e224-356">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-356">N/A</span></span>|  
|<span data-ttu-id="8e224-357">öka villkor</span><span class="sxs-lookup"><span data-stu-id="8e224-357">increment-condition</span></span>|<span data-ttu-id="8e224-358">hello booleskt uttryck som anger om hello begäran ska räknas mot hello kvoten (`true`)</span><span class="sxs-lookup"><span data-stu-id="8e224-358">hello boolean expression specifying if hello request should be counted towards hello quota (`true`)</span></span>|<span data-ttu-id="8e224-359">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-359">No</span></span>|<span data-ttu-id="8e224-360">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-360">N/A</span></span>|  
|<span data-ttu-id="8e224-361">förnyelseperioden</span><span class="sxs-lookup"><span data-stu-id="8e224-361">renewal-period</span></span>|<span data-ttu-id="8e224-362">hello tidsperiod i sekunder, efter vilken hello kvoten återställs.</span><span class="sxs-lookup"><span data-stu-id="8e224-362">hello time period in seconds after which hello quota resets.</span></span>|<span data-ttu-id="8e224-363">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-363">Yes</span></span>|<span data-ttu-id="8e224-364">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-364">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-365">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-365">Usage</span></span>  
 <span data-ttu-id="8e224-366">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-366">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-367">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-367">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-368">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-368">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="8e224-369"><a name="ValidateJWT"></a>Validera JWT</span><span class="sxs-lookup"><span data-stu-id="8e224-369"><a name="ValidateJWT"></a> Validate JWT</span></span>  
 <span data-ttu-id="8e224-370">Hej `validate-jwt` princip tillämpar existens och giltigheten för en JWT extraheras från antingen angivna HTTP-huvudet eller angivna frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="8e224-370">hello `validate-jwt` policy enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="8e224-371">Hej `validate-jwt` principen kräver att hello `exp` registrerade anspråk är som ingår i hello JWT-token, såvida inte `require-expiration-time` attribut anges och ställa in också`false`.</span><span class="sxs-lookup"><span data-stu-id="8e224-371">hello `validate-jwt` policy requires that hello `exp` registered claim is inlcuded in hello JWT token, unless `require-expiration-time` attribute is specified and set too`false`.</span></span>  
> <span data-ttu-id="8e224-372">Hej `validate-jwt` stöder HS256 och RS256 signering algoritmer.</span><span class="sxs-lookup"><span data-stu-id="8e224-372">hello `validate-jwt` policy supports HS256 and RS256 signing algorithms.</span></span> <span data-ttu-id="8e224-373">För HS256 anges hello nyckel infogad i hello princip hello base64-kodat format.</span><span class="sxs-lookup"><span data-stu-id="8e224-373">For HS256 hello key must be provided inline within hello policy in hello base64 encoded form.</span></span> <span data-ttu-id="8e224-374">För RS256 har hello-nyckel toobe tillhandahålla via en öppen ID configuration slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8e224-374">For RS256 hello key has toobe provide via an Open ID configuration endpoint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="8e224-375">Principframställning</span><span class="sxs-lookup"><span data-stu-id="8e224-375">Policy statement</span></span>  
  
```xml  
<validate-jwt   
    header-name="name of http header containing hello token (use query-parameter-name attribute if hello token is passed in hello URL)"   
    failed-validation-httpcode="http status code tooreturn on failure"   
    failed-validation-error-message="error message tooreturn on failure"   
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
    <claim name="name of hello claim as it appears in hello token" match="all|any">  
      <value>claim value as it is expected tooappear in hello token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of hello configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="8e224-376">Exempel</span><span class="sxs-lookup"><span data-stu-id="8e224-376">Examples</span></span>  
  
#### <a name="azure-mobile-services-token-validation"></a><span data-ttu-id="8e224-377">Azure Mobile Services token verifiering</span><span class="sxs-lookup"><span data-stu-id="8e224-377">Azure Mobile Services token validation</span></span>  
  
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
  
#### <a name="azure-active-directory-token-validation"></a><span data-ttu-id="8e224-378">Azure Active Directory token verifiering</span><span class="sxs-lookup"><span data-stu-id="8e224-378">Azure Active Directory token validation</span></span>  
  
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

  
#### <a name="azure-active-directory-b2c-token-validation"></a><span data-ttu-id="8e224-379">Azure Active Directory B2C-token verifiering</span><span class="sxs-lookup"><span data-stu-id="8e224-379">Azure Active Directory B2C token validation</span></span>  
  
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
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a><span data-ttu-id="8e224-380">Auktorisera åtkomst toooperations baserat på token anspråk</span><span class="sxs-lookup"><span data-stu-id="8e224-380">Authorize access toooperations based on token claims</span></span>  
 <span data-ttu-id="8e224-381">Det här exemplet illustrerar hur toouse hello [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) princip toopre-auktorisera åtkomst toooperations baserat på token anspråk.</span><span class="sxs-lookup"><span data-stu-id="8e224-381">This example shows how toouse hello [Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) policy toopre-authorize access toooperations based on token claims.</span></span> <span data-ttu-id="8e224-382">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too13:50.</span><span class="sxs-lookup"><span data-stu-id="8e224-382">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too13:50.</span></span> <span data-ttu-id="8e224-383">Vidarebefordra snabb too15:00 toosee hello principer som konfigurerats i hello redigeraren och sedan too18:50 för en demonstration av anropa en funktion från hello developer-portalen både med och utan hello krävs autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="8e224-383">Fast forward too15:00 toosee hello policies configured in hello policy editor and then too18:50 for a demonstration of calling an operation from hello developer portal both with and without hello required authorization token.</span></span>  
  
```xml  
<!-- Copy hello following snippet into hello inbound section at hello api (or higher) level toopre-authorize access toooperations based on token claims -->  
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
  
### <a name="elements"></a><span data-ttu-id="8e224-384">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-384">Elements</span></span>  
  
|<span data-ttu-id="8e224-385">Element</span><span class="sxs-lookup"><span data-stu-id="8e224-385">Element</span></span>|<span data-ttu-id="8e224-386">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-386">Description</span></span>|<span data-ttu-id="8e224-387">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-387">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="8e224-388">Validera jwt</span><span class="sxs-lookup"><span data-stu-id="8e224-388">validate-jwt</span></span>|<span data-ttu-id="8e224-389">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="8e224-389">Root element.</span></span>|<span data-ttu-id="8e224-390">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-390">Yes</span></span>|  
|<span data-ttu-id="8e224-391">målgrupper</span><span class="sxs-lookup"><span data-stu-id="8e224-391">audiences</span></span>|<span data-ttu-id="8e224-392">Innehåller en lista över godkända målgruppen anspråk som kan finnas på hello-token.</span><span class="sxs-lookup"><span data-stu-id="8e224-392">Contains a list of acceptable audience claims that can be present on hello token.</span></span> <span data-ttu-id="8e224-393">Om flera målgruppen värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas.</span><span class="sxs-lookup"><span data-stu-id="8e224-393">If multiple audience values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span> <span data-ttu-id="8e224-394">Du måste ange minst en målgrupp.</span><span class="sxs-lookup"><span data-stu-id="8e224-394">At least one audience must be specified.</span></span>|<span data-ttu-id="8e224-395">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-395">No</span></span>|  
|<span data-ttu-id="8e224-396">utfärdaren signering nycklar</span><span class="sxs-lookup"><span data-stu-id="8e224-396">issuer-signing-keys</span></span>|<span data-ttu-id="8e224-397">En lista över Base64-kodad säkerhet nycklar som används toovalidate signerade token.</span><span class="sxs-lookup"><span data-stu-id="8e224-397">A list of Base64-encoded security keys used toovalidate signed tokens.</span></span> <span data-ttu-id="8e224-398">Om det finns flera säkerhetsnycklar och sedan varje nyckel testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas (användbart för token förnyelse).</span><span class="sxs-lookup"><span data-stu-id="8e224-398">If multiple security keys are present, then each key is tried until either all are exhausted (in which case validation fails) or until one succeeds (useful for token rollover).</span></span> <span data-ttu-id="8e224-399">Viktiga delar har en valfri `id` attribut som används toomatch mot `kid` anspråk.</span><span class="sxs-lookup"><span data-stu-id="8e224-399">Key elements have an optional `id` attribute used toomatch against `kid` claim.</span></span>|<span data-ttu-id="8e224-400">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-400">No</span></span>|  
|<span data-ttu-id="8e224-401">certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="8e224-401">issuers</span></span>|<span data-ttu-id="8e224-402">En lista över godkända säkerhetsobjekt som utfärdats hello-token.</span><span class="sxs-lookup"><span data-stu-id="8e224-402">A list of acceptable principals that issued hello token.</span></span> <span data-ttu-id="8e224-403">Om flera utfärdaren värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas.</span><span class="sxs-lookup"><span data-stu-id="8e224-403">If multiple issuer values are present, then each value is tried until either all are exhausted (in which case validation fails) or until one succeeds.</span></span>|<span data-ttu-id="8e224-404">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-404">No</span></span>|  
|<span data-ttu-id="8e224-405">openid-config</span><span class="sxs-lookup"><span data-stu-id="8e224-405">openid-config</span></span>|<span data-ttu-id="8e224-406">hello-element som används för att ange en kompatibel öppna ID configuration-slutpunkt som signering nycklar och utfärdaren kan hämtas.</span><span class="sxs-lookup"><span data-stu-id="8e224-406">hello element used for specifying a compliant Open ID configuration endpoint from which signing keys and issuer can be obtained.</span></span>|<span data-ttu-id="8e224-407">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-407">No</span></span>|  
|<span data-ttu-id="8e224-408">krävs anspråk</span><span class="sxs-lookup"><span data-stu-id="8e224-408">required-claims</span></span>|<span data-ttu-id="8e224-409">Innehåller en lista över anspråk som förväntat toobe finns på hello token för den toobe anses vara giltigt.</span><span class="sxs-lookup"><span data-stu-id="8e224-409">Contains a list of claims expected toobe present on hello token for it toobe considered valid.</span></span> <span data-ttu-id="8e224-410">När hello `match` attribut har angetts för`all` var anspråksvärdet i hello princip måste finnas i hello token för verifiering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8e224-410">When hello `match` attribute is set too`all` every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8e224-411">När hello `match` attribut har angetts för`any` minst ett anspråk måste finnas i hello token för verifiering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8e224-411">When hello `match` attribute is set too`any` at least one claim must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8e224-412">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-412">No</span></span>|  
|<span data-ttu-id="8e224-413">zumo huvudnyckeln</span><span class="sxs-lookup"><span data-stu-id="8e224-413">zumo-master-key</span></span>|<span data-ttu-id="8e224-414">Huvudnyckeln för token som utfärdats av Azure Mobile Services</span><span class="sxs-lookup"><span data-stu-id="8e224-414">Master key for tokens issued by Azure Mobile Services</span></span>|<span data-ttu-id="8e224-415">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-415">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="8e224-416">Attribut</span><span class="sxs-lookup"><span data-stu-id="8e224-416">Attributes</span></span>  
  
|<span data-ttu-id="8e224-417">Namn</span><span class="sxs-lookup"><span data-stu-id="8e224-417">Name</span></span>|<span data-ttu-id="8e224-418">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8e224-418">Description</span></span>|<span data-ttu-id="8e224-419">Krävs</span><span class="sxs-lookup"><span data-stu-id="8e224-419">Required</span></span>|<span data-ttu-id="8e224-420">Standard</span><span class="sxs-lookup"><span data-stu-id="8e224-420">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="8e224-421">förskjutning av klockan</span><span class="sxs-lookup"><span data-stu-id="8e224-421">clock-skew</span></span>|<span data-ttu-id="8e224-422">TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="8e224-422">Timespan.</span></span> <span data-ttu-id="8e224-423">Innehåller vissa små handtag om hello-token upphör att gälla anspråk finns i hello token och har passerat hello aktuellt datum / tid.</span><span class="sxs-lookup"><span data-stu-id="8e224-423">Provides some small leeway in case hello token's expiration claim is present in hello token and is past hello current date / time.</span></span>|<span data-ttu-id="8e224-424">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-424">No</span></span>|<span data-ttu-id="8e224-425">0 sekunder</span><span class="sxs-lookup"><span data-stu-id="8e224-425">0 seconds</span></span>|  
|<span data-ttu-id="8e224-426">misslyckades---valideringsfelmeddelande</span><span class="sxs-lookup"><span data-stu-id="8e224-426">failed-validation-error-message</span></span>|<span data-ttu-id="8e224-427">Fel meddelande tooreturn i hello HTTP svarstexten om hello JWT inte klarar valideringen.</span><span class="sxs-lookup"><span data-stu-id="8e224-427">Error message tooreturn in hello HTTP response body if hello JWT does not pass validation.</span></span> <span data-ttu-id="8e224-428">Det här meddelandet måste ha specialtecken (escaped) korrekt.</span><span class="sxs-lookup"><span data-stu-id="8e224-428">This message must have any special characters properly escaped.</span></span>|<span data-ttu-id="8e224-429">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-429">No</span></span>|<span data-ttu-id="8e224-430">Standardfelmeddelandet beror på verifieringsfel, till exempel ”JWT finns inte”.</span><span class="sxs-lookup"><span data-stu-id="8e224-430">Default error message depends on validation issue, for example "JWT not present."</span></span>|  
|<span data-ttu-id="8e224-431">misslyckades-validering-httpcode</span><span class="sxs-lookup"><span data-stu-id="8e224-431">failed-validation-httpcode</span></span>|<span data-ttu-id="8e224-432">HTTP-Status kod tooreturn om hello JWT inte valideras.</span><span class="sxs-lookup"><span data-stu-id="8e224-432">HTTP Status code tooreturn if hello JWT doesn't pass validation.</span></span>|<span data-ttu-id="8e224-433">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-433">No</span></span>|<span data-ttu-id="8e224-434">401</span><span class="sxs-lookup"><span data-stu-id="8e224-434">401</span></span>|  
|<span data-ttu-id="8e224-435">huvudets namn</span><span class="sxs-lookup"><span data-stu-id="8e224-435">header-name</span></span>|<span data-ttu-id="8e224-436">hello namnet på hello HTTP-huvud innehåller hello-token.</span><span class="sxs-lookup"><span data-stu-id="8e224-436">hello name of hello HTTP header holding hello token.</span></span>|<span data-ttu-id="8e224-437">Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="8e224-437">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8e224-438">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-438">N/A</span></span>|  
|<span data-ttu-id="8e224-439">id</span><span class="sxs-lookup"><span data-stu-id="8e224-439">id</span></span>|<span data-ttu-id="8e224-440">Hej `id` -attributet på hello `key` -elementet kan du toospecify hello sträng som matchas mot `kid` anspråk i hello token (om sådan finns) toofind ut hello lämpliga nycklar toouse för signaturverifiering.</span><span class="sxs-lookup"><span data-stu-id="8e224-440">hello `id` attribute on hello `key` element allows you toospecify hello string that will be matched against `kid` claim in hello token (if present) toofind out hello appropriate key toouse for signature validation.</span></span>|<span data-ttu-id="8e224-441">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-441">No</span></span>|<span data-ttu-id="8e224-442">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-442">N/A</span></span>|  
|<span data-ttu-id="8e224-443">Matcha</span><span class="sxs-lookup"><span data-stu-id="8e224-443">match</span></span>|<span data-ttu-id="8e224-444">Hej `match` -attributet på hello `claim` element anger om varje anspråksvärde i hello princip måste finnas i hello token för verifiering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8e224-444">hello `match` attribute on hello `claim` element specifies whether every claim value in hello policy must be present in hello token for validation toosucceed.</span></span> <span data-ttu-id="8e224-445">Möjliga värden:</span><span class="sxs-lookup"><span data-stu-id="8e224-445">Possible values are:</span></span><br /><br /> <span data-ttu-id="8e224-446">-                          `all`-varje anspråksvärdet i hello princip måste finnas i hello token för verifiering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8e224-446">-                          `all` - every claim value in hello policy must be present in hello token for validation toosucceed.</span></span><br /><br /> <span data-ttu-id="8e224-447">-                          `any`-minst en anspråksvärdet måste finnas i hello token för verifiering toosucceed.</span><span class="sxs-lookup"><span data-stu-id="8e224-447">-                          `any` - at least one claim value must be present in hello token for validation toosucceed.</span></span>|<span data-ttu-id="8e224-448">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-448">No</span></span>|<span data-ttu-id="8e224-449">Alla</span><span class="sxs-lookup"><span data-stu-id="8e224-449">all</span></span>|  
|<span data-ttu-id="8e224-450">paremeter frågenamnet</span><span class="sxs-lookup"><span data-stu-id="8e224-450">query-paremeter-name</span></span>|<span data-ttu-id="8e224-451">hello namnet på hello hello frågeparameter hålla hello-token.</span><span class="sxs-lookup"><span data-stu-id="8e224-451">hello name of hello hello query parameter holding hello token.</span></span>|<span data-ttu-id="8e224-452">Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.</span><span class="sxs-lookup"><span data-stu-id="8e224-452">Either `header-name` or `query-paremeter-name` must be specified; but not both.</span></span>|<span data-ttu-id="8e224-453">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-453">N/A</span></span>|  
|<span data-ttu-id="8e224-454">kräver förfallotid</span><span class="sxs-lookup"><span data-stu-id="8e224-454">require-expiration-time</span></span>|<span data-ttu-id="8e224-455">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="8e224-455">Boolean.</span></span> <span data-ttu-id="8e224-456">Anger om ett förfallodatum anspråk krävs i hello-token.</span><span class="sxs-lookup"><span data-stu-id="8e224-456">Specifies whether an expiration claim is required in hello token.</span></span>|<span data-ttu-id="8e224-457">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-457">No</span></span>|<span data-ttu-id="8e224-458">SANT</span><span class="sxs-lookup"><span data-stu-id="8e224-458">true</span></span>|
|<span data-ttu-id="8e224-459">kräver schema</span><span class="sxs-lookup"><span data-stu-id="8e224-459">require-scheme</span></span>|<span data-ttu-id="8e224-460">Hej namnet på hello token-schemat, t.ex. ”Ägar”.</span><span class="sxs-lookup"><span data-stu-id="8e224-460">hello name of hello token scheme, e.g. "Bearer".</span></span> <span data-ttu-id="8e224-461">När det här attributet anges säkerställer hello princip som angetts schemat finns i hello auktorisering huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="8e224-461">When this attribute is set, hello policy will ensure that specified scheme is present in hello Authorization header value.</span></span>|<span data-ttu-id="8e224-462">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-462">No</span></span>|<span data-ttu-id="8e224-463">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-463">N/A</span></span>|
|<span data-ttu-id="8e224-464">Kräv-signerad-token</span><span class="sxs-lookup"><span data-stu-id="8e224-464">require-signed-tokens</span></span>|<span data-ttu-id="8e224-465">Booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="8e224-465">Boolean.</span></span> <span data-ttu-id="8e224-466">Anger om nödvändiga toobe signerade token.</span><span class="sxs-lookup"><span data-stu-id="8e224-466">Specifies whether a token is required toobe signed.</span></span>|<span data-ttu-id="8e224-467">Nej</span><span class="sxs-lookup"><span data-stu-id="8e224-467">No</span></span>|<span data-ttu-id="8e224-468">SANT</span><span class="sxs-lookup"><span data-stu-id="8e224-468">true</span></span>|  
|<span data-ttu-id="8e224-469">URL: en</span><span class="sxs-lookup"><span data-stu-id="8e224-469">url</span></span>|<span data-ttu-id="8e224-470">Öppna ID configuration slutpunkts-URL från där du kan hämta öppna ID configuration metadata.</span><span class="sxs-lookup"><span data-stu-id="8e224-470">Open ID configuration endpoint URL from where Open ID configuration metadata can be obtained.</span></span> <span data-ttu-id="8e224-471">Använd hello följande URL: en för Azure Active Directory: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` ersätter din klient katalognamn, t.ex. `contoso.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="8e224-471">For Azure Active Directory use hello following URL: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` substituting your directory tenant name, e.g. `contoso.onmicrosoft.com`.</span></span>|<span data-ttu-id="8e224-472">Ja</span><span class="sxs-lookup"><span data-stu-id="8e224-472">Yes</span></span>|<span data-ttu-id="8e224-473">Saknas</span><span class="sxs-lookup"><span data-stu-id="8e224-473">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="8e224-474">Användning</span><span class="sxs-lookup"><span data-stu-id="8e224-474">Usage</span></span>  
 <span data-ttu-id="8e224-475">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="8e224-475">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="8e224-476">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="8e224-476">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="8e224-477">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="8e224-477">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="8e224-478">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e224-478">Next steps</span></span>
<span data-ttu-id="8e224-479">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="8e224-479">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
