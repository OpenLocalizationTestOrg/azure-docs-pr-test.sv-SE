---
title: aaaAzure API Management-principer | Microsoft Docs
description: "Läs mer om hello principer kan användas i Azure API Management."
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
ms.openlocfilehash: 1c468ff37d73359f1dd694b91e20c2ca04f8934e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-policies"></a><span data-ttu-id="7c862-103">API Management-principer</span><span class="sxs-lookup"><span data-stu-id="7c862-103">API Management policies</span></span>
<span data-ttu-id="7c862-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="7c862-104">This section provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="7c862-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7c862-105">For information on adding and configuring policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
  
 <span data-ttu-id="7c862-106">Principer är en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7c862-106">Policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="7c862-107">Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API.</span><span class="sxs-lookup"><span data-stu-id="7c862-107">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="7c862-108">Populära instruktioner med Formatkonvertering från XML-tooJSON och anropa hastighet begränsa toorestrict hello mängden inkommande samtal från en utvecklare.</span><span class="sxs-lookup"><span data-stu-id="7c862-108">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="7c862-109">Det finns många fler principer out of box hello.</span><span class="sxs-lookup"><span data-stu-id="7c862-109">Many more policies are available out of hello box.</span></span>  
  
 <span data-ttu-id="7c862-110">Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars.</span><span class="sxs-lookup"><span data-stu-id="7c862-110">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="7c862-111">Vissa principer, till exempel hello [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) och [ange variabel](api-management-advanced-policies.md#set-variable) principer är baserade på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="7c862-111">Some policies such as hello [Control flow](api-management-advanced-policies.md#choose) and [Set variable](api-management-advanced-policies.md#set-variable) policies are based on policy expressions.</span></span> <span data-ttu-id="7c862-112">Mer information finns i [avancerade principer](api-management-advanced-policies.md#AdvancedPolicies) och [principuttrycken](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="7c862-112">For more information, see [Advanced policies](api-management-advanced-policies.md#AdvancedPolicies) and [Policy expressions](api-management-policy-expressions.md).</span></span>  
  
##  <span data-ttu-id="7c862-113"><a name="ProxyPolicies"></a>Principer</span><span class="sxs-lookup"><span data-stu-id="7c862-113"><a name="ProxyPolicies"></a> Policies</span></span>  
  
-   [<span data-ttu-id="7c862-114">Principer för begränsning av åtkomst</span><span class="sxs-lookup"><span data-stu-id="7c862-114">Access restriction policies</span></span>](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   <span data-ttu-id="7c862-115">[Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="7c862-115">[Check HTTP header](api-management-access-restriction-policies.md#CheckHTTPHeader) - Enforces existence and/or value of a HTTP Header.</span></span>  
  
    -   <span data-ttu-id="7c862-116">[Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c862-116">[Limit call rate by subscription](api-management-access-restriction-policies.md#LimitCallRate) - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="7c862-117">[Gränsen anropet frekvensen av nyckeln](api-management-access-restriction-policies.md#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="7c862-117">[Limit call rate by key](api-management-access-restriction-policies.md#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="7c862-118">[Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="7c862-118">[Restrict caller IPs](api-management-access-restriction-policies.md#RestrictCallerIPs) - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>  
  
    -   <span data-ttu-id="7c862-119">[Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c862-119">[Set usage quota by subscription](api-management-access-restriction-policies.md#SetUsageQuota) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>  
  
    -   <span data-ttu-id="7c862-120">[Kvot för uppsättningen av nyckeln](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="7c862-120">[Set usage quota by key](api-management-access-restriction-policies.md#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>  
  
    -   <span data-ttu-id="7c862-121">[Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="7c862-121">[Validate JWT](api-management-access-restriction-policies.md#ValidateJWT) - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>  
  
-   [<span data-ttu-id="7c862-122">Avancerade principer</span><span class="sxs-lookup"><span data-stu-id="7c862-122">Advanced policies</span></span>](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   <span data-ttu-id="7c862-123">[Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principen uttryck baserat på hello utvärdering av booleskt uttryck.</span><span class="sxs-lookup"><span data-stu-id="7c862-123">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello evaluation of Boolean expressions.</span></span>  
  
    -   <span data-ttu-id="7c862-124">[Vidarebefordra begäran](api-management-advanced-policies.md#ForwardRequest) -vidarebefordrar hello begäran toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="7c862-124">[Forward request](api-management-advanced-policies.md#ForwardRequest) - Forwards hello request toohello backend service.</span></span>  
  
    -   <span data-ttu-id="7c862-125">[Logga tooEvent hubb](api-management-advanced-policies.md#log-to-eventhub) -skickar meddelanden i hello angivet format tooa meddelandemål definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="7c862-125">[Log tooEvent Hub](api-management-advanced-policies.md#log-to-eventhub) - Sends messages in hello specified format tooa message target defined by a Logger entity.</span></span>  
  
    -   <span data-ttu-id="7c862-126">[Försök](api-management-advanced-policies.md#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="7c862-126">[Retry](api-management-advanced-policies.md#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="7c862-127">Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="7c862-127">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
    -   <span data-ttu-id="7c862-128">[Returnera svar](api-management-advanced-policies.md#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="7c862-128">[Return response](api-management-advanced-policies.md#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>  
  
    -   <span data-ttu-id="7c862-129">[Skicka förfrågan om enkelriktade](api-management-advanced-policies.md#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="7c862-129">[Send one way request](api-management-advanced-policies.md#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
    -   <span data-ttu-id="7c862-130">[Skicka förfrågan](api-management-advanced-policies.md#SendRequest) -skickar en begäran toohello specificerat URL: en.</span><span class="sxs-lookup"><span data-stu-id="7c862-130">[Send request](api-management-advanced-policies.md#SendRequest) - Sends a request toohello specified URL.</span></span>  
  
    -   <span data-ttu-id="7c862-131">[Ange variabel](api-management-advanced-policies.md#set-variable) -bevara ett värde i en namngiven kontexten variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="7c862-131">[Set variable](api-management-advanced-policies.md#set-variable) - Persist a value in a named context variable for later access.</span></span>  
  
    -   <span data-ttu-id="7c862-132">[Ange metoden](api-management-advanced-policies.md#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="7c862-132">[Set request method](api-management-advanced-policies.md#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
    -   <span data-ttu-id="7c862-133">[Ange statuskoden](api-management-advanced-policies.md#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.</span><span class="sxs-lookup"><span data-stu-id="7c862-133">[Set status code](api-management-advanced-policies.md#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
    -   <span data-ttu-id="7c862-134">[Spåra](api-management-advanced-policies.md#Trace) -lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="7c862-134">[Trace](api-management-advanced-policies.md#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
    -   <span data-ttu-id="7c862-135">[Vänta](api-management-advanced-policies.md#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer toocomplete innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="7c862-135">[Wait](api-management-advanced-policies.md#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
-   [<span data-ttu-id="7c862-136">Autentiseringsprinciper</span><span class="sxs-lookup"><span data-stu-id="7c862-136">Authentication policies</span></span>](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   <span data-ttu-id="7c862-137">[Autentisera med grundläggande](api-management-authentication-policies.md#Basic) -autentisering med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="7c862-137">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
    -   <span data-ttu-id="7c862-138">[Autentisera med klientcertifikatet](api-management-authentication-policies.md#ClientCertificate) -autentisering med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="7c862-138">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
-   [<span data-ttu-id="7c862-139">Cachelagringsprinciper</span><span class="sxs-lookup"><span data-stu-id="7c862-139">Caching policies</span></span>](api-management-caching-policies.md#CachingPolicies)  
  
    -   <span data-ttu-id="7c862-140">[Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="7c862-140">[Get from cache](api-management-caching-policies.md#GetFromCache) - Perform cache look up and return a valid cached response when available.</span></span>  
  
    -   <span data-ttu-id="7c862-141">[Lagra toocache](api-management-caching-policies.md#StoreToCache) -cacheminnen svar enligt toohello angivna konfigurationen av cachen.</span><span class="sxs-lookup"><span data-stu-id="7c862-141">[Store toocache](api-management-caching-policies.md#StoreToCache) - Caches response according toohello specified cache control configuration.</span></span>  
  
    -   <span data-ttu-id="7c862-142">[Hämta ett värde från cache](api-management-caching-policies.md#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.</span><span class="sxs-lookup"><span data-stu-id="7c862-142">[Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>  
  
    -   <span data-ttu-id="7c862-143">[Lagrar värdet i cache](api-management-caching-policies.md#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7c862-143">[Store value in cache](api-management-caching-policies.md#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>  
  
    -   <span data-ttu-id="7c862-144">[Ta bort värdet från cache](api-management-caching-policies.md#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7c862-144">[Remove value from cache](api-management-caching-policies.md#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>  
  
-   [<span data-ttu-id="7c862-145">Principer mellan domäner</span><span class="sxs-lookup"><span data-stu-id="7c862-145">Cross domain policies</span></span>](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   <span data-ttu-id="7c862-146">[Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="7c862-146">[Allow cross-domain calls](api-management-cross-domain-policies.md#AllowCrossDomainCalls) - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>  
  
    -   <span data-ttu-id="7c862-147">[CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="7c862-147">[CORS](api-management-cross-domain-policies.md#CORS) - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>  
  
    -   <span data-ttu-id="7c862-148">[Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="7c862-148">[JSONP](api-management-cross-domain-policies.md#JSONP) - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>  
  
-   [<span data-ttu-id="7c862-149">Omvandlingsprinciper</span><span class="sxs-lookup"><span data-stu-id="7c862-149">Transformation policies</span></span>](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   <span data-ttu-id="7c862-150">[Konvertera JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – konverterar begäran eller svar body från JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="7c862-150">[Convert JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) - Converts request or response body from JSON tooXML.</span></span>  
  
    -   <span data-ttu-id="7c862-151">[Konvertera XML-tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – konverterar begäran eller svar body från XML-tooJSON.</span><span class="sxs-lookup"><span data-stu-id="7c862-151">[Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) - Converts request or response body from XML tooJSON.</span></span>  
  
    -   <span data-ttu-id="7c862-152">[Sök och Ersätt strängen i brödtexten](api-management-transformation-policies.md#Findandreplacestringinbody) - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="7c862-152">[Find and replace string in body](api-management-transformation-policies.md#Findandreplacestringinbody) - Finds a request or response substring and replaces it with a different substring.</span></span>  
  
    -   <span data-ttu-id="7c862-153">[Maskera URL: er i innehåll](api-management-transformation-policies.md#MaskURLSContent) -skriver (masker) länkar hello svar body så att de pekar toohello motsvarande länk via hello gateway.</span><span class="sxs-lookup"><span data-stu-id="7c862-153">[Mask URLs in content](api-management-transformation-policies.md#MaskURLSContent) - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>  
  
    -   <span data-ttu-id="7c862-154">[Ange serverdelstjänst](api-management-transformation-policies.md#SetBackendService) -ändras hello serverdelstjänst för en inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="7c862-154">[Set backend service](api-management-transformation-policies.md#SetBackendService) - Changes hello backend service for an incoming request.</span></span>  
  
    -   <span data-ttu-id="7c862-155">[Konfigurera brödtext](api-management-transformation-policies.md#SetBody) -anger hello meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="7c862-155">[Set body](api-management-transformation-policies.md#SetBody) - Sets hello message body for incoming and outgoing requests.</span></span>  
  
    -   <span data-ttu-id="7c862-156">[Ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) - tilldelar en värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="7c862-156">[Set HTTP header](api-management-transformation-policies.md#SetHTTPheader) - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>  
  
    -   <span data-ttu-id="7c862-157">[Ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.</span><span class="sxs-lookup"><span data-stu-id="7c862-157">[Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) - Adds, replaces value of, or deletes request query string parameter.</span></span>  
  
    -   <span data-ttu-id="7c862-158">[Skriv om URL: en](api-management-transformation-policies.md#RewriteURL) -konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7c862-158">[Rewrite URL](api-management-transformation-policies.md#RewriteURL) - Converts a request URL from its public form toohello form expected by hello web service.</span></span>  
  
    -   <span data-ttu-id="7c862-159">[Transformera XML med hjälp av en XSLT](api-management-transformation-policies.md#XSLTransform) -gäller en XSL-transformation tooXML i hello begäran eller svar.</span><span class="sxs-lookup"><span data-stu-id="7c862-159">[Transform XML using an XSLT](api-management-transformation-policies.md#XSLTransform) - Applies an XSL transformation tooXML in hello request or response body.</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="7c862-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c862-160">Next steps</span></span>
<span data-ttu-id="7c862-161">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="7c862-161">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  
