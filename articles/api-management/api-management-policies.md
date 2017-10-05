---
title: Azure API Management-principer | Microsoft Docs
description: "Läs mer om principerna som är tillgängliga för användning i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 1cc460cb-8e67-41aa-bc76-bbafc1892798
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 485dc3a87a81dc67f5144596a30d498293d6b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-policies"></a><span data-ttu-id="37e9a-103">API Management-principer</span><span class="sxs-lookup"><span data-stu-id="37e9a-103">API Management policies</span></span>
<span data-ttu-id="37e9a-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="37e9a-104">This section provides a reference for the following API Management policies.</span></span> <span data-ttu-id="37e9a-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="37e9a-105">For information on adding and configuring policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
  
 <span data-ttu-id="37e9a-106">Principer är en kraftfull funktion för systemet som tillåter utgivaren för att ändra funktionssättet för API: et genom konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="37e9a-106">Policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="37e9a-107">Principer är en samling av instruktioner som utförs i tur och ordning på begäran eller svar på en API.</span><span class="sxs-lookup"><span data-stu-id="37e9a-107">Policies are a collection of Statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="37e9a-108">Populära instruktioner med Formatkonvertering från XML till JSON och anropa hastighet begränsa om du vill begränsa mängden inkommande samtal från en utvecklare.</span><span class="sxs-lookup"><span data-stu-id="37e9a-108">Popular Statements include format conversion from XML to JSON and call rate limiting to restrict the amount of incoming calls from a developer.</span></span> <span data-ttu-id="37e9a-109">Många fler principer är tillgängliga direkt.</span><span class="sxs-lookup"><span data-stu-id="37e9a-109">Many more policies are available out of the box.</span></span>  
  
 <span data-ttu-id="37e9a-110">Principuttryck kan användas som attributvärden eller textvärden i API Management-principer, under förutsättning att principen tillåter det.</span><span class="sxs-lookup"><span data-stu-id="37e9a-110">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="37e9a-111">Vissa principer som [Kontrollflöde](api-management-advanced-policies.md#choose) och [Ange variabel](api-management-advanced-policies.md#set-variable) baseras på principuttryck.</span><span class="sxs-lookup"><span data-stu-id="37e9a-111">Some policies such as the [Control flow](api-management-advanced-policies.md#choose) and [Set variable](api-management-advanced-policies.md#set-variable) policies are based on policy expressions.</span></span> <span data-ttu-id="37e9a-112">Mer information finns i [avancerade principer](api-management-advanced-policies.md#AdvancedPolicies) och [principuttrycken](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="37e9a-112">For more information, see [Advanced policies](api-management-advanced-policies.md#AdvancedPolicies) and [Policy expressions](api-management-policy-expressions.md).</span></span>  
  
##  <span data-ttu-id="37e9a-113"><a name="ProxyPolicies"></a>Principer</span><span class="sxs-lookup"><span data-stu-id="37e9a-113"><a name="ProxyPolicies"></a> Policies</span></span>  
  
-   [<span data-ttu-id="37e9a-114">Principer för begränsning av åtkomst</span><span class="sxs-lookup"><span data-stu-id="37e9a-114">Access restriction policies</span></span>](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   <span data-ttu-id="37e9a-115">[Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="37e9a-115">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
    -   <span data-ttu-id="37e9a-116">[Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37e9a-116">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="37e9a-117">[Gränsen anropet frekvensen av nyckeln](api-management-access-restriction-policies.md#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="37e9a-117">[Limit call rate by key](api-management-access-restriction-policies.md#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="37e9a-118">[Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="37e9a-118">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
    -   <span data-ttu-id="37e9a-119">[Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -du kan tillämpa en förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37e9a-119">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="37e9a-120">[Kvot för uppsättningen av nyckeln](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -du kan tillämpa en förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="37e9a-120">[Set usage quota by key](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Allows you to enforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="37e9a-121">[Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="37e9a-121">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
-   [<span data-ttu-id="37e9a-122">Avancerade principer</span><span class="sxs-lookup"><span data-stu-id="37e9a-122">Advanced policies</span></span>](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   <span data-ttu-id="37e9a-123">[Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principen uttryck baserat på tillämpningen av booleska uttryck.</span><span class="sxs-lookup"><span data-stu-id="37e9a-123">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on the evaluation of Boolean expressions.</span></span>  
  
    -   <span data-ttu-id="37e9a-124">[Vidarebefordra begäran](api-management-advanced-policies.md#ForwardRequest) -vidarebefordrar begäran till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="37e9a-124">[Forward request](api-management-advanced-policies.md#ForwardRequest) - Forwards the request to the backend service.</span></span>  
  
    -   <span data-ttu-id="37e9a-125">[Loggen till Event Hub](api-management-advanced-policies.md#log-to-eventhub) -skickar meddelanden i det angivna formatet till ett meddelandemål som definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="37e9a-125">[Log to Event Hub](api-management-advanced-policies.md#log-to-eventhub) - Sends messages in the specified format to a message target defined by a Logger entity.</span></span>  
  
    -   <span data-ttu-id="37e9a-126">[Försök](api-management-advanced-policies.md#Retry) -återförsök körningen av de slutna principrapporter om och tills villkoret är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="37e9a-126">[Retry](api-management-advanced-policies.md#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="37e9a-127">Körningen upprepas med de angivna intervall och upp till angivet antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="37e9a-127">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>  
  
    -   <span data-ttu-id="37e9a-128">[Returnera svar](api-management-advanced-policies.md#ReturnResponse) -avbryts körningen i pipeline och returnerar det angivna svaret direkt till anroparen.</span><span class="sxs-lookup"><span data-stu-id="37e9a-128">[Return response](api-management-advanced-policies.md#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span>  
  
    -   <span data-ttu-id="37e9a-129">[Skicka förfrågan om enkelriktade](api-management-advanced-policies.md#SendOneWayRequest) -skickar en begäran till den angivna URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="37e9a-129">[Send one way request](api-management-advanced-policies.md#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>  
  
    -   <span data-ttu-id="37e9a-130">[Skicka förfrågan](api-management-advanced-policies.md#SendRequest) -skickar en begäran till angiven URL.</span><span class="sxs-lookup"><span data-stu-id="37e9a-130">[Send request](api-management-advanced-policies.md#SendRequest) - Sends a request to the specified URL.</span></span>  
  
    -   <span data-ttu-id="37e9a-131">[Ange variabel](api-management-advanced-policies.md#set-variable) -bevara ett värde i en namngiven kontexten variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="37e9a-131">[Set variable](api-management-advanced-policies.md#set-variable) - Persist a value in a named context variable for later access.</span></span>  
  
    -   <span data-ttu-id="37e9a-132">[Ange metoden](api-management-advanced-policies.md#SetRequestMethod) -kan du ändra HTTP-metoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="37e9a-132">[Set request method](api-management-advanced-policies.md#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>  
  
    -   <span data-ttu-id="37e9a-133">[Ange statuskoden](api-management-advanced-policies.md#SetStatus) -ändrar HTTP-statuskoden till det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="37e9a-133">[Set status code](api-management-advanced-policies.md#SetStatus) - Changes the HTTP status code to the specified value.</span></span>  
  
    -   <span data-ttu-id="37e9a-134">[Spåra](api-management-advanced-policies.md#Trace) -lägger till en sträng i den [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="37e9a-134">[Trace](api-management-advanced-policies.md#Trace) - Adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
    -   <span data-ttu-id="37e9a-135">[Vänta](api-management-advanced-policies.md#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer för att slutföra innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="37e9a-135">[Wait](api-management-advanced-policies.md#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies to complete before proceeding.</span></span>  
  
-   [<span data-ttu-id="37e9a-136">Autentiseringsprinciper</span><span class="sxs-lookup"><span data-stu-id="37e9a-136">Authentication policies</span></span>](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   <span data-ttu-id="37e9a-137">[Autentisera med grundläggande](api-management-authentication-policies.md#Basic) -autentisering med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="37e9a-137">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
    -   <span data-ttu-id="37e9a-138">[Autentisera med klientcertifikatet](api-management-authentication-policies.md#ClientCertificate) -autentisering med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="37e9a-138">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
-   [<span data-ttu-id="37e9a-139">Cachelagringsprinciper</span><span class="sxs-lookup"><span data-stu-id="37e9a-139">Caching policies</span></span>](api-management-caching-policies.md#CachingPolicies)  
  
    -   <span data-ttu-id="37e9a-140">[Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="37e9a-140">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached response when available.</span></span>  
  
    -   <span data-ttu-id="37e9a-141">[Arkivet ska cachelagras](api-management-caching-policies.md#StoreToCache) -cachelagrar svaret enligt den angivna konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="37e9a-141">[Store to cache](api-management-caching-policies.md#StoreToCache) - Caches response according to the specified cache control configuration.</span></span>  
  
    -   <span data-ttu-id="37e9a-142">[Hämta ett värde från cache](api-management-caching-policies.md#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.</span><span class="sxs-lookup"><span data-stu-id="37e9a-142">[Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="37e9a-143">[Lagrar värdet i cache](api-management-caching-policies.md#StoreToCacheByKey) -lagra ett objekt i cacheminnet av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="37e9a-143">[Store value in cache](api-management-caching-policies.md#StoreToCacheByKey) - Store an item in the cache by key.</span></span>  
  
    -   <span data-ttu-id="37e9a-144">[Ta bort värdet från cache](api-management-caching-policies.md#RemoveCacheByKey) -ta bort ett objekt i cacheminnet av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="37e9a-144">[Remove value from cache](api-management-caching-policies.md#RemoveCacheByKey) - Remove an item in the cache by key.</span></span>  
  
-   [<span data-ttu-id="37e9a-145">Principer mellan domäner</span><span class="sxs-lookup"><span data-stu-id="37e9a-145">Cross domain policies</span></span>](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   <span data-ttu-id="37e9a-146">[Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör API: et åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="37e9a-146">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes the API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
    -   <span data-ttu-id="37e9a-147">[CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöd till en åtgärd eller en API för att tillåta domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="37e9a-147">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support to an operation or an API to allow cross-domain calls from browser-based clients.</span></span>  
  
    -   <span data-ttu-id="37e9a-148">[Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) -lägger till JSON med stöd för utfyllnad (hanteras JSONP) till en åtgärd eller en API för att tillåta domänerna anrop från JavaScript-webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="37e9a-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support to an operation or an API to allow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
-   [<span data-ttu-id="37e9a-149">Omvandlingsprinciper</span><span class="sxs-lookup"><span data-stu-id="37e9a-149">Transformation policies</span></span>](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   <span data-ttu-id="37e9a-150">[Konvertera JSON till XML](api-management-transformation-policies.md#ConvertJSONtoXML) – konverterar begäran eller svar body från JSON till XML.</span><span class="sxs-lookup"><span data-stu-id="37e9a-150">[Convert JSON to XML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON to XML.</span></span>  
  
    -   <span data-ttu-id="37e9a-151">[Konvertera XML till JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – konverterar begäran eller svar body från XML till JSON.</span><span class="sxs-lookup"><span data-stu-id="37e9a-151">[Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML to JSON.</span></span>  
  
    -   <span data-ttu-id="37e9a-152">[Sök och Ersätt strängen i brödtexten](api-management-transformation-policies.md#Findandreplacestringinbody) - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="37e9a-152">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
    -   <span data-ttu-id="37e9a-153">[Maskera URL: er i innehåll](api-management-transformation-policies.md#MaskURLSContent) -skriver (masker) länkar i svaret body så att de pekar på motsvarande länk via gatewayen.</span><span class="sxs-lookup"><span data-stu-id="37e9a-153">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in the response body so that they point to the equivalent link via the gateway.</span></span>  
  
    -   <span data-ttu-id="37e9a-154">[Ange serverdelstjänst](api-management-transformation-policies.md#SetBackendService) -ändras serverdelstjänst för en inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="37e9a-154">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes the backend service for an incoming request.</span></span>  
  
    -   <span data-ttu-id="37e9a-155">[Konfigurera brödtext](api-management-transformation-policies.md#SetBody) -anger meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="37e9a-155">[Set body](api-management-transformation-policies.md#SetBody) - Sets the message body for incoming and outgoing requests.</span></span>  
  
    -   <span data-ttu-id="37e9a-156">[Ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) – tilldelar ett värde till en befintlig svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="37e9a-156">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value to an existing response and/or request header or adds a new response and/or request header.</span></span>  
  
    -   <span data-ttu-id="37e9a-157">[Ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.</span><span class="sxs-lookup"><span data-stu-id="37e9a-157">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
    -   <span data-ttu-id="37e9a-158">[Skriv om URL: en](api-management-transformation-policies.md#RewriteURL) -konverterar en begärd URL från dess offentliga form i formuläret som förväntades av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="37e9a-158">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form to the form expected by the web service.</span></span>  
  
    -   <span data-ttu-id="37e9a-159">[Transformera XML med hjälp av en XSLT](api-management-transformation-policies.md#XSLTransform) -tillämpar en XSL-transformation på XML i begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="37e9a-159">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation to XML in the request or response body.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="37e9a-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37e9a-160">Next steps</span></span>
<span data-ttu-id="37e9a-161">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="37e9a-161">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
