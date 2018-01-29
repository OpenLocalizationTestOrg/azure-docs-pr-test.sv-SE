---
title: "Med API Management-tjänsten för att generera HTTP-begäranden"
description: "Lär dig att använda principer för förfrågan och svar i API-hantering för att anropa externa tjänster från din API"
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 4539c0fa-21ef-4b1c-a1d4-d89a38c242fa
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 6b7f1268ea4893307713931e7288f5d38c5ee080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="using-external-services-from-the-azure-api-management-service"></a>Med hjälp av externa tjänster från Azure API Management-tjänsten
Principerna som är tillgängliga i Azure API Management-tjänsten kan göra en mängd olika användbara arbete baserat enbart på den inkommande begäranden, utgående svar och grundläggande konfigurationsinformation. Dock kan interagera med externa tjänster från API Management principer öppnar många fler möjligheter.

Vi har tidigare sett hur vi kan interagera med den [Azure Event Hub-tjänsten för loggning, övervakning och analytics](api-management-log-to-eventhub-sample.md). I den här artikeln visar vi principer som gör att du samverka med eventuella externa HTTP-baserade tjänsten. Dessa principer kan användas för att utlösa remote händelser eller för att hämta information som används för att ändra den ursprungliga begäran och svar på något sätt.

## <a name="send-one-way-request"></a>En-sätt-begäran om att skicka
Eventuellt enklaste externa interaktionen är fire-och glömmer formatet för begäran som gör att en extern tjänst ska meddelas om någon typ av viktig händelse. Vi kan använda princip för åtkomstkontroll flödet `choose` att identifiera någon form av villkoret som vi vill och sedan om villkoret är uppfyllt, vi kan göra en extern HTTP-begäran med hjälp av den [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) princip. Detta kan bero på en begäran om att ett meddelandesystem som Hipchat eller Slack eller ett e-postmeddelande API som SendGrid eller MailChimp eller för kritiska Supportincidenter liknande PagerDuty. Alla dessa meddelandesystem har enkel HTTP-APIs som vi enkelt kan anropa.

### <a name="alerting-with-slack"></a>Varningar med Slack
Exemplet nedan visar hur du skickar ett meddelande till en Slack chatt-rummet om HTTP-status svarskoden är större än eller lika med 500. Ett Intervallfel 500 tyder på problem med vårt backend API som klienten i vårt API inte kan lösa sig själva. Det krävs någon slags åtgärd på vår sida.  

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

Slack har begreppet inkommande web hook. När du konfigurerar en inkommande web hook genererar Slack en särskild URL som kan du göra en enkel POST-begäran och skicka ett meddelande till Slack-kanal. JSON-meddelandetext som vi skapar baseras på ett format som definierats av Slack.

![Slack Web Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Är brand och glömmer tillräckligt bra?
Det finns vissa kompromisser när du använder en fire-och glömmer typ av begäran. Om du av någon anledning misslyckas begäran och felet rapporteras inte. I den här specifika situation garanteras komplexitet med sekundära fel reporting system och ytterligare kostnaden för att vänta på svaret inte. För scenarier där det är viktigt att kontrollera svar, sedan [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) princip är ett bättre alternativ.

## <a name="send-request"></a>Skicka begäran
Den `send-request` principen gör det möjligt med hjälp av en extern tjänst att utföra komplexa bearbetningsfunktioner och returnera data för API management-tjänsten som kan användas för behandling av princip för ytterligare.

### <a name="authorizing-reference-tokens"></a>Auktorisera referens token
En större funktion för API Management skyddar serverdelsresurser. Om tillståndet-server som används av din API skapar [JWT-tokens](http://jwt.io/) som en del av dess OAuth2-flöde som [Azure Active Directory](../active-directory/active-directory-aadconnect.md) matchar, kan du använda den `validate-jwt` policyn för att verifiera tokens giltighet. Men vissa auktorisering servrar skapar vad kallas [referera token](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) som inte kan verifieras utan att göra ett anrop till servern för auktorisering.

### <a name="standardized-introspection"></a>Standardiserade introspection
Tidigare har funnits ingen standardiserade möjlighet att kontrollera en referens-token med en server för auktorisering. Men en nyligen föreslagen standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publicerades av IETF som definierar hur en resurs-server kan verifiera giltigheten för en token.

### <a name="extracting-the-token"></a>Extrahera token
Det första steget är att hämta token från Authorization-huvud. Huvudets värde bör vara formaterad med den `Bearer` auktoriseringsschema, ett blanksteg och sedan tillståndet token enligt [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Det finns tyvärr fall där schemat auktorisering utelämnas. För att kontot för den här vid parsning av, vi dela huvudvärde på en sida och välj den sista strängen returnerade strängmatris. Det finns en lösning för felaktigt formaterat auktorisering huvuden.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-the-validation-request"></a>Skickar verifieringsbegäran
När vi har autentiseringstoken kan göra vi begäran att validera token. RFC 7662 anropar den här processen introspection och kräver att du `POST` HTML-formulär introspection resurs. HTML-formulär minst måste innehålla ett nyckel/värde-par med nyckel `token`. Denna begäran till auktorisering servern måste också autentiseras för att säkerställa att skadliga klienter inte kan gå fiske för giltig token.

```xml
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
```

### <a name="checking-the-response"></a>Kontrollerar svaret
Den `response-variable-name` attributet används för att ge åtkomst returnerade svaret. Namn definierad i den här egenskapen kan användas som en nyckel till den `context.Variables` ordlista att komma åt den `IResponse` objekt.

Vi kan hämta innehållet från objektet response och RFC 7622 talar om för oss att svaret måste vara ett JSON-objekt och måste innehålla minst en egenskap som kallas `active` som är ett booleskt värde. När `active` är true och sedan token ska vara giltigt.

### <a name="reporting-failure"></a>Rapportera fel
Vi använder en `<choose>` princip för att identifiera om token är ogiltig och i så fall returneras ett 401 svar.

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

Enligt [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) som beskriver hur `bearer` token ska användas, vi också returnera en `WWW-Authenticate` huvud med 401-svar. WWW-autentisering är avsedd att instruera en klient på hur du skapar en korrekt godkände begäran. På grund av flera olika metoder som är möjligt med OAuth2 framework är det svårt att kommunicera all information som behövs. Som tur är har att hjälpa [klienter identifiera hur du ska godkänna begäranden till resursservern](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Slutlig lösning
Kombinera alla delar få vi följande princip:

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
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

Detta är endast ett av flera exempel på hur `send-request` principen kan användas för att integrera praktiska externa tjänster i processen för begäranden och -svar som passerar genom API Management-tjänsten.

## <a name="response-composition"></a>Svaret sammansättning
Den `send-request` principen kan användas för att utöka en primär förfrågan till backend-systemet som vi såg i föregående exempel, eller den kan användas som en fullständig Ersätt för backend-anropet. Med den här tekniken kan vi enkelt skapa sammansatta resurser som aggregeras från flera olika system.

### <a name="building-a-dashboard"></a>Skapa en instrumentpanel
Ibland vill kunna att avslöja information som finns i flera backend-system, till exempel, för att ge en instrumentpanel. KPI: er som kommer från alla olika-servrar, men du vill inte ge direktåtkomst till dem och det är bra om all information som kan hämtas i en enskild begäran. Del av backend-informationen måste kanske vissa segmentering och tärning och lite Rensa först! Att cachelagra sammansatta resursen är en användbar för att minska belastningen backend som du vet att användarna har en vana hammering på F5 för att se om deras underperforming mått kan ändras.    

### <a name="faking-the-resource"></a>Faking resursen
Det första steget att bygga vår instrumentpanelen resurs är att konfigurera en ny funktion i API Management publisher portal. Det här är en platshållare för åtgärd som används för att konfigurera policyn sammansättning för att bygga vår dynamisk resurs.

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Gör begäranden
En gång i `dashboard` åtgärden har skapats kan vi konfigurera en princip för åtgärden. 

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Det första steget är att extrahera alla frågeparametrar från den inkommande begäranden, så att vi kan vidarebefordra dem till vår serverdelen. I det här exemplet är vår instrumentpanelen visar information baserat på en viss tidsperiod en därför har en `fromDate` och `toDate` parameter. Vi kan använda den `set-variable` princip för att hämta informationen från URL-begäran.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

När vi har den här informationen kan vi göra begäranden till backend-system. Varje begäran skapar en ny URL med parameterinformation och anropar dess respektive server och lagrar svaret i en variabel i kontexten.

```xml
<send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
  <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
  <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>

<send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
<set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
  <set-method>GET</set-method>
</send-request>
```

Dessa begäranden körs i sekvens, vilket inte är idealiskt. I en kommande version kommer vi introducerar en ny princip som kallas `wait` som gör att alla dessa begäranden till köra parallellt.

### <a name="responding"></a>Svara
Att konstruera sammansatta svaret vi kan använda den [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) princip. Den `set-body` element kan använda ett uttryck för att skapa en ny `JObject` med alla komponenten garantier inbäddade som egenskaper.

```xml
<return-response response-variable-name="existing response variable">
  <set-status code="200" reason="OK" />
  <set-header name="Content-Type" exists-action="override">
    <value>application/json</value>
  </set-header>
  <set-body>
    @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                  new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                  new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                  new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                  ).ToString())
  </set-body>
</return-response>
```

Fullständig principen ser ut som följer:

```xml
<policies>
    <inbound>

  <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
  <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <base />
    </outbound>
</policies>
```

I konfigurationen för platshållaren innebär åtgärden vi konfigurera instrumentpanelen resursen ska cachelagras för minst en timme eftersom vi förstår vilka slags data att även om det är en timme som är inaktuellt, fortfarande är tillräckligt effektivt att förmedla värdefull information till användarna.

## <a name="summary"></a>Sammanfattning
Azure API Management-tjänsten ger flexibla principer som kan tillämpas selektivt på HTTP-trafik och möjliggör sammansättning av backend-tjänster. Om du vill förbättra din API-gateway med aviseringar funktion, verifiering, verifiering funktioner eller skapa nya sammansatta resurser baserat på flera backend-tjänster på `send-request` och tillhörande principer öppnar en värld av möjligheter.

## <a name="watch-a-video-overview-of-these-policies"></a>Titta på en videoöversikt över dessa principer
Mer information om den [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), och [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) principer som beskrivs i den här artikeln får du titta på följande videon.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

