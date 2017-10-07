---
title: "aaaAzure principreferens för API Management"
description: "Läs mer om hello principer tillgängliga tooconfigure API-hantering."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7ee194f4d6f172bf836c789cbe8ab732e18a7e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-policy-reference"></a><span data-ttu-id="fd5ef-103">Principreferens för Azure API Management</span><span class="sxs-lookup"><span data-stu-id="fd5ef-103">Azure API Management Policy Reference</span></span>
<span data-ttu-id="fd5ef-104">Det här avsnittet innehåller ett index för hello principer i hello [principreferens för API Management][API Management policy reference].</span><span class="sxs-lookup"><span data-stu-id="fd5ef-104">This section provides an index for hello policies in hello [API Management policy reference][API Management policy reference].</span></span> <span data-ttu-id="fd5ef-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management][Policies in API Management].</span><span class="sxs-lookup"><span data-stu-id="fd5ef-105">For information on adding and configuring policies, see [Policies in API Management][Policies in API Management].</span></span>

<span data-ttu-id="fd5ef-106">Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-106">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="fd5ef-107">Vissa principer, till exempel hello [Åtkomstkontrollflödet] [ Control flow] och [ange variabel] [ Set variable] principer är baserade på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-107">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="fd5ef-108">Mer information finns i [avancerade principer] [ Advanced policies] och [principuttrycken][Policy expressions]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-108">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions]</span></span>

## <a name="policy-reference-index"></a><span data-ttu-id="fd5ef-109">Principreferensindex</span><span class="sxs-lookup"><span data-stu-id="fd5ef-109">Policy reference index</span></span>
* <span data-ttu-id="fd5ef-110">[Principer för begränsning av åtkomst][Access restriction policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-110">[Access restriction policies][Access restriction policies]</span></span>
  * <span data-ttu-id="fd5ef-111">[Kontrollera HTTP-huvudet] [ Check HTTP header] -tillämpar existens och/eller värdet för ett HTTP-huvud.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-111">[Check HTTP header][Check HTTP header] - Enforces existence and/or value of a HTTP Header.</span></span>
  * <span data-ttu-id="fd5ef-112">[Gränsen anropet frekvensen av prenumerationen] [ Limit call rate by subscription] -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-112">[Limit call rate by subscription][Limit call rate by subscription] - Prevents API usage spikes by limiting call rate, on a per subscription basis.</span></span>
  * <span data-ttu-id="fd5ef-113">[Gränsen anropet frekvensen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-113">[Limit call rate by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - Prevents API usage spikes by limiting call rate, on a per key basis.</span></span>
  * <span data-ttu-id="fd5ef-114">[Begränsa anroparen IP-adresser] [ Restrict caller IPs] -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-114">[Restrict caller IPs][Restrict caller IPs] - Filters (allows/denies) calls from specific IP addresses and/or address ranges.</span></span>
  * <span data-ttu-id="fd5ef-115">[Kvot för uppsättningen av prenumerationen] [ Set usage quota by subscription] -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-115">[Set usage quota by subscription][Set usage quota by subscription] - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per subscription basis.</span></span>
  * <span data-ttu-id="fd5ef-116">[Kvot för uppsättningen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-116">[Set usage quota by key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - Allows you tooenforce a renewable or lifetime call volume and/or bandwidth quota, on a per key basis.</span></span>
  * <span data-ttu-id="fd5ef-117">[Validera JWT] [ Validate JWT] -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-117">[Validate JWT][Validate JWT] - Enforces existence and validity of a JWT extracted from either a specified HTTP Header or a specified query parameter.</span></span>
* <span data-ttu-id="fd5ef-118">[Avancerade principer][Advanced policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-118">[Advanced policies][Advanced policies]</span></span>
  * <span data-ttu-id="fd5ef-119">[Åtkomstkontrollflödet] [ Control flow] - villkorligt gäller principrapporter utifrån hello resultat av hello utvärdering av boolesk [uttryck][expressions].</span><span class="sxs-lookup"><span data-stu-id="fd5ef-119">[Control flow][Control flow] - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions][expressions].</span></span>
  * <span data-ttu-id="fd5ef-120">[Vidarebefordra begäran] [ Forward request] -vidarebefordrar hello begäran toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-120">[Forward request][Forward request] - Forwards hello request toohello backend service.</span></span>
  * <span data-ttu-id="fd5ef-121">[Logga tooEvent hubb] [ Log tooEvent Hub] -skickar meddelanden i hello angivet format tooa meddelandemål definieras av en [loggaren](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitet.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-121">[Log tooEvent Hub][Log tooEvent Hub] - Sends messages in hello specified format tooa message target defined by a [Logger](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.</span></span>
  * <span data-ttu-id="fd5ef-122">[Försök](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-122">[Retry](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="fd5ef-123">Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-123">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>
  * <span data-ttu-id="fd5ef-124">[Returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-124">[Return response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span>
  * <span data-ttu-id="fd5ef-125">[Skicka förfrågan om enkelriktade](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-125">[Send one way request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>
  * <span data-ttu-id="fd5ef-126">[Skicka förfrågan](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -skickar en begäran toohello specificerat URL: en.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-126">[Send request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - Sends a request toohello specified URL.</span></span>
  * <span data-ttu-id="fd5ef-127">[Ange metoden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-127">[Set request method](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>
  * <span data-ttu-id="fd5ef-128">[Ange statusen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-128">[Set status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>
  * <span data-ttu-id="fd5ef-129">[Ange variabel] [ Set variable] -bevara ett värde i en namngiven [kontexten] [ context] variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-129">[Set variable][Set variable] - Persist a value in a named [context][context] variable for later access.</span></span>
  * <span data-ttu-id="fd5ef-130">[Spåra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -lägger till en sträng i hello [API Inspector](api-management-howto-api-inspector.md) utdata.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-130">[Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Adds a string into hello [API Inspector](api-management-howto-api-inspector.md) output.</span></span>
  * <span data-ttu-id="fd5ef-131">[Vänta](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) – väntar på slutna skicka begäran hämta värdet från cache eller styra flödet principer toocomplete innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-131">[Wait](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - Waits for enclosed Send request, Get value from cache, or Control flow policies toocomplete before proceeding.</span></span>
* <span data-ttu-id="fd5ef-132">[Principer för autentisering][Authentication policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-132">[Authentication policies][Authentication policies]</span></span>
  * <span data-ttu-id="fd5ef-133">[Autentisera med grundläggande] [ Authenticate with Basic] -autentisering med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-133">[Authenticate with Basic][Authenticate with Basic] - Authenticate with a backend service using Basic authentication.</span></span>
  * <span data-ttu-id="fd5ef-134">[Autentisera med klientcertifikatet] [ Authenticate with client certificate] -autentisering med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-134">[Authenticate with client certificate][Authenticate with client certificate] - Authenticate with a backend service using client certificates.</span></span>
* <span data-ttu-id="fd5ef-135">[Principer för cachelagring][Caching policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-135">[Caching policies][Caching policies]</span></span> 
  * <span data-ttu-id="fd5ef-136">[Hämta från cache] [ Get from cache] -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-136">[Get from cache][Get from cache] - Perform cache look up and return a valid cached response when available.</span></span>
  * <span data-ttu-id="fd5ef-137">[Lagra toocache] [ Store toocache] -cacheminnen svar enligt toohello angivna konfigurationen av cachen.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-137">[Store toocache][Store toocache] - Caches response according toohello specified cache control configuration.</span></span>
  * <span data-ttu-id="fd5ef-138">[Hämta ett värde från cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-138">[Get value from cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Retrieve a cached item by key.</span></span>
  * <span data-ttu-id="fd5ef-139">[Lagrar värdet i cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-139">[Store value in cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store an item in hello cache by key.</span></span>
  * <span data-ttu-id="fd5ef-140">[Ta bort värdet från cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-140">[Remove value from cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Remove an item in hello cache by key.</span></span>
* <span data-ttu-id="fd5ef-141">[Mellan domänprinciper][Cross domain policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-141">[Cross domain policies][Cross domain policies]</span></span> 
  * <span data-ttu-id="fd5ef-142">[Tillåt mellan domäner kan anropa] [ Allow cross-domain calls] -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-142">[Allow cross-domain calls][Allow cross-domain calls] - Makes hello API accessible from Adobe Flash and Microsoft Silverlight browser-based clients.</span></span>
  * <span data-ttu-id="fd5ef-143">[CORS] [ CORS] -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-143">[CORS][CORS] - Adds cross-origin resource sharing (CORS) support tooan operation or an API tooallow cross-domain calls from browser-based clients.</span></span>
  * <span data-ttu-id="fd5ef-144">[Hanteras JSONP] [ JSONP] – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-144">[JSONP][JSONP] - Adds JSON with padding (JSONP) support tooan operation or an API tooallow cross-domain calls from JavaScript browser-based clients.</span></span>
* <span data-ttu-id="fd5ef-145">[Principer för anspråksomvandling][Transformation policies]</span><span class="sxs-lookup"><span data-stu-id="fd5ef-145">[Transformation policies][Transformation policies]</span></span> 
  * <span data-ttu-id="fd5ef-146">[Konvertera JSON tooXML] [ Convert JSON tooXML] – konverterar begäran eller svar body från JSON tooXML.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-146">[Convert JSON tooXML][Convert JSON tooXML] - Converts request or response body from JSON tooXML.</span></span>
  * <span data-ttu-id="fd5ef-147">[Konvertera XML-tooJSON] [ Convert XML tooJSON] – konverterar begäran eller svar body från XML-tooJSON.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-147">[Convert XML tooJSON][Convert XML tooJSON] - Converts request or response body from XML tooJSON.</span></span>
  * <span data-ttu-id="fd5ef-148">[Sök och Ersätt strängen i brödtexten] [ Find and replace string in body] - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-148">[Find and replace string in body][Find and replace string in body] - Finds a request or response substring and replaces it with a different substring.</span></span>
  * <span data-ttu-id="fd5ef-149">[Maskera URL: er i innehåll] [ Mask URLs in content] -skriver (masker) länkar hello svar body så att de pekar toohello motsvarande länk via hello gateway.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-149">[Mask URLs in content][Mask URLs in content] - Re-writes (masks) links in hello response body so that they point toohello equivalent link via hello gateway.</span></span>
  * <span data-ttu-id="fd5ef-150">[Ange serverdelstjänst] [ Set backend service] -ändras hello serverdelstjänst för en inkommande begäran.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-150">[Set backend service][Set backend service] - Changes hello backend service for an incoming request.</span></span>
  * <span data-ttu-id="fd5ef-151">[Konfigurera brödtext] [ Set body] -anger hello meddelandetexten för inkommande och utgående förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-151">[Set body][Set body] - Sets hello message body for incoming and outgoing requests.</span></span>
  * <span data-ttu-id="fd5ef-152">[Ange HTTP-huvudet] [ Set HTTP header] - tilldelar en värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-152">[Set HTTP header][Set HTTP header] - Assigns a value tooan existing response and/or request header or adds a new response and/or request header.</span></span>
  * <span data-ttu-id="fd5ef-153">[Ange frågesträngparametern] [ Set query string parameter] – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-153">[Set query string parameter][Set query string parameter] - Adds, replaces value of, or deletes request query string parameter.</span></span>
  * <span data-ttu-id="fd5ef-154">[Skriv om URL: en] [ Rewrite URL] -konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-154">[Rewrite URL][Rewrite URL] - Converts a request URL from its public form toohello form expected by hello web service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fd5ef-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fd5ef-155">Next steps</span></span>
<span data-ttu-id="fd5ef-156">Mer information om principuttrycken finns i följande video hello.</span><span class="sxs-lookup"><span data-stu-id="fd5ef-156">For more information on policy expressions, see hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log tooEvent Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store toocache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON tooXML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML tooJSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


