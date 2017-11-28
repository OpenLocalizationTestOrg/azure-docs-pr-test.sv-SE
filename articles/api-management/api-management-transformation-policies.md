---
title: "Principer för anspråksomvandling till Azure API Management | Microsoft Docs"
description: "Mer information om principer för anspråksomvandling tillgängligt för användning i Azure API Management."
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
ms.openlocfilehash: c2bed904b82c569b28a6e00d0cc9b49107c148dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-transformation-policies"></a><span data-ttu-id="0ee94-103">API Management-principer för anspråksomvandling</span><span class="sxs-lookup"><span data-stu-id="0ee94-103">API Management transformation policies</span></span>
<span data-ttu-id="0ee94-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="0ee94-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="0ee94-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="0ee94-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="0ee94-106"><a name="TransformationPolicies"></a>Principer för anspråksomvandling</span><span class="sxs-lookup"><span data-stu-id="0ee94-106"><a name="TransformationPolicies"></a> Transformation policies</span></span>  
  
-   <span data-ttu-id="0ee94-107">[Konvertera JSON till XML](api-management-transformation-policies.md#ConvertJSONtoXML) – konverterar begäran eller svar body från JSON till XML.</span><span class="sxs-lookup"><span data-stu-id="0ee94-107">[Convert JSON to XML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON to XML.</span></span>  
  
-   <span data-ttu-id="0ee94-108">[Konvertera XML till JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – konverterar begäran eller svar body från XML till JSON.</span><span class="sxs-lookup"><span data-stu-id="0ee94-108">[Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML to JSON.</span></span>  
  
-   <span data-ttu-id="0ee94-109">[Sök och Ersätt strängen i brödtexten](api-management-transformation-policies.md#Findandreplacestringinbody) - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="0ee94-109">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
-   <span data-ttu-id="0ee94-110">[Maskera URL: er i innehåll](api-management-transformation-policies.md#MaskURLSContent) -skriver (masker) länkar i svaret body så att de pekar på motsvarande länk via gatewayen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-110">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>  
  
-   <span data-ttu-id="0ee94-111">[Ange serverdelstjänst](api-management-transformation-policies.md#SetBackendService) -ändras serverdelstjänst för en inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-111">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes the backend service for an incoming request.</span></span>  
  
-   <span data-ttu-id="0ee94-112">[Konfigurera brödtext](api-management-transformation-policies.md#SetBody) -anger meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-112">[Set body](api-management-transformation-policies.md#SetBody) - Sets the message body for incoming and outgoing requests.</span></span>  
  
-   <span data-ttu-id="0ee94-113">[Ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) – tilldelar ett värde till en befintlig svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="0ee94-113">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
-   <span data-ttu-id="0ee94-114">[Ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-114">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
-   <span data-ttu-id="0ee94-115">[Skriv om URL: en](api-management-transformation-policies.md#RewriteURL) -konverterar en begärd URL från dess offentliga form i formuläret som förväntades av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-115">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form to the form expected by the web service.</span></span>  
  
-   <span data-ttu-id="0ee94-116">[Transformera XML med hjälp av en XSLT](api-management-transformation-policies.md#XSLTransform) -tillämpar en XSL-transformation på XML i begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-116">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation to XML in the request or response body.</span></span>  
  
##  <span data-ttu-id="0ee94-117"><a name="ConvertJSONtoXML"></a>Konvertera JSON till XML</span><span class="sxs-lookup"><span data-stu-id="0ee94-117"><a name="ConvertJSONtoXML"></a> Convert JSON to XML</span></span>  
 <span data-ttu-id="0ee94-118">Den `json-to-xml` princip konverterar brödtext begäran eller ett svar från JSON till XML.</span><span class="sxs-lookup"><span data-stu-id="0ee94-118">The `json-to-xml` policy converts a request or response body from JSON to XML.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-119">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-119">Policy statement</span></span>  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-120">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-120">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="0ee94-121">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-121">Elements</span></span>  
  
|<span data-ttu-id="0ee94-122">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-122">Name</span></span>|<span data-ttu-id="0ee94-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-123">Description</span></span>|<span data-ttu-id="0ee94-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-124">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-125">JSON-xml</span><span class="sxs-lookup"><span data-stu-id="0ee94-125">json-to-xml</span></span>|<span data-ttu-id="0ee94-126">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-126">Root element.</span></span>|<span data-ttu-id="0ee94-127">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-127">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ee94-128">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-128">Attributes</span></span>  
  
|<span data-ttu-id="0ee94-129">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-129">Name</span></span>|<span data-ttu-id="0ee94-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-130">Description</span></span>|<span data-ttu-id="0ee94-131">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-131">Required</span></span>|<span data-ttu-id="0ee94-132">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-132">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-133">tillämpa</span><span class="sxs-lookup"><span data-stu-id="0ee94-133">apply</span></span>|<span data-ttu-id="0ee94-134">Attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-134">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-135">-alltid - alltid gäller konvertering.</span><span class="sxs-lookup"><span data-stu-id="0ee94-135">-   always - always apply conversion.</span></span><br /><span data-ttu-id="0ee94-136">-innehåll typ-json - Konvertera endast om response Content-Type-huvud anger förekomsten av JSON.</span><span class="sxs-lookup"><span data-stu-id="0ee94-136">-   content-type-json - convert only if response Content-Type header indicates presence of JSON.</span></span>|<span data-ttu-id="0ee94-137">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-137">Yes</span></span>|<span data-ttu-id="0ee94-138">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-138">N/A</span></span>|  
|<span data-ttu-id="0ee94-139">Överväg att acceptera-huvud</span><span class="sxs-lookup"><span data-stu-id="0ee94-139">consider-accept-header</span></span>|<span data-ttu-id="0ee94-140">Attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-140">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-141">tillämpa - true - konvertering om JSON begärs i begäran Accept-huvud.</span><span class="sxs-lookup"><span data-stu-id="0ee94-141">-   true - apply conversion if JSON is requested in request Accept header.</span></span><br /><span data-ttu-id="0ee94-142">-alltid gäller false - konvertering.</span><span class="sxs-lookup"><span data-stu-id="0ee94-142">-   false -always apply conversion.</span></span>|<span data-ttu-id="0ee94-143">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-143">No</span></span>|<span data-ttu-id="0ee94-144">SANT</span><span class="sxs-lookup"><span data-stu-id="0ee94-144">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-145">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-145">Usage</span></span>  
 <span data-ttu-id="0ee94-146">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-146">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-147">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="0ee94-147">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="0ee94-148">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-148">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-149"><a name="ConvertXMLtoJSON"></a>Konvertera XML till JSON</span><span class="sxs-lookup"><span data-stu-id="0ee94-149"><a name="ConvertXMLtoJSON"></a> Convert XML to JSON</span></span>  
 <span data-ttu-id="0ee94-150">Den `xml-to-json` princip konverterar brödtext begäran eller ett svar från XML till JSON.</span><span class="sxs-lookup"><span data-stu-id="0ee94-150">The `xml-to-json` policy converts a request or response body from XML to JSON.</span></span> <span data-ttu-id="0ee94-151">Den här principen kan användas för att modernisera API: er baserat på backend endast XML-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="0ee94-151">This policy can be used to modernize APIs based on XML-only backend web services.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-152">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-152">Policy statement</span></span>  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-153">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-153">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="0ee94-154">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-154">Elements</span></span>  
  
|<span data-ttu-id="0ee94-155">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-155">Name</span></span>|<span data-ttu-id="0ee94-156">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-156">Description</span></span>|<span data-ttu-id="0ee94-157">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-157">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-158">XML-json</span><span class="sxs-lookup"><span data-stu-id="0ee94-158">xml-to-json</span></span>|<span data-ttu-id="0ee94-159">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-159">Root element.</span></span>|<span data-ttu-id="0ee94-160">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-160">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ee94-161">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-161">Attributes</span></span>  
  
|<span data-ttu-id="0ee94-162">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-162">Name</span></span>|<span data-ttu-id="0ee94-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-163">Description</span></span>|<span data-ttu-id="0ee94-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-164">Required</span></span>|<span data-ttu-id="0ee94-165">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-165">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-166">typ</span><span class="sxs-lookup"><span data-stu-id="0ee94-166">kind</span></span>|<span data-ttu-id="0ee94-167">Attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-167">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-168">javascript-anpassad - konverterade JSON har ett eget JavaScript-utvecklare formulär.</span><span class="sxs-lookup"><span data-stu-id="0ee94-168">-   javascript-friendly - the converted JSON has a form friendly to JavaScript developers.</span></span><br /><span data-ttu-id="0ee94-169">-direkt - visar konverterade JSON det ursprungliga XML-dokumentets struktur.</span><span class="sxs-lookup"><span data-stu-id="0ee94-169">-   direct - the converted JSON reflects the original XML document's structure.</span></span>|<span data-ttu-id="0ee94-170">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-170">Yes</span></span>|<span data-ttu-id="0ee94-171">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-171">N/A</span></span>|  
|<span data-ttu-id="0ee94-172">tillämpa</span><span class="sxs-lookup"><span data-stu-id="0ee94-172">apply</span></span>|<span data-ttu-id="0ee94-173">Attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-173">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-174">-alltid - konvertera alltid.</span><span class="sxs-lookup"><span data-stu-id="0ee94-174">-   always - convert always.</span></span><br /><span data-ttu-id="0ee94-175">-innehåll-typ-xml - Konvertera endast om response Content-Type-huvud anger förekomsten av XML.</span><span class="sxs-lookup"><span data-stu-id="0ee94-175">-   content-type-xml - convert only if response Content-Type header indicates presence of XML.</span></span>|<span data-ttu-id="0ee94-176">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-176">Yes</span></span>|<span data-ttu-id="0ee94-177">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-177">N/A</span></span>|  
|<span data-ttu-id="0ee94-178">Överväg att acceptera-huvud</span><span class="sxs-lookup"><span data-stu-id="0ee94-178">consider-accept-header</span></span>|<span data-ttu-id="0ee94-179">Attributet måste anges till ett av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-179">The attribute must be set to one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-180">tillämpa - true - konvertering om begärs XML i begäran Accept-huvud.</span><span class="sxs-lookup"><span data-stu-id="0ee94-180">-   true - apply conversion if XML is requested in request Accept header.</span></span><br /><span data-ttu-id="0ee94-181">-alltid gäller false - konvertering.</span><span class="sxs-lookup"><span data-stu-id="0ee94-181">-   false -always apply conversion.</span></span>|<span data-ttu-id="0ee94-182">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-182">No</span></span>|<span data-ttu-id="0ee94-183">SANT</span><span class="sxs-lookup"><span data-stu-id="0ee94-183">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-184">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-184">Usage</span></span>  
 <span data-ttu-id="0ee94-185">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-185">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-186">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="0ee94-186">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="0ee94-187">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-187">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-188"><a name="Findandreplacestringinbody"></a>Sök och Ersätt strängen i brödtext</span><span class="sxs-lookup"><span data-stu-id="0ee94-188"><a name="Findandreplacestringinbody"></a> Find and replace string in body</span></span>  
 <span data-ttu-id="0ee94-189">Den `find-and-replace` princip söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="0ee94-189">The `find-and-replace` policy finds a request or response substring and replaces it with a different substring.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-190">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-190">Policy statement</span></span>  
  
```xml  
<find-and-replace from="what to replace" to="replacement" />  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-191">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-191">Example</span></span>  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a><span data-ttu-id="0ee94-192">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-192">Elements</span></span>  
  
|<span data-ttu-id="0ee94-193">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-193">Name</span></span>|<span data-ttu-id="0ee94-194">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-194">Description</span></span>|<span data-ttu-id="0ee94-195">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-195">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-196">Sök och Ersätt</span><span class="sxs-lookup"><span data-stu-id="0ee94-196">find-and-replace</span></span>|<span data-ttu-id="0ee94-197">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-197">Root element.</span></span>|<span data-ttu-id="0ee94-198">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-198">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ee94-199">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-199">Attributes</span></span>  
  
|<span data-ttu-id="0ee94-200">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-200">Name</span></span>|<span data-ttu-id="0ee94-201">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-201">Description</span></span>|<span data-ttu-id="0ee94-202">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-202">Required</span></span>|<span data-ttu-id="0ee94-203">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-203">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-204">Från</span><span class="sxs-lookup"><span data-stu-id="0ee94-204">from</span></span>|<span data-ttu-id="0ee94-205">Sträng att söka efter.</span><span class="sxs-lookup"><span data-stu-id="0ee94-205">The string to search for.</span></span>|<span data-ttu-id="0ee94-206">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-206">Yes</span></span>|<span data-ttu-id="0ee94-207">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-207">N/A</span></span>|  
|<span data-ttu-id="0ee94-208">till</span><span class="sxs-lookup"><span data-stu-id="0ee94-208">to</span></span>|<span data-ttu-id="0ee94-209">Ersättningssträngen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-209">The replacement string.</span></span> <span data-ttu-id="0ee94-210">Ange ersättning nollängd för att ta bort söksträngen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-210">Specify a zero length replacement string to remove the search string.</span></span>|<span data-ttu-id="0ee94-211">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-211">Yes</span></span>|<span data-ttu-id="0ee94-212">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-212">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-213">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-213">Usage</span></span>  
 <span data-ttu-id="0ee94-214">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-214">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-215">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="0ee94-215">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="0ee94-216">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-216">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-217"><a name="MaskURLSContent"></a>Mask-URL: er i innehåll</span><span class="sxs-lookup"><span data-stu-id="0ee94-217"><a name="MaskURLSContent"></a> Mask URLs in content</span></span>  
 <span data-ttu-id="0ee94-218">Den `redirect-content-urls` princip skriver (masker) länkar i svarstexten så att de pekar på motsvarande länk via gatewayen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-218">The `redirect-content-urls` policy re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span> <span data-ttu-id="0ee94-219">Använda i avsnittet utgående för att Skriv svaret brödtext länkar så att de pekar på gatewayen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-219">Use in the outbound section to re-write response body links to make them point to the gateway.</span></span> <span data-ttu-id="0ee94-220">Använd i avsnittet inkommande för en motsatt effekt.</span><span class="sxs-lookup"><span data-stu-id="0ee94-220">Use in the inbound section for an opposite effect.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ee94-221">Den här principen ändras inte alla värden i huvudet som `Location` huvuden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-221">This policy does not change any header values such as `Location` headers.</span></span> <span data-ttu-id="0ee94-222">Du kan ändra värden i huvudet i [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip.</span><span class="sxs-lookup"><span data-stu-id="0ee94-222">To change header values, use the [set-header](api-management-transformation-policies.md#SetHTTPheader) policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-223">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-223">Policy statement</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-224">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-224">Example</span></span>  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a><span data-ttu-id="0ee94-225">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-225">Elements</span></span>  
  
|<span data-ttu-id="0ee94-226">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-226">Name</span></span>|<span data-ttu-id="0ee94-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-227">Description</span></span>|<span data-ttu-id="0ee94-228">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-228">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-229">omdirigerings-innehåll-URL: er</span><span class="sxs-lookup"><span data-stu-id="0ee94-229">redirect-content-urls</span></span>|<span data-ttu-id="0ee94-230">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-230">Root element.</span></span>|<span data-ttu-id="0ee94-231">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-231">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-232">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-232">Usage</span></span>  
 <span data-ttu-id="0ee94-233">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-233">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-234">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="0ee94-234">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="0ee94-235">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-235">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-236"><a name="SetBackendService"></a>Ange backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="0ee94-236"><a name="SetBackendService"></a> Set backend service</span></span>  
 <span data-ttu-id="0ee94-237">Använd den `set-backend-service` för att omdirigera en inkommande begäran till en annan serverdelen än den som angetts i API-inställningarna för den åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-237">Use the `set-backend-service` policy to redirect an incoming request to a different backend than the one specified in the API settings for that operation.</span></span> <span data-ttu-id="0ee94-238">Den här principen ändras backend service bas-URL till den inkommande begäranden till den som anges i principen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-238">This policy changes the backend service base URL of the incoming request to the one specified in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-239">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-239">Policy statement</span></span>  
  
```xml  
<set-backend-service base-url="base URL of the backend service" />  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-240">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-240">Example</span></span>  
  
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
<span data-ttu-id="0ee94-241">I det här exemplet skickar ange backend service princip förfrågningar baserat på versionsvärdet frågesträngen som skickas till en annan serverdelstjänst än den som anges i Programmeringsgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-241">In this example the set backend service policy routes requests based on the version value passed in the query string to a different backend service than the one specified in the API.</span></span>
  
<span data-ttu-id="0ee94-242">Den grundläggande Webbadressen för backend-tjänsten är ursprungligen härleds från API-inställningarna.</span><span class="sxs-lookup"><span data-stu-id="0ee94-242">Initially the backend service base URL is derived from the API settings.</span></span> <span data-ttu-id="0ee94-243">Så den begärda Webbadressen `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` blir `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` där `http://contoso.com/api/10.4/` är backend-tjänst-URL som anges i inställningarna för API.</span><span class="sxs-lookup"><span data-stu-id="0ee94-243">So the request URL `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` becomes `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` where `http://contoso.com/api/10.4/` is the backend service URL specified in the API settings.</span></span>  
  
<span data-ttu-id="0ee94-244">När den [< Välj\> ](api-management-advanced-policies.md#choose) Principframställning tillämpas den grundläggande Webbadressen för backend-tjänst kan ändra igen antingen `http://contoso.com/api/8.2` eller `http://contoso.com/api/9.1`, beroende på värdet på Frågeparametern version begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-244">When the [<choose\>](api-management-advanced-policies.md#choose) policy statement is applied the backend service base URL may change again either to `http://contoso.com/api/8.2` or `http://contoso.com/api/9.1`, depending on the value of the version request query parameter.</span></span> <span data-ttu-id="0ee94-245">Om värdet är till exempel `"2013-15"` sista begäran URL blir `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span><span class="sxs-lookup"><span data-stu-id="0ee94-245">For example, if the value is `"2013-15"` the final request URL becomes `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.</span></span>  
  
<span data-ttu-id="0ee94-246">Om ytterligare transformering av begäran är önskade, andra [principer för Anspråksomvandling](api-management-transformation-policies.md#TransformationPolicies) kan användas.</span><span class="sxs-lookup"><span data-stu-id="0ee94-246">If further transformation of the request is desired, other [Transformation policies](api-management-transformation-policies.md#TransformationPolicies) can be used.</span></span> <span data-ttu-id="0ee94-247">Till exempel för att ta bort Frågeparametern versionen nu att begäran omdirigeras till en specifik version-serverdelen av [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) princip kan användas för att ta bort Versionsattributet nu redundant.</span><span class="sxs-lookup"><span data-stu-id="0ee94-247">For example, to remove the version query parameter now that the request is being routed to a version specific backend, the  [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policy can be used to remove the now redundant version attribute.</span></span>  
  
### <a name="example"></a><span data-ttu-id="0ee94-248">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-248">Example</span></span>  
  
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
<span data-ttu-id="0ee94-249">I det här exemplet dirigerar principen begäran till en service fabric serverdel använder frågesträngen användar-ID som partitionsnyckel och använder den primära repliken av partitionen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-249">In this example the policy routes the request to a service fabric backend, using the userId query string as the partition key and using the primary replica of the partition.</span></span>  

### <a name="elements"></a><span data-ttu-id="0ee94-250">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-250">Elements</span></span>  
  
|<span data-ttu-id="0ee94-251">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-251">Name</span></span>|<span data-ttu-id="0ee94-252">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-252">Description</span></span>|<span data-ttu-id="0ee94-253">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-253">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-254">set-backend-tjänst</span><span class="sxs-lookup"><span data-stu-id="0ee94-254">set-backend-service</span></span>|<span data-ttu-id="0ee94-255">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-255">Root element.</span></span>|<span data-ttu-id="0ee94-256">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-256">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ee94-257">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-257">Attributes</span></span>  
  
|<span data-ttu-id="0ee94-258">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-258">Name</span></span>|<span data-ttu-id="0ee94-259">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-259">Description</span></span>|<span data-ttu-id="0ee94-260">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-260">Required</span></span>|<span data-ttu-id="0ee94-261">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-261">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-262">bas-url</span><span class="sxs-lookup"><span data-stu-id="0ee94-262">base-url</span></span>|<span data-ttu-id="0ee94-263">Nya backend service bas-URL.</span><span class="sxs-lookup"><span data-stu-id="0ee94-263">New backend service base URL.</span></span>|<span data-ttu-id="0ee94-264">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-264">No</span></span>|<span data-ttu-id="0ee94-265">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-265">N/A</span></span>|  
|<span data-ttu-id="0ee94-266">backend-id</span><span class="sxs-lookup"><span data-stu-id="0ee94-266">backend-id</span></span>|<span data-ttu-id="0ee94-267">Identifierare för att dirigera till serverdelen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-267">Identifier of the backend to route to.</span></span>|<span data-ttu-id="0ee94-268">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-268">No</span></span>|<span data-ttu-id="0ee94-269">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-269">N/A</span></span>|  
|<span data-ttu-id="0ee94-270">SA partitionsnyckel</span><span class="sxs-lookup"><span data-stu-id="0ee94-270">sf-partition-key</span></span>|<span data-ttu-id="0ee94-271">Gäller endast när serverdelen är en tjänst för Service Fabric och anges med backend-id.</span><span class="sxs-lookup"><span data-stu-id="0ee94-271">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="0ee94-272">Används för att lösa en specifik partition från Namnmatchningstjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-272">Used to resolve a specific partition from the name resolution service.</span></span>|<span data-ttu-id="0ee94-273">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-273">No</span></span>|<span data-ttu-id="0ee94-274">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-274">N/A</span></span>|  
|<span data-ttu-id="0ee94-275">SA-replik-typ</span><span class="sxs-lookup"><span data-stu-id="0ee94-275">sf-replica-type</span></span>|<span data-ttu-id="0ee94-276">Gäller endast när serverdelen är en tjänst för Service Fabric och anges med backend-id.</span><span class="sxs-lookup"><span data-stu-id="0ee94-276">Only applicable when the backend is a Service Fabric service and is specified using 'backend-id'.</span></span> <span data-ttu-id="0ee94-277">Styr om begäran ska skickas till primär eller sekundär replik av en partition.</span><span class="sxs-lookup"><span data-stu-id="0ee94-277">Controls if the request should go to the primary or secondary replica of a partition.</span></span> |<span data-ttu-id="0ee94-278">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-278">No</span></span>|<span data-ttu-id="0ee94-279">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-279">N/A</span></span>|    
|<span data-ttu-id="0ee94-280">SA Lös villkor</span><span class="sxs-lookup"><span data-stu-id="0ee94-280">sf-resolve-condition</span></span>|<span data-ttu-id="0ee94-281">Gäller endast när serverdelen är en tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0ee94-281">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="0ee94-282">Identifiera om anropet till Service Fabric-serverdelen har upprepas med nya lösningar-villkor.</span><span class="sxs-lookup"><span data-stu-id="0ee94-282">Condition identifying if the call to Service Fabric backend has to be repeated with new resolution.</span></span>|<span data-ttu-id="0ee94-283">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-283">No</span></span>|<span data-ttu-id="0ee94-284">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-284">N/A</span></span>|    
|<span data-ttu-id="0ee94-285">SA-tjänsten-instansnamn</span><span class="sxs-lookup"><span data-stu-id="0ee94-285">sf-service-instance-name</span></span>|<span data-ttu-id="0ee94-286">Gäller endast när serverdelen är en tjänst för Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="0ee94-286">Only applicable when the backend is a Service Fabric service.</span></span> <span data-ttu-id="0ee94-287">Du kan ändra instanser av tjänsten vid körning.</span><span class="sxs-lookup"><span data-stu-id="0ee94-287">Allows to change service instances at runtime.</span></span> |<span data-ttu-id="0ee94-288">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-288">No</span></span>|<span data-ttu-id="0ee94-289">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-289">N/A</span></span>|    

### <a name="usage"></a><span data-ttu-id="0ee94-290">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-290">Usage</span></span>  
 <span data-ttu-id="0ee94-291">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-291">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-292">**Avsnitt i princip:** inkommande, backend</span><span class="sxs-lookup"><span data-stu-id="0ee94-292">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="0ee94-293">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-293">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-294"><a name="SetBody"></a>Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="0ee94-294"><a name="SetBody"></a> Set body</span></span>  
 <span data-ttu-id="0ee94-295">Använd den `set-body` Grupprincip för att ange brödtext för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-295">Use the `set-body` policy to set the message body for incoming and outgoing requests.</span></span> <span data-ttu-id="0ee94-296">Åtkomst till meddelandetexten som du kan använda den `context.Request.Body` egenskapen eller `context.Response.Body`, beroende på om principen finns i avsnittet inkommande eller utgående.</span><span class="sxs-lookup"><span data-stu-id="0ee94-296">To access the message body you can use the `context.Request.Body` property or the `context.Response.Body`, depending on whether the policy is in the inbound or outbound section.</span></span>  
  
> [!IMPORTANT]
>  <span data-ttu-id="0ee94-297">Observera att som standard när du använder meddelandet body med `context.Request.Body` eller `context.Response.Body`, den ursprungliga meddelandetexten försvinner och måste anges genom att returnera innehållet i uttrycket.</span><span class="sxs-lookup"><span data-stu-id="0ee94-297">Note that by default when you access the message body using `context.Request.Body` or `context.Response.Body`, the original message body is lost and must be set by returning the body back in the expression.</span></span> <span data-ttu-id="0ee94-298">Om du vill bevara brödtext innehållet, ange den `preserveContent` parameter till `true` vid åtkomst av meddelandet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-298">To preserve the body content, set the `preserveContent` parameter to `true` when accessing the message.</span></span> <span data-ttu-id="0ee94-299">Om `preserveContent` är inställd på `true` och en annan text returnerades av uttrycket returnerade brödtexten används.</span><span class="sxs-lookup"><span data-stu-id="0ee94-299">If `preserveContent` is set to `true` and a different body is returned by the expression, the returned body is used.</span></span>  
>   
>  <span data-ttu-id="0ee94-300">Observera följande när du använder den `set-body` princip.</span><span class="sxs-lookup"><span data-stu-id="0ee94-300">Please note the following considerations when using the `set-body` policy.</span></span>  
>   
>  -   <span data-ttu-id="0ee94-301">Om du använder den `set-body` princip för att returnera en ny eller uppdaterad text som du inte behöver ange `preserveContent` till `true` eftersom tillhandahåller du uttryckligen det nya innehållet i brödtexten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-301">If you are using the `set-body` policy to return a new or updated body you don't need to set `preserveContent` to `true` because you are explicitly supplying the new body contents.</span></span>  
> -   <span data-ttu-id="0ee94-302">Bevara innehållet i ett svar i pipeline för inkommande passar inte eftersom det inte finns något svar ännu.</span><span class="sxs-lookup"><span data-stu-id="0ee94-302">Preserving the content of a response in the inbound pipeline doesn't make sense because there is no response yet.</span></span>  
> -   <span data-ttu-id="0ee94-303">Bevara innehållet i en begäran i pipeline för utgående vara inte meningsfullt eftersom begäran har redan skickats till serverdelen nu.</span><span class="sxs-lookup"><span data-stu-id="0ee94-303">Preserving the content of a request in the outbound pipeline doesn't make sense because the request has already been sent to the backend at this point.</span></span>  
> -   <span data-ttu-id="0ee94-304">Om den här principen används när det finns ingen brödtext, genereras till exempel i en inkommande GET ett undantag.</span><span class="sxs-lookup"><span data-stu-id="0ee94-304">If this policy is used when there is no message body, for example in an inbound GET, an exception is thrown.</span></span>  
  
 <span data-ttu-id="0ee94-305">Mer information finns i `context.Request.Body`, `context.Response.Body`, och `IMessage` avsnitten i den [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) tabell.</span><span class="sxs-lookup"><span data-stu-id="0ee94-305">For more information, see the `context.Request.Body`, `context.Response.Body`, and the `IMessage` sections in the [Context variable](api-management-policy-expressions.md#ContextVariables) table.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-306">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-306">Policy statement</span></span>  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a><span data-ttu-id="0ee94-307">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-307">Examples</span></span>  
  
#### <a name="literal-text-example"></a><span data-ttu-id="0ee94-308">Citat-exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-308">Literal text example</span></span>  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-the-body-as-a-string-note-that-we-are-preserving-the-original-request-body-so-that-we-can-access-it-later-in-the-pipeline"></a><span data-ttu-id="0ee94-309">Exempel åtkomst till innehållet som en sträng.</span><span class="sxs-lookup"><span data-stu-id="0ee94-309">Example accessing the body as a string.</span></span> <span data-ttu-id="0ee94-310">Observera att vi bevarar den ursprungliga begärandetexten så att vi kan komma åt den senare i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="0ee94-310">Note that we are preserving the original request body so that we can access it later in the pipeline.</span></span>
  
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
  
#### <a name="example-accessing-the-body-as-a-jobject-note-that-since-we-are-not-reserving-the-original-request-body-accesing-it-later-in-the-pipeline-will-result-in-an-exception"></a><span data-ttu-id="0ee94-311">Exempel åtkomst till innehållet som en JObject.</span><span class="sxs-lookup"><span data-stu-id="0ee94-311">Example accessing the body as a JObject.</span></span> <span data-ttu-id="0ee94-312">Observera att eftersom vi inte reserverar ursprungliga begärandetexten, öppnar den senare i pipeline resulterar i ett undantag.</span><span class="sxs-lookup"><span data-stu-id="0ee94-312">Note that since we are not reserving the original request body, accesing it later in the pipeline will result in an exception.</span></span>  
  
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
  
#### <a name="filter-response-based-on-product"></a><span data-ttu-id="0ee94-313">Filtrera respons baserat på produkt</span><span class="sxs-lookup"><span data-stu-id="0ee94-313">Filter response based on product</span></span>  
 <span data-ttu-id="0ee94-314">Det här exemplet illustrerar hur du utför innehållsfiltrering genom att ta bort dataelement från svar togs emot från serverdelstjänsten när du använder den `Starter` produkten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-314">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="0ee94-315">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 34:30.</span><span class="sxs-lookup"><span data-stu-id="0ee94-315">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="0ee94-316">Börja med 31:50 att se en översikt över [mörkt Sky prognos API: N](https://developer.forecast.io/) används för den här demon.</span><span class="sxs-lookup"><span data-stu-id="0ee94-316">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
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

### <a name="using-liquid-templates-with-set-body"></a><span data-ttu-id="0ee94-317">Med hjälp av flytande mallar med set-brödtext</span><span class="sxs-lookup"><span data-stu-id="0ee94-317">Using Liquid templates with set body</span></span> 
<span data-ttu-id="0ee94-318">Den `set-body` principen kan konfigureras för att använda den [flytande](https://shopify.github.io/liquid/basics/introduction/) templating språk till transfom brödtexten i en begäran eller ett svar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-318">The `set-body` policy can be configured to use the [Liquid](https://shopify.github.io/liquid/basics/introduction/) templating language to transfom the body of a request or response.</span></span> <span data-ttu-id="0ee94-319">Detta kan vara mycket effektivt om du behöver helt Omforma formatet för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-319">This can be very effective if you need to completely reshape the format of your message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ee94-320">Implementeringen av flytande används i den `set-body` principen är konfigurerad i 'C# mode'.</span><span class="sxs-lookup"><span data-stu-id="0ee94-320">The implementation of Liquid used in the `set-body` policy is configured in 'C# mode'.</span></span> <span data-ttu-id="0ee94-321">Detta är särskilt viktigt när du gör saker, till exempel filtrering.</span><span class="sxs-lookup"><span data-stu-id="0ee94-321">This is particularly important when doing things such as filtering.</span></span> <span data-ttu-id="0ee94-322">Exempel med hjälp av ett filter kräver användning av Pascal skiftläge och C# datum formatering t.ex.:</span><span class="sxs-lookup"><span data-stu-id="0ee94-322">As an example, using a date filter requires the use of Pascal casing and C# date formatting e.g.:</span></span>
>
> <span data-ttu-id="0ee94-323">{{body.foo.startDateTime| Datum: ”yyyyMMddTHH:mm:ddZ”}}</span><span class="sxs-lookup"><span data-stu-id="0ee94-323">{{body.foo.startDateTime| Date:"yyyyMMddTHH:mm:ddZ"}}</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ee94-324">För att kunna bindas till en XML-meddelandetext med mallen flytande, använda en `set-header` Grupprincip för att ange Content-Type antingen application/xml, text/xml (eller någon typ som slutar med + xml) till, en JSON-meddelandetext, måste det vara application/json, text/json (eller någon typ som slutar med + JSON).</span><span class="sxs-lookup"><span data-stu-id="0ee94-324">In order to correctly bind to an XML body using the Liquid template, use a `set-header` policy to set Content-Type to either application/xml, text/xml (or any type ending with +xml); for a JSON body, it must be application/json, text/json (or any type ending with +json).</span></span>

#### <a name="convert-json-to-soap-using-a-liquid-template"></a><span data-ttu-id="0ee94-325">Konvertera JSON till SOAP med en flytande mall</span><span class="sxs-lookup"><span data-stu-id="0ee94-325">Convert JSON to SOAP using a Liquid template</span></span>
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

#### <a name="tranform-json-using-a-liquid-template"></a><span data-ttu-id="0ee94-326">Tranform JSON med en flytande mall</span><span class="sxs-lookup"><span data-stu-id="0ee94-326">Tranform JSON using a Liquid template</span></span>
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a><span data-ttu-id="0ee94-327">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-327">Elements</span></span>  
  
|<span data-ttu-id="0ee94-328">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-328">Name</span></span>|<span data-ttu-id="0ee94-329">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-329">Description</span></span>|<span data-ttu-id="0ee94-330">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-330">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-331">Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="0ee94-331">set-body</span></span>|<span data-ttu-id="0ee94-332">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-332">Root element.</span></span> <span data-ttu-id="0ee94-333">Innehåller brödtexten eller ett uttryck som returnerar en brödtext.</span><span class="sxs-lookup"><span data-stu-id="0ee94-333">Contains the body text or an expressions that returns a body.</span></span>|<span data-ttu-id="0ee94-334">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-334">Yes</span></span>|  

### <a name="properties"></a><span data-ttu-id="0ee94-335">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="0ee94-335">Properties</span></span>  
  
|<span data-ttu-id="0ee94-336">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-336">Name</span></span>|<span data-ttu-id="0ee94-337">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-337">Description</span></span>|<span data-ttu-id="0ee94-338">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-338">Required</span></span>|<span data-ttu-id="0ee94-339">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-339">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-340">mall</span><span class="sxs-lookup"><span data-stu-id="0ee94-340">template</span></span>|<span data-ttu-id="0ee94-341">Används för att ändra läget för templating som ange brödtext princip kommer att köras.</span><span class="sxs-lookup"><span data-stu-id="0ee94-341">Used to change the templating mode that the set body policy will run in.</span></span> <span data-ttu-id="0ee94-342">För närvarande är det enda värdet som stöds:</span><span class="sxs-lookup"><span data-stu-id="0ee94-342">Currently the only supported value is:</span></span><br /><br /><span data-ttu-id="0ee94-343">-flytande - ange brödtext princip kommer att använda flytande templating motorn</span><span class="sxs-lookup"><span data-stu-id="0ee94-343">- liquid - the set body policy will use the liquid templating engine</span></span> |<span data-ttu-id="0ee94-344">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-344">No</span></span>|<span data-ttu-id="0ee94-345">flytande</span><span class="sxs-lookup"><span data-stu-id="0ee94-345">liquid</span></span>|  

<span data-ttu-id="0ee94-346">För att komma åt information om begäran och svar, kan flytande mallen bindas till ett kontextobjekt med följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0ee94-346">For accessing information about the request and response, the Liquid template can bind to a context object with the following properties:</span></span> <br />
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



### <a name="usage"></a><span data-ttu-id="0ee94-347">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-347">Usage</span></span>  
 <span data-ttu-id="0ee94-348">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-348">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-349">**Avsnitt i princip:** inkommande, utgående backend</span><span class="sxs-lookup"><span data-stu-id="0ee94-349">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="0ee94-350">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-350">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-351"><a name="SetHTTPheader"></a>Ange HTTP-huvud</span><span class="sxs-lookup"><span data-stu-id="0ee94-351"><a name="SetHTTPheader"></a> Set HTTP header</span></span>  
 <span data-ttu-id="0ee94-352">Den `set-header` princip tilldelar ett värde till en befintlig svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="0ee94-352">The `set-header` policy assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
 <span data-ttu-id="0ee94-353">Infogar en lista över HTTP-huvuden i ett HTTP-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0ee94-353">Inserts a list of HTTP headers into an HTTP message.</span></span> <span data-ttu-id="0ee94-354">När den placeras i en inkommande pipeline i den här principen anger HTTP-huvuden för begäran som skickas till Måltjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-354">When placed in an inbound pipeline, this policy sets the HTTP headers for the request being passed to the target service.</span></span> <span data-ttu-id="0ee94-355">När den placeras i en utgående pipeline i den här principen anger HTTP-huvuden för svar som skickas till en gateway-klienten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-355">When placed in an outbound pipeline, this policy sets the HTTP headers for the response being sent to the gateway’s client.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-356">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-356">Policy statement</span></span>  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with the same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a><span data-ttu-id="0ee94-357">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-357">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="0ee94-358">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-358">Example</span></span>  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="0ee94-359">Vidarebefordra kontextinformation till serverdelstjänsten</span><span class="sxs-lookup"><span data-stu-id="0ee94-359">Forward context information to the backend service</span></span>  
 <span data-ttu-id="0ee94-360">Det här exemplet visar hur du tillämpa principen på API-nivå för att leverera sammanhangsinformation till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-360">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="0ee94-361">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 10:30.</span><span class="sxs-lookup"><span data-stu-id="0ee94-361">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="0ee94-362">Det finns en demonstration av anropa en funktion i developer-portalen där du kan se principen på arbetet vid 12:10.</span><span class="sxs-lookup"><span data-stu-id="0ee94-362">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward some context information, user id and the region the gateway is hosted in, to the backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 <span data-ttu-id="0ee94-363">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="0ee94-363">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="0ee94-364">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-364">Elements</span></span>  
  
|<span data-ttu-id="0ee94-365">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-365">Name</span></span>|<span data-ttu-id="0ee94-366">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-366">Description</span></span>|<span data-ttu-id="0ee94-367">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-367">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-368">set-huvud</span><span class="sxs-lookup"><span data-stu-id="0ee94-368">set-header</span></span>|<span data-ttu-id="0ee94-369">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-369">Root element.</span></span>|<span data-ttu-id="0ee94-370">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-370">Yes</span></span>|  
|<span data-ttu-id="0ee94-371">värde</span><span class="sxs-lookup"><span data-stu-id="0ee94-371">value</span></span>|<span data-ttu-id="0ee94-372">Anger värdet för huvudet anges.</span><span class="sxs-lookup"><span data-stu-id="0ee94-372">Specifies the value of the header to be set.</span></span> <span data-ttu-id="0ee94-373">För flera huvuden med samma namn Lägg till ytterligare `value` element.</span><span class="sxs-lookup"><span data-stu-id="0ee94-373">For multiple headers with the same name add additional `value` elements.</span></span>|<span data-ttu-id="0ee94-374">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-374">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="0ee94-375">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="0ee94-375">Properties</span></span>  
  
|<span data-ttu-id="0ee94-376">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-376">Name</span></span>|<span data-ttu-id="0ee94-377">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-377">Description</span></span>|<span data-ttu-id="0ee94-378">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-378">Required</span></span>|<span data-ttu-id="0ee94-379">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-379">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-380">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="0ee94-380">exists-action</span></span>|<span data-ttu-id="0ee94-381">Anger vilken åtgärd som ska vidtas när huvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="0ee94-381">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="0ee94-382">Det här attributet måste ha något av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-382">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-383">-åsidosätt - ersätter värdet för befintliga-huvud.</span><span class="sxs-lookup"><span data-stu-id="0ee94-383">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="0ee94-384">-skip - ersätter inte det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-384">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="0ee94-385">-Tillägg - lägger till värdet på det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-385">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="0ee94-386">-delete - tar bort huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-386">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="0ee94-387">Om värdet är `override` ta med flera poster med samma namn resulterar i sidhuvudet har angetts enligt alla poster (som visas flera gånger); endast listade värden anges i resultatet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-387">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="0ee94-388">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-388">No</span></span>|<span data-ttu-id="0ee94-389">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="0ee94-389">override</span></span>|  
|<span data-ttu-id="0ee94-390">namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-390">name</span></span>|<span data-ttu-id="0ee94-391">Anger namnet på huvudet som ska anges.</span><span class="sxs-lookup"><span data-stu-id="0ee94-391">Specifies name of the header to be set.</span></span>|<span data-ttu-id="0ee94-392">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-392">Yes</span></span>|<span data-ttu-id="0ee94-393">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-393">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-394">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-394">Usage</span></span>  
 <span data-ttu-id="0ee94-395">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-395">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-396">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="0ee94-396">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="0ee94-397">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-397">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-398"><a name="SetQueryStringParameter"></a>Ange frågesträngparametern</span><span class="sxs-lookup"><span data-stu-id="0ee94-398"><a name="SetQueryStringParameter"></a> Set query string parameter</span></span>  
 <span data-ttu-id="0ee94-399">Den `set-query-parameter` principen läggs till, ersätter värde, eller tar bort begära frågesträngparametern.</span><span class="sxs-lookup"><span data-stu-id="0ee94-399">The `set-query-parameter` policy adds, replaces value of, or deletes request query string parameter.</span></span> <span data-ttu-id="0ee94-400">Kan användas för att skicka fråga parametrar förväntades av backend-tjänsten som är valfria eller aldrig i begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-400">Can be used to pass query parameters expected by the backend service which are optional or never present in the request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-401">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-401">Policy statement</span></span>  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with the same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a><span data-ttu-id="0ee94-402">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-402">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="0ee94-403">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-403">Example</span></span>  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with the same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-to-the-backend-service"></a><span data-ttu-id="0ee94-404">Vidarebefordra kontextinformation till serverdelstjänsten</span><span class="sxs-lookup"><span data-stu-id="0ee94-404">Forward context information to the backend service</span></span>  
 <span data-ttu-id="0ee94-405">Det här exemplet visar hur du tillämpa principen på API-nivå för att leverera sammanhangsinformation till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-405">This example shows how to apply policy at the API level to supply context information to the backend service.</span></span> <span data-ttu-id="0ee94-406">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 10:30.</span><span class="sxs-lookup"><span data-stu-id="0ee94-406">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 10:30.</span></span> <span data-ttu-id="0ee94-407">Det finns en demonstration av anropa en funktion i developer-portalen där du kan se principen på arbetet vid 12:10.</span><span class="sxs-lookup"><span data-stu-id="0ee94-407">At 12:10 there is a demo of calling an operation in the developer portal where you can see the policy at work.</span></span>  
  
```xml  
<!-- Copy this snippet into the inbound element to forward a piece of context, product name in this example, to the backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 <span data-ttu-id="0ee94-408">Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="0ee94-408">For more information, see [Policy expressions](api-management-policy-expressions.md) and [Context variable](api-management-policy-expressions.md#ContextVariables).</span></span>  
  
### <a name="elements"></a><span data-ttu-id="0ee94-409">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-409">Elements</span></span>  
  
|<span data-ttu-id="0ee94-410">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-410">Name</span></span>|<span data-ttu-id="0ee94-411">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-411">Description</span></span>|<span data-ttu-id="0ee94-412">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-412">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-413">Ange frågeparameter</span><span class="sxs-lookup"><span data-stu-id="0ee94-413">set-query-parameter</span></span>|<span data-ttu-id="0ee94-414">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-414">Root element.</span></span>|<span data-ttu-id="0ee94-415">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-415">Yes</span></span>|  
|<span data-ttu-id="0ee94-416">värde</span><span class="sxs-lookup"><span data-stu-id="0ee94-416">value</span></span>|<span data-ttu-id="0ee94-417">Anger värdet för Frågeparametern anges.</span><span class="sxs-lookup"><span data-stu-id="0ee94-417">Specifies the value of the query parameter to be set.</span></span> <span data-ttu-id="0ee94-418">För flera frågeparametrar med samma namn Lägg till ytterligare `value` element.</span><span class="sxs-lookup"><span data-stu-id="0ee94-418">For multiple query parameters with the same name add additional `value` elements.</span></span>|<span data-ttu-id="0ee94-419">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-419">Yes</span></span>|  
  
### <a name="properties"></a><span data-ttu-id="0ee94-420">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="0ee94-420">Properties</span></span>  
  
|<span data-ttu-id="0ee94-421">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-421">Name</span></span>|<span data-ttu-id="0ee94-422">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-422">Description</span></span>|<span data-ttu-id="0ee94-423">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-423">Required</span></span>|<span data-ttu-id="0ee94-424">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-424">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-425">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="0ee94-425">exists-action</span></span>|<span data-ttu-id="0ee94-426">Anger vilken åtgärd som ska vidtas när Frågeparametern har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="0ee94-426">Specifies what action to take when the query parameter is already specified.</span></span> <span data-ttu-id="0ee94-427">Det här attributet måste ha något av följande värden.</span><span class="sxs-lookup"><span data-stu-id="0ee94-427">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="0ee94-428">-åsidosätt - ersätter befintliga parameterns värde.</span><span class="sxs-lookup"><span data-stu-id="0ee94-428">-   override - replaces the value of the existing parameter.</span></span><br /><span data-ttu-id="0ee94-429">-skip - ersätter inte befintliga parametervärdet för frågan.</span><span class="sxs-lookup"><span data-stu-id="0ee94-429">-   skip - does not replace the existing query parameter value.</span></span><br /><span data-ttu-id="0ee94-430">-Tillägg - lägger till värdet till det befintliga frågeparametervärdet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-430">-   append - appends the value to the existing query parameter value.</span></span><br /><span data-ttu-id="0ee94-431">-delete - tar bort Frågeparametern från begäran.</span><span class="sxs-lookup"><span data-stu-id="0ee94-431">-   delete - removes the query parameter from the request.</span></span><br /><br /> <span data-ttu-id="0ee94-432">Om värdet är `override` ta med flera poster med samma namn resulterar i Frågeparametern anges enligt alla poster (som visas flera gånger); endast listade värden anges i resultatet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-432">When set to `override` enlisting multiple entries with the same name results in the query parameter being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="0ee94-433">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-433">No</span></span>|<span data-ttu-id="0ee94-434">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="0ee94-434">override</span></span>|  
|<span data-ttu-id="0ee94-435">namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-435">name</span></span>|<span data-ttu-id="0ee94-436">Anger namnet på Frågeparametern anges.</span><span class="sxs-lookup"><span data-stu-id="0ee94-436">Specifies name of the query parameter to be set.</span></span>|<span data-ttu-id="0ee94-437">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-437">Yes</span></span>|<span data-ttu-id="0ee94-438">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-438">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-439">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-439">Usage</span></span>  
 <span data-ttu-id="0ee94-440">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-440">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-441">**Avsnitt i princip:** inkommande, backend</span><span class="sxs-lookup"><span data-stu-id="0ee94-441">**Policy sections:** inbound, backend</span></span>  
  
-   <span data-ttu-id="0ee94-442">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-442">**Policy scopes:** global, product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-443"><a name="RewriteURL"></a>Omarbetning URL</span><span class="sxs-lookup"><span data-stu-id="0ee94-443"><a name="RewriteURL"></a> Rewrite URL</span></span>  
 <span data-ttu-id="0ee94-444">Den `rewrite-uri` princip konverterar en begärd URL från dess offentliga form i formuläret som förväntades av webbtjänsten som visas i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="0ee94-444">The `rewrite-uri` policy converts a request URL from its public form to the form expected by the web service, as shown in the following example.</span></span>  
  
-   <span data-ttu-id="0ee94-445">Offentlig URL-`http://api.example.com/storenumber/ordernumber`</span><span class="sxs-lookup"><span data-stu-id="0ee94-445">Public URL - `http://api.example.com/storenumber/ordernumber`</span></span>  
  
-   <span data-ttu-id="0ee94-446">URL-begäran-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span><span class="sxs-lookup"><span data-stu-id="0ee94-446">Request URL - `http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`</span></span>  
  
 <span data-ttu-id="0ee94-447">Den här principen kan användas när en mänsklig och/eller webbläsare eget URL bör omvandlas till URL-formatet som förväntas av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="0ee94-447">This policy can be used when a human and/or browser-friendly URL should be transformed into the URL format expected by the web service.</span></span> <span data-ttu-id="0ee94-448">Den här principen behöver bara tillämpas när exponera en alternativ URL-format, till exempel ren URL: er, RESTful-URL: er, användarvänliga URL: er eller SEO eget URL: er som är enbart strukturella URL: er som inte innehåller en frågesträng och i stället innehåller endast sökvägen till resursen ( efter schemat och behörighet).</span><span class="sxs-lookup"><span data-stu-id="0ee94-448">This policy only needs to be applied when exposing an alternative URL format, such as clean URLs, RESTful URLs, user-friendly URLs or SEO-friendly URLs that are purely structural URLs that do not contain a query string and instead contain only the path of the resource (after the scheme and the authority).</span></span> <span data-ttu-id="0ee94-449">Detta sker ofta för dess estetiska egenskaper, användbarhet eller sökmotor optimeringssyfte (SEO).</span><span class="sxs-lookup"><span data-stu-id="0ee94-449">This is often done for aesthetic, usability, or search engine optimization (SEO) purposes.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="0ee94-450">Du kan bara lägga till frågan string-parametrar med principer.</span><span class="sxs-lookup"><span data-stu-id="0ee94-450">You can only add query string parameters using the policy.</span></span> <span data-ttu-id="0ee94-451">Du kan inte lägga till extra sökväg i mallparametrarna i URL: en för omarbetning.</span><span class="sxs-lookup"><span data-stu-id="0ee94-451">You cannot add extra template path parameters in the rewrite URL.</span></span>  

### <a name="policy-statement"></a><span data-ttu-id="0ee94-452">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-452">Policy statement</span></span>  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a><span data-ttu-id="0ee94-453">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-453">Example</span></span>  
  
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
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
<!-- Assuming incoming request is /get?a=b&c=d and operation template is set to /get?a={b} -->
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

### <a name="elements"></a><span data-ttu-id="0ee94-454">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-454">Elements</span></span>  
  
|<span data-ttu-id="0ee94-455">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-455">Name</span></span>|<span data-ttu-id="0ee94-456">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-456">Description</span></span>|<span data-ttu-id="0ee94-457">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-457">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-458">omarbetning-uri</span><span class="sxs-lookup"><span data-stu-id="0ee94-458">rewrite-uri</span></span>|<span data-ttu-id="0ee94-459">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-459">Root element.</span></span>|<span data-ttu-id="0ee94-460">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-460">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="0ee94-461">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-461">Attributes</span></span>  
  
|<span data-ttu-id="0ee94-462">Attribut</span><span class="sxs-lookup"><span data-stu-id="0ee94-462">Attribute</span></span>|<span data-ttu-id="0ee94-463">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-463">Description</span></span>|<span data-ttu-id="0ee94-464">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-464">Required</span></span>|<span data-ttu-id="0ee94-465">Standard</span><span class="sxs-lookup"><span data-stu-id="0ee94-465">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="0ee94-466">mall</span><span class="sxs-lookup"><span data-stu-id="0ee94-466">template</span></span>|<span data-ttu-id="0ee94-467">Den faktiska webbtjänst-URL med någon fråga string-parametrar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-467">The actual web service URL with any query string parameters.</span></span> <span data-ttu-id="0ee94-468">När du använder uttryck måste hela värdet vara ett uttryck.</span><span class="sxs-lookup"><span data-stu-id="0ee94-468">When using expressions, the whole value must be an expression.</span></span>|<span data-ttu-id="0ee94-469">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-469">Yes</span></span>|<span data-ttu-id="0ee94-470">Saknas</span><span class="sxs-lookup"><span data-stu-id="0ee94-470">N/A</span></span>|  
|<span data-ttu-id="0ee94-471">Kopiera omatchade parametrar</span><span class="sxs-lookup"><span data-stu-id="0ee94-471">copy-unmatched-params</span></span>|<span data-ttu-id="0ee94-472">Anger om Frågeparametrar i inkommande begäran finns inte i den ursprungliga mallen URL läggs till den URL som definieras i Skriv mallen</span><span class="sxs-lookup"><span data-stu-id="0ee94-472">Specifies whether query parameters in the incoming request not present in the original URL template are added to the URL defined by the re-write template</span></span>|<span data-ttu-id="0ee94-473">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-473">No</span></span>|<span data-ttu-id="0ee94-474">SANT</span><span class="sxs-lookup"><span data-stu-id="0ee94-474">true</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-475">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-475">Usage</span></span>  
 <span data-ttu-id="0ee94-476">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-476">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-477">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="0ee94-477">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="0ee94-478">**Princip för scope:** produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-478">**Policy scopes:** product, API, operation</span></span>  
  
##  <span data-ttu-id="0ee94-479"><a name="XSLTransform"></a>Transformera XML med hjälp av en XSLT-fil</span><span class="sxs-lookup"><span data-stu-id="0ee94-479"><a name="XSLTransform"></a> Transform XML using an XSLT</span></span>  
 <span data-ttu-id="0ee94-480">Den `Transform XML using an XSLT` principen gäller en XSL-transformation på XML i begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="0ee94-480">The `Transform XML using an XSLT` policy applies an XSL transformation to XML in the request or response body.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="0ee94-481">Principframställning</span><span class="sxs-lookup"><span data-stu-id="0ee94-481">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="0ee94-482">Exempel</span><span class="sxs-lookup"><span data-stu-id="0ee94-482">Example</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="0ee94-483">Element</span><span class="sxs-lookup"><span data-stu-id="0ee94-483">Elements</span></span>  
  
|<span data-ttu-id="0ee94-484">Namn</span><span class="sxs-lookup"><span data-stu-id="0ee94-484">Name</span></span>|<span data-ttu-id="0ee94-485">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0ee94-485">Description</span></span>|<span data-ttu-id="0ee94-486">Krävs</span><span class="sxs-lookup"><span data-stu-id="0ee94-486">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="0ee94-487">XSL-Transformation</span><span class="sxs-lookup"><span data-stu-id="0ee94-487">xsl-transform</span></span>|<span data-ttu-id="0ee94-488">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-488">Root element.</span></span>|<span data-ttu-id="0ee94-489">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-489">Yes</span></span>|  
|<span data-ttu-id="0ee94-490">Parametern</span><span class="sxs-lookup"><span data-stu-id="0ee94-490">parameter</span></span>|<span data-ttu-id="0ee94-491">Används för att definiera variabler som används i listan i transformeringen</span><span class="sxs-lookup"><span data-stu-id="0ee94-491">Used to define variables used in the transform</span></span>|<span data-ttu-id="0ee94-492">Nej</span><span class="sxs-lookup"><span data-stu-id="0ee94-492">No</span></span>|  
|<span data-ttu-id="0ee94-493">XSL: stylesheet</span><span class="sxs-lookup"><span data-stu-id="0ee94-493">xsl:stylesheet</span></span>|<span data-ttu-id="0ee94-494">Formatmallen rotelementet.</span><span class="sxs-lookup"><span data-stu-id="0ee94-494">Root stylesheet element.</span></span> <span data-ttu-id="0ee94-495">Alla element och attribut som definierats inom följer standarden [XSLT-specifikationen](http://www.w3.org/TR/xslt)</span><span class="sxs-lookup"><span data-stu-id="0ee94-495">All elements and attributes defined within follow the standard [XSLT specification](http://www.w3.org/TR/xslt)</span></span>|<span data-ttu-id="0ee94-496">Ja</span><span class="sxs-lookup"><span data-stu-id="0ee94-496">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="0ee94-497">Användning</span><span class="sxs-lookup"><span data-stu-id="0ee94-497">Usage</span></span>  
 <span data-ttu-id="0ee94-498">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="0ee94-498">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="0ee94-499">**Avsnitt i princip:** inkommande, utgående</span><span class="sxs-lookup"><span data-stu-id="0ee94-499">**Policy sections:** inbound, outbound</span></span>  
  
-   <span data-ttu-id="0ee94-500">**Princip för scope:** globala, produkt, API, åtgärden</span><span class="sxs-lookup"><span data-stu-id="0ee94-500">**Policy scopes:** global, product, API, operation</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="0ee94-501">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ee94-501">Next steps</span></span>
<span data-ttu-id="0ee94-502">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="0ee94-502">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
