---
title: aaaAzure API Management avancerade principer | Microsoft Docs
description: "Läs mer om hello avancerade principer som är tillgängliga för användning i Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8a13348b-7856-428f-8e35-9e4273d94323
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 8245e7a4c9d432b7b4d362192e357829fcabad55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-advanced-policies"></a>API Management avancerade principer
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AdvancedPolicies"></a>Avancerade principer  
  
-   [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principrapporter utifrån hello resultat av hello utvärdering av boolesk [uttryck](api-management-policy-expressions.md).  
  
-   [Vidarebefordra begäran](#ForwardRequest) -vidarebefordrar hello begäran toohello serverdelstjänst.

-   [Begränsa samtidighet](#LimitConcurrency) -förhindrar omslutna principer från att köras med mer än hello angivet antal begäranden i taget.
  
-   [Logga tooEvent hubb](#log-to-eventhub) -skickar meddelanden i hello angetts format tooan Event Hub definieras av en loggaren entitet. 

-   [Mock svar](#mock-response) -avbryts körningen i pipeline och returnerar svaret mocked direkt toohello anroparen.
  
-   [Försök](#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt. Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.  
  
-   [Returnera svar](#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen. 
  
-   [Skicka förfrågan om enkelriktade](#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.  
  
-   [Skicka förfrågan](#SendRequest) -skickar en begäran toohello specificerat URL: en.  

-   [Ange HTTP-proxy](#SetHttpProxy) -tillåter tooroute vidarebefordrade begäranden via en HTTP-proxy.  

-   [Ange metoden](#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.  
  
-   [Ange statuskoden](#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.  
  
-   [Ange variabel](api-management-advanced-policies.md#set-variable) -kvarstår ett värde i en namngiven [kontexten](api-management-policy-expressions.md#ContextVariables) variabel för senare användning.  

-   [Spåra](#Trace) -lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.  
  
-   [Vänta](#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer toocomplete innan du fortsätter.  
  
##  <a name="choose"></a>Kontrollflöde  
 Hej `choose` principen gäller omslutna princip uttryck baserat på hello resultatet av bedömning av booleska uttryck, liknande tooan if-then-else eller växel konstruera i ett programmeringsspråk.  
  
###  <a name="ChoosePolicyStatement"></a>Principframställning  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements toobe applied if hello above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements toobe applied if none of hello above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 hello princip för åtkomstkontroll flödet måste innehålla minst ett `<when/>` element. Hej `<otherwise/>` element är valfria. Villkoren i `<when/>` element utvärderas i ordning efter deras utseende i hello princip. Principen instruktion(er) omgiven hello först `<when/>` element med villkoret attribut är lika med `true` tillämpas. Principer för omgiven hello `<otherwise/>` element, i förekommande fall, kommer att tillämpas om alla av hello `<when/>` elementattribut för villkoret är `false`.  
  
### <a name="examples"></a>Exempel  
  
####  <a name="ChooseExample"></a>Exempel  
 hello exemplet nedan visar en [ange variabel](api-management-advanced-policies.md#set-variable) principen och två principer för åtkomstkontroll flödet.  
  
 hello Ange variabeln princip är i hello inkommande avsnittet och skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som anges tootrue om hello `User-Agent` begäran huvudet innehåller hello text `iPad` eller `iPhone`.  
  
 hello första kontrollen flödet principen är i hello inkommande avsnittet och villkorligt gäller en av två [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) principer beroende på hello värdet för hello `isMobile` kontexten variabeln.  
  
 hello andra kontrollen flödet princip är utgående under hello och villkorligt gäller hello [konvertera XML-tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) principen när `isMobile` har angetts för`true`.  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a>Exempel  
 Det här exemplet illustrerar hur tooperform innehållsfiltrering genom att ta bort dataelement från hello svar togs emot från hello backend-tjänsten när du använder hello `Starter` produkten. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too34:30. Starta en översikt över vid 31:50 toosee [hello mörkt Sky prognos API](https://developer.forecast.io/) används för den här demon.  
  
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
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Välj|Rotelementet.|Ja|  
|När|Hej villkoret toouse för hello `if` eller `ifelse` delar av hello `choose` princip. Om hello `choose` princip har flera `when` avsnitt, de utvärderas i turordning. En gång hello `condition` av ett när utvärderar element för`true`, ingen ytterligare `when` villkor utvärderas.|Ja|  
|Annars|Innehåller hello princip fragment toobe används om inget av hello `when` villkor utvärdera för`true`.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|  
|---------------|-----------------|--------------|  
|villkor = ”booleskt uttryck &#124; Booleskt konstant ”|hello booleskt uttryck eller konstant tooevaluated när hello som innehåller `when` Principframställning utvärderas.|Ja|  
  
###  <a name="ChooseUsage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="ForwardRequest"></a>Vidarebefordra begäran  
 Hej `forward-request` princip vidarebefordrar hello inkommande begäran toohello serverdelstjänst anges i begäran hello [kontexten](api-management-policy-expressions.md#ContextVariables). hello backend-tjänstens URL anges i hello API [inställningar](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) och kan ändras med hjälp av hello [ange serverdelstjänst](api-management-transformation-policies.md) princip.  
  
> [!NOTE]
>  Ta bort den här grupprincipresultat i hello begäran inte vidarebefordras toohello backend-tjänsten och hello principer utgående under hello utvärderas direkt vid hello slutförande av hello principer i hello inkommande avsnitt.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="example"></a>Exempel  
 hello vidarebefordras följande API säkerhetsnivå för alla toohello serverdelstjänst med en timeout på 60 sekunder.  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Exempel  
 Den här åtgärden säkerhetsnivå för använder hello `base` elementet tooinherit hello backend princip från hello överordnade API-nivå omfattningen.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Exempel  
 Den här åtgärden säkerhetsnivå för uttryckligen vidarebefordrar alla begäranden toohello serverdelstjänst med en tidsgräns på 120 och ärver inte hello överordnade API-nivå backend-princip.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note hello absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Exempel  
 Den här åtgärden säkerhetsnivå för inte vidarebefordrar begäranden toohello serverdelstjänst.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding toobackend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|vidarebefordra begäran|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|timeout = ”heltal”|Det går inte att hello timeout-intervall i sekunder innan hello anropet toohello serverdelstjänst.|Nej|Ingen tidsgräns|  
|Följ omdirigeringar = ”true &#124; FALSE ”|Anger om omdirigeringar från hello serverdelstjänst är följt av hello gateway eller returnerade toohello anroparen.|Nej|FALSKT|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** backend  
  
-   **Princip för scope:** alla scope  
  
##  <a name="LimitConcurrency"></a>Gränsen för samtidighet  
 Hej `limit-concurrency` princip förhindrar att omslutna principer körning av fler än det angivna antalet förfrågningar hello vid en given tidpunkt. Vid överstiger tröskelvärdet hello läggs nya begäranden tooa kö tills hello maximal Kölängd uppnås. När kön uttömning misslyckas nya begäranden omedelbart.
  
###  <a name="LimitConcurrencyStatement"></a>Principframställning  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Exempel  
  
####  <a name="ChooseExample"></a>Exempel  
 hello visar exemplet nedan hur toolimit antal begäranden vidarebefordras tooa backend baserat på hello värde för en variabel i kontexten.
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3" timeout="60">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|    
|gränsen för samtidighet|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|--------------|  
|key|En sträng. Uttryck tillåts. Anger hello samtidighet scope. Kan delas av flera principer.|Ja|Saknas|  
|Max antal|Ett heltal. Anger maximalt antal begäranden som tillåts tooenter hello princip.|Ja|Saknas|  
|timeout|Ett heltal. Uttryck tillåts. Anger hello antalet sekunder som en begäran ska vänta tooenter ett scope innan åtgärden misslyckas med ”403 för många begäranden”|Nej|Infinity|  
|Max Kölängd|Ett heltal. Uttryck tillåts. Anger hello maximala längd. Inkommande begäranden försök tooenter denna policy kommer att avslutas med ”403 för många begäranden” omedelbart när hello kön är slut.|Nej|Infinity|  
  
###  <a name="ChooseUsage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  

##  <a name="log-to-eventhub"></a>Logga tooEvent Hub  
 Hej `log-to-eventhub` princip skickar meddelanden i hello angetts format tooan Event Hub definieras av en loggaren entitet. Som namnet antyder används hello principen för att spara valda begäran eller svar omständighetsinformation för analys online eller offline.  
  
> [!NOTE]
>  Stegvisa instruktioner om hur du konfigurerar en händelsehubb och loggning av händelser, se [hur toolog API Management händelser med Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Exempel  
 Valfri sträng kan användas som hello värdet toobe loggas i Händelsehubbar. I det här exemplet hello datum och tid tjänstnamn för distribution, förfrågnings-id, ip-adress och åtgärdsnamn för alla inkommande samtal är loggade toohello händelsehubb loggaren registrerats med hello `contoso-logger` id.  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|loggen till eventhub|Rotelementet. hello-värdet för det här elementet är hello sträng toolog tooyour händelsehubb.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|  
|---------------|-----------------|--------------|  
|loggaren-id|hello-id för hello loggaren registrerats API Management-tjänsten.|Ja|  
|partitions-id|Anger hello index för hello partition där meddelanden skickas.|Valfri. Det här attributet kan inte användas om `partition-key` används.|  
|Partitionsnyckeln|Anger hello-värde som används för tilldelning av partitionen när meddelanden skickas.|Valfri. Det här attributet kan inte användas om `partition-id` används.|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  

##  <a name="mock-response"></a>Fingerad svar  
Hej `mock-response`, som hello namn innebär, är används toomock API: er och åtgärder. Den normala pipelinekörningen avbryter och returnerar en mocked svar toohello anropare. hello princip försöker alltid tooreturn svar för högsta återgivning. Den föredrar svar innehåll exempel, när det finns tillgängligt. Den genererar exempel svar från scheman, när scheman har angetts och exempel finns inte. Om varken exempel eller scheman hittas returneras svar med inget innehåll.
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Exempel  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|mock-svar|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|--------------|  
|statuskod|Anger Svarets statuskod och använda tooselect motsvarande exempel eller schema.|Nej|200|  
|innehållstyp|Anger `Content-Type` huvudvärde svar och använda tooselect motsvarande exempel eller schema.|Nej|Ingen|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående, vid fel  
  
-   **Princip för scope:** alla scope

##  <a name="Retry"></a>Försök igen  
 Hej `retry` princip körs en gång dess underordnade principer och sedan försöker körningen tills hello försök `condition` blir `false` eller försök `count` är slut.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a>Exempel  
 I hello försöks följande exempel begäran forewarding in tooten tider med exponentiell försök algoritm. Eftersom `first-fast-retry` anges toofalse, alla nya försök är ämne toohello exponsntial försök algoritm.  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|retry|Rotelementet. Kan innehålla andra principer som dess underordnade element.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|Villkor|En boolesk literal eller [uttryck](api-management-policy-expressions.md) anger om återförsök ska stoppas (`false`) eller fortsatte (`true`).|Ja|Saknas|  
|Antal|Ett positivt tal som anger hello maximalt antal återförsök tooattempt.|Ja|Saknas|  
|interval|Ett positivt tal i sekunder att ange hello intervall mellan försök hello.|Ja|Saknas|  
|Max-intervall|Ett positivt tal i sekunder att ange hello maximalt intervall mellan försök hello. Det är används tooimplement en algoritm exponentiell försök igen.|Nej|Saknas|  
|delta|Ett positivt tal i sekunder att ange hello vänta intervall ökning. Det är används tooimplement hello linjär och exponentiella försök algoritmer.|Nej|Saknas|  
|första-fast-återförsök|Om anges för `true` , hello första nytt försök utförs omedelbart.|Nej|`false`|  
  
> [!NOTE]
>  När endast hello `interval` anges **fast** intervall för nya försök utförs.  
>  När endast hello `interval` och `delta` har angetts en **linjär** intervall försök algoritmen används, där väntetiden mellan försöken är beräknade bl.a hello följande formel - `interval + (count - 1)*delta`.  
>  När hello `interval`, `max-interval` och `delta` anges, **exponentiell** intervall försök algoritmen används där hello väntetiden mellan hello försök ökar exponentiellt från hello värdet för `interval`toohello värdet `max-interval` enligt följande toohello forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Observera att ärvs underordnade principbegränsningar för användning av den här principen.  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="ReturnResponse"></a>Returnera svar  
 Hej `return-response` princip avbryter pipelinekörningen och returnerar ett standardvärde eller anpassade svar toohello anroparen. Standard-svaret är `200 OK` med ingen brödtext. Anpassade svar kan anges via en kontext variabel eller princip-instruktioner. När både tillhandahålls ändras hello svaret som ingår i hello kontexten variabeln vid hello principrapporter innan de returneras toohello anroparen.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|returnera svar|Rotelementet.|Ja|  
|set-huvud|En [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip-satsen.|Nej|  
|Ange brödtext|En [set brödtext](api-management-transformation-policies.md#SetBody) princip-satsen.|Nej|  
|Ange status|En [Ange status](api-management-advanced-policies.md#SetStatus) princip-satsen.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|  
|---------------|-----------------|--------------|  
|svaret variabelnamn|hello namnet på hello kontexten variabel refereras från, till exempel en uppströms [-begäran om att skicka](api-management-advanced-policies.md#SendRequest) principen och som innehåller en `Response` objekt|Valfri.|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="SendOneWayRequest"></a>Skicka förfrågan om ett sätt  
 Hej `send-one-way-request` princip skickar hello tillhandahålls begäran toohello specificerat URL: en utan att vänta på ett svar.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Exempel  
 Exempel principen visar ett exempel på hur hello `send-one-way-request` princip toosend ett meddelande tooa Slack chatt rum om hello HTTP-svarskoden är större än eller lika med too500. Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|en-sätt-begäran om att skicka|Rotelementet.|Ja|  
|URL: en|hello-URL för hello-begäran.|Inga om läge = kopian. Annars Ja.|  
|Metoden|hello HTTP-metoden för hello-begäran.|Inga om läge = kopian. Annars Ja.|  
|sidhuvud|Huvudet i begäran. Använda flera huvud-element för flera huvuden för begäran.|Nej|  
|Brödtext|Hej begärandetexten.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|mode = ”sträng”|Anger om detta är en ny begäran eller en kopia av hello aktuella begäran. I utgående läge, läge = kopiera initieras inte hello frågans brödtext.|Nej|Ny|  
|namn|Anger hello hello huvud toobe mängden.|Ja|Saknas|  
|Det finns åtgärd|Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts. Det här attributet måste ha något av följande värden hello.<br /><br /> -åsidosätt - ersätter hello värdet för befintliga hello-huvud.<br />-skip - ersätter inte hello befintliga huvudvärde.<br />-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.<br />-delete - tar bort hello huvudet från hello-begäran.<br /><br /> När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.|Nej|åsidosätt|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="SendRequest"></a>Skicka förfrågan  
 Hej `send-request` princip skickar hello tillhandahålls begäran toohello angiven URL, väntar på längre än hello ange timeout-värde.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Exempel  
 Det här exemplet visar enkelriktade tooverify en referens-token med en server för auktorisering. Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request tooToken Server toovalidate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Skicka begäran|Rotelementet.|Ja|  
|URL: en|hello-URL för hello-begäran.|Inga om läge = kopian. Annars Ja.|  
|Metoden|hello HTTP-metoden för hello-begäran.|Inga om läge = kopian. Annars Ja.|  
|sidhuvud|Huvudet i begäran. Använda flera huvud-element för flera huvuden för begäran.|Nej|  
|Brödtext|Hej begärandetexten.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|mode = ”sträng”|Anger om detta är en ny begäran eller en kopia av hello aktuella begäran. I utgående läge, läge = kopiera initieras inte hello frågans brödtext.|Nej|Ny|  
|svaret variabelnamn = ”sträng”|Om den inte finns `context.Response` används.|Nej|Saknas|  
|timeout = ”heltal”|hello timeout-intervall i sekunder innan anropet hello toohello URL misslyckas.|Nej|60|  
|Ignorera fel|Om true och hello-begäran resulterar i ett fel:<br /><br /> – Om svaret variabelnamn angavs innehåller ett null-värde.<br />– Om svaret variabelnamn inte har angetts, kontext. Begäran kommer inte att uppdateras.|Nej|FALSKT|  
|namn|Anger hello hello huvud toobe mängden.|Ja|Saknas|  
|Det finns åtgärd|Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts. Det här attributet måste ha något av följande värden hello.<br /><br /> -åsidosätt - ersätter hello värdet för befintliga hello-huvud.<br />-skip - ersätter inte hello befintliga huvudvärde.<br />-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.<br />-delete - tar bort hello huvudet från hello-begäran.<br /><br /> När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.|Nej|åsidosätt|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="SetHttpProxy"></a>Ange HTTP-proxy  
 Hej `proxy` principen kan du tooroute begäranden vidarebefordras toobackends via en HTTP-proxy. Endast HTTP (HTTPS inte) stöds mellan hello gateway och hello proxy. Grundläggande och NTLM-autentisering.
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Exempel  
Observera hello användning av [egenskaper](api-management-howto-properties.md) som värden för hello användarnamn och lösenord tooavoid lagra känslig information i hello dokument.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Proxy|Rotelementet|Ja|  

### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|URL = ”sträng”|Proxy-URL i hello form av http://host:port.|Ja|Saknas|  
|UserName = ”sträng”|Användarnamnet toobe används för autentisering med hello proxy.|Nej|Saknas|  
|lösenord = ”sträng”|Lösenordet toobe används för autentisering med hello proxy.|Nej|Saknas|  

### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** alla scope  

##  <a name="SetRequestMethod"></a>Set-metod för begäran  
 Hej `set-method` principen kan du toochange hello HTTP-frågemetoden för en begäran.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Exempel  
 Det här exemplet princip som använder hello `set-method` princip visas ett exempel på skickas ett meddelande tooa Slack chatt-rum om hello HTTP-svarskoden är större än eller lika med too500. Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|set-metod|Rotelementet. hello anger hello elementet hello HTTP-metod.|Ja|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, vid fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="SetStatus"></a>Ange statuskoden  
 Hej `set-status` principen anger hello HTTP-status kod toohello angivet värde.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Exempel  
 Det här exemplet illustrerar hur tooreturn 401 svar om hello autentiseringstoken är ogiltig. Mer information finns i [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Ange status|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|kod = ”heltal”|hello HTTP-status koden tooreturn.|Ja|Saknas|  
|Orsak = ”sträng”|En beskrivning av hello orsak för att returnera hello-statuskod.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** utgående, backend fel  
  
-   **Princip för scope:** alla scope  

##  <a name="set-variable"></a>Ange variabel  
 Hej `set-variable` princip deklarerar en [kontexten](api-management-policy-expressions.md#ContextVariables) variabeln och tilldelar den ett värde som angetts via en [uttryck](api-management-policy-expressions.md) eller en teckensträng. Om hello uttryck innehåller ett litteralvärde som kommer att konverteras tooa sträng och hello Värdets typ hello blir `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a>Principframställning  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a>Exempel  
 hello exemplet nedan visar en uppsättning variabeln princip i hello inkommande avsnitt. Ange variabeln principen skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som anges tootrue om hello `User-Agent` begäran huvudet innehåller hello text `iPad` eller `iPhone`.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Ange variabel|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|  
|---------------|-----------------|--------------|  
|namn|hello namnet på hello-variabeln.|Ja|  
|värde|hello värdet för hello variabel. Detta kan vara ett uttryck eller ett litteralvärde.|Ja|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
###  <a name="set-variableAllowedTypes"></a>Tillåtna typer  
 Uttryck som används i hello `set-variable` princip måste returnera hello följande grundläggande typer.  
  
-   System.Boolean  
  
-   System.SByte  
  
-   System.Byte  
  
-   System.UInt16  
  
-   System.UInt32  
  
-   System.UInt64  
  
-   System.Int16  
  
-   System.Int32  
  
-   System.Int64  
  
-   System.Decimal  
  
-   System.Single  
  
-   System.Double  
  
-   System.Guid  
  
-   System.String  
  
-   System.Char  
  
-   System.DateTime  
  
-   System.TimeSpan  
  
-   System.Byte?  
  
-   System.UInt16?  
  
-   System.UInt32?  
  
-   System.UInt64?  
  
-   System.Int16?  
  
-   System.Int32?  
  
-   System.Int64?  
  
-   System.Decimal?  
  
-   System.Single?  
  
-   System.Double?  
  
-   System.Guid?  
  
-   System.String?  
  
-   System.Char?  
  
-   System.DateTime?  

##  <a name="Trace"></a>Spårning  
 Hej `trace` princip lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata. hello princip körs endast när spårning utlöses, d.v.s. `Ocp-Apim-Trace` huvud är närvarande och ange för`true` och `Ocp-Apim-Subscription-Key` begärandehuvudet finns och innehåller en giltig nyckel som är associerad med hello-administratörskonto.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Spårning|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|Källa|Sträng literal meningsfulla toohello trace viewer och att ange hello källan för hello-meddelande.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** alla scope  
  
##  <a name="Wait"></a>Vänta  
 Hej `wait` principen Kör sina direkt underordnade principer parallellt och väntar på alla eller en av dess omedelbara underordnade principer toocomplete innan den har slutförts. hello vänta princip kan ha sina direkt underordnade principer [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), och [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Exempel  
 I följande exempel hello finns två `choose` principer som direkt underordnade principer för hello `wait` princip. Var och en av dessa `choose` principer körs parallellt. Varje `choose` princip försöker tooretrieve cachelagrade värde. Om det finns en cache-miss, kallas tooprovide hello värde med en serverdelstjänst. I det här exemplet hello `wait` principen inte slutföras förrän alla dess omedelbara underordnade principer slutföra eftersom hello `for` attribut har angetts för`all`.   I det här exemplet hello kontexten variabler (`execute-branch-one`, `value-one`, `execute-branch-two`, och `value-two`) deklareras utanför hello omfattning exempel principen.  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Vänta|Rotelementet. Kan innehålla som underordnade element endast `send-request`, `cache-lookup-value`, och `choose` principer.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|för|Avgör om hello `wait` princip väntar på att alla direkt underordnade principer toobe slutförts eller bara en. Tillåtna värden är:<br /><br /> -   `all`-Vänta tills alla direkt underordnade principer toocomplete<br />-alla - vänta tills alla direkt underordnade princip toocomplete. När hello första omedelbara underordnade principen är klar hello `wait` princip har slutförts och körningen av alla andra principer för omedelbart underordnade avslutas.|Nej|Alla|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend  
  
-   **Princip för scope:**alla scope  
  
## <a name="next-steps"></a>Nästa steg
Arbeta med principer, Läs mer:
-   [Principer för i API-hantering](api-management-howto-policies.md) 
-   [Principuttryck](api-management-policy-expressions.md)
