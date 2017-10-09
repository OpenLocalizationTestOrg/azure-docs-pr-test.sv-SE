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
# <a name="using-external-services-from-hello-azure-api-management-service"></a><span data-ttu-id="f8f3a-103">Med hjälp av externa tjänster från hello Azure API Management-tjänsten</span><span class="sxs-lookup"><span data-stu-id="f8f3a-103">Using external services from hello Azure API Management service</span></span>
<span data-ttu-id="f8f3a-104">hello-principer som är tillgängliga i Azure API Management-tjänsten kan göra en mängd olika användbara arbete utifrån rent hello inkommande begäran, hello utgående svar och grundläggande konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-104">hello policies available in Azure API Management service can do a wide range of useful work based purely on hello incoming request, hello outgoing response and basic configuration information.</span></span> <span data-ttu-id="f8f3a-105">Men som kan toointeract med externa tjänster från API Management principer öppnar många fler möjligheter.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-105">However, being able toointeract with external services from API Management policies opens up many more opportunities.</span></span>

<span data-ttu-id="f8f3a-106">Vi har tidigare sett hur vi kan interagera med hello [Azure Event Hub-tjänsten för loggning, övervakning och analytics](api-management-log-to-eventhub-sample.md).</span><span class="sxs-lookup"><span data-stu-id="f8f3a-106">We have previously seen how we can interact with hello [Azure Event Hub service for logging, monitoring and analytics](api-management-log-to-eventhub-sample.md).</span></span> <span data-ttu-id="f8f3a-107">I den här artikeln visar vi principer som gör toointeract med alla externa HTTP-baserade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-107">In this article we will demonstrate policies that allow you toointeract with any external HTTP based service.</span></span> <span data-ttu-id="f8f3a-108">Dessa principer kan användas för att utlösa remote händelser eller för att hämta information som ska använda toomanipulate hello ursprungliga begäran och svar på något sätt.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-108">These policies can be used for triggering remote events or for retrieving information that will be used toomanipulate hello original request and response in some way.</span></span>

## <a name="send-one-way-request"></a><span data-ttu-id="f8f3a-109">En-sätt-begäran om att skicka</span><span class="sxs-lookup"><span data-stu-id="f8f3a-109">Send-One-Way-Request</span></span>
<span data-ttu-id="f8f3a-110">Eventuellt hello enklaste externa interaktion är hello fire-och glömmer formatet för begäran som gör att en extern tjänst toobe meddelande om någon typ av viktig händelse.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-110">Possibly hello simplest external interaction is hello fire-and-forget style of request that allows an external service toobe notified of some kind of important event.</span></span> <span data-ttu-id="f8f3a-111">Vi kan använda hello princip för åtkomstkontroll flödet `choose` toodetect någon typ av villkor som är intresserad av och sedan om hello villkoret är uppfyllt, kan vi göra en extern HTTP-begäran med hjälp av hello [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) princip.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-111">We can use hello control flow policy `choose` toodetect any kind of condition that we are interested in and then, if hello condition is satisfied, we can make an external HTTP request using hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) policy.</span></span> <span data-ttu-id="f8f3a-112">Detta kan bero på en begäran tooa meddelandesystem som Hipchat eller Slack eller ett e-postmeddelande API som SendGrid eller MailChimp eller för kritiska Supportincidenter liknande PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-112">This could be a request tooa messaging system like Hipchat or Slack, or a mail API like SendGrid or MailChimp, or for critical support incidents something like PagerDuty.</span></span> <span data-ttu-id="f8f3a-113">Alla dessa meddelandesystem har enkel HTTP-APIs som vi enkelt kan anropa.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-113">All of these messaging systems have simple HTTP APIs that we can easily invoke.</span></span>

### <a name="alerting-with-slack"></a><span data-ttu-id="f8f3a-114">Varningar med Slack</span><span class="sxs-lookup"><span data-stu-id="f8f3a-114">Alerting with Slack</span></span>
<span data-ttu-id="f8f3a-115">hello som följande exempel visar hur toosend ett meddelande tooa Slack chatt-rummet om hello HTTP-svarskoden status är större än eller lika med too500.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-115">hello following example demonstrates how toosend a message tooa Slack chat room if hello HTTP response status code is greater than or equal too500.</span></span> <span data-ttu-id="f8f3a-116">Ett Intervallfel 500 anger ett problem med våra API-serverdel som hello klienten i vårt API inte kan lösa sig själva.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-116">A 500 range error indicates a problem with our backend API that hello client of our API cannot resolve themselves.</span></span> <span data-ttu-id="f8f3a-117">Det krävs någon slags åtgärd på vår sida.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-117">It usually requires some kind of intervention on our part.</span></span>  

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

<span data-ttu-id="f8f3a-118">Slack har hello begreppet inkommande web hook.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-118">Slack has hello notion of inbound web hooks.</span></span> <span data-ttu-id="f8f3a-119">När du konfigurerar en inkommande web hook genererar Slack en särskild URL som gör att du toodo en enkel POST-begäran och toopass ett meddelande till hello Slack-kanal.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-119">When configuring an inbound web hook, Slack generates a special URL which allows you toodo a simple POST request and toopass a message into hello Slack channel.</span></span> <span data-ttu-id="f8f3a-120">hello JSON-meddelandetext som vi skapar baseras på ett format som definierats av Slack.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-120">hello JSON body that we create is based on a format defined by Slack.</span></span>

![Slack Web Hook](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a><span data-ttu-id="f8f3a-122">Är brand och glömmer tillräckligt bra?</span><span class="sxs-lookup"><span data-stu-id="f8f3a-122">Is fire and forget good enough?</span></span>
<span data-ttu-id="f8f3a-123">Det finns vissa kompromisser när du använder en fire-och glömmer typ av begäran.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-123">There are certain tradeoffs when using a fire-and-forget style of request.</span></span> <span data-ttu-id="f8f3a-124">Om du av någon anledning misslyckas hello begäran och hello fel rapporteras inte.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-124">If for some reason, hello request fails, then hello failure will not be reported.</span></span> <span data-ttu-id="f8f3a-125">I den här specifika situation garanteras hello komplexitet med en sekundär fel rapporteringssystem hello ytterligare kostnaden för att vänta på hello svar inte.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-125">In this particular situation, hello complexity of having a secondary failure reporting system and hello additional performance cost of waiting for hello response is not warranted.</span></span> <span data-ttu-id="f8f3a-126">Scenarier där det är viktigt toocheck hello svar hello [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) princip är ett bättre alternativ.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-126">For scenarios where it is essential toocheck hello response, then hello [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy is a better option.</span></span>

## <a name="send-request"></a><span data-ttu-id="f8f3a-127">Skicka begäran</span><span class="sxs-lookup"><span data-stu-id="f8f3a-127">Send-Request</span></span>
<span data-ttu-id="f8f3a-128">Hej `send-request` principen gör det möjligt med hjälp av en extern tjänst tooperform komplexa bearbetning av funktioner och returnera data toohello API management-tjänst som kan användas för behandling av princip för ytterligare.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-128">hello `send-request` policy enables using an external service tooperform complex processing functions and return data toohello API management service that can be used for further policy processing.</span></span>

### <a name="authorizing-reference-tokens"></a><span data-ttu-id="f8f3a-129">Auktorisera referens token</span><span class="sxs-lookup"><span data-stu-id="f8f3a-129">Authorizing reference tokens</span></span>
<span data-ttu-id="f8f3a-130">En större funktion för API Management skyddar serverdelsresurser.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-130">A major function of API Management is protecting backend resources.</span></span> <span data-ttu-id="f8f3a-131">Om hello auktorisering server som används av din API skapar [JWT-tokens](http://jwt.io/) som en del av dess OAuth2-flöde som [Azure Active Directory](../active-directory/active-directory-aadconnect.md) matchar, kan du använda hello `validate-jwt` princip tooverify hello giltighet hello-token.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-131">If hello authorization server used by your API creates [JWT tokens](http://jwt.io/) as part of its OAuth2 flow, as [Azure Active Directory](../active-directory/active-directory-aadconnect.md) does, then you can use hello `validate-jwt` policy tooverify hello validity of hello token.</span></span> <span data-ttu-id="f8f3a-132">Men vissa auktorisering servrar skapar vad kallas [referera token](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) som inte kan verifieras utan att göra en anropet tillbaka toohello auktorisering server.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-132">However, some authorization servers create what are called [reference tokens](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) that cannot be verified without making a call back toohello authorization server.</span></span>

### <a name="standardized-introspection"></a><span data-ttu-id="f8f3a-133">Standardiserade introspection</span><span class="sxs-lookup"><span data-stu-id="f8f3a-133">Standardized introspection</span></span>
<span data-ttu-id="f8f3a-134">I hello som tidigare har funnits ingen standardiserade möjlighet att kontrollera en referens-token med en server för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-134">In hello past there has been no standardized way of verifying a reference token with an authorization server.</span></span> <span data-ttu-id="f8f3a-135">Men en nyligen föreslagen standard [RFC 7662](https://tools.ietf.org/html/rfc7662) publicerades av hello IETF som definierar hur en resurs-server kan verifiera hello giltigheten för en token.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-135">However a recently proposed standard [RFC 7662](https://tools.ietf.org/html/rfc7662) was published by hello IETF that defines how a resource server can verify hello validity of a token.</span></span>

### <a name="extracting-hello-token"></a><span data-ttu-id="f8f3a-136">Extrahera hello-token</span><span class="sxs-lookup"><span data-stu-id="f8f3a-136">Extracting hello token</span></span>
<span data-ttu-id="f8f3a-137">hello första steget är tooextract hello token från hello Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-137">hello first step is tooextract hello token from hello Authorization header.</span></span> <span data-ttu-id="f8f3a-138">hello huvudets värde bör vara formaterad med hello `Bearer` auktoriseringsschema, ett blanksteg och sedan hello auktorisering token enligt [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span><span class="sxs-lookup"><span data-stu-id="f8f3a-138">hello header value should be formatted with hello `Bearer` authorization scheme, a single space and then hello authorization token as per [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1).</span></span> <span data-ttu-id="f8f3a-139">Det finns tyvärr fall där hello auktorisering schemat utelämnas.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-139">Unfortunately there are cases where hello authorization scheme is omitted.</span></span> <span data-ttu-id="f8f3a-140">tooaccount för den här vid parsning av, vi dela hello huvudvärde på ett blanksteg och välj hello sista strängen från hello returneras en matris med strängar.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-140">tooaccount for this when parsing, we split hello header value on a space and select hello last string from hello returned array of strings.</span></span> <span data-ttu-id="f8f3a-141">Det finns en lösning för felaktigt formaterat auktorisering huvuden.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-141">This provides a workaround for badly formatted authorization headers.</span></span>

```xml
<set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />
```

### <a name="making-hello-validation-request"></a><span data-ttu-id="f8f3a-142">Hello begäran validering</span><span class="sxs-lookup"><span data-stu-id="f8f3a-142">Making hello validation request</span></span>
<span data-ttu-id="f8f3a-143">När vi har hello autentiseringstoken kan göra vi hello begäran toovalidate hello token.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-143">Once we have hello authorization token, we can make hello request toovalidate hello token.</span></span> <span data-ttu-id="f8f3a-144">RFC 7662 anropar den här processen introspection och kräver att du `POST` en HTML-formulär toohello introspection resurs.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-144">RFC 7662 calls this process introspection and requires that you `POST` a HTML form toohello introspection resource.</span></span> <span data-ttu-id="f8f3a-145">hello HTML-formulär minst måste innehålla ett nyckel/värde-par med hello nyckel `token`.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-145">hello HTML form must at least contain a key/value pair with hello key `token`.</span></span> <span data-ttu-id="f8f3a-146">Den här begäran toohello auktorisering servern måste också vara autentiserade tooensure som skadlig klienter inte kan gå fiske för giltig token.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-146">This request toohello authorization server must also be authenticated tooensure that malicious clients cannot go trawling for valid tokens.</span></span>

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

### <a name="checking-hello-response"></a><span data-ttu-id="f8f3a-147">Kontrollerar hello-svar</span><span class="sxs-lookup"><span data-stu-id="f8f3a-147">Checking hello response</span></span>
<span data-ttu-id="f8f3a-148">Hej `response-variable-name` attributet är används toogive åtkomst hello returnerade svar.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-148">hello `response-variable-name` attribute is used toogive access hello returned response.</span></span> <span data-ttu-id="f8f3a-149">hello namn har definierats i den här egenskapen kan användas som en nyckel till hello `context.Variables` ordlista tooaccess hello `IResponse` objekt.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-149">hello name defined in this property can be used as a key into hello `context.Variables` dictionary tooaccess hello `IResponse` object.</span></span>

<span data-ttu-id="f8f3a-150">Från hello svarsobjekt kan vi hämta hello brödtext och RFC 7622 talar om för oss att hello svaret måste vara ett JSON-objekt och måste innehålla minst en egenskap som kallas `active` som är ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-150">From hello response object we can retrieve hello body and RFC 7622 tells us that hello response must be a JSON object and must contain at least a property called `active` that is a boolean value.</span></span> <span data-ttu-id="f8f3a-151">När `active` är true och sedan hello token ska vara giltigt.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-151">When `active` is true then hello token is considered valid.</span></span>

### <a name="reporting-failure"></a><span data-ttu-id="f8f3a-152">Rapportera fel</span><span class="sxs-lookup"><span data-stu-id="f8f3a-152">Reporting failure</span></span>
<span data-ttu-id="f8f3a-153">Vi använder en `<choose>` princip toodetect om hello token är ogiltig och i så fall returneras ett 401 svar.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-153">We use a `<choose>` policy toodetect if hello token is invalid and if so, return a 401 response.</span></span>

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

<span data-ttu-id="f8f3a-154">Enligt [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) som beskriver hur `bearer` token ska användas, vi också returnera en `WWW-Authenticate` huvud med hello 401 svar.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-154">As per [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) which describes how `bearer` tokens should be used, we also return a `WWW-Authenticate` header with hello 401 response.</span></span> <span data-ttu-id="f8f3a-155">hello WWW-autentisering är avsedd tooinstruct en klient på hur tooconstruct en korrekt godkände begäran.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-155">hello WWW-Authenticate is intended tooinstruct a client on how tooconstruct a properly authorized request.</span></span> <span data-ttu-id="f8f3a-156">På grund av toohello flera olika metoder som är möjligt med hello OAuth2 framework, är det svårt toocommunicate alla hello behövs information.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-156">Due toohello wide variety of approaches possible with hello OAuth2 framework, it is difficult toocommunicate all hello needed information.</span></span> <span data-ttu-id="f8f3a-157">Lyckligtvis har pågår toohelp [klienter identifiera hur tooproperly godkänna begäranden tooa resursservern](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span><span class="sxs-lookup"><span data-stu-id="f8f3a-157">Fortunately there are efforts underway toohelp [clients discover how tooproperly authorize requests tooa resource server](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).</span></span>

### <a name="final-solution"></a><span data-ttu-id="f8f3a-158">Slutlig lösning</span><span class="sxs-lookup"><span data-stu-id="f8f3a-158">Final solution</span></span>
<span data-ttu-id="f8f3a-159">Kombinera alla hello delar hämta vi hello följande princip:</span><span class="sxs-lookup"><span data-stu-id="f8f3a-159">Putting all hello pieces together, we get hello following policy:</span></span>

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

<span data-ttu-id="f8f3a-160">Detta är endast ett av flera exempel på hur hello `send-request` policy kan vara användbart används toointegrate externa tjänster till hello av begäranden och -svar som passerar genom hello API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-160">This is only one of many examples of how hello `send-request` policy can be used toointegrate useful external services into hello process of requests and responses flowing through hello API Management service.</span></span>

## <a name="response-composition"></a><span data-ttu-id="f8f3a-161">Svaret sammansättning</span><span class="sxs-lookup"><span data-stu-id="f8f3a-161">Response Composition</span></span>
<span data-ttu-id="f8f3a-162">Hej `send-request` principen kan användas för att utöka primära förfrågningen tooa backend system som vi såg i hello föregående exempel, eller den kan användas som en fullständig Ersätt för hello backend-anrop.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-162">hello `send-request` policy can be used for enhancing a primary request tooa backend system, as we saw in hello previous example, or it can be used as a complete replace for of hello backend call.</span></span> <span data-ttu-id="f8f3a-163">Med den här tekniken kan vi enkelt skapa sammansatta resurser som aggregeras från flera olika system.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-163">Using this technique we can easily create composite resources that are aggregated from multiple different systems.</span></span>

### <a name="building-a-dashboard"></a><span data-ttu-id="f8f3a-164">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="f8f3a-164">Building a dashboard</span></span>
<span data-ttu-id="f8f3a-165">Ibland vill du toobe kan tooexpose information som finns i flera backend-system, till exempel toodrive en instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-165">Sometimes you want toobe able tooexpose information that exists in multiple backend systems, for example, toodrive a dashboard.</span></span> <span data-ttu-id="f8f3a-166">hello KPI: er som kommer från alla olika-servrar, men du föredrar att inte tooprovide direktåtkomst toothem och det är bra om gick att hämta alla hello information i en enskild begäran.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-166">hello KPIs come from all different back-ends, but you would prefer not tooprovide direct access toothem and it would be nice if all hello information could be retrieved in a single request.</span></span> <span data-ttu-id="f8f3a-167">Vissa av hello backend information behöver kanske vissa segmentering och tärning och lite Rensa först!</span><span class="sxs-lookup"><span data-stu-id="f8f3a-167">Perhaps some of hello backend information needs some slicing and dicing and a little sanitizing first!</span></span> <span data-ttu-id="f8f3a-168">Som kan toocache att sammansatta resursen är en användbar tooreduce hello backend läses in som du vet att användarna har en vana hammering hello F5-tangenten i ordning toosee om deras underperforming mått kan ändras.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-168">Being able toocache that composite resource would be a useful tooreduce hello backend load as you know users have a habit of hammering hello F5 key in order toosee if their underperforming metrics might change.</span></span>    

### <a name="faking-hello-resource"></a><span data-ttu-id="f8f3a-169">Faking hello-resurs</span><span class="sxs-lookup"><span data-stu-id="f8f3a-169">Faking hello resource</span></span>
<span data-ttu-id="f8f3a-170">Hej första steg toobuilding våra instrumentpanelen resursen är tooconfigure en ny åtgärd i hello API Management publisher portal.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-170">hello first step toobuilding our dashboard resource is tooconfigure a new operation in hello API Management publisher portal.</span></span> <span data-ttu-id="f8f3a-171">Detta är en platshållare för åtgärden används tooconfigure våra sammansättning princip toobuild våra dynamisk resurs.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-171">This will be a placeholder operation used tooconfigure our composition policy toobuild our dynamic resource.</span></span>

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-hello-requests"></a><span data-ttu-id="f8f3a-173">Hello-begäranden</span><span class="sxs-lookup"><span data-stu-id="f8f3a-173">Making hello requests</span></span>
<span data-ttu-id="f8f3a-174">En gång hello `dashboard` åtgärden har skapats kan vi konfigurera en princip för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-174">Once hello `dashboard` operation has been created we can configure a policy specifically for that operation.</span></span> 

![Instrumentpanel för åtgärden](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

<span data-ttu-id="f8f3a-176">hello första steget är tooextract alla frågeparametrar Hej inkommande begäran, så att vi kan vidarebefordra dem tooour backend.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-176">hello first step  is tooextract any query parameters from hello incoming request, so that we can forward them tooour backend.</span></span> <span data-ttu-id="f8f3a-177">I det här exemplet är vår instrumentpanelen visar information baserat på en viss tidsperiod en därför har en `fromDate` och `toDate` parameter.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-177">In this example our dashboard is showing information based on a period of time an therefore has a `fromDate` and `toDate` parameter.</span></span> <span data-ttu-id="f8f3a-178">Vi kan använda hello `set-variable` principinformation tooextract hello från hello URL-begäran.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-178">We can use hello `set-variable` policy tooextract hello information from hello request URL.</span></span>

```xml
<set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
<set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">
```

<span data-ttu-id="f8f3a-179">När vi har den här informationen kan vi göra begäranden tooall hello serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-179">Once we have this information we can make requests tooall hello backend systems.</span></span> <span data-ttu-id="f8f3a-180">Varje begäran skapar en ny URL med hello parameterinformation och anropar dess respektive server och lagrar hello svar i en variabel i kontexten.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-180">Each request constructs a new URL with hello parameter information and calls its respective server and stores hello response in a context variable.</span></span>

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

<span data-ttu-id="f8f3a-181">Dessa begäranden körs i sekvens, vilket inte är idealiskt.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-181">These requests will execute in sequence, which is not ideal.</span></span> <span data-ttu-id="f8f3a-182">I en kommande version kommer vi introducerar en ny princip som kallas `wait` som gör att alla dessa begäranden tooexecute parallellt.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-182">In an upcoming release we will be introducing a new policy called `wait` that will enable all of these requests tooexecute in parallel.</span></span>

### <a name="responding"></a><span data-ttu-id="f8f3a-183">Svara</span><span class="sxs-lookup"><span data-stu-id="f8f3a-183">Responding</span></span>
<span data-ttu-id="f8f3a-184">tooconstruct hello sammansatta svar vi kan använda hello [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) princip.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-184">tooconstruct hello composite response we can use hello [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policy.</span></span> <span data-ttu-id="f8f3a-185">Hej `set-body` element kan använda ett uttryck tooconstruct en ny `JObject` med alla hello komponenten garantier inbäddade som egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-185">hello `set-body` element can use an expression tooconstruct a new `JObject` with all hello component representations embedded as properties.</span></span>

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

<span data-ttu-id="f8f3a-186">hello hela policyn ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="f8f3a-186">hello complete policy looks as follows:</span></span>

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

<span data-ttu-id="f8f3a-187">I hello konfigurationen av hello platshållare åtgärden vi konfigurera cachelagras hello instrumentpanelen resurs toobe för minst en timme eftersom vi förstår hello uppbyggnad hello data innebär att även om det är en timme som är inaktuellt, fortfarande är tillräckligt effektivt tooconvey värdefull information toohello användare.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-187">In hello configuration of hello placeholder operation we can configure hello dashboard resource toobe cached for at least an hour because we understand hello nature of hello data means that even if it is an hour out of date, it will still be sufficiently effective tooconvey valuable information toohello users.</span></span>

## <a name="summary"></a><span data-ttu-id="f8f3a-188">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f8f3a-188">Summary</span></span>
<span data-ttu-id="f8f3a-189">Azure API Management-tjänsten ger flexibla principer som kan vara selektivt används tooHTTP trafik och möjliggör sammansättning av backend-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-189">Azure API Management service provides flexible policies that can be selectively applied tooHTTP traffic and enables composition of backend services.</span></span> <span data-ttu-id="f8f3a-190">Om du vill tooenhance din API-gateway med aviseringar funktion, verifiering, verifiering funktioner eller skapa nya sammansatta resurser baserat på flera backend-tjänster, hello `send-request` och tillhörande principer öppnar en värld av möjligheter.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-190">Whether you want tooenhance your API gateway with alerting functions, verification, validation capabilities or create new composite resources based on multiple backend services, hello `send-request` and related policies open a world of possibilities.</span></span>

## <a name="watch-a-video-overview-of-these-policies"></a><span data-ttu-id="f8f3a-191">Titta på en videoöversikt över dessa principer</span><span class="sxs-lookup"><span data-stu-id="f8f3a-191">Watch a video overview of these policies</span></span>
<span data-ttu-id="f8f3a-192">Mer information om hello [-en-sätt-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), och [returnera svar](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) principer som beskrivs i den här artikeln får du titta på hello följande video.</span><span class="sxs-lookup"><span data-stu-id="f8f3a-192">For more information on hello [send-one-way-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest), and [return-response](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) policies covered in this article, please watch hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Send-Request-and-Return-Response-Policies/player]
> 
> 

