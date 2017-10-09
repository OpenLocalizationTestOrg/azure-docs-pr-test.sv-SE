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
# <a name="api-management-transformation-policies"></a>API Management-principer för anspråksomvandling
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="TransformationPolicies"></a>Principer för anspråksomvandling  
  
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
  
##  <a name="ConvertJSONtoXML"></a>Konvertera JSON tooXML  
 Hej `json-to-xml` princip konverterar brödtext begäran eller ett svar från JSON tooXML.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<json-to-xml apply="always | content-type-json" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Exempel  
  
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
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|JSON-xml|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|tillämpa|hello-attributet måste anges tooone av hello följande värden.<br /><br /> -alltid - alltid gäller konvertering.<br />-innehåll typ-json - Konvertera endast om response Content-Type-huvud anger förekomsten av JSON.|Ja|Saknas|  
|Överväg att acceptera-huvud|hello-attributet måste anges tooone av hello följande värden.<br /><br /> tillämpa - true - konvertering om JSON begärs i begäran Accept-huvud.<br />-alltid gäller false - konvertering.|Nej|SANT|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående, vid fel  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="ConvertXMLtoJSON"></a>Konvertera XML-tooJSON  
 Hej `xml-to-json` princip konverterar brödtext begäran eller ett svar från XML-tooJSON. Den här principen kan vara används toomodernize API: er baserat på backend endast XML-webbtjänster.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<xml-to-json kind="javascript-friendly | direct" apply="always | content-type-xml" consider-accept-header="true | false"/>  
```  
  
### <a name="example"></a>Exempel  
  
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
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|XML-json|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|typ|hello-attributet måste anges tooone av hello följande värden.<br /><br /> -javascript anpassad - hello konverteras JSON har ett eget tooJavaScript utvecklare för formuläret.<br />-direkt - visar hello konverterade JSON hello ursprungliga XML-dokumentets struktur.|Ja|Saknas|  
|tillämpa|hello-attributet måste anges tooone av hello följande värden.<br /><br /> -alltid - konvertera alltid.<br />-innehåll-typ-xml - Konvertera endast om response Content-Type-huvud anger förekomsten av XML.|Ja|Saknas|  
|Överväg att acceptera-huvud|hello-attributet måste anges tooone av hello följande värden.<br /><br /> tillämpa - true - konvertering om begärs XML i begäran Accept-huvud.<br />-alltid gäller false - konvertering.|Nej|SANT|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående, vid fel  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="Findandreplacestringinbody"></a>Sök och Ersätt strängen i brödtext  
 Hej `find-and-replace` princip söker efter en begäran eller ett svar delsträng och ersätter den med en annan understräng.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<find-and-replace from="what tooreplace" to="replacement" />  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<find-and-replace from="notebook" to="laptop" />  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Sök och Ersätt|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Från|hello sträng toosearch för.|Ja|Saknas|  
|till|hello ersättningssträngen. Ange ersättning sträng tooremove hello Sök nollängd.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="MaskURLSContent"></a>Mask-URL: er i innehåll  
 Hej `redirect-content-urls` princip skriver (masker) länkar i hello svarstexten så att de pekar toohello motsvarande länk via hello gateway. Använd i hello utgående avsnittet toore Skriv svaret brödtext länkar toomake dem punkt toohello gateway. Använd i hello inkommande avsnittet för en motsatt effekt.  
  
> [!NOTE]
>  Den här principen ändras inte alla värden i huvudet som `Location` huvuden. toochange huvudvärden använda hello [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<redirect-content-urls />  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|omdirigerings-innehåll-URL: er|Rotelementet.|Ja|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="SetBackendService"></a>Ange backend-tjänst  
 Använd hello `set-backend-service` princip tooredirect en inkommande begäran tooa olika backend än hello som anges i inställningarna för hello API för åtgärden. Den här principen ändras hello backend service bas-URL för hello inkommande begäran toohello som har angetts i hello princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-backend-service base-url="base URL of hello backend service" />  
```  
  
### <a name="example"></a>Exempel  
  
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
I det här exemplet hello dirigerar ange backend service princip förfrågningar baserat på hello versionsvärdet som överförts hello frågan sträng tooa olika serverdelstjänst än hello en anges i hello API.
  
Ursprungligen är hello grundläggande Webbadressen för backend-tjänsten härledd från hello API-inställningarna. Så hello URL-begäran `https://contoso.azure-api.net/api/partners/15?version=2013-05&subscription-key=abcdef` blir `http://contoso.com/api/10.4/partners/15?version=2013-05&subscription-key=abcdef` där `http://contoso.com/api/10.4/` är hello backend tjänst-URL som anges i inställningarna för hello API.  
  
När hello [< Välj\> ](api-management-advanced-policies.md#choose) Principframställning tillämpas hello backend service bas-URL kan ändra igen antingen för`http://contoso.com/api/8.2` eller `http://contoso.com/api/9.1`, beroende på hello värdet för Frågeparametern för hello version begäran. Till exempel om hello är värdet `"2013-15"` hello slutliga begäran URL blir `http://contoso.com/api/8.2/partners/15?version=2013-05&subscription-key=abcdef`.  
  
Om ytterligare transformering av hello-begäran är önskade, andra [principer för Anspråksomvandling](api-management-transformation-policies.md#TransformationPolicies) kan användas. Till exempel tooremove hello version fråga parametern nu när hello begäran håller på att dirigeras tooa version specifika serverdel hello [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) policy kan vara används tooremove hello nu redundant versionsattribut.  
  
### <a name="example"></a>Exempel  
  
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
I det här exemplet hello princip vägar hello begäran tooa service fabric serverdel använder hello userId frågesträngen som hello partitionsnyckel hello primär replik av hello partitionen.  

### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|set-backend-tjänst|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|bas-url|Nya backend service bas-URL.|Nej|Saknas|  
|backend-id|Identifierare för hello backend tooroute till.|Nej|Saknas|  
|SA partitionsnyckel|Gäller endast när hello serverdelen är en tjänst för Service Fabric och anges med backend-id. Använda tooresolve en specifik partition från hello namnmatchningstjänst.|Nej|Saknas|  
|SA-replik-typ|Gäller endast när hello serverdelen är en tjänst för Service Fabric och anges med backend-id. Styr om hello begäran ska gå toohello primär eller sekundär replik av en partition. |Nej|Saknas|    
|SA Lös villkor|Gäller endast när hello serverdelen är en tjänst för Service Fabric. Villkoret identifierar har om hello anropa tooService Fabric backend toobe upprepas med nya lösningar.|Nej|Saknas|    
|SA-tjänsten-instansnamn|Gäller endast när hello serverdelen är en tjänst för Service Fabric. Tillåter toochange instanser av tjänsten vid körning. |Nej|Saknas|    

### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, backend  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="SetBody"></a>Ange brödtext  
 Använd hello `set-body` princip tooset hello meddelandetexten för inkommande och utgående förfrågningar. tooaccess hello meddelandetexten som du kan använda hello `context.Request.Body` egenskap eller hello `context.Response.Body`, beroende på om hello princip i hello inkommande eller utgående avsnitt.  
  
> [!IMPORTANT]
>  Observera att som standard när du använder hello meddelande brödtext med `context.Request.Body` eller `context.Response.Body`, ursprungliga hälsningsmeddelande brödtext går förlorad och måste anges genom att returnera hello brödtext tillbaka i hello uttryck. toopreserve hello brödtext innehåll, ange hello `preserveContent` parameter för`true` vid åtkomst till hello-meddelande. Om `preserveContent` har angetts för`true` och en annan text returneras av hello uttryck hello returnerade brödtext används.  
>   
>  Observera följande överväganden när du använder hello hello `set-body` princip.  
>   
>  -   Om du använder hello `set-body` princip tooreturn en ny eller uppdaterad text som du inte behöver tooset `preserveContent` för`true` eftersom tillhandahåller du uttryckligen hello nya innehållet i meddelandetexten.  
> -   Bevara hello innehållet i ett svar i hello inkommande pipeline passar inte eftersom det inte finns något svar ännu.  
> -   Bevara hello innehållet i en begäran i hello utgående pipeline vara inte meningsfullt eftersom hello begäran har redan skickats toohello backend nu.  
> -   Om den här principen används när det finns ingen brödtext, genereras till exempel i en inkommande GET ett undantag.  
  
 Mer information finns i hello `context.Request.Body`, `context.Response.Body`, och hello `IMessage` avsnitt i hello [kontexten variabeln](api-management-policy-expressions.md#ContextVariables) tabell.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-body>new body value as text</set-body>  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="literal-text-example"></a>Citat-exempel  
  
```xml  
<set-body>Hello world!</set-body>  
```  
  
#### <a name="example-accessing-hello-body-as-a-string-note-that-we-are-preserving-hello-original-request-body-so-that-we-can-access-it-later-in-hello-pipeline"></a>Exempel åtkomst till hello brödtext som en sträng. Observera att vi bevarar hello ursprungliga begärandetexten så att vi kan komma åt den senare i hello pipeline.
  
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
  
#### <a name="example-accessing-hello-body-as-a-jobject-note-that-since-we-are-not-reserving-hello-original-request-body-accesing-it-later-in-hello-pipeline-will-result-in-an-exception"></a>Exempel åtkomst till hello brödtext som en JObject. Observera att eftersom vi inte reserverar hello ursprungliga begärantext öppnar det senare i hello pipeline kommer att resultera i ett undantag.  
  
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
  
#### <a name="filter-response-based-on-product"></a>Filtrera respons baserat på produkt  
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

### <a name="using-liquid-templates-with-set-body"></a>Med hjälp av flytande mallar med set-brödtext 
Hej `set-body` policy kan vara konfigurerade toouse hello [flytande](https://shopify.github.io/liquid/basics/introduction/) templating språk tootransfom hello brödtexten i en begäran eller ett svar. Detta kan vara mycket effektivt om du behöver toocompletely ändra form hello format för meddelandet.

> [!IMPORTANT]
> Hej implementering av flytande som används i hello `set-body` principen är konfigurerad i 'C# mode'. Detta är särskilt viktigt när du gör saker, till exempel filtrering. Exempel med hjälp av ett filter kräver hello Pascal skiftläge och C# datum formatering t.ex.:
>
> {{body.foo.startDateTime| Datum: ”yyyyMMddTHH:mm:ddZ”}}

> [!IMPORTANT]
> I ordning toocorrectly bind tooan XML-meddelandetext med hello flytande mall, använda en `set-header` princip tooset Content-Type tooeither application/xml, text/xml (eller någon typ som slutar med + xml); för JSON-meddelandetext det måste vara application/json, text/json (eller någon typ avslutas med + json).

#### <a name="convert-json-toosoap-using-a-liquid-template"></a>Konvertera JSON tooSOAP med en flytande mall
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

#### <a name="tranform-json-using-a-liquid-template"></a>Tranform JSON med en flytande mall
```xml
{
"order": {
    "id": "{{body.customer.purchase.identifier}}",
    "summary": "{{body.customer.purchase.orderShortDesc}}"
    }
}
```

### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Ange brödtext|Rotelementet. Innehåller hello brödtext eller ett uttryck som returnerar en brödtext.|Ja|  

### <a name="properties"></a>Egenskaper  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|mall|Använda toochange hello templating läge som hello ange brödtext princip körs i. För närvarande är hello stöds endast värdet:<br /><br />-flytande - hello ange brödtext princip kommer att använda hello flytande templating motorn |Nej|flytande|  

För att komma åt information om hello förfrågan och svar, binda hello flytande mallen tooa context-objektet med hello följande egenskaper: <br />
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



### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="SetHTTPheader"></a>Ange HTTP-huvud  
 Hej `set-header` principen tilldelas värdet tooan befintliga svar och/eller huvudet i begäran eller lägger till nya svar och/eller begäran-huvud.  
  
 Infogar en lista över HTTP-huvuden i ett HTTP-meddelande. När den placeras i en inkommande pipeline, anger den här principen hello HTTP-huvuden för hello-begäran som skickas toohello Måltjänsten. När den placeras i en utgående pipeline, anger den här principen hello HTTP-huvuden för hello svar skickas toohello gateway-klienten.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-header name="header name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple headers with hello same name add additional value elements-->  
</set-header>  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="example"></a>Exempel  
  
```xml  
<set-header name="some header name" exists-action="override">  
    <value>20</value>   
</set-header>  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Vidarebefordra kontexten information toohello serverdelstjänst  
 Det här exemplet visar hur tooapply princip hello API-nivå toosupply kontexten information toohello serverdelstjänst. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too10:30. Det finns en demonstration av anropa en funktion i hello developer-portalen där du kan se hello princip på arbetet vid 12:10.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward some context information, user id and hello region hello gateway is hosted in, toohello backend service for logging or evaluation -->  
<set-header name="x-request-context-data" exists-action="override">  
  <value>@(context.User.Id)</value>  
  <value>@(context.Deployment.Region)</value>  
</set-header>  
```  
  
 Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|set-huvud|Rotelementet.|Ja|  
|värde|Anger hello värdet i hello huvud toobe uppsättning. Flera huvuden med hello samma namn lägga till ytterligare `value` element.|Ja|  
  
### <a name="properties"></a>Egenskaper  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Det finns åtgärd|Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts. Det här attributet måste ha något av följande värden hello.<br /><br /> -åsidosätt - ersätter hello värdet för befintliga hello-huvud.<br />-skip - ersätter inte hello befintliga huvudvärde.<br />-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.<br />-delete - tar bort hello huvudet från hello-begäran.<br /><br /> När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.|Nej|åsidosätt|  
|namn|Anger namnet på hello huvud toobe uppsättning.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående backend fel  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="SetQueryStringParameter"></a>Ange frågesträngparametern  
 Hej `set-query-parameter` principen läggs till, ersätter värde, eller tar bort begära frågesträngparametern. Kan vara används toopass frågeparametrar förväntades av hello serverdelstjänst som är valfria eller aldrig finns i hello-begäran.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<set-query-parameter name="param name" exists-action="override | skip | append | delete">  
    <value>value</value> <!--for multiple parameters with hello same name add additional value elements-->  
</set-query-parameter>  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="example"></a>Exempel  
  
```xml  
  
<set-query-parameter>  
  <parameter name="api-key" exists-action="skip">  
    <value>12345678901</value>  
  </parameter>  
  <!-- for multiple parameters with hello same name add additional value elements -->  
</set-query-parameter>  
  
```  
  
#### <a name="forward-context-information-toohello-backend-service"></a>Vidarebefordra kontexten information toohello serverdelstjänst  
 Det här exemplet visar hur tooapply princip hello API-nivå toosupply kontexten information toohello serverdelstjänst. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too10:30. Det finns en demonstration av anropa en funktion i hello developer-portalen där du kan se hello princip på arbetet vid 12:10.  
  
```xml  
<!-- Copy this snippet into hello inbound element tooforward a piece of context, product name in this example, toohello backend service for logging or evaluation -->  
<set-query-parameter name="x-product-name" exists-action="override">  
  <value>@(context.Product.Name)</value>  
</set-query-parameter>  
  
```  
  
 Mer information finns i [principuttrycken](api-management-policy-expressions.md) och [kontexten variabeln](api-management-policy-expressions.md#ContextVariables).  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Ange frågeparameter|Rotelementet.|Ja|  
|värde|Anger hello värdet i hello frågan toobe parameteruppsättning. För flera frågeparametrar med hello samma namn lägga till ytterligare `value` element.|Ja|  
  
### <a name="properties"></a>Egenskaper  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Det finns åtgärd|Anger vilken åtgärd tootake när hello frågeparameter har redan angetts. Det här attributet måste ha något av följande värden hello.<br /><br /> -åsidosätt - ersätter hello värdet för befintliga hello-parametern.<br />-skip - ersätter inte hello befintliga frågeparametervärdet.<br />-Tillägg - lägger till hello värdet toohello befintliga frågeparametervärdet.<br />-delete - tar bort hello frågeparameter från hello-begäran.<br /><br /> När värdet för`override` ta med flera poster med hello samma namn resulterar i hello frågeparameter som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.|Nej|åsidosätt|  
|namn|Anger namnet på hello frågan toobe parameteruppsättning.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, backend  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="RewriteURL"></a>Omarbetning URL  
 Hej `rewrite-uri` princip konverterar en begärd URL från dess offentliga formuläret toohello form som förväntades av hello-webbtjänsten, som visas i följande exempel hello.  
  
-   Offentlig URL-`http://api.example.com/storenumber/ordernumber`  
  
-   URL-begäran-`http://api.example.com/v2/US/hardware/storenumber&ordernumber?City&State`  
  
 Den här principen kan användas när en mänsklig och/eller webbläsare eget URL bör omvandlas till hello URL-format förväntades av hello-webbtjänsten. Den här principen bara behöver toobe tillämpas när exponera en alternativ URL-format, till exempel ren URL: er, RESTful-URL: er, användarvänliga URL: er eller SEO eget URL: er som är enbart strukturella URL: er som inte innehåller en frågesträng och i stället innehåller endast hello sökvägen till hello resurs (efter hello schemat och hello). Detta sker ofta för dess estetiska egenskaper, användbarhet eller sökmotor optimeringssyfte (SEO).  
  
> [!NOTE]
>  Du kan bara lägga till sträng frågeparametrar Hej användarprincip. Du kan inte lägga till extra sökväg mallparametrar i hello omarbetning URL.  

### <a name="policy-statement"></a>Principframställning  
  
```xml  
<rewrite-uri template="uri template" copy-unmatched-params="true | false" />  
```  
  
### <a name="example"></a>Exempel  
  
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

### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|omarbetning-uri|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Attribut|Beskrivning|Krävs|Standard|  
|---------------|-----------------|--------------|-------------|  
|mall|hello faktiska webbtjänst-URL med någon fråga string-parametrar. När du använder uttryck måste hello hela värdet vara ett uttryck.|Ja|Saknas|  
|Kopiera omatchade parametrar|Anger om frågeparametrar Hej inkommande begäran finns inte i hello ursprungliga URL: en mall läggs toohello URL som definierats av hello Skriv mall|Nej|SANT|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** produkt, API, åtgärden  
  
##  <a name="XSLTransform"></a>Transformera XML med hjälp av en XSLT-fil  
 Hej `Transform XML using an XSLT` principen gäller en XSL-transformation tooXML i hello begäran eller svar.  
  
### <a name="policy-statement"></a>Principframställning  
  
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
  
### <a name="example"></a>Exempel  
  
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
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|XSL-Transformation|Rotelementet.|Ja|  
|Parametern|Använda toodefine variabler som används i hello transformeringen|Nej|  
|XSL: stylesheet|Formatmallen rotelementet. Alla element och attribut som definierats inom följer hello standard [XSLT-specifikationen](http://www.w3.org/TR/xslt)|Ja|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  
