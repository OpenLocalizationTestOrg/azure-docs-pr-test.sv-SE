---
title: "principer för anspråksomvandling aaaAzure API-hantering | Microsoft Docs"
description: "Läs mer om hello omvandling principer kan användas i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 7406a8ce-5f9c-4fae-9b0f-e574befb2ee9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2891cc52d0017b717b3c12a98bc4941b5fd7ea78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-transformation-policies"></a><span data-ttu-id="cbed2-103">API Management-principer för anspråksomvandling</span><span class="sxs-lookup"><span data-stu-id="cbed2-103">API Management transformation policies</span></span>
<span data-ttu-id="cbed2-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="cbed2-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="cbed2-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="cbed2-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="cbed2-106"><a name="TransformationPolicies"></a>Principer för anspråksomvandling</span><span class="sxs-lookup"><span data-stu-id="cbed2-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="cbed2-107">[Konvertera JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – konverterar begäran eller svar body från JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="cbed2-107">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
-   <span data-ttu-id="cbed2-108">[Konvertera XML-tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – konverterar begäran eller svar body från XML-tooJSON.</span><span class="sxs-lookup"><span data-stu-id="cbed2-108">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
-   <span data-ttu-id="cbed2-109">[Sök och Ersätt strängen i brödtexten](api-management-transformation-policies.md#Findandreplacestringinbody) - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="cbed2-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="cbed2-110">[Maskera URL: er i innehåll](api-management-transformation-policies.md#MaskURLSContent) -skriver (masker) länkar hello svar body så att de pekar toohello motsvarande länk via hello gateway.</span><span class="sxs-lookup"><span data-stu-id="cbed2-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
-   <span data-ttu-id="cbed2-111">[Ange serverdelstjänst](api-management-transformation-policies.md#SetBackendService) -ändras hello serverdelstjänst för en inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="cbed2-112">[Konfigurera brödtext](api-management-transformation-policies.md#SetBody) -anger hello meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="cbed2-113">[Ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) - tilldelar en värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="cbed2-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="cbed2-114">[Ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="cbed2-115">[Skriv om URL: en](api-management-transformation-policies.md#RewriteURL) -konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
-   <span data-ttu-id="cbed2-116">[Transformera XML med hjälp av en XSLT](api-management-transformation-policies.md#XSLTransform) -gäller en XSL-transformation tooXML i hello begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
##  <span data-ttu-id="cbed2-117"><a name="ConvertJSONtoXML"></a>Konvertera JSON tooXML</span><span class="sxs-lookup"><span data-stu-id="cbed2-117"><a name="ConvertJSONtoXML"></a> Convert JSON tooXML</span></span>  
 <span data-ttu-id="cbed2-118">Hej `json-to-xml` princip konverterar brödtext begäran eller ett svar från JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="cbed2-118">hello `json-to-xml` policy converts a request or response body from JSON tooXML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-119">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-120">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-120">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <json-to-xml apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="cbed2-121">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-121">Elements</span></span>  
  
|<span data-ttu-id="cbed2-122">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-122">Name</span></span>|<span data-ttu-id="cbed2-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-123">Description</span></span>|<span data-ttu-id="cbed2-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-125">JSON-xml</span><span class="sxs-lookup"><span data-stu-id="cbed2-125">json-to-xml</span></span>|<span data-ttu-id="cbed2-126">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-126">Root element.</span></span>|<span data-ttu-id="cbed2-127">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="cbed2-128">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-128">Attributes</span></span>  
  
|<span data-ttu-id="cbed2-129">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-129">Name</span></span>|<span data-ttu-id="cbed2-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-130">Description</span></span>|<span data-ttu-id="cbed2-131">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-131">Required</span></span>|<span data-ttu-id="cbed2-132">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-133">tillämpa</span><span class="sxs-lookup"><span data-stu-id="cbed2-133">apply</span></span>|<span data-ttu-id="cbed2-134">hello-attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-134">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-135">-alltid - alltid gäller konvertering.</span><span class="sxs-lookup"><span data-stu-id="cbed2-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="cbed2-136">-innehåll typ-json - Konvertera endast om response Content-Type-huvud anger förekomsten av JSON.</span><span class="sxs-lookup"><span data-stu-id="cbed2-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="cbed2-137">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-137">Yes</span></span>|<span data-ttu-id="cbed2-138">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-138">N/A</span></span>|  
|<span data-ttu-id="cbed2-139">Överväg att acceptera-huvud</span><span class="sxs-lookup"><span data-stu-id="cbed2-139">consider-accept-header</span></span>|<span data-ttu-id="cbed2-140">hello-attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-140">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-141">tillämpa - true - konvertering om JSON begärs i begäran Accept-huvud.</span><span class="sxs-lookup"><span data-stu-id="cbed2-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="cbed2-142">-alltid gäller false - konvertering.</span><span class="sxs-lookup"><span data-stu-id="cbed2-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="cbed2-143">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-143">No</span></span>|<span data-ttu-id="cbed2-144">SANT</span><span class="sxs-lookup"><span data-stu-id="cbed2-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-145">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-145">Usage</span></span>  
 <span data-ttu-id="cbed2-146">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-146">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-147">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="cbed2-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="cbed2-148">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-149"><a name="ConvertXMLtoJSON"></a>Konvertera XML-tooJSON</span><span class="sxs-lookup"><span data-stu-id="cbed2-149"><a name="ConvertXMLtoJSON"></a> Convert XML tooJSON</span></span>  
 <span data-ttu-id="cbed2-150">Hej `xml-to-json` princip konverterar brödtext begäran eller ett svar från XML-tooJSON.</span><span class="sxs-lookup"><span data-stu-id="cbed2-150">hello `xml-to-json` policy converts a request or response body from XML tooJSON.</span></span> <span data-ttu-id="cbed2-151">Den här principen kan vara används toomodernize API: er baserat på backend endast XML-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="cbed2-151">This policy can be used toomodernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-152">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-153">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-153">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
        <xml-to-json kind="direct" apply="always" consider-accept-header="false" />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="cbed2-154">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-154">Elements</span></span>  
  
|<span data-ttu-id="cbed2-155">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-155">Name</span></span>|<span data-ttu-id="cbed2-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-156">Description</span></span>|<span data-ttu-id="cbed2-157">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-158">XML-json</span><span class="sxs-lookup"><span data-stu-id="cbed2-158">xml-to-json</span></span>|<span data-ttu-id="cbed2-159">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-159">Root element.</span></span>|<span data-ttu-id="cbed2-160">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="cbed2-161">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-161">Attributes</span></span>  
  
|<span data-ttu-id="cbed2-162">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-162">Name</span></span>|<span data-ttu-id="cbed2-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-163">Description</span></span>|<span data-ttu-id="cbed2-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-164">Required</span></span>|<span data-ttu-id="cbed2-165">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-166">typ</span><span class="sxs-lookup"><span data-stu-id="cbed2-166">kind</span></span>|<span data-ttu-id="cbed2-167">hello-attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-167">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-168">-javascript anpassad - hello konverteras JSON har ett eget tooJavaScript utvecklare för formuläret.</span><span class="sxs-lookup"><span data-stu-id="cbed2-168">-   javascript-friendly - hello converted JSON has a form friendly tooJavaScript developers.</span></span><br /><span data-ttu-id="cbed2-169">-direkt - visar hello konverterade JSON hello ursprungliga XML-dokumentets struktur.</span><span class="sxs-lookup"><span data-stu-id="cbed2-169">-   direct - hello converted JSON reflects hello original XML document's structure.</span></span>|<span data-ttu-id="cbed2-170">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-170">Yes</span></span>|<span data-ttu-id="cbed2-171">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-171">N/A</span></span>|  
|<span data-ttu-id="cbed2-172">tillämpa</span><span class="sxs-lookup"><span data-stu-id="cbed2-172">apply</span></span>|<span data-ttu-id="cbed2-173">hello-attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-173">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-174">-alltid - konvertera alltid.</span><span class="sxs-lookup"><span data-stu-id="cbed2-174">-   always - convert always.</span></span><br /><span data-ttu-id="cbed2-175">-innehåll-typ-xml - Konvertera endast om response Content-Type-huvud anger förekomsten av XML.</span><span class="sxs-lookup"><span data-stu-id="cbed2-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="cbed2-176">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-176">Yes</span></span>|<span data-ttu-id="cbed2-177">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-177">N/A</span></span>|  
|<span data-ttu-id="cbed2-178">Överväg att acceptera-huvud</span><span class="sxs-lookup"><span data-stu-id="cbed2-178">consider-accept-header</span></span>|<span data-ttu-id="cbed2-179">hello-attributet måste anges tooone av hello följande värden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-179">hello attribute must be set tooone of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-180">tillämpa - true - konvertering om begärs XML i begäran Accept-huvud.</span><span class="sxs-lookup"><span data-stu-id="cbed2-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="cbed2-181">-alltid gäller false - konvertering.</span><span class="sxs-lookup"><span data-stu-id="cbed2-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="cbed2-182">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-182">No</span></span>|<span data-ttu-id="cbed2-183">SANT</span><span class="sxs-lookup"><span data-stu-id="cbed2-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-184">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-184">Usage</span></span>  
 <span data-ttu-id="cbed2-185">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-185">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-186">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="cbed2-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="cbed2-187">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-188"><a name="Findandreplacestringinbody"></a>Sök och Ersätt strängen i brödtext</span><span class="sxs-lookup"><span data-stu-id="cbed2-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="cbed2-189">Hej `find-and-replace` princip söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="cbed2-189">hello `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-190">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-191">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="cbed2-192">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-192">Elements</span></span>  
  
|<span data-ttu-id="cbed2-193">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-193">Name</span></span>|<span data-ttu-id="cbed2-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-194">Description</span></span>|<span data-ttu-id="cbed2-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-196">Sök och Ersätt</span><span class="sxs-lookup"><span data-stu-id="cbed2-196">find-and-replace</span></span>|<span data-ttu-id="cbed2-197">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-197">Root element.</span></span>|<span data-ttu-id="cbed2-198">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="cbed2-199">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-199">Attributes</span></span>  
  
|<span data-ttu-id="cbed2-200">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-200">Name</span></span>|<span data-ttu-id="cbed2-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-201">Description</span></span>|<span data-ttu-id="cbed2-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-202">Required</span></span>|<span data-ttu-id="cbed2-203">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-204">Från</span><span class="sxs-lookup"><span data-stu-id="cbed2-204">from</span></span>|<span data-ttu-id="cbed2-205">hello sträng toosearch för.</span><span class="sxs-lookup"><span data-stu-id="cbed2-205">hello string toosearch for.</span></span>|<span data-ttu-id="cbed2-206">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-206">Yes</span></span>|<span data-ttu-id="cbed2-207">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-207">N/A</span></span>|  
|<span data-ttu-id="cbed2-208">till</span><span class="sxs-lookup"><span data-stu-id="cbed2-208">to</span></span>|<span data-ttu-id="cbed2-209">hello ersättningssträngen.</span><span class="sxs-lookup"><span data-stu-id="cbed2-209">hello replacement string.</span></span> <span data-ttu-id="cbed2-210">Ange ersättning sträng tooremove hello Sök nollängd.</span><span class="sxs-lookup"><span data-stu-id="cbed2-210">Specify a zero length replacement string tooremove hello search string.</span></span>|<span data-ttu-id="cbed2-211">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-211">Yes</span></span>|<span data-ttu-id="cbed2-212">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-213">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-213">Usage</span></span>  
 <span data-ttu-id="cbed2-214">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-214">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-215">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="cbed2-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="cbed2-216">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-217"><a name="MaskURLSContent"></a>Mask-URL: er i innehåll</span><span class="sxs-lookup"><span data-stu-id="cbed2-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="cbed2-218">Hej `redirect-content-urls` princip skriver (masker) länkar i hello svarstexten så att de pekar toohello motsvarande länk via hello gateway.</span><span class="sxs-lookup"><span data-stu-id="cbed2-218">hello `redirect-content-urls` policy re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span> <span data-ttu-id="cbed2-219">Använd i hello utgående avsnittet toore Skriv svaret brödtext länkar toomake dem punkt toohello gateway.</span><span class="sxs-lookup"><span data-stu-id="cbed2-219">Use in hello outbound section toore-write response body links toomake them point toohello gateway.</span></span> <span data-ttu-id="cbed2-220">Använd i hello inkommande avsnittet för en motsatt effekt.</span><span class="sxs-lookup"><span data-stu-id="cbed2-220">Use in hello inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="cbed2-221">Den här principen ändras inte alla värden i huvudet som `Location` huvuden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="cbed2-222">toochange huvudvärden använda hello [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip.</span><span class="sxs-lookup"><span data-stu-id="cbed2-222">toochange header values, use hello [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-223">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-224">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="cbed2-225">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-225">Elements</span></span>  
  
|<span data-ttu-id="cbed2-226">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-226">Name</span></span>|<span data-ttu-id="cbed2-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-227">Description</span></span>|<span data-ttu-id="cbed2-228">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-229">omdirigerings-innehåll-URL: er</span><span class="sxs-lookup"><span data-stu-id="cbed2-229">redirect-content-urls</span></span>|<span data-ttu-id="cbed2-230">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-230">Root element.</span></span>|<span data-ttu-id="cbed2-231">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-232">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-232">Usage</span></span>  
 <span data-ttu-id="cbed2-233">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-233">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-234">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="cbed2-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="cbed2-235">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-236"><a name="SetBackendService"></a>Ange backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="cbed2-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="cbed2-237">Använd hello `set-backend-service` princip tooredirect en inkommande begäran tooa olika backend än hello som anges i inställningarna för hello API för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="cbed2-237">Use hello `set-backend-service` policy tooredirect an incoming request tooa different backend than hello one specified in hello API settings for that operation.</span></span> <span data-ttu-id="cbed2-238">Den här principen ändras hello backend service bas-URL för hello inkommande begäran toohello som har angetts i hello princip.</span><span class="sxs-lookup"><span data-stu-id="cbed2-238">This policy changes hello backend service base URL of hello incoming request toohello one specified in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-239">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-240">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-240">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <choose>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2013-05")">  
                <set-backend-service base-url="http://contoso.com/api/8.2/" />  
            </when>  
            <when condition="@(context.Request.Url.Query.GetValueOrDefault("version") == "2014-03")">  
                <set-backend-service base-url="http://contoso.com/api/9.1/" />  
            </when>  
        </choose>  
        <base />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
<span data-ttu-id="cbed2-241">I det här exemplet hello dirigerar ange backend service princip förfrågningar baserat på hello versionsvärdet som överförts hello frågan sträng tooa olika serverdelstjänst än hello en anges i hello API.</span><span class="sxs-lookup"><span data-stu-id="cbed2-241">In this example hello set backend service policy routes requests based on hello version value passed in hello query string tooa different backend service than hello one specified in hello API.</span></span>
  
<span data-ttu-id="cbed2-242">Ursprungligen är hello grundläggande Webbadressen för backend-tjänsten härledd från hello API-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="cbed2-242">Initially hello backend service base URL is derived from hello API settings.</span></span> <span data-ttu-id="cbed2-243">Så hello URL-begäran `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` blir `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` där `http://contoso.com/api/10.4/` är hello backend tjänst-URL som anges i inställningarna för hello API.</span><span class="sxs-lookup"><span data-stu-id="cbed2-243">So hello request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is hello backend service URL specified in hello API settings.</span></span>  
  
<span data-ttu-id="cbed2-244">När hello [< Välj\> ](api-management-advanced-policies.md#choose) Principframställning tillämpas hello backend service bas-URL kan ändra igen antingen för`http://contoso.com/api/8.2` eller `http://contoso.com/api/9.1`, beroende på hello värdet för Frågeparametern för hello version begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-244">When hello [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied hello backend service base URL may change again either too`http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on hello value of hello version request query parameter.</span></span> <span data-ttu-id="cbed2-245">Till exempel om hello är värdet `"2013-15"` hello slutliga begäran URL blir `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="cbed2-245">For example, if hello value is `"2013-15"` hello final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="cbed2-246">Om ytterligare transformering av hello-begäran är önskade, andra [principer för Anspråksomvandling](api-management-transformation-policies.md#TransformationPolicies) kan användas.</span><span class="sxs-lookup"><span data-stu-id="cbed2-246">If further transformation of hello request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="cbed2-247">Till exempel tooremove hello version fråga parametern nu när hello begäran håller på att dirigeras tooa version specifika serverdel hello [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) policy kan vara används tooremove hello nu redundant versionsattribut.</span><span class="sxs-lookup"><span data-stu-id="cbed2-247">For example, tooremove hello version query parameter now that hello request is being routed tooa version specific backend, hello  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used tooremove hello now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="cbed2-248">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-248">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <set-backend-service backend-id="my-sf-service" sf-partition-key="@(context.Request.Url.Query.GetValueOrDefault("userId","")" sf-replica-type="primary" /> 
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
<span data-ttu-id="cbed2-249">I det här exemplet hello princip vägar hello begäran tooa service fabric serverdel använder hello userId frågesträngen som hello partitionsnyckel hello primär replik av hello partitionen.</span><span class="sxs-lookup"><span data-stu-id="cbed2-249">In this example hello policy routes hello request tooa service fabric backend, using hello userId query string as hello partition key and using hello primary replica of hello partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="cbed2-250">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-250">Elements</span></span>  
  
|<span data-ttu-id="cbed2-251">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-251">Name</span></span>|<span data-ttu-id="cbed2-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-252">Description</span></span>|<span data-ttu-id="cbed2-253">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-254">set-backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="cbed2-254">set-backend-service</span></span>|<span data-ttu-id="cbed2-255">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-255">Root element.</span></span>|<span data-ttu-id="cbed2-256">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="cbed2-257">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-257">Attributes</span></span>  
  
|<span data-ttu-id="cbed2-258">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-258">Name</span></span>|<span data-ttu-id="cbed2-259">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-259">Description</span></span>|<span data-ttu-id="cbed2-260">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-260">Required</span></span>|<span data-ttu-id="cbed2-261">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-262">bas-url</span><span class="sxs-lookup"><span data-stu-id="cbed2-262">base-url</span></span>|<span data-ttu-id="cbed2-263">Nya backend service bas-URL.</span><span class="sxs-lookup"><span data-stu-id="cbed2-263">New backend service base URL.</span></span>|<span data-ttu-id="cbed2-264">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-264">No</span></span>|<span data-ttu-id="cbed2-265">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-265">N/A</span></span>|  
|<span data-ttu-id="cbed2-266">backend-id</span><span class="sxs-lookup"><span data-stu-id="cbed2-266">backend-id</span></span>|<span data-ttu-id="cbed2-267">Identifierare för hello backend tooroute till.</span><span class="sxs-lookup"><span data-stu-id="cbed2-267">Identifier of hello backend tooroute to.</span></span>|<span data-ttu-id="cbed2-268">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-268">No</span></span>|<span data-ttu-id="cbed2-269">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-269">N/A</span></span>|  
|<span data-ttu-id="cbed2-270">SA partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="cbed2-270">sf-partition-key</span></span>|<span data-ttu-id="cbed2-271">Gäller endast när hello serverdelen är en tjänst för Service Fabric och anges med backend-id.</span><span class="sxs-lookup"><span data-stu-id="cbed2-271">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="cbed2-272">Använda tooresolve en specifik partition från hello namnmatchningstjänst.</span><span class="sxs-lookup"><span data-stu-id="cbed2-272">Used tooresolve a specific partition from hello name resolution service.</span></span>|<span data-ttu-id="cbed2-273">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-273">No</span></span>|<span data-ttu-id="cbed2-274">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-274">N/A</span></span>|  
|<span data-ttu-id="cbed2-275">SA-replik-typ</span><span class="sxs-lookup"><span data-stu-id="cbed2-275">sf-replica-type</span></span>|<span data-ttu-id="cbed2-276">Gäller endast när hello serverdelen är en tjänst för Service Fabric och anges med backend-id.</span><span class="sxs-lookup"><span data-stu-id="cbed2-276">Only applicable when hello backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="cbed2-277">Styr om hello begäran ska gå toohello primär eller sekundär replik av en partition.</span><span class="sxs-lookup"><span data-stu-id="cbed2-277">Controls if hello request should go toohello primary or secondary replica of a partition.</span></span> |<span data-ttu-id="cbed2-278">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-278">No</span></span>|<span data-ttu-id="cbed2-279">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-279">N/A</span></span>|    
|<span data-ttu-id="cbed2-280">SA Lös villkor</span><span class="sxs-lookup"><span data-stu-id="cbed2-280">sf-resolve-condition</span></span>|<span data-ttu-id="cbed2-281">Gäller endast när hello serverdelen är en tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cbed2-281">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="cbed2-282">Villkoret identifierar har om hello anropa tooService Fabric backend toobe upprepas med nya lösningar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-282">Condition identifying if hello call tooService Fabric backend has toobe repeated with new resolution.</span></span>|<span data-ttu-id="cbed2-283">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-283">No</span></span>|<span data-ttu-id="cbed2-284">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-284">N/A</span></span>|    
|<span data-ttu-id="cbed2-285">SA-tjänsten-instansnamn</span><span class="sxs-lookup"><span data-stu-id="cbed2-285">sf-service-instance-name</span></span>|<span data-ttu-id="cbed2-286">Gäller endast när hello serverdelen är en tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="cbed2-286">Only applicable when hello backend is a Service Fabric service.</span></span> <span data-ttu-id="cbed2-287">Tillåter toochange instanser av tjänsten vid körning.</span><span class="sxs-lookup"><span data-stu-id="cbed2-287">Allows toochange service instances at runtime.</span></span> |<span data-ttu-id="cbed2-288">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-288">No</span></span>|<span data-ttu-id="cbed2-289">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="cbed2-290">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-290">Usage</span></span>  
 <span data-ttu-id="cbed2-291">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-291">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-292">**Avsnitt i princip:** inkommande, backend</span><span class="sxs-lookup"><span data-stu-id="cbed2-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="cbed2-293">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-294"><a name="SetBody"></a>Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="cbed2-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="cbed2-295">Använd hello `set-body` princip tooset hello meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-295">Use hello `set-body` policy tooset hello message body for incoming and outgoing requests.</span></span> <span data-ttu-id="cbed2-296">tooaccess hello meddelandetexten som du kan använda hello `context.Request.Body` egenskap eller hello `context.Response.Body`, beroende på om hello princip i hello inkommande eller utgående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cbed2-296">tooaccess hello message body you can use hello `context.Request.Body` property or hello `context.Response.Body`, depending on whether hello policy is in hello inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="cbed2-297">Observera att som standard när du använder hello meddelande brödtext med `context.Request.Body` eller `context.Response.Body`, ursprungliga hälsningsmeddelande brödtext går förlorad och måste anges genom att returnera hello brödtext tillbaka i hello uttryck.</span><span class="sxs-lookup"><span data-stu-id="cbed2-297">Note that by default when you access hello message body using `context.Request.Body` or `context.Response.Body`, hello original message body is lost and must be set by returning hello body back in hello expression.</span></span> <span data-ttu-id="cbed2-298">toopreserve hello brödtext innehåll, ange hello `preserveContent` parameter för`true` vid åtkomst till hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbed2-298">toopreserve hello body content, set hello `preserveContent` parameter too`true` when accessing hello message.</span></span> <span data-ttu-id="cbed2-299">Om `preserveContent` har angetts för`true` och en annan text returneras av hello uttryck hello returnerade brödtext används.</span><span class="sxs-lookup"><span data-stu-id="cbed2-299">If `preserveContent` is set too`true` and a different body is returned by hello expression, hello returned body is used.</span></span>  
>   
>  <span data-ttu-id="cbed2-300">Observera följande överväganden när du använder hello hello `set-body` princip.</span><span class="sxs-lookup"><span data-stu-id="cbed2-300">Please note hello following considerations when using hello `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="cbed2-301">Om du använder hello `set-body` princip tooreturn en ny eller uppdaterad text som du inte behöver tooset `preserveContent` för`true` eftersom tillhandahåller du uttryckligen hello nya innehållet i meddelandetexten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-301">If you are using hello `set-body` policy tooreturn a new or updated body you don't need tooset `preserveContent` too`true` because you are explicitly supplying hello new body contents.</span></span>  
> -   <span data-ttu-id="cbed2-302">Bevara hello innehållet i ett svar i hello inkommande pipeline passar inte eftersom det inte finns något svar ännu.</span><span class="sxs-lookup"><span data-stu-id="cbed2-302">Preserving hello content of a response in hello inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="cbed2-303">Bevara hello innehållet i en begäran i hello utgående pipeline vara inte meningsfullt eftersom hello begäran har redan skickats toohello backend nu.</span><span class="sxs-lookup"><span data-stu-id="cbed2-303">Preserving hello content of a request in hello outbound pipeline doesn't make sense because hello request has already been sent toohello backend at this point.</span></span>  
> -   <span data-ttu-id="cbed2-304">Om den här principen används när det finns ingen brödtext, genereras till exempel i en inkommande GET ett undantag.</span><span class="sxs-lookup"><span data-stu-id="cbed2-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="cbed2-305">Mer information finns i hello `context.Request.Body`, `context.Response.Body`, och hello `IMessage` avsnitt i hello [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) tabell.</span><span class="sxs-lookup"><span data-stu-id="cbed2-305">For more information, see hello `context.Request.Body`, `context.Response.Body`, and hello `IMessage` sections in hello [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-306">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="cbed2-307">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="cbed2-308">Citat-exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a><span data-ttu-id="cbed2-309">Exempel åtkomst till hello brödtext som en sträng.</span><span class="sxs-lookup"><span data-stu-id="cbed2-309">Example accessing hello body as a string.</span></span> <span data-ttu-id="cbed2-310">Observera att vi bevarar hello ursprungliga begärandetexten så att vi kan komma åt den senare i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="cbed2-310">Note that we are preserving hello original request body so that we can access it later in hello pipeline.</span></span>
  
```xml  
<set-body>  
@{   
    string inBody = context.Request.Body.As<string>(preserveContent: true);   
    if (inBody[0] =='c') {   
        inBody[0] = 'm';   
    }   
    return inBody;   
}  
</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a><span data-ttu-id="cbed2-311">Exempel åtkomst till hello brödtext som en JObject.</span><span class="sxs-lookup"><span data-stu-id="cbed2-311">Example accessing hello body as a JObject.</span></span> <span data-ttu-id="cbed2-312">Observera att eftersom vi inte reserverar hello ursprungliga begärantext öppnar det senare i hello pipeline kommer att resultera i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="cbed2-312">Note that since we are not reserving hello original request body, accesing it later in hello pipeline will result in an exception.</span></span>  
  
```xml  
<set-body>   
@{   
    JObject inBody = context.Request.Body.As<JObject>();   
    if (inBody.attribute == <tag>) {   
        inBody[0] = 'm';   
    }   
    return inBody.ToString();   
}   
</set-body>  
  
```  
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="cbed2-313">Filtrera respons baserat på produkt</span><span class="sxs-lookup"><span data-stu-id="cbed2-313">Filter response based on product</span></span>  
 <span data-ttu-id="cbed2-314">Det här exemplet illustrerar hur tooperform innehållsfiltrering genom att ta bort dataelement från hello svar togs emot från hello backend-tjänsten när du använder hello `Starter` produkten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-314">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="cbed2-315">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too34:30.</span><span class="sxs-lookup"><span data-stu-id="cbed2-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="cbed2-316">Starta en översikt över vid 31:50 toosee [hello mörkt Sky prognos API](https://developer.forecast.io/) används för den här demon.</span><span class="sxs-lookup"><span data-stu-id="cbed2-316">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into hello outbound section tooremove a number of data elements from hello response received from hello backend service based on hello name of hello api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="cbed2-317">Med hjälp av flytande mallar med set-brödtext</span><span class="sxs-lookup"><span data-stu-id="cbed2-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="cbed2-318">Hej `set-body` policy kan vara konfigurerade toouse hello [flytande](https://shopify.github.io/liquid/basics/introduction/) templating språk tootransfom hello brödtexten i en begäran eller ett svar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-318">hello `set-body` policy can be configured toouse hello [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language tootransfom hello body of a request or response.</span></span> <span data-ttu-id="cbed2-319">Detta kan vara mycket effektivt om du behöver toocompletely ändra form hello format för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-319">This can be very effective if you need toocompletely reshape hello format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbed2-320">Hej implementering av flytande som används i hello `set-body` principen är konfigurerad i 'C# mode'.</span><span class="sxs-lookup"><span data-stu-id="cbed2-320">hello implementation of Liquid used in hello `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="cbed2-321">Detta är särskilt viktigt när du gör saker, till exempel filtrering.</span><span class="sxs-lookup"><span data-stu-id="cbed2-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="cbed2-322">Exempel med hjälp av ett filter kräver hello Pascal skiftläge och C# datum formatering t.ex.:</span><span class="sxs-lookup"><span data-stu-id="cbed2-322">As an example, using a date filter requires hello use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="cbed2-323">{{body.foo.startDateTime| Datum: ”yyyyMMddTHH:mm:ddZ”}}</span><span class="sxs-lookup"><span data-stu-id="cbed2-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbed2-324">I ordning toocorrectly bind tooan XML-meddelandetext med hello flytande mall, använda en `set-header` princip tooset Content-Type tooeither application/xml, text/xml (eller någon typ som slutar med + xml); för JSON-meddelandetext det måste vara application/json, text/json (eller någon typ avslutas med + json).</span><span class="sxs-lookup"><span data-stu-id="cbed2-324">In order toocorrectly bind tooan XML body using hello Liquid template, use a `set-header` policy tooset Content-Type tooeither application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-toosoap-using-a-liquid-template"></a><span data-ttu-id="cbed2-325">Konvertera JSON tooSOAP med en flytande mall</span><span class="sxs-lookup"><span data-stu-id="cbed2-325">Convert JSON tooSOAP using a Liquid template</span></span>
```xml
<set-body template="liquid">
    <soap:Envelope xmlns="http://tempuri.org/" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <GetOpenOrders>
                <cust>{{body.getOpenOrders.cust}}</cust>
            </GetOpenOrders>
        </soap:Body>
    </soap:Envelope>
</set-body>
```

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="cbed2-326">Tranform JSON med en flytande mall</span><span class="sxs-lookup"><span data-stu-id="cbed2-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="cbed2-327">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-327">Elements</span></span>  
  
|<span data-ttu-id="cbed2-328">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-328">Name</span></span>|<span data-ttu-id="cbed2-329">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-329">Description</span></span>|<span data-ttu-id="cbed2-330">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-331">Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="cbed2-331">set-body</span></span>|<span data-ttu-id="cbed2-332">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-332">Root element.</span></span> <span data-ttu-id="cbed2-333">Innehåller hello brödtext eller ett uttryck som returnerar en brödtext.</span><span class="sxs-lookup"><span data-stu-id="cbed2-333">Contains hello body text or an expressions that returns a body.</span></span>|<span data-ttu-id="cbed2-334">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="cbed2-335">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="cbed2-335">Properties</span></span>  
  
|<span data-ttu-id="cbed2-336">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-336">Name</span></span>|<span data-ttu-id="cbed2-337">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-337">Description</span></span>|<span data-ttu-id="cbed2-338">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-338">Required</span></span>|<span data-ttu-id="cbed2-339">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-340">mall</span><span class="sxs-lookup"><span data-stu-id="cbed2-340">template</span></span>|<span data-ttu-id="cbed2-341">Använda toochange hello templating läge som hello ange brödtext princip körs i.</span><span class="sxs-lookup"><span data-stu-id="cbed2-341">Used toochange hello templating mode that hello set body policy will run in.</span></span> <span data-ttu-id="cbed2-342">För närvarande är hello stöds endast värdet:</span><span class="sxs-lookup"><span data-stu-id="cbed2-342">Currently hello only supported value is:</span></span><br /><br /><span data-ttu-id="cbed2-343">-flytande - hello ange brödtext princip kommer att använda hello flytande templating motorn</span><span class="sxs-lookup"><span data-stu-id="cbed2-343">- liquid - hello set body policy will use hello liquid templating engine</span></span> |<span data-ttu-id="cbed2-344">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-344">No</span></span>|<span data-ttu-id="cbed2-345">flytande</span><span class="sxs-lookup"><span data-stu-id="cbed2-345">liquid</span></span>|  

<span data-ttu-id="cbed2-346">För att komma åt information om hello förfrågan och svar, binda hello flytande mallen tooa context-objektet med hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="cbed2-346">For accessing information about hello request and response, hello Liquid template can bind tooa context object with hello following properties:</span></span> <br />
<pre>context.
    Request.
        Url
        Method
        OriginalMethod
        OriginalUrl
        IpAddress
        MatchedParameters
        HasBody
        ClientCertificates
        Headers

    Response.
        StatusCode
        Method
        Headers
Url.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString

OriginalUrl.
    Scheme
    Host
    Port
    Path
    Query
    QueryString
    ToUri
    ToString
</pre>



### <a name="usage"></a><span data-ttu-id="cbed2-347">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-347">Usage</span></span>  
 <span data-ttu-id="cbed2-348">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-348">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-349">**Avsnitt i princip:** inkommande, utgående backend</span><span class="sxs-lookup"><span data-stu-id="cbed2-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="cbed2-350">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-351"><a name="SetHTTPheader"></a>Ange HTTP-huvud</span><span class="sxs-lookup"><span data-stu-id="cbed2-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="cbed2-352">Hej `set-header` principen tilldelas värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="cbed2-352">hello `set-header` policy assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="cbed2-353">Infogar en lista över HTTP-huvuden i ett HTTP-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cbed2-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="cbed2-354">När den placeras i en inkommande pipeline, anger den här principen hello HTTP-huvuden för hello-begäran som skickas toohello Måltjänsten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-354">When placed in an inbound pipeline, this policy sets hello HTTP headers for hello request being passed toohello target service.</span></span> <span data-ttu-id="cbed2-355">När den placeras i en utgående pipeline, anger den här principen hello HTTP-huvuden för hello svar skickas toohello gateway-klienten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-355">When placed in an outbound pipeline, this policy sets hello HTTP headers for hello response being sent toohello gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-356">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="cbed2-357">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="cbed2-358">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="cbed2-359">Vidarebefordra kontexten information toohello serverdelstjänst</span><span class="sxs-lookup"><span data-stu-id="cbed2-359">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="cbed2-360">Det här exemplet visar hur tooapply princip hello API-nivå toosupply kontexten information toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="cbed2-360">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="cbed2-361">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too10:30.</span><span class="sxs-lookup"><span data-stu-id="cbed2-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="cbed2-362">Det finns en demonstration av anropa en funktion i hello developer-portalen där du kan se hello princip på arbetet vid 12:10.</span><span class="sxs-lookup"><span data-stu-id="cbed2-362">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="cbed2-363">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="cbed2-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="cbed2-364">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-364">Elements</span></span>  
  
|<span data-ttu-id="cbed2-365">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-365">Name</span></span>|<span data-ttu-id="cbed2-366">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-366">Description</span></span>|<span data-ttu-id="cbed2-367">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-368">set-huvud</span><span class="sxs-lookup"><span data-stu-id="cbed2-368">set-header</span></span>|<span data-ttu-id="cbed2-369">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-369">Root element.</span></span>|<span data-ttu-id="cbed2-370">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-370">Yes</span></span>|  
|<span data-ttu-id="cbed2-371">värde</span><span class="sxs-lookup"><span data-stu-id="cbed2-371">value</span></span>|<span data-ttu-id="cbed2-372">Anger hello värdet i hello huvud toobe uppsättning.</span><span class="sxs-lookup"><span data-stu-id="cbed2-372">Specifies hello value of hello header toobe set.</span></span> <span data-ttu-id="cbed2-373">Flera huvuden med hello samma namn lägga till ytterligare `value` element.</span><span class="sxs-lookup"><span data-stu-id="cbed2-373">For multiple headers with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="cbed2-374">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="cbed2-375">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="cbed2-375">Properties</span></span>  
  
|<span data-ttu-id="cbed2-376">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-376">Name</span></span>|<span data-ttu-id="cbed2-377">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-377">Description</span></span>|<span data-ttu-id="cbed2-378">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-378">Required</span></span>|<span data-ttu-id="cbed2-379">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-380">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="cbed2-380">exists-action</span></span>|<span data-ttu-id="cbed2-381">Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="cbed2-381">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="cbed2-382">Det här attributet måste ha något av följande värden hello.</span><span class="sxs-lookup"><span data-stu-id="cbed2-382">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-383">-åsidosätt - ersätter hello värdet för befintliga hello-huvud.</span><span class="sxs-lookup"><span data-stu-id="cbed2-383">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="cbed2-384">-skip - ersätter inte hello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="cbed2-384">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="cbed2-385">-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="cbed2-385">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="cbed2-386">-delete - tar bort hello huvudet från hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-386">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="cbed2-387">När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="cbed2-387">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="cbed2-388">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-388">No</span></span>|<span data-ttu-id="cbed2-389">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="cbed2-389">override</span></span>|  
|<span data-ttu-id="cbed2-390">namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-390">name</span></span>|<span data-ttu-id="cbed2-391">Anger namnet på hello huvud toobe uppsättning.</span><span class="sxs-lookup"><span data-stu-id="cbed2-391">Specifies name of hello header toobe set.</span></span>|<span data-ttu-id="cbed2-392">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-392">Yes</span></span>|<span data-ttu-id="cbed2-393">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-394">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-394">Usage</span></span>  
 <span data-ttu-id="cbed2-395">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-395">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-396">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="cbed2-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="cbed2-397">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-398"><a name="SetQueryStringParameter"></a>Ange frågesträngparametern</span><span class="sxs-lookup"><span data-stu-id="cbed2-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="cbed2-399">Hej `set-query-parameter` principen läggs till, ersätter värde, eller tar bort begära frågesträngparametern.</span><span class="sxs-lookup"><span data-stu-id="cbed2-399">hello `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="cbed2-400">Kan vara används toopass frågeparametrar förväntades av hello serverdelstjänst som är valfria eller aldrig finns i hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-400">Can be used toopass query parameters expected by hello backend service which are optional or never present in hello request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-401">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="cbed2-402">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="cbed2-403">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a><span data-ttu-id="cbed2-404">Vidarebefordra kontexten information toohello serverdelstjänst</span><span class="sxs-lookup"><span data-stu-id="cbed2-404">Forward context information toohello backend service</span></span>  
 <span data-ttu-id="cbed2-405">Det här exemplet visar hur tooapply princip hello API-nivå toosupply kontexten information toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="cbed2-405">This example shows how tooapply policy at hello API level toosupply context information toohello backend service.</span></span> <span data-ttu-id="cbed2-406">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too10:30.</span><span class="sxs-lookup"><span data-stu-id="cbed2-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too10:30.</span></span> <span data-ttu-id="cbed2-407">Det finns en demonstration av anropa en funktion i hello developer-portalen där du kan se hello princip på arbetet vid 12:10.</span><span class="sxs-lookup"><span data-stu-id="cbed2-407">At 12:10 there is a demo of calling an operation in hello developer portal where you can see hello policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="cbed2-408">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="cbed2-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="cbed2-409">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-409">Elements</span></span>  
  
|<span data-ttu-id="cbed2-410">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-410">Name</span></span>|<span data-ttu-id="cbed2-411">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-411">Description</span></span>|<span data-ttu-id="cbed2-412">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-413">Ange frågeparameter</span><span class="sxs-lookup"><span data-stu-id="cbed2-413">set-query-parameter</span></span>|<span data-ttu-id="cbed2-414">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-414">Root element.</span></span>|<span data-ttu-id="cbed2-415">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-415">Yes</span></span>|  
|<span data-ttu-id="cbed2-416">värde</span><span class="sxs-lookup"><span data-stu-id="cbed2-416">value</span></span>|<span data-ttu-id="cbed2-417">Anger hello värdet i hello frågan toobe parameteruppsättning.</span><span class="sxs-lookup"><span data-stu-id="cbed2-417">Specifies hello value of hello query parameter toobe set.</span></span> <span data-ttu-id="cbed2-418">För flera frågeparametrar med hello samma namn lägga till ytterligare `value` element.</span><span class="sxs-lookup"><span data-stu-id="cbed2-418">For multiple query parameters with hello same name add additional `value` elements.</span></span>|<span data-ttu-id="cbed2-419">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="cbed2-420">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="cbed2-420">Properties</span></span>  
  
|<span data-ttu-id="cbed2-421">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-421">Name</span></span>|<span data-ttu-id="cbed2-422">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-422">Description</span></span>|<span data-ttu-id="cbed2-423">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-423">Required</span></span>|<span data-ttu-id="cbed2-424">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-425">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="cbed2-425">exists-action</span></span>|<span data-ttu-id="cbed2-426">Anger vilken åtgärd tootake när hello frågeparameter har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="cbed2-426">Specifies what action tootake when hello query parameter is already specified.</span></span> <span data-ttu-id="cbed2-427">Det här attributet måste ha något av följande värden hello.</span><span class="sxs-lookup"><span data-stu-id="cbed2-427">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="cbed2-428">-åsidosätt - ersätter hello värdet för befintliga hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="cbed2-428">-   override - replaces hello value of hello existing parameter.</span></span><br /><span data-ttu-id="cbed2-429">-skip - ersätter inte hello befintliga frågeparametervärdet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-429">-   skip - does not replace hello existing query parameter value.</span></span><br /><span data-ttu-id="cbed2-430">-Tillägg - lägger till hello värdet toohello befintliga frågeparametervärdet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-430">-   append - appends hello value toohello existing query parameter value.</span></span><br /><span data-ttu-id="cbed2-431">-delete - tar bort hello frågeparameter från hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="cbed2-431">-   delete - removes hello query parameter from hello request.</span></span><br /><br /> <span data-ttu-id="cbed2-432">När värdet för`override` ta med flera poster med hello samma namn resulterar i hello frågeparameter som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="cbed2-432">When set too`override` enlisting multiple entries with hello same name results in hello query parameter being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="cbed2-433">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-433">No</span></span>|<span data-ttu-id="cbed2-434">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="cbed2-434">override</span></span>|  
|<span data-ttu-id="cbed2-435">namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-435">name</span></span>|<span data-ttu-id="cbed2-436">Anger namnet på hello frågan toobe parameteruppsättning.</span><span class="sxs-lookup"><span data-stu-id="cbed2-436">Specifies name of hello query parameter toobe set.</span></span>|<span data-ttu-id="cbed2-437">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-437">Yes</span></span>|<span data-ttu-id="cbed2-438">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-439">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-439">Usage</span></span>  
 <span data-ttu-id="cbed2-440">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-440">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-441">**Avsnitt i princip:** inkommande, backend</span><span class="sxs-lookup"><span data-stu-id="cbed2-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="cbed2-442">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-443"><a name="RewriteURL"></a>Omarbetning URL</span><span class="sxs-lookup"><span data-stu-id="cbed2-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="cbed2-444">Hej `rewrite-uri` princip konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="cbed2-444">hello `rewrite-uri` policy converts a request URL from its public form toohello form expected by hello web service, as shown in hello following example.</span></span>  
  
-   <span data-ttu-id="cbed2-445">Offentlig URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="cbed2-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="cbed2-446">URL-begäran-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="cbed2-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="cbed2-447">Den här principen kan användas när en mänsklig och/eller webbläsare eget URL bör omvandlas till hello URL-format förväntades av hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="cbed2-447">This policy can be used when a human and/or browser-friendly URL should be transformed into hello URL format expected by hello web service.</span></span> <span data-ttu-id="cbed2-448">Den här principen bara behöver toobe tillämpas när exponera en alternativ URL-format, till exempel ren URL: er, RESTful-URL: er, användarvänliga URL: er eller SEO eget URL: er som är enbart strukturella URL: er som inte innehåller en frågesträng och i stället innehåller endast hello sökvägen till hello resurs (efter hello schemat och hello).</span><span class="sxs-lookup"><span data-stu-id="cbed2-448">This policy only needs toobe applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only hello path of hello resource (after hello scheme and hello authority).</span></span> <span data-ttu-id="cbed2-449">Detta sker ofta för dess estetiska egenskaper, användbarhet eller sökmotor optimeringssyfte (SEO).</span><span class="sxs-lookup"><span data-stu-id="cbed2-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="cbed2-450">Du kan bara lägga till sträng frågeparametrar Hej användarprincip.</span><span class="sxs-lookup"><span data-stu-id="cbed2-450">You can only add query string parameters using hello policy.</span></span> <span data-ttu-id="cbed2-451">Du kan inte lägga till extra sökväg mallparametrar i hello omarbetning URL.</span><span class="sxs-lookup"><span data-stu-id="cbed2-451">You cannot add extra template path parameters in hello rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="cbed2-452">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-453">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-453">Example</span></span>  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/v2/US/hardware/{storenumber}&{ordernumber}?City=city&State=state" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put?c=d -->
```  
```xml
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set too/get?a={b} -->
<policies>  
    <inbound>  
        <base />  
        <rewrite-uri template="/put" copy-unmatched-params="false" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
<!-- Resulting URL will be /put -->
```

### <a name="elements"></a><span data-ttu-id="cbed2-454">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-454">Elements</span></span>  
  
|<span data-ttu-id="cbed2-455">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-455">Name</span></span>|<span data-ttu-id="cbed2-456">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-456">Description</span></span>|<span data-ttu-id="cbed2-457">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-458">omarbetning-uri</span><span class="sxs-lookup"><span data-stu-id="cbed2-458">rewrite-uri</span></span>|<span data-ttu-id="cbed2-459">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-459">Root element.</span></span>|<span data-ttu-id="cbed2-460">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="cbed2-461">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-461">Attributes</span></span>  
  
|<span data-ttu-id="cbed2-462">Attribut</span><span class="sxs-lookup"><span data-stu-id="cbed2-462">Attribute</span></span>|<span data-ttu-id="cbed2-463">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-463">Description</span></span>|<span data-ttu-id="cbed2-464">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-464">Required</span></span>|<span data-ttu-id="cbed2-465">Standard</span><span class="sxs-lookup"><span data-stu-id="cbed2-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="cbed2-466">mall</span><span class="sxs-lookup"><span data-stu-id="cbed2-466">template</span></span>|<span data-ttu-id="cbed2-467">hello faktiska webbtjänst-URL med någon fråga string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-467">hello actual web service URL with any query string parameters.</span></span> <span data-ttu-id="cbed2-468">När du använder uttryck måste hello hela värdet vara ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="cbed2-468">When using expressions, hello whole value must be an expression.</span></span>|<span data-ttu-id="cbed2-469">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-469">Yes</span></span>|<span data-ttu-id="cbed2-470">Saknas</span><span class="sxs-lookup"><span data-stu-id="cbed2-470">N/A</span></span>|  
|<span data-ttu-id="cbed2-471">Kopiera omatchade parametrar</span><span class="sxs-lookup"><span data-stu-id="cbed2-471">copy-unmatched-params</span></span>|<span data-ttu-id="cbed2-472">Anger om frågeparametrar Hej inkommande begäran finns inte i hello ursprungliga URL: en mall läggs toohello URL som definierats av hello Skriv mall</span><span class="sxs-lookup"><span data-stu-id="cbed2-472">Specifies whether query parameters in hello incoming request not present in hello original URL template are added toohello URL defined by hello re-write template</span></span>|<span data-ttu-id="cbed2-473">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-473">No</span></span>|<span data-ttu-id="cbed2-474">SANT</span><span class="sxs-lookup"><span data-stu-id="cbed2-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-475">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-475">Usage</span></span>  
 <span data-ttu-id="cbed2-476">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-476">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-477">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="cbed2-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="cbed2-478">**Princip för scope:** produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="cbed2-479"><a name="XSLTransform"></a>Transformera XML med hjälp av en XSLT-fil</span><span class="sxs-lookup"><span data-stu-id="cbed2-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="cbed2-480">Hej `Transform XML using an XSLT` principen gäller en XSL-transformation tooXML i hello begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="cbed2-480">hello `Transform XML using an XSLT` policy applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="cbed2-481">Principframställning</span><span class="sxs-lookup"><span data-stu-id="cbed2-481">Policy statement</span></span>  
  
```xml  
<xsl-transform>  
    <parameter name="User-Agent">@(context.Request.Headers.GetValueOrDefault("User-Agent","non-specified"))</parameter>  
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
        <xsl:output method="xml" indent="yes" />  
        <xsl:param name="User-Agent" />  
        <xsl:template match="* | @* | node()">  
            <xsl:copy>  
                <xsl:if test="self::* and not(parent::*)">  
                    <xsl:attribute name="User-Agent">  
                        <xsl:value-of select="$User-Agent" />  
                    </xsl:attribute>  
                </xsl:if>  
                <xsl:apply-templates select="* | @* | node()" />  
            </xsl:copy>  
        </xsl:template>  
    </xsl:stylesheet>  
  </xsl-transform>  
```  
  
### <a name="example"></a><span data-ttu-id="cbed2-482">Exempel</span><span class="sxs-lookup"><span data-stu-id="cbed2-482">Example</span></span>  
  
```xml  
<policies>  
  <inbound>  
      <base />  
  </inbound>  
  <outbound>  
      <base />  
      <xsl-transform>  
        <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">  
            <xsl:output omit-xml-declaration="yes" method="xml" indent="yes" />  
            <!-- Copy all nodes directly-->  
            <xsl:template match="node()| @*|*">  
                <xsl:copy>  
                    <xsl:apply-templates select="@* | node()|*" />  
                </xsl:copy>  
            </xsl:template>  
        </xsl:stylesheet>  
    </xsl-transform>  
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a><span data-ttu-id="cbed2-483">Element</span><span class="sxs-lookup"><span data-stu-id="cbed2-483">Elements</span></span>  
  
|<span data-ttu-id="cbed2-484">Namn</span><span class="sxs-lookup"><span data-stu-id="cbed2-484">Name</span></span>|<span data-ttu-id="cbed2-485">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cbed2-485">Description</span></span>|<span data-ttu-id="cbed2-486">Krävs</span><span class="sxs-lookup"><span data-stu-id="cbed2-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="cbed2-487">XSL-Transformation</span><span class="sxs-lookup"><span data-stu-id="cbed2-487">xsl-transform</span></span>|<span data-ttu-id="cbed2-488">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-488">Root element.</span></span>|<span data-ttu-id="cbed2-489">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-489">Yes</span></span>|  
|<span data-ttu-id="cbed2-490">Parametern</span><span class="sxs-lookup"><span data-stu-id="cbed2-490">parameter</span></span>|<span data-ttu-id="cbed2-491">Använda toodefine variabler som används i hello transformeringen</span><span class="sxs-lookup"><span data-stu-id="cbed2-491">Used toodefine variables used in hello transform</span></span>|<span data-ttu-id="cbed2-492">Nej</span><span class="sxs-lookup"><span data-stu-id="cbed2-492">No</span></span>|  
|<span data-ttu-id="cbed2-493">XSL: stylesheet</span><span class="sxs-lookup"><span data-stu-id="cbed2-493">xsl:stylesheet</span></span>|<span data-ttu-id="cbed2-494">Formatmallen rotelementet.</span><span class="sxs-lookup"><span data-stu-id="cbed2-494">Root stylesheet element.</span></span> <span data-ttu-id="cbed2-495">Alla element och attribut som definierats inom följer hello standard [XSLT-specifikationen](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="cbed2-495">All elements and attributes defined within follow hello standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="cbed2-496">Ja</span><span class="sxs-lookup"><span data-stu-id="cbed2-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="cbed2-497">Användning</span><span class="sxs-lookup"><span data-stu-id="cbed2-497">Usage</span></span>  
 <span data-ttu-id="cbed2-498">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="cbed2-498">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="cbed2-499">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="cbed2-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="cbed2-500">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="cbed2-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="cbed2-501">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cbed2-501">Next steps</span></span>
<span data-ttu-id="cbed2-502">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="cbed2-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
