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
# <a name="api-management-caching-policies"></a>Cachelagring API Management-principer
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="CachingPolicies"></a>Principer för cachelagring  
  
-   Svaret cachelagring principer  
  
    -   [Hämta från cache](api-management-caching-policies.md#GetFromCache) -utföra cache Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt.  
  
    -   [Lagra toocache](api-management-caching-policies.md#StoreToCache) - cacheminnen svar enligt toohello angivna konfigurationen.  
  
-   Värdet cachelagring principer  
  
    -   [Hämta ett värde från cache](#GetFromCacheByKey) -hämta ett cachelagrade objekt med nyckel.  
  
    -   [Lagrar värdet i cache](#StoreToCacheByKey) -lagra ett objekt i hello cache av nyckeln.  
  
    -   [Ta bort värdet från cache](#RemoveCacheByKey) -ta bort ett objekt i hello cache av nyckeln.  
  
##  <a name="GetFromCache"></a>Hämta från cache  
 Använd hello `cache-lookup` tooperform principcachen Leta upp och returnera ett giltigt cachelagrade svar när det är tillgängligt. Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod. Svaret cachelagring minskar bandbredden och bearbetningskrav införts på hello backend web server och driftmiljön svarstid, uppfattas av API-konsumenter.  
  
> [!NOTE]
>  Den här principen måste ha en motsvarande [Store toocache](api-management-caching-policies.md#StoreToCache) princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
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
  
### <a name="examples"></a>Exempel  
  
#### <a name="example"></a>Exempel  
  
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
  
#### <a name="example-using-policy-expressions"></a>Exempel på användning av princip uttryck  
 Det här exemplet illustrerar hur tootooconfigure API Management svaret som matchar hello svar cachelagring av hello serverdelstjänst som anges av hello cachevaraktighet säkerhetskopieras tjänstens `Cache-Control` direktiv. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too25:25.  
  
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
  
 Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|cache-sökning|Rotelementet.|Ja|  
|variera av huvud|Börjar cachelagra svar per värde för angivna huvudet, till exempel acceptera Accept-Charset Accept-Encoding, acceptera-språk, auktorisering, förväntat, från värden, If-Match.|Nej|  
|variera-av--frågeparameter|Starta cachelagring svar per värde för angivna Frågeparametrar. Ange en eller flera parametrar. Använd semikolon som avgränsare. Om inget anges används alla Frågeparametrar.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Tillåt privat-svar-cachelagring|När värdet för`true`, tillåter cachelagring av begäranden som innehåller ett Authorization-huvud.|Nej|FALSKT|  
|underordnade-cachelagring-typ|Det här attributet måste anges tooone av hello följande värden.<br /><br /> -Ingen - underordnade cachelagring tillåts inte.<br />-privat - cachelagring underordnade privata.<br />-offentlig - cachelagring privata och delade underordnade.|Nej|Ingen|  
|must-revalidate|När efterföljande cachelagring av det här attributet aktiverar eller inaktiverar hello `must-revalidate` cache-control-direktiv i gateway-svar.|Nej|SANT|  
|variera av utvecklare|Ställa in också`true` toocache svar per developer nyckel.|Nej|FALSKT|  
|variera-av-utvecklare-grupper|Ställa in också`true` toocache svar per användarroll.|Nej|FALSKT|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** API, åtgärden, produkt  
  
##  <a name="StoreToCache"></a>Store toocache  
 Hej `cache-store` princip cacheminnen svar enligt toohello angetts cacheinställningarna. Den här principen kan tillämpas i fall där svar innehåll är statiskt under en viss tidsperiod. Svaret cachelagring minskar bandbredden och bearbetningskrav införts på hello backend web server och driftmiljön svarstid, uppfattas av API-konsumenter.  
  
> [!NOTE]
>  Den här principen måste ha en motsvarande [hämta från cache](api-management-caching-policies.md#GetFromCache) princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<cache-store duration="seconds" />  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="example"></a>Exempel  
  
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
  
#### <a name="example-using-policy-expressions"></a>Exempel på användning av princip uttryck  
 Det här exemplet illustrerar hur tootooconfigure API Management svaret som matchar hello svar cachelagring av hello serverdelstjänst som anges av hello cachevaraktighet säkerhetskopieras tjänstens `Cache-Control` direktiv. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too25:25.  
  
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
  
 Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|cache-lagra|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Varaktighet|Time-to-live av hello cachelagras poster som har angetts i sekunder.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** utgående  
  
-   **Princip för scope:** API, åtgärden, produkt  
  
##  <a name="GetFromCacheByKey"></a>Hämta ett värde från cache  
 Använd hello `cache-lookup-value` princip tooperform cachelagra sökning av nyckeln och cachelagrade returvärde. hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.  
  
> [!NOTE]
>  Den här principen måste ha en motsvarande [lagrar värdet i cache](#StoreToCacheByKey) princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<cache-lookup-value key="cache key value"   
    default-value="value toouse if cache lookup resulted in a miss"   
    variable-name="name of a variable looked up value is assigned to" />  
```  
  
### <a name="example"></a>Exempel  
 Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-lookup-value  
    key="@("userprofile-" + context.Variables["enduserid"])"    
    variable-name="userprofile" />  
  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|cache-sökning-värde|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|standard-värde|Ett värde som ska tilldelas toohello variabeln om hello cache nyckelsökningen resulterade i en miss. Om det här attributet inte anges `null` är tilldelad.|Nej|`null`|  
|key|Cachelagra nyckelvärdet toouse i hello sökning.|Ja|Saknas|  
|variabeln namn|Namnet på hello [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) hello slås upp värdet kommer att tilldelas, om sökning lyckas. Om sökning resulterar i en miss, hello variabeln tilldelas hello värdet för hello `default-value` attribut eller `null`om hello `default-value` attributet utelämnas.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** globala API, åtgärden, produkt  
  
##  <a name="StoreToCacheByKey"></a>Lagrar värdet i cache  
 Hej `cache-store-value` utför cachelagring av nyckeln. hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.  
  
> [!NOTE]
>  Den här principen måste ha en motsvarande [hämta värdet från cache](#GetFromCacheByKey) princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<cache-store-value key="cache key value" value="value toocache" duration="seconds" />  
```  
  
### <a name="example"></a>Exempel  
 Mer information och exempel på den här principen finns [anpassade cachelagring i Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/).  
  
```xml  
<cache-store-value  
    key="@("userprofile-" + context.Variables["enduserid"])"  
    value="@((string)context.Variables["userprofile"])" duration="100000" />  
  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|cache-lagra-värde|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Varaktighet|Värdet cachelagras för hello tillhandahålls duration-värde som anges i sekunder.|Ja|Saknas|  
|key|Cache key hello värdet lagras.|Ja|Saknas|  
|värde|hello värdet toobe cachelagras.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** globala API, åtgärden, produkt  
  
###  <a name="RemoveCacheByKey"></a>Ta bort värdet från cache  
 Hej `cache-remove-value` tar bort en cachelagrade objekt som identifieras av dess nyckel. hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck.  
  
#### <a name="policy-statement"></a>Principframställning  
  
```xml  
  
<cache-remove-value key="cache key value"/>  
  
```  
  
#### <a name="example"></a>Exempel  
  
```xml  
  
<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>  
  
```  
  
#### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|cache-remove-värde|Rotelementet.|Ja|  
  
#### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|key|hello nyckeln för hello tidigare cachelagrats värdet toobe bort från cachen i hello.|Ja|Saknas|  
  
#### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** globala API, åtgärden, produkt  
  

## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  