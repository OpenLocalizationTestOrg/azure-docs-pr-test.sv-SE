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
# <a name="azure-api-management-policy-reference"></a>Principreferens för Azure API Management
Det här avsnittet innehåller ett index för hello principer i hello [principreferens för API Management][API Management policy reference]. Mer information om att lägga till och konfigurera principer finns [principer i API Management][Policies in API Management].

Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars. Vissa principer, till exempel hello [Åtkomstkontrollflödet] [ Control flow] och [ange variabel] [ Set variable] principer är baserade på principuttrycken. Mer information finns i [avancerade principer] [ Advanced policies] och [principuttrycken][Policy expressions]

## <a name="policy-reference-index"></a>Principreferensindex
* [Principer för begränsning av åtkomst][Access restriction policies]
  * [Kontrollera HTTP-huvudet] [ Check HTTP header] -tillämpar existens och/eller värdet för ett HTTP-huvud.
  * [Gränsen anropet frekvensen av prenumerationen] [ Limit call rate by subscription] -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.
  * [Gränsen anropet frekvensen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.
  * [Begränsa anroparen IP-adresser] [ Restrict caller IPs] -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.
  * [Kvot för uppsättningen av prenumerationen] [ Set usage quota by subscription] -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.
  * [Kvot för uppsättningen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.
  * [Validera JWT] [ Validate JWT] -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.
* [Avancerade principer][Advanced policies]
  * [Åtkomstkontrollflödet] [ Control flow] - villkorligt gäller principrapporter utifrån hello resultat av hello utvärdering av boolesk [uttryck][expressions].
  * [Vidarebefordra begäran] [ Forward request] -vidarebefordrar hello begäran toohello serverdelstjänst.
  * [Logga tooEvent hubb] [ Log tooEvent Hub] -skickar meddelanden i hello angivet format tooa meddelandemål definieras av en [loggaren](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entitet.
  * [Försök](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt. Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.
  * [Returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen.
  * [Skicka förfrågan om enkelriktade](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.
  * [Skicka förfrågan](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -skickar en begäran toohello specificerat URL: en.
  * [Ange metoden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.
  * [Ange statusen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.
  * [Ange variabel] [ Set variable] -bevara ett värde i en namngiven [kontexten] [ context] variabel för senare användning.
  * [Spåra](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -lägger till en sträng i hello [API Inspector](api-management-howto-api-inspector.md) utdata.
  * [Vänta](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) – väntar på slutna skicka begäran hämta värdet från cache eller styra flödet principer toocomplete innan du fortsätter.
* [Principer för autentisering][Authentication policies]
  * [Autentisera med grundläggande] [ Authenticate with Basic] -autentisering med en serverdelstjänst som använder grundläggande autentisering.
  * [Autentisera med klientcertifikatet] [ Authenticate with client certificate] -autentisering med en serverdelstjänst som använder klientcertifikat.
* [Principer för cachelagring][Caching policies] 
  * [Hämta från cache] [ Get from cache] -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.
  * [Lagra toocache] [ Store toocache] -cacheminnen svar enligt toohello angivna konfigurationen av cachen.
  * [Hämta ett värde från cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.
  * [Lagrar värdet i cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.
  * [Ta bort värdet från cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.
* [Mellan domänprinciper][Cross domain policies] 
  * [Tillåt mellan domäner kan anropa] [ Allow cross-domain calls] -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.
  * [CORS] [ CORS] -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.
  * [Hanteras JSONP] [ JSONP] – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.
* [Principer för anspråksomvandling][Transformation policies] 
  * [Konvertera JSON tooXML] [ Convert JSON tooXML] – konverterar begäran eller svar body från JSON tooXML.
  * [Konvertera XML-tooJSON] [ Convert XML tooJSON] – konverterar begäran eller svar body från XML-tooJSON.
  * [Sök och Ersätt strängen i brödtexten] [ Find and replace string in body] - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.
  * [Maskera URL: er i innehåll] [ Mask URLs in content] -skriver (masker) länkar hello svar body så att de pekar toohello motsvarande länk via hello gateway.
  * [Ange serverdelstjänst] [ Set backend service] -ändras hello serverdelstjänst för en inkommande begäran.
  * [Konfigurera brödtext] [ Set body] -anger hello meddelandetexten för inkommande och utgående förfrågningar.
  * [Ange HTTP-huvudet] [ Set HTTP header] - tilldelar en värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.
  * [Ange frågesträngparametern] [ Set query string parameter] – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.
  * [Skriv om URL: en] [ Rewrite URL] -konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten.

## <a name="next-steps"></a>Nästa steg
Mer information om principuttrycken finns i följande video hello.

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


