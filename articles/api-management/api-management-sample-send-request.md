---
title: "aaaUsing API Management-tjänsten toogenerate HTTP-begäranden"
description: "Läs toouse principer för förfrågan och svar i API Management toocall externa tjänster från din API"
services: api-management
documentationcenter: 
author: darrelmiller
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
ms.openlocfilehash: 8002ee453057513340328d99f298703c3b3a9531
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-external-services-from-hello-azure-api-management-service"></a>Med hjälp av externa tjänster från hello Azure API Management-tjänsten
hello-principer som är tillgängliga i Azure API Management-tjänsten kan göra en mängd olika användbara arbete utifrån rent hello inkommande begäran, hello utgående svar och grundläggande konfigurationsinformation. Men som kan toointeract med externa tjänster från API Management principer öppnar många fler möjligheter.

Vi har tidigare sett hur vi kan interagera med hello [Azure Event Hub-tjänsten för loggning, övervakning och analytics](api-management-log-to-eventhub-sample.md). I den här artikeln visar vi principer som gör toointeract med alla externa HTTP-baserade tjänsten. Dessa principer kan användas för att utlösa remote händelser eller för att hämta information som ska använda toomanipulate hello ursprungliga begäran och svar på något sätt.

## <a name="send-one-way-request"></a>En-sätt-begäran om att skicka
Eventuellt hello enklaste externa interaktion är hello fire-och glömmer formatet för begäran som gör att en extern tjänst toobe meddelande om någon typ av viktig händelse. Vi kan använda hello princip för åtkomstkontroll flödet `choose` toodetect någon typ av villkor som är intresserad av och sedan om hello villkoret är uppfyllt, kan vi göra en extern HTTP-begäran med hjälp av hello [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) princip. Detta kan bero på en begäran tooa meddelandesystem som Hipchat eller Slack eller ett e-postmeddelande API som SendGrid eller MailChimp eller för kritiska Supportincidenter liknande PagerDuty. Alla dessa meddelandesystem har enkel HTTP-APIs som vi enkelt kan anropa.

### <a name="alerting-with-slack"></a>Varningar med Slack
hello som följande exempel visar hur toosend ett meddelande tooa Slack chatt-rummet om hello HTTP-svarskoden status är större än eller lika med too500. Ett Intervallfel 500 anger ett problem med våra API-serverdel som hello klienten i vårt API inte kan lösa sig själva. Det krävs någon slags åtgärd på vår sida.  

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

Slack har hello begreppet inkommande web hook. När du konfigurerar en inkommande web hook genererar Slack en särskild URL som gör att du toodo en enkel POST-begäran och toopass ett meddelande till hello Slack-kanal. hello JSON-meddelandetext som vi skapar baseras på ett format som definierats av Slack.

![Slack Web Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Är brand och glömmer tillräckligt bra?
Det finns vissa kompromisser när du använder en fire-och glömmer typ av begäran. Om du av någon anledning misslyckas hello begäran och hello fel rapporteras inte. I den här specifika situation garanteras hello komplexitet med en sekundär fel rapporteringssystem hello ytterligare kostnaden för att vänta på hello svar inte. Scenarier där det är viktigt toocheck hello svar hello [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) princip är ett bättre alternativ.

## <a name="send-request"></a>Skicka begäran
Hej `send-request` principen gör det möjligt med hjälp av en extern tjänst tooperform komplexa bearbetning av funktioner och returnera data toohello API management-tjänst som kan användas för behandling av princip för ytterligare.

### <a name="authorizing-reference-tokens"></a>Auktorisera referens token
En större funktion för API Management skyddar serverdelsresurser. Om hello auktorisering server som används av din API skapar [JWT-tokens](http://jwt.io/) som en del av dess OAuth2-flöde som [Azure Active Directory](../active-directory/active-directory-aadconnect.md) matchar, kan du använda hello `validate-jwt` princip tooverify hello giltighet hello-token. Men vissa auktorisering servrar skapar vad kallas [referera token](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) som inte kan verifieras utan att göra en anropet tillbaka toohello auktorisering server.

### <a name="standardized-introspection"></a>Standardiserade introspection
I hello som tidigare har funnits ingen standardiserade möjlighet att kontrollera en referens-token med en server för auktorisering. Men en nyligen föreslagen standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publicerades av hello IETF som definierar hur en resurs-server kan verifiera hello giltigheten för en token.

### <a name="extracting-hello-token"></a>Extrahera hello-token
hello första steget är tooextract hello token från hello Authorization-huvud. hello huvudets värde bör vara formaterad med hello `Bearer` auktoriseringsschema, ett blanksteg och sedan hello auktorisering token enligt [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Det finns tyvärr fall där hello auktorisering schemat utelämnas. tooaccount för den här vid parsning av, vi dela hello huvudvärde på ett blanksteg och välj hello sista strängen från hello returneras en matris med strängar. Det finns en lösning för felaktigt formaterat auktorisering huvuden.

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a>Hello begäran validering
När vi har hello autentiseringstoken kan göra vi hello begäran toovalidate hello token. RFC 7662 anropar den här processen introspection och kräver att du `POST` en HTML-formulär toohello introspection resurs. hello HTML-formulär minst måste innehålla ett nyckel/värde-par med hello nyckel `token`. Den här begäran toohello auktorisering servern måste också vara autentiserade tooensure som skadlig klienter inte kan gå fiske för giltig token.

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

### <a name="checking-hello-response"></a>Kontrollerar hello-svar
Hej `response-variable-name` attributet är används toogive åtkomst hello returnerade svar. hello namn har definierats i den här egenskapen kan användas som en nyckel till hello `context.Variables` ordlista tooaccess hello `IResponse` objekt.

Från hello svarsobjekt kan vi hämta hello brödtext och RFC 7622 talar om för oss att hello svaret måste vara ett JSON-objekt och måste innehålla minst en egenskap som kallas `active` som är ett booleskt värde. När `active` är true och sedan hello token ska vara giltigt.

### <a name="reporting-failure"></a>Rapportera fel
Vi använder en `<choose>` princip toodetect om hello token är ogiltig och i så fall returneras ett 401 svar.

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

Enligt [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) som beskriver hur `bearer` token ska användas, vi också returnera en `WWW-Authenticate` huvud med hello 401 svar. hello WWW-autentisering är avsedd tooinstruct en klient på hur tooconstruct en korrekt godkände begäran. På grund av toohello flera olika metoder som är möjligt med hello OAuth2 framework, är det svårt toocommunicate alla hello behövs information. Lyckligtvis har pågår toohelp [klienter identifiera hur tooproperly godkänna begäranden tooa resursservern](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Slutlig lösning
Kombinera alla hello delar hämta vi hello följande princip:

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

Detta är endast ett av flera exempel på hur hello `send-request` policy kan vara användbart används toointegrate externa tjänster till hello av begäranden och -svar som passerar genom hello API Management-tjänsten.

## <a name="response-composition"></a>Svaret sammansättning
Hej `send-request` principen kan användas för att utöka primära förfrågningen tooa backend system som vi såg i hello föregående exempel, eller den kan användas som en fullständig Ersätt för hello backend-anrop. Med den här tekniken kan vi enkelt skapa sammansatta resurser som aggregeras från flera olika system.

### <a name="building-a-dashboard"></a>Skapa en instrumentpanel
Ibland vill du toobe kan tooexpose information som finns i flera backend-system, till exempel toodrive en instrumentpanel. hello KPI: er som kommer från alla olika-servrar, men du föredrar att inte tooprovide direktåtkomst toothem och det är bra om gick att hämta alla hello information i en enskild begäran. Vissa av hello backend information behöver kanske vissa segmentering och tärning och lite Rensa först! Som kan toocache att sammansatta resursen är en användbar tooreduce hello backend läses in som du vet att användarna har en vana hammering hello F5-tangenten i ordning toosee om deras underperforming mått kan ändras.    

### <a name="faking-hello-resource"></a>Faking hello-resurs
Hej första steg toobuilding våra instrumentpanelen resursen är tooconfigure en ny åtgärd i hello API Management publisher portal. Detta är en platshållare för åtgärden används tooconfigure våra sammansättning princip toobuild våra dynamisk resurs.

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a>Hello-begäranden
En gång hello `dashboard` åtgärden har skapats kan vi konfigurera en princip för åtgärden. 

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

hello första steget är tooextract alla frågeparametrar Hej inkommande begäran, så att vi kan vidarebefordra dem tooour backend. I det här exemplet är vår instrumentpanelen visar information baserat på en viss tidsperiod en därför har en `fromDate` och `toDate` parameter. Vi kan använda hello `set-variable` principinformation tooextract hello från hello URL-begäran.

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

När vi har den här informationen kan vi göra begäranden tooall hello serverdelssystem. Varje begäran skapar en ny URL med hello parameterinformation och anropar dess respektive server och lagrar hello svar i en variabel i kontexten.

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

Dessa begäranden körs i sekvens, vilket inte är idealiskt. I en kommande version kommer vi introducerar en ny princip som kallas `wait` som gör att alla dessa begäranden tooexecute parallellt.

### <a name="responding"></a>Svara
tooconstruct hello sammansatta svar vi kan använda hello [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) princip. Hej `set-body` element kan använda ett uttryck tooconstruct en ny `JObject` med alla hello komponenten garantier inbäddade som egenskaper.

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

hello hela policyn ser ut som följer:

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

I hello konfigurationen av hello platshållare åtgärden vi konfigurera cachelagras hello instrumentpanelen resurs toobe för minst en timme eftersom vi förstår hello uppbyggnad hello data innebär att även om det är en timme som är inaktuellt, fortfarande är tillräckligt effektivt tooconvey värdefull information toohello användare.

## <a name="summary"></a>Sammanfattning
Azure API Management-tjänsten ger flexibla principer som kan vara selektivt används tooHTTP trafik och möjliggör sammansättning av backend-tjänster. Om du vill tooenhance din API-gateway med aviseringar funktion, verifiering, verifiering funktioner eller skapa nya sammansatta resurser baserat på flera backend-tjänster, hello `send-request` och tillhörande principer öppnar en värld av möjligheter.

## <a name="watch-a-video-overview-of-these-policies"></a>Titta på en videoöversikt över dessa principer
Mer information om hello [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), och [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) principer som beskrivs i den här artikeln får du titta på hello följande video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

