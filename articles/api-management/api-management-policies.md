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
# <a name="api-management-policies"></a>API Management-principer
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](api-management-howto-policies.md).  
  
 Principer är en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration. Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API. Populära instruktioner med Formatkonvertering från XML-tooJSON och anropa hastighet begränsa toorestrict hello mängden inkommande samtal från en utvecklare. Det finns många fler principer out of box hello.  
  
 Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars. Vissa principer, till exempel hello [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) och [ange variabel](api-management-advanced-policies.md#set-variable) principer är baserade på principuttrycken. Mer information finns i [avancerade principer](api-management-advanced-policies.md#AdvancedPolicies) och [principuttrycken](api-management-policy-expressions.md).  
  
##  <a name="ProxyPolicies"></a>Principer  
  
-   [Principer för begränsning av åtkomst](api-management-access-restriction-policies.md#AccessRestrictionPolicies)  
  
    -   [Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.  
  
    -   [Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.  
  
    -   [Gränsen anropet frekvensen av nyckeln](api-management-access-restriction-policies.md#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.  
  
    -   [Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.  
  
    -   [Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.  
  
    -   [Kvot för uppsättningen av nyckeln](api-management-access-restriction-policies.md#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.  
  
    -   [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.  
  
-   [Avancerade principer](api-management-advanced-policies.md#AdvancedPolicies)  
  
    -   [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principen uttryck baserat på hello utvärdering av booleskt uttryck.  
  
    -   [Vidarebefordra begäran](api-management-advanced-policies.md#ForwardRequest) -vidarebefordrar hello begäran toohello serverdelstjänst.  
  
    -   [Logga tooEvent hubb](api-management-advanced-policies.md#log-to-eventhub) -skickar meddelanden i hello angivet format tooa meddelandemål definieras av en loggaren entitet.  
  
    -   [Försök](api-management-advanced-policies.md#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt. Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.  
  
    -   [Returnera svar](api-management-advanced-policies.md#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen.  
  
    -   [Skicka förfrågan om enkelriktade](api-management-advanced-policies.md#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.  
  
    -   [Skicka förfrågan](api-management-advanced-policies.md#SendRequest) -skickar en begäran toohello specificerat URL: en.  
  
    -   [Ange variabel](api-management-advanced-policies.md#set-variable) -bevara ett värde i en namngiven kontexten variabel för senare användning.  
  
    -   [Ange metoden](api-management-advanced-policies.md#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.  
  
    -   [Ange statuskoden](api-management-advanced-policies.md#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.  
  
    -   [Spåra](api-management-advanced-policies.md#Trace) -lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.  
  
    -   [Vänta](api-management-advanced-policies.md#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer toocomplete innan du fortsätter.  
  
-   [Autentiseringsprinciper](api-management-authentication-policies.md#AuthenticationPolicies)  
  
    -   [Autentisera med grundläggande](api-management-authentication-policies.md#Basic) -autentisering med en serverdelstjänst som använder grundläggande autentisering.  
  
    -   [Autentisera med klientcertifikatet](api-management-authentication-policies.md#ClientCertificate) -autentisering med en serverdelstjänst som använder klientcertifikat.  
  
-   [Cachelagringsprinciper](api-management-caching-policies.md#CachingPolicies)  
  
    -   [Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.  
  
    -   [Lagra toocache](api-management-caching-policies.md#StoreToCache) -cacheminnen svar enligt toohello angivna konfigurationen av cachen.  
  
    -   [Hämta ett värde från cache](api-management-caching-policies.md#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.  
  
    -   [Lagrar värdet i cache](api-management-caching-policies.md#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.  
  
    -   [Ta bort värdet från cache](api-management-caching-policies.md#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.  
  
-   [Principer mellan domäner](api-management-cross-domain-policies.md#CrossDomainPolicies)  
  
    -   [Tillåt mellan domäner kan anropa](api-management-cross-domain-policies.md#AllowCrossDomainCalls) -gör hello API åtkomlig från Adobe Flash och Microsoft Silverlight webbläsarbaserade klienter.  
  
    -   [CORS](api-management-cross-domain-policies.md#CORS) -lägger till resursdelning för korsande ursprung (CORS) stöder tooan åtgärden eller en API tooallow domänerna anrop från webbläsarbaserade klienter.  
  
    -   [Hanteras JSONP](api-management-cross-domain-policies.md#JSONP) – lägger till JSON med utfyllnad (hanteras JSONP) stöd tooan åtgärden eller en API tooallow domänerna anrop från JavaScript webbläsarbaserade klienter.  
  
-   [Omvandlingsprinciper](api-management-transformation-policies.md#TransformationPolicies)  
  
    -   [Konvertera JSON tooXML](api-management-transformation-policies.md#ConvertJSONtoXML) – konverterar begäran eller svar body från JSON tooXML.  
  
    -   [Konvertera XML-tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) – konverterar begäran eller svar body från XML-tooJSON.  
  
    -   [Sök och Ersätt strängen i brödtexten](api-management-transformation-policies.md#Findandreplacestringinbody) - söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.  
  
    -   [Maskera URL: er i innehåll](api-management-transformation-policies.md#MaskURLSContent) -skriver (masker) länkar hello svar body så att de pekar toohello motsvarande länk via hello gateway.  
  
    -   [Ange serverdelstjänst](api-management-transformation-policies.md#SetBackendService) -ändras hello serverdelstjänst för en inkommande begäran.  
  
    -   [Konfigurera brödtext](api-management-transformation-policies.md#SetBody) -anger hello meddelandetexten för inkommande och utgående förfrågningar.  
  
    -   [Ange HTTP-huvudet](api-management-transformation-policies.md#SetHTTPheader) - tilldelar en värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.  
  
    -   [Ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) – lägger till, ersätter värdet eller tar bort frågesträngparametern för begäran.  
  
    -   [Skriv om URL: en](api-management-transformation-policies.md#RewriteURL) -konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten.  
  
    -   [Transformera XML med hjälp av en XSLT](api-management-transformation-policies.md#XSLTransform) -gäller en XSL-transformation tooXML i hello begäran eller svar.  
  
## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  
