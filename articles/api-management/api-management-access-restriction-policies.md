---
title: "principer för begränsning av aaaAzure API Management åtkomst | Microsoft Docs"
description: "Läs mer om hello åtkomst för programvarubegränsning kan användas i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 034febe3-465f-4840-9fc6-c448ef520b0f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 0ef368c2781d9a5cf9eaaa41a47489c904ed3198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-access-restriction-policies"></a>Principer för begränsning av API Management-åtkomst
Det här avsnittet innehåller en referens för hello följande API Management-principer. Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AccessRestrictionPolicies"></a>Principer för begränsning av åtkomst  
  
-   [Kontrollera HTTP-huvudet](api-management-access-restriction-policies.md#CheckHTTPHeader) -tillämpar existens och/eller värdet för ett HTTP-huvud.  
  
-   [Gränsen anropet frekvensen av prenumerationen](api-management-access-restriction-policies.md#LimitCallRate) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per prenumeration.  
  
-   [Gränsen anropet frekvensen av nyckeln](#LimitCallRateByKey) -förhindrar API användning ger spikar i diagrammet genom att begränsa anropet frekvensen på grundval av per nyckel.  
  
-   [Begränsa anroparen IP-adresser](api-management-access-restriction-policies.md#RestrictCallerIPs) -filter (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.  
  
-   [Kvot för uppsättningen av prenumerationen](api-management-access-restriction-policies.md#SetUsageQuota) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.  
  
-   [Kvot för uppsättningen av nyckeln](#SetUsageQuotaByKey) -kan du tooenforce förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel.  
  
-   [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) -tillämpar förekomst och giltigheten för en JWT extraheras från ett angivna HTTP-sidhuvud eller en angiven frågeparameter.  
  
##  <a name="CheckHTTPHeader"></a>Kontrollera HTTP-huvud  
 Använd hello `check-header` princip tooenforce att en begäran har en angivna HTTP-huvudet. Alternativt kan du kontrollera toosee om hello-huvud har ett specifikt värde eller Sök efter en tillåtna värdeintervallet. Om hello kontrollen misslyckas hello princip avslutar bearbeta begäran och returnerar HTTP-status kod och fel hälsningsmeddelande anges av hello princip.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<check-header name="header name" failed-check-httpcode="code" failed-check-error-message="message" ignore-case="True">  
    <value>Value1</value>  
    <value>Value2</value>  
</check-header>  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<check-header name="Authorization" failed-check-httpcode="401" failed-check-error-message="Not authorized" ignore-case="false">  
    <value>f6dc69a089844cf6b2019bae6d36fac8</value>  
</check-header>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Kontrollera-huvud|Rotelementet.|Ja|  
|värde|Tillåtet värde för HTTP-huvud. När flera element med värdet har angetts, betraktas lyckas hello kontrollera om någon av hello värden finns en matchning.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|misslyckades-check--felmeddelande|Fel meddelande tooreturn i hello HTTP svarstexten om hello huvudet finns inte eller har ett ogiltigt värde. Det här meddelandet måste ha specialtecken (escaped) korrekt.|Ja|Saknas|  
|misslyckades-check-httpcode|HTTP-Status kod tooreturn om hello huvudet finns inte eller har ett ogiltigt värde.|Ja|Saknas|  
|huvudets namn|hello namnet på hello HTTP-huvudet toocheck.|Ja|Saknas|  
|Ignorera skiftläge|Kan ställas in tooTrue eller False. Om set tooTrue fallet ignoreras när hello huvudvärde jämförs hello uppsättning godkända värden.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande, utgående  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="LimitCallRate"></a>Gränsen anropet frekvensen av prenumerationen  
 Hej `rate-limit` princip förhindrar API användning toppar på grundval av per prenumeration genom att begränsa hello anropa hastighet tooa anges antalet per angiven tidsperiod. När den här principen utlöses hello anroparen tar emot en `429 Too Many Requests` Svarets statuskod.  
  
> [!IMPORTANT]
>  Den här principen kan användas en gång per dokument.  
>   
>  [Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<rate-limit calls="number" renewal-period="seconds">  
    <api name="name" calls="number" renewal-period="seconds">  
        <operation name="name" calls="number" renewal-period="seconds" />  
    </api>  
</rate-limit>  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit calls="20" renewal-period="90" />  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Ställ in gräns|Rotelementet.|Ja|  
|api|Lägg till en eller flera av dessa element tooimpose en anropet hastighetsbegränsning på API: er i hello produkten. Produkt- och API anropa räntesats gränser som är oberoende av varandra.|Nej|  
|åtgärden|Lägg till en eller flera av dessa element tooimpose en anropet hastighetsbegränsning på åtgärder i en API. Produkt-, API- och åtgärden anropa räntesats gränser som är oberoende av varandra.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|namn|hello namnet på hello API för vilka tooapply hello hastighetsbegränsning.|Ja|Saknas|  
|anrop|Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.|Ja|Saknas|  
|förnyelseperioden|hello tidsperiod i sekunder, efter vilken hello kvoten återställs.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** produkt  
  
##  <a name="LimitCallRateByKey"></a>Gränsen anropet frekvensen av nyckel  
 Hej `rate-limit-by-key` princip förhindrar API användning toppar på grundval av per nyckel genom att begränsa hello anropa hastighet tooa anges antalet per angiven tidsperiod. hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck. Valfria steg villkor kan läggas till toospecify vilka begäranden som ska räknas till hello gränsen. När den här principen utlöses hello anroparen tar emot en `429 Too Many Requests` Svarets statuskod.  
  
 Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Den här principen kan användas en gång per dokument.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<rate-limit-by-key calls="number"  
                   renewal-period="seconds"   
                   increment-condition="condition"   
                   counter-key="key value" />  
  
```  
  
### <a name="example"></a>Exempel  
 I följande exempel hello, aktiveras hello hastighetsbegränsning hello anroparen IP-adressen.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <rate-limit-by-key  calls="10"  
              renewal-period="60"  
              increment-condition="@(context.Response.StatusCode == 200)"  
              counter-key="@(context.Request.IpAddress)"/>  
    </inbound>  
    <outbound>  
        <base />          
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|Ställ in gräns|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|anrop|Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.|Ja|Saknas|  
|motparterna nyckel|hello viktiga toouse för hello hastighet gränsen principen.|Ja|Saknas|  
|öka villkor|hello booleskt uttryck som anger om hello begäran ska räknas mot hello kvoten (`true`).|Nej|Saknas|  
|förnyelseperioden|hello tidsperiod i sekunder, efter vilken hello kvoten återställs.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="RestrictCallerIPs"></a>Begränsa anroparen IP-adresser  
 Hej `ip-filter` princip filtrerar (tillåter/nekar)-anrop från särskilda IP-adresser och/eller -adressintervall.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<ip-filter action="allow | forbid">  
    <address>address</address>  
    <address-range from="address" to="address" />  
</ip-filter>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|IP-filter|Rotelementet.|Ja|  
|Adress|Anger en IP-adress på vilken toofilter.|Minst en `address` eller `address-range` -element krävs.|  
|adressintervallet från = ”adress” till = ”adress”|Anger ett intervall med IP-adress på vilken toofilter.|Minst en `address` eller `address-range` -element krävs.|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|adressintervallet från = ”adress” till = ”adress”|Ett intervall med IP-adresser tooallow eller neka åtkomst för.|Krävs när hello `address-range` elementet används.|Saknas|  
|IP-filteråtgärd = ”Tillåt &#124; förbjuda ”|Anger om anrop ska tillåtas eller inte för hello angivna IP-adresser och intervall.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="SetUsageQuota"></a>Ange kvot för prenumeration  
 Hej `quota` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per prenumeration.  
  
> [!IMPORTANT]
>  Den här principen kan användas en gång per dokument.  
>   
>  [Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">  
    <api name="name" calls="number" bandwidth="kilobytes">  
        <operation name="name" calls="number" bandwidth="kilobytes" />  
    </api>  
</quota>  
```  
  
### <a name="example"></a>Exempel  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota calls="10000" bandwidth="40000" renewal-period="3600" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|kvot|Rotelementet.|Ja|  
|api|Lägg till en eller flera av dessa element tooimpose en kvot på API: er i hello produkten. Produkt- och API-kvoter tillämpas oberoende av varandra.|Nej|  
|åtgärden|Lägg till en eller flera av dessa element tooimpose en kvot på åtgärder i en API. Produkt-, API- och åtgärden kvoter tillämpas oberoende av varandra.|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|namn|hello namnet på hello API eller åtgärden som hello kvoten gäller.|Ja|Saknas|  
|Bandbredd|Hej maximalt antal kilobyte får användas under hello tidsintervall som angetts i hello `renewal-period`.|Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.|Saknas|  
|anrop|Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.|Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.|Saknas|  
|förnyelseperioden|hello tidsperiod i sekunder, efter vilken hello kvoten återställs.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** produkt  
  
##  <a name="SetUsageQuotaByKey"></a>Kvot för uppsättningen av nyckel  
 Hej `quota-by-key` princip tillämpar förnyas eller livstid anropet volym och/eller bandbredd kvoten på grundval av per nyckel. hello nyckeln kan ha ett godtyckligt värde och anges vanligtvis med ett principuttryck. Valfria steg villkor kan läggas till toospecify vilka begäranden som ska räknas till hello kvoten.  
  
 Mer information och exempel på den här principen finns [avancerade begäran begränsning med Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-flexible-throttling/).  
  
> [!IMPORTANT]
>  Den här principen kan användas en gång per dokument.  
>   
>  [Principuttrycken](api-management-policy-expressions.md) kan inte användas i någon av hello attributen för den här principen.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<quota-by-key calls="number"   
              bandwidth="kilobytes"   
              renewal-period="seconds"  
              increment-condition="condition"   
              counter-key="key value" />  
  
```  
  
### <a name="example"></a>Exempel  
 I följande exempel hello, aktiveras hello kvoten av hello anroparen IP-adress.  
  
```xml  
<policies>  
    <inbound>  
        <base />  
        <quota-by-key calls="10000" bandwidth="40000" renewal-period="3600"  
                      increment-condition="@(context.Response.StatusCode >= 200 && context.Response.StatusCode < 400)"  
                      counter-key="@(context.Request.IpAddress)" />  
    </inbound>  
    <outbound>  
        <base />  
    </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Element  
  
|Namn|Beskrivning|Krävs|  
|----------|-----------------|--------------|  
|kvot|Rotelementet.|Ja|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|Bandbredd|Hej maximalt antal kilobyte får användas under hello tidsintervall som angetts i hello `renewal-period`.|Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.|Saknas|  
|anrop|Hej maximala totala antal anrop som får användas under hello tidsintervall som angetts i hello `renewal-period`.|Antingen `calls`, `bandwidth`, eller båda tillsammans måste anges.|Saknas|  
|motparterna nyckel|hello viktiga toouse för hello kvoten principen.|Ja|Saknas|  
|öka villkor|hello booleskt uttryck som anger om hello begäran ska räknas mot hello kvoten (`true`)|Nej|Saknas|  
|förnyelseperioden|hello tidsperiod i sekunder, efter vilken hello kvoten återställs.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
##  <a name="ValidateJWT"></a>Validera JWT  
 Hej `validate-jwt` princip tillämpar existens och giltigheten för en JWT extraheras från antingen angivna HTTP-huvudet eller angivna frågeparameter.  
  
> [!IMPORTANT]
>  Hej `validate-jwt` principen kräver att hello `exp` registrerade anspråk är som ingår i hello JWT-token, såvida inte `require-expiration-time` attribut anges och ställa in också`false`.  
> Hej `validate-jwt` stöder HS256 och RS256 signering algoritmer. För HS256 anges hello nyckel infogad i hello princip hello base64-kodat format. För RS256 har hello-nyckel toobe tillhandahålla via en öppen ID configuration slutpunkt.  
  
### <a name="policy-statement"></a>Principframställning  
  
```xml  
<validate-jwt   
    header-name="name of http header containing hello token (use query-parameter-name attribute if hello token is passed in hello URL)"   
    failed-validation-httpcode="http status code tooreturn on failure"   
    failed-validation-error-message="error message tooreturn on failure"   
    require-expiration-time="true|false"
    require-scheme="scheme"
    require-signed-tokens="true|false"   
    clock-skew="allowed clock skew in seconds">  
  <issuer-signing-keys>  
    <key>base64 encoded signing key</key>  
    <!-- if there are multiple keys, then add additional key elements -->  
  </issuer-signing-keys>  
  <audiences>  
    <audience>audience string</audience>  
    <!-- if there are multiple possible audiences, then add additional audience elements -->  
  </audiences>  
  <issuers>  
    <issuer>issuer string</issuer>  
    <!-- if there are multiple possible issuers, then add additional issuer elements -->  
  </issuers>  
  <required-claims>  
    <claim name="name of hello claim as it appears in hello token" match="all|any">  
      <value>claim value as it is expected tooappear in hello token</value>  
      <!-- if there is more than one allowed values, then add additional value elements -->  
    </claim>  
    <!-- if there are multiple possible allowed values, then add additional value elements -->  
  </required-claims>  
  <openid-config url="full URL of hello configuration endpoint, e.g. https://login.constoso.com/openid-configuration" />  
  <zumo-master-key id="key identifier">key value</zumo-master-key>  
</validate-jwt>  
  
```  
  
### <a name="examples"></a>Exempel  
  
#### <a name="azure-mobile-services-token-validation"></a>Azure Mobile Services token verifiering  
  
```xml  
<validate-jwt header-name="x-zumo-auth" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Supplied access token is invalid.">  
    <issuers>  
        <issuer>urn:microsoft:windows-azure:zumo</issuer>  
    </issuers>  
    <audiences>  
        <audience>Facebook</audience>  
    </audiences>  
    <issuer-signing-keys>  
        <zumo-master-key id="0">insert key here</zumo-master-key>  
    </issuer-signing-keys>  
</validate-jwt>  
```  
  
#### <a name="azure-active-directory-token-validation"></a>Azure Active Directory token verifiering  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration" />  
    <audiences>
        <audience>25eef6e4-c905-4a07-8eb4-0d08d5df8b3f</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  

  
#### <a name="azure-active-directory-b2c-token-validation"></a>Azure Active Directory B2C-token verifiering  
  
```xml  
<validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">  
    <openid-config url="https://login.microsoftonline.com/tfp/contoso.onmicrosoft.com/b2c_1_signin/v2.0/.well-known/openid-configuration" />
    <audiences>
        <audience>d313c4e4-de5f-4197-9470-e509a2f0b806</audience>
    </audiences>
    <required-claims>  
        <claim name="id" match="all">  
            <value>insert claim here</value>  
        </claim>  
    </required-claims>  
</validate-jwt>  
```  
  
#### <a name="authorize-access-toooperations-based-on-token-claims"></a>Auktorisera åtkomst toooperations baserat på token anspråk  
 Det här exemplet illustrerar hur toouse hello [Validera JWT](api-management-access-restriction-policies.md#ValidateJWT) princip toopre-auktorisera åtkomst toooperations baserat på token anspråk. En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too13:50. Vidarebefordra snabb too15:00 toosee hello principer som konfigurerats i hello redigeraren och sedan too18:50 för en demonstration av anropa en funktion från hello developer-portalen både med och utan hello krävs autentiseringstoken.  
  
```xml  
<!-- Copy hello following snippet into hello inbound section at hello api (or higher) level toopre-authorize access toooperations based on token claims -->  
<set-variable name="signingKey" value="insert signing key here" />  
<choose>  
  <when condition="@(context.Request.Method.Equals("patch",StringComparison.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="edit">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <when condition="@(new [] {"post", "put"}.Contains(context.Request.Method,StringComparer.OrdinalIgnoreCase))">  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
      <required-claims>  
        <claim name="create">  
          <value>true</value>  
        </claim>  
      </required-claims>  
    </validate-jwt>  
  </when>  
  <otherwise>  
    <validate-jwt header-name="Authorization">  
      <issuer-signing-keys>  
        <key>@((string)context.Variables["signingKey"])</key>  
      </issuer-signing-keys>  
    </validate-jwt>  
  </otherwise>  
</choose>  
```  
  
### <a name="elements"></a>Element  
  
|Element|Beskrivning|Krävs|  
|-------------|-----------------|--------------|  
|Validera jwt|Rotelementet.|Ja|  
|målgrupper|Innehåller en lista över godkända målgruppen anspråk som kan finnas på hello-token. Om flera målgruppen värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas. Du måste ange minst en målgrupp.|Nej|  
|utfärdaren signering nycklar|En lista över Base64-kodad säkerhet nycklar som används toovalidate signerade token. Om det finns flera säkerhetsnycklar och sedan varje nyckel testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas (användbart för token förnyelse). Viktiga delar har en valfri `id` attribut som används toomatch mot `kid` anspråk.|Nej|  
|certifikatutfärdare|En lista över godkända säkerhetsobjekt som utfärdats hello-token. Om flera utfärdaren värden finns och sedan varje värde testas förrän antingen alla uttöms (i så fall verifieringen misslyckas) eller tills ett lyckas.|Nej|  
|openid-config|hello-element som används för att ange en kompatibel öppna ID configuration-slutpunkt som signering nycklar och utfärdaren kan hämtas.|Nej|  
|krävs anspråk|Innehåller en lista över anspråk som förväntat toobe finns på hello token för den toobe anses vara giltigt. När hello `match` attribut har angetts för`all` var anspråksvärdet i hello princip måste finnas i hello token för verifiering toosucceed. När hello `match` attribut har angetts för`any` minst ett anspråk måste finnas i hello token för verifiering toosucceed.|Nej|  
|zumo huvudnyckeln|Huvudnyckeln för token som utfärdats av Azure Mobile Services|Nej|  
  
### <a name="attributes"></a>Attribut  
  
|Namn|Beskrivning|Krävs|Standard|  
|----------|-----------------|--------------|-------------|  
|förskjutning av klockan|TimeSpan. Innehåller vissa små handtag om hello-token upphör att gälla anspråk finns i hello token och har passerat hello aktuellt datum / tid.|Nej|0 sekunder|  
|misslyckades---valideringsfelmeddelande|Fel meddelande tooreturn i hello HTTP svarstexten om hello JWT inte klarar valideringen. Det här meddelandet måste ha specialtecken (escaped) korrekt.|Nej|Standardfelmeddelandet beror på verifieringsfel, till exempel ”JWT finns inte”.|  
|misslyckades-validering-httpcode|HTTP-Status kod tooreturn om hello JWT inte valideras.|Nej|401|  
|huvudets namn|hello namnet på hello HTTP-huvud innehåller hello-token.|Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.|Saknas|  
|id|Hej `id` -attributet på hello `key` -elementet kan du toospecify hello sträng som matchas mot `kid` anspråk i hello token (om sådan finns) toofind ut hello lämpliga nycklar toouse för signaturverifiering.|Nej|Saknas|  
|Matcha|Hej `match` -attributet på hello `claim` element anger om varje anspråksvärde i hello princip måste finnas i hello token för verifiering toosucceed. Möjliga värden:<br /><br /> -                          `all`-varje anspråksvärdet i hello princip måste finnas i hello token för verifiering toosucceed.<br /><br /> -                          `any`-minst en anspråksvärdet måste finnas i hello token för verifiering toosucceed.|Nej|Alla|  
|paremeter frågenamnet|hello namnet på hello hello frågeparameter hålla hello-token.|Antingen `header-name` eller `query-paremeter-name` måste vara anges, men inte båda.|Saknas|  
|kräver förfallotid|Booleskt värde. Anger om ett förfallodatum anspråk krävs i hello-token.|Nej|SANT|
|kräver schema|Hej namnet på hello token-schemat, t.ex. ”Ägar”. När det här attributet anges säkerställer hello princip som angetts schemat finns i hello auktorisering huvudvärde.|Nej|Saknas|
|Kräv-signerad-token|Booleskt värde. Anger om nödvändiga toobe signerade token.|Nej|SANT|  
|URL: en|Öppna ID configuration slutpunkts-URL från där du kan hämta öppna ID configuration metadata. Använd hello följande URL: en för Azure Active Directory: `https://login.microsoftonline.com/{tenant-name}/.well-known/openid-configuration` ersätter din klient katalognamn, t.ex. `contoso.onmicrosoft.com`.|Ja|Saknas|  
  
### <a name="usage"></a>Användning  
 Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **Avsnitt i princip:** inkommande  
  
-   **Princip för scope:** globala, produkt, API, åtgärden  
  
## <a name="next-steps"></a>Nästa steg
Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).  
