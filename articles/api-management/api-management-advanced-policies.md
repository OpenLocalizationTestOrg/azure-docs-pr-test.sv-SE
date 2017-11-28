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
# <a name="api-management-advanced-policies"></a><span data-ttu-id="fc237-103">API Management avancerade principer</span><span class="sxs-lookup"><span data-stu-id="fc237-103">API Management advanced policies</span></span>
<span data-ttu-id="fc237-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="fc237-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="fc237-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="fc237-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="fc237-106"><a name="AdvancedPolicies"></a>Avancerade principer</span><span class="sxs-lookup"><span data-stu-id="fc237-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="fc237-107">[Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principrapporter utifrån hello resultat av hello utvärdering av boolesk [uttryck](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="fc237-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on hello results of hello evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="fc237-108">[Vidarebefordra begäran](#ForwardRequest) -vidarebefordrar hello begäran toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="fc237-108">[Forward request](#ForwardRequest) - Forwards hello request toohello backend service.</span></span>

-   <span data-ttu-id="fc237-109">[Begränsa samtidighet](#LimitConcurrency) -förhindrar omslutna principer från att köras med mer än hello angivet antal begäranden i taget.</span><span class="sxs-lookup"><span data-stu-id="fc237-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than hello specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="fc237-110">[Logga tooEvent hubb](#log-to-eventhub) -skickar meddelanden i hello angetts format tooan Event Hub definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="fc237-110">[Log tooEvent Hub](#log-to-eventhub) - Sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="fc237-111">[Mock svar](#mock-response) -avbryts körningen i pipeline och returnerar svaret mocked direkt toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fc237-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly toohello caller.</span></span>
  
-   <span data-ttu-id="fc237-112">[Försök](#Retry) -återförsök körningen av hello omslutna hanteringsprinciper, om och tills hello villkor är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="fc237-112">[Retry](#Retry) - Retries execution of hello enclosed policy statements, if and until hello condition is met.</span></span> <span data-ttu-id="fc237-113">Körningen upprepas på hello angivna intervall och in toohello angetts antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="fc237-113">Execution will repeat at hello specified time intervals and up toohello specified retry count.</span></span>  
  
-   <span data-ttu-id="fc237-114">[Returnera svar](#ReturnResponse) -avbryts pipeline körning och returnerar hello angetts svar direkt toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fc237-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns hello specified response directly toohello caller.</span></span> 
  
-   <span data-ttu-id="fc237-115">[Skicka förfrågan om enkelriktade](#SendOneWayRequest) -skickar en begäran toohello specificerat URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="fc237-115">[Send one way request](#SendOneWayRequest) - Sends a request toohello specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="fc237-116">[Skicka förfrågan](#SendRequest) -skickar en begäran toohello specificerat URL: en.</span><span class="sxs-lookup"><span data-stu-id="fc237-116">[Send request](#SendRequest) - Sends a request toohello specified URL.</span></span>  

-   <span data-ttu-id="fc237-117">[Ange HTTP-proxy](#SetHttpProxy) -tillåter tooroute vidarebefordrade begäranden via en HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="fc237-117">[Set HTTP proxy](#SetHttpProxy) - Allows you tooroute forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="fc237-118">[Ange metoden](#SetRequestMethod) -tillåter toochange hello HTTP-metoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-118">[Set request method](#SetRequestMethod) - Allows you toochange hello HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="fc237-119">[Ange statuskoden](#SetStatus) -ändringar hello HTTP-status kod toohello angivet värde.</span><span class="sxs-lookup"><span data-stu-id="fc237-119">[Set status code](#SetStatus) - Changes hello HTTP status code toohello specified value.</span></span>  
  
-   <span data-ttu-id="fc237-120">[Ange variabel](api-management-advanced-policies.md#set-variable) -kvarstår ett värde i en namngiven [kontexten](api-management-policy-expressions.md#ContextVariables) variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="fc237-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="fc237-121">[Spåra](#Trace) -lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="fc237-121">[Trace](#Trace) - Adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="fc237-122">[Vänta](#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer toocomplete innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="fc237-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies toocomplete before proceeding.</span></span>  
  
##  <span data-ttu-id="fc237-123"><a name="choose"></a>Kontrollflöde</span><span class="sxs-lookup"><span data-stu-id="fc237-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="fc237-124">Hej `choose` principen gäller omslutna princip uttryck baserat på hello resultatet av bedömning av booleska uttryck, liknande tooan if-then-else eller växel konstruera i ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="fc237-124">hello `choose` policy applies enclosed policy statements based on hello outcome of evaluation of Boolean expressions, similar tooan if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="fc237-125"><a name="ChoosePolicyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
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
  
 <span data-ttu-id="fc237-126">hello princip för åtkomstkontroll flödet måste innehålla minst ett `<when/>` element.</span><span class="sxs-lookup"><span data-stu-id="fc237-126">hello control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="fc237-127">Hej `<otherwise/>` element är valfria.</span><span class="sxs-lookup"><span data-stu-id="fc237-127">hello `<otherwise/>` element is optional.</span></span> <span data-ttu-id="fc237-128">Villkoren i `<when/>` element utvärderas i ordning efter deras utseende i hello princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-128">Conditions in `<when/>` elements are evaluated in order of their appearance within hello policy.</span></span> <span data-ttu-id="fc237-129">Principen instruktion(er) omgiven hello först `<when/>` element med villkoret attribut är lika med `true` tillämpas.</span><span class="sxs-lookup"><span data-stu-id="fc237-129">Policy statement(s) enclosed within hello first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="fc237-130">Principer för omgiven hello `<otherwise/>` element, i förekommande fall, kommer att tillämpas om alla av hello `<when/>` elementattribut för villkoret är `false`.</span><span class="sxs-lookup"><span data-stu-id="fc237-130">Policies enclosed within hello `<otherwise/>` element, if present, will be applied if all of hello `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="fc237-131">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-131">Examples</span></span>  
  
####  <span data-ttu-id="fc237-132"><a name="ChooseExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="fc237-133">hello exemplet nedan visar en [ange variabel](api-management-advanced-policies.md#set-variable) principen och två principer för åtkomstkontroll flödet.</span><span class="sxs-lookup"><span data-stu-id="fc237-133">hello following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="fc237-134">hello Ange variabeln princip är i hello inkommande avsnittet och skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som anges tootrue om hello `User-Agent` begäran huvudet innehåller hello text `iPad` eller `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="fc237-134">hello set variable policy is in hello inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="fc237-135">hello första kontrollen flödet principen är i hello inkommande avsnittet och villkorligt gäller en av två [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) principer beroende på hello värdet för hello `isMobile` kontexten variabeln.</span><span class="sxs-lookup"><span data-stu-id="fc237-135">hello first control flow policy is also in hello inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on hello value of hello `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="fc237-136">hello andra kontrollen flödet princip är utgående under hello och villkorligt gäller hello [konvertera XML-tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) principen när `isMobile` har angetts för`true`.</span><span class="sxs-lookup"><span data-stu-id="fc237-136">hello second control flow policy is in hello outbound section and conditionally applies hello [Convert XML tooJSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set too`true`.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="fc237-137">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-137">Example</span></span>  
 <span data-ttu-id="fc237-138">Det här exemplet illustrerar hur tooperform innehållsfiltrering genom att ta bort dataelement från hello svar togs emot från hello backend-tjänsten när du använder hello `Starter` produkten.</span><span class="sxs-lookup"><span data-stu-id="fc237-138">This example shows how tooperform content filtering by removing data elements from hello response received from hello backend service when using hello `Starter` product.</span></span> <span data-ttu-id="fc237-139">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och framåt too34:30.</span><span class="sxs-lookup"><span data-stu-id="fc237-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward too34:30.</span></span> <span data-ttu-id="fc237-140">Starta en översikt över vid 31:50 toosee [hello mörkt Sky prognos API](https://developer.forecast.io/) används för den här demon.</span><span class="sxs-lookup"><span data-stu-id="fc237-140">Start  at 31:50 toosee an overview of [hello Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-141">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-141">Elements</span></span>  
  
|<span data-ttu-id="fc237-142">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-142">Element</span></span>|<span data-ttu-id="fc237-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-143">Description</span></span>|<span data-ttu-id="fc237-144">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-145">Välj</span><span class="sxs-lookup"><span data-stu-id="fc237-145">choose</span></span>|<span data-ttu-id="fc237-146">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-146">Root element.</span></span>|<span data-ttu-id="fc237-147">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-147">Yes</span></span>|  
|<span data-ttu-id="fc237-148">När</span><span class="sxs-lookup"><span data-stu-id="fc237-148">when</span></span>|<span data-ttu-id="fc237-149">Hej villkoret toouse för hello `if` eller `ifelse` delar av hello `choose` princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-149">hello condition toouse for hello `if` or `ifelse` parts of hello `choose` policy.</span></span> <span data-ttu-id="fc237-150">Om hello `choose` princip har flera `when` avsnitt, de utvärderas i turordning.</span><span class="sxs-lookup"><span data-stu-id="fc237-150">If hello `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="fc237-151">En gång hello `condition` av ett när utvärderar element för`true`, ingen ytterligare `when` villkor utvärderas.</span><span class="sxs-lookup"><span data-stu-id="fc237-151">Once hello `condition` of a when element evaluates too`true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="fc237-152">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-152">Yes</span></span>|  
|<span data-ttu-id="fc237-153">Annars</span><span class="sxs-lookup"><span data-stu-id="fc237-153">otherwise</span></span>|<span data-ttu-id="fc237-154">Innehåller hello princip fragment toobe används om inget av hello `when` villkor utvärdera för`true`.</span><span class="sxs-lookup"><span data-stu-id="fc237-154">Contains hello policy snippet toobe used if none of hello `when` conditions evaluate too`true`.</span></span>|<span data-ttu-id="fc237-155">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-156">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-156">Attributes</span></span>  
  
|<span data-ttu-id="fc237-157">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-157">Attribute</span></span>|<span data-ttu-id="fc237-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-158">Description</span></span>|<span data-ttu-id="fc237-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="fc237-160">villkor = ”booleskt uttryck &#124; Booleskt konstant ”</span><span class="sxs-lookup"><span data-stu-id="fc237-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="fc237-161">hello booleskt uttryck eller konstant tooevaluated när hello som innehåller `when` Principframställning utvärderas.</span><span class="sxs-lookup"><span data-stu-id="fc237-161">hello Boolean expression or constant tooevaluated when hello containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="fc237-162">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-162">Yes</span></span>|  
  
###  <span data-ttu-id="fc237-163"><a name="ChooseUsage"></a>Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="fc237-164">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-164">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-165">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-166">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-167"><a name="ForwardRequest"></a>Vidarebefordra begäran</span><span class="sxs-lookup"><span data-stu-id="fc237-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="fc237-168">Hej `forward-request` princip vidarebefordrar hello inkommande begäran toohello serverdelstjänst anges i begäran hello [kontexten](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="fc237-168">hello `forward-request` policy forwards hello incoming request toohello backend service specified in hello request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="fc237-169">hello backend-tjänstens URL anges i hello API [inställningar](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) och kan ändras med hjälp av hello [ange serverdelstjänst](api-management-transformation-policies.md) princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-169">hello backend service URL is specified in hello API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using hello [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="fc237-170">Ta bort den här grupprincipresultat i hello begäran inte vidarebefordras toohello backend-tjänsten och hello principer utgående under hello utvärderas direkt vid hello slutförande av hello principer i hello inkommande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fc237-170">Removing this policy results in hello request not being forwarded toohello backend service and hello policies in hello outbound section are evaluated immediately upon hello successful completion of hello policies in hello inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-171">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="fc237-172">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="fc237-173">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-173">Example</span></span>  
 <span data-ttu-id="fc237-174">hello vidarebefordras följande API säkerhetsnivå för alla toohello serverdelstjänst med en timeout på 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="fc237-174">hello following API level policy forwards all requests toohello backend service with a timeout interval of 60 seconds.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="fc237-175">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-175">Example</span></span>  
 <span data-ttu-id="fc237-176">Den här åtgärden säkerhetsnivå för använder hello `base` elementet tooinherit hello backend princip från hello överordnade API-nivå omfattningen.</span><span class="sxs-lookup"><span data-stu-id="fc237-176">This operation level policy uses hello `base` element tooinherit hello backend policy from hello parent API level scope.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="fc237-177">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-177">Example</span></span>  
 <span data-ttu-id="fc237-178">Den här åtgärden säkerhetsnivå för uttryckligen vidarebefordrar alla begäranden toohello serverdelstjänst med en tidsgräns på 120 och ärver inte hello överordnade API-nivå backend-princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-178">This operation level policy explicitly forwards all requests toohello backend service with a timeout of 120 and does not inherit hello parent API level backend policy.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="fc237-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-179">Example</span></span>  
 <span data-ttu-id="fc237-180">Den här åtgärden säkerhetsnivå för inte vidarebefordrar begäranden toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="fc237-180">This operation level policy does not forward requests toohello backend service.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-181">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-181">Elements</span></span>  
  
|<span data-ttu-id="fc237-182">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-182">Element</span></span>|<span data-ttu-id="fc237-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-183">Description</span></span>|<span data-ttu-id="fc237-184">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-185">vidarebefordra begäran</span><span class="sxs-lookup"><span data-stu-id="fc237-185">forward-request</span></span>|<span data-ttu-id="fc237-186">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-186">Root element.</span></span>|<span data-ttu-id="fc237-187">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-188">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-188">Attributes</span></span>  
  
|<span data-ttu-id="fc237-189">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-189">Attribute</span></span>|<span data-ttu-id="fc237-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-190">Description</span></span>|<span data-ttu-id="fc237-191">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-191">Required</span></span>|<span data-ttu-id="fc237-192">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-193">timeout = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="fc237-193">timeout="integer"</span></span>|<span data-ttu-id="fc237-194">Det går inte att hello timeout-intervall i sekunder innan hello anropet toohello serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="fc237-194">hello timeout interval in seconds before hello call toohello backend service fails.</span></span>|<span data-ttu-id="fc237-195">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-195">No</span></span>|<span data-ttu-id="fc237-196">Ingen tidsgräns</span><span class="sxs-lookup"><span data-stu-id="fc237-196">No timeout</span></span>|  
|<span data-ttu-id="fc237-197">Följ omdirigeringar = ”true &#124; FALSE ”</span><span class="sxs-lookup"><span data-stu-id="fc237-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="fc237-198">Anger om omdirigeringar från hello serverdelstjänst är följt av hello gateway eller returnerade toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fc237-198">Specifies whether redirects from hello backend service are followed by hello gateway or returned toohello caller.</span></span>|<span data-ttu-id="fc237-199">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-199">No</span></span>|<span data-ttu-id="fc237-200">FALSKT</span><span class="sxs-lookup"><span data-stu-id="fc237-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-201">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-201">Usage</span></span>  
 <span data-ttu-id="fc237-202">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-202">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-203">**Avsnitt i princip:** backend</span><span class="sxs-lookup"><span data-stu-id="fc237-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="fc237-204">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-205"><a name="LimitConcurrency"></a>Gränsen för samtidighet</span><span class="sxs-lookup"><span data-stu-id="fc237-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="fc237-206">Hej `limit-concurrency` princip förhindrar att omslutna principer körning av fler än det angivna antalet förfrågningar hello vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="fc237-206">hello `limit-concurrency` policy prevents enclosed policies from executing by more than hello specified number of requests at a given time.</span></span> <span data-ttu-id="fc237-207">Vid överstiger tröskelvärdet hello läggs nya begäranden tooa kö tills hello maximal Kölängd uppnås.</span><span class="sxs-lookup"><span data-stu-id="fc237-207">Upon exceeding hello threshold, new requests are added tooa queue, until hello maximum queue length is achieved.</span></span> <span data-ttu-id="fc237-208">När kön uttömning misslyckas nya begäranden omedelbart.</span><span class="sxs-lookup"><span data-stu-id="fc237-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="fc237-209"><a name="LimitConcurrencyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="fc237-210">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-210">Examples</span></span>  
  
####  <span data-ttu-id="fc237-211"><a name="ChooseExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="fc237-212">hello visar exemplet nedan hur toolimit antal begäranden vidarebefordras tooa backend baserat på hello värde för en variabel i kontexten.</span><span class="sxs-lookup"><span data-stu-id="fc237-212">hello following example demonstrates how toolimit number of requests forwarded tooa backend based on hello value of a context variable.</span></span>
 
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

### <a name="elements"></a><span data-ttu-id="fc237-213">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-213">Elements</span></span>  
  
|<span data-ttu-id="fc237-214">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-214">Element</span></span>|<span data-ttu-id="fc237-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-215">Description</span></span>|<span data-ttu-id="fc237-216">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="fc237-217">gränsen för samtidighet</span><span class="sxs-lookup"><span data-stu-id="fc237-217">limit-concurrency</span></span>|<span data-ttu-id="fc237-218">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-218">Root element.</span></span>|<span data-ttu-id="fc237-219">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-220">Attributes</span></span>  
  
|<span data-ttu-id="fc237-221">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-221">Attribute</span></span>|<span data-ttu-id="fc237-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-222">Description</span></span>|<span data-ttu-id="fc237-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-223">Required</span></span>|<span data-ttu-id="fc237-224">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="fc237-225">key</span><span class="sxs-lookup"><span data-stu-id="fc237-225">key</span></span>|<span data-ttu-id="fc237-226">En sträng.</span><span class="sxs-lookup"><span data-stu-id="fc237-226">A string.</span></span> <span data-ttu-id="fc237-227">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="fc237-227">Expression allowed.</span></span> <span data-ttu-id="fc237-228">Anger hello samtidighet scope.</span><span class="sxs-lookup"><span data-stu-id="fc237-228">Specifies hello concurrency scope.</span></span> <span data-ttu-id="fc237-229">Kan delas av flera principer.</span><span class="sxs-lookup"><span data-stu-id="fc237-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="fc237-230">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-230">Yes</span></span>|<span data-ttu-id="fc237-231">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-231">N/A</span></span>|  
|<span data-ttu-id="fc237-232">Max antal</span><span class="sxs-lookup"><span data-stu-id="fc237-232">max-count</span></span>|<span data-ttu-id="fc237-233">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="fc237-233">An integer.</span></span> <span data-ttu-id="fc237-234">Anger maximalt antal begäranden som tillåts tooenter hello princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-234">Specifies a maximum number of requests that are allowed tooenter hello policy.</span></span>|<span data-ttu-id="fc237-235">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-235">Yes</span></span>|<span data-ttu-id="fc237-236">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-236">N/A</span></span>|  
|<span data-ttu-id="fc237-237">timeout</span><span class="sxs-lookup"><span data-stu-id="fc237-237">timeout</span></span>|<span data-ttu-id="fc237-238">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="fc237-238">An integer.</span></span> <span data-ttu-id="fc237-239">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="fc237-239">Expression allowed.</span></span> <span data-ttu-id="fc237-240">Anger hello antalet sekunder som en begäran ska vänta tooenter ett scope innan åtgärden misslyckas med ”403 för många begäranden”</span><span class="sxs-lookup"><span data-stu-id="fc237-240">Specifies hello number of seconds a request should wait tooenter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="fc237-241">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-241">No</span></span>|<span data-ttu-id="fc237-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="fc237-242">Infinity</span></span>|  
|<span data-ttu-id="fc237-243">Max Kölängd</span><span class="sxs-lookup"><span data-stu-id="fc237-243">max-queue-length</span></span>|<span data-ttu-id="fc237-244">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="fc237-244">An integer.</span></span> <span data-ttu-id="fc237-245">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="fc237-245">Expression allowed.</span></span> <span data-ttu-id="fc237-246">Anger hello maximala längd.</span><span class="sxs-lookup"><span data-stu-id="fc237-246">Specifies hello maximum queue length.</span></span> <span data-ttu-id="fc237-247">Inkommande begäranden försök tooenter denna policy kommer att avslutas med ”403 för många begäranden” omedelbart när hello kön är slut.</span><span class="sxs-lookup"><span data-stu-id="fc237-247">Incoming requests trying tooenter this policy will be terminated with “403 Too Many Requests” immediately when hello queue is exhausted.</span></span>|<span data-ttu-id="fc237-248">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-248">No</span></span>|<span data-ttu-id="fc237-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="fc237-249">Infinity</span></span>|  
  
###  <span data-ttu-id="fc237-250"><a name="ChooseUsage"></a>Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="fc237-251">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-251">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-252">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-253">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="fc237-254"><a name="log-to-eventhub"></a>Logga tooEvent Hub</span><span class="sxs-lookup"><span data-stu-id="fc237-254"><a name="log-to-eventhub"></a> Log tooEvent Hub</span></span>  
 <span data-ttu-id="fc237-255">Hej `log-to-eventhub` princip skickar meddelanden i hello angetts format tooan Event Hub definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="fc237-255">hello `log-to-eventhub` policy sends messages in hello specified format tooan Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="fc237-256">Som namnet antyder används hello principen för att spara valda begäran eller svar omständighetsinformation för analys online eller offline.</span><span class="sxs-lookup"><span data-stu-id="fc237-256">As its name implies, hello policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="fc237-257">Stegvisa instruktioner om hur du konfigurerar en händelsehubb och loggning av händelser, se [hur toolog API Management händelser med Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="fc237-257">For a step-by-step guide on configuring an event hub and logging events, see [How toolog API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-258">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of hello logger entity" partition-id="index of hello partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string toobe logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-259">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-259">Example</span></span>  
 <span data-ttu-id="fc237-260">Valfri sträng kan användas som hello värdet toobe loggas i Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="fc237-260">Any string can be used as hello value toobe logged in Event Hubs.</span></span> <span data-ttu-id="fc237-261">I det här exemplet hello datum och tid tjänstnamn för distribution, förfrågnings-id, ip-adress och åtgärdsnamn för alla inkommande samtal är loggade toohello händelsehubb loggaren registrerats med hello `contoso-logger` id.</span><span class="sxs-lookup"><span data-stu-id="fc237-261">In this example hello date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged toohello event hub Logger registered with hello `contoso-logger` id.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-262">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-262">Elements</span></span>  
  
|<span data-ttu-id="fc237-263">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-263">Element</span></span>|<span data-ttu-id="fc237-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-264">Description</span></span>|<span data-ttu-id="fc237-265">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-266">loggen till eventhub</span><span class="sxs-lookup"><span data-stu-id="fc237-266">log-to-eventhub</span></span>|<span data-ttu-id="fc237-267">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-267">Root element.</span></span> <span data-ttu-id="fc237-268">hello-värdet för det här elementet är hello sträng toolog tooyour händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="fc237-268">hello value of this element is hello string toolog tooyour event hub.</span></span>|<span data-ttu-id="fc237-269">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-270">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-270">Attributes</span></span>  
  
|<span data-ttu-id="fc237-271">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-271">Attribute</span></span>|<span data-ttu-id="fc237-272">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-272">Description</span></span>|<span data-ttu-id="fc237-273">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="fc237-274">loggaren-id</span><span class="sxs-lookup"><span data-stu-id="fc237-274">logger-id</span></span>|<span data-ttu-id="fc237-275">hello-id för hello loggaren registrerats API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="fc237-275">hello id of hello Logger registered with your API Management service.</span></span>|<span data-ttu-id="fc237-276">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-276">Yes</span></span>|  
|<span data-ttu-id="fc237-277">partitions-id</span><span class="sxs-lookup"><span data-stu-id="fc237-277">partition-id</span></span>|<span data-ttu-id="fc237-278">Anger hello index för hello partition där meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="fc237-278">Specifies hello index of hello partition where messages are sent.</span></span>|<span data-ttu-id="fc237-279">Valfri.</span><span class="sxs-lookup"><span data-stu-id="fc237-279">Optional.</span></span> <span data-ttu-id="fc237-280">Det här attributet kan inte användas om `partition-key` används.</span><span class="sxs-lookup"><span data-stu-id="fc237-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="fc237-281">Partitionsnyckeln</span><span class="sxs-lookup"><span data-stu-id="fc237-281">partition-key</span></span>|<span data-ttu-id="fc237-282">Anger hello-värde som används för tilldelning av partitionen när meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="fc237-282">Specifies hello value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="fc237-283">Valfri.</span><span class="sxs-lookup"><span data-stu-id="fc237-283">Optional.</span></span> <span data-ttu-id="fc237-284">Det här attributet kan inte användas om `partition-id` används.</span><span class="sxs-lookup"><span data-stu-id="fc237-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-285">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-285">Usage</span></span>  
 <span data-ttu-id="fc237-286">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-286">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-287">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-288">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="fc237-289"><a name="mock-response"></a>Fingerad svar</span><span class="sxs-lookup"><span data-stu-id="fc237-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="fc237-290">Hej `mock-response`, som hello namn innebär, är används toomock API: er och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="fc237-290">hello `mock-response`, as hello name implies, is used toomock APIs and operations.</span></span> <span data-ttu-id="fc237-291">Den normala pipelinekörningen avbryter och returnerar en mocked svar toohello anropare.</span><span class="sxs-lookup"><span data-stu-id="fc237-291">It aborts normal pipeline execution and returns a mocked response toohello caller.</span></span> <span data-ttu-id="fc237-292">hello princip försöker alltid tooreturn svar för högsta återgivning.</span><span class="sxs-lookup"><span data-stu-id="fc237-292">hello policy always tries tooreturn responses of highest fidelity.</span></span> <span data-ttu-id="fc237-293">Den föredrar svar innehåll exempel, när det finns tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="fc237-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="fc237-294">Den genererar exempel svar från scheman, när scheman har angetts och exempel finns inte.</span><span class="sxs-lookup"><span data-stu-id="fc237-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="fc237-295">Om varken exempel eller scheman hittas returneras svar med inget innehåll.</span><span class="sxs-lookup"><span data-stu-id="fc237-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-296">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="fc237-297">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, hello content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, hello content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="fc237-298">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-298">Elements</span></span>  
  
|<span data-ttu-id="fc237-299">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-299">Element</span></span>|<span data-ttu-id="fc237-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-300">Description</span></span>|<span data-ttu-id="fc237-301">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-302">mock-svar</span><span class="sxs-lookup"><span data-stu-id="fc237-302">mock-response</span></span>|<span data-ttu-id="fc237-303">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-303">Root element.</span></span>|<span data-ttu-id="fc237-304">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-305">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-305">Attributes</span></span>  
  
|<span data-ttu-id="fc237-306">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-306">Attribute</span></span>|<span data-ttu-id="fc237-307">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-307">Description</span></span>|<span data-ttu-id="fc237-308">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-308">Required</span></span>|<span data-ttu-id="fc237-309">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="fc237-310">statuskod</span><span class="sxs-lookup"><span data-stu-id="fc237-310">status-code</span></span>|<span data-ttu-id="fc237-311">Anger Svarets statuskod och använda tooselect motsvarande exempel eller schema.</span><span class="sxs-lookup"><span data-stu-id="fc237-311">Specifies response status code and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="fc237-312">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-312">No</span></span>|<span data-ttu-id="fc237-313">200</span><span class="sxs-lookup"><span data-stu-id="fc237-313">200</span></span>|  
|<span data-ttu-id="fc237-314">innehållstyp</span><span class="sxs-lookup"><span data-stu-id="fc237-314">content-type</span></span>|<span data-ttu-id="fc237-315">Anger `Content-Type` huvudvärde svar och använda tooselect motsvarande exempel eller schema.</span><span class="sxs-lookup"><span data-stu-id="fc237-315">Specifies `Content-Type` response header value and is used tooselect corresponding example or schema.</span></span>|<span data-ttu-id="fc237-316">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-316">No</span></span>|<span data-ttu-id="fc237-317">Ingen</span><span class="sxs-lookup"><span data-stu-id="fc237-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-318">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-318">Usage</span></span>  
 <span data-ttu-id="fc237-319">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-319">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-320">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="fc237-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="fc237-321">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="fc237-322"><a name="Retry"></a>Försök igen</span><span class="sxs-lookup"><span data-stu-id="fc237-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="fc237-323">Hej `retry` princip körs en gång dess underordnade principer och sedan försöker körningen tills hello försök `condition` blir `false` eller försök `count` är slut.</span><span class="sxs-lookup"><span data-stu-id="fc237-323">hello             `retry` policy executes its child policies once and then retries their execution until hello retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-324">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-324">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="fc237-325">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-325">Example</span></span>  
 <span data-ttu-id="fc237-326">I hello försöks följande exempel begäran forewarding in tooten tider med exponentiell försök algoritm.</span><span class="sxs-lookup"><span data-stu-id="fc237-326">In hello following example request forewarding is retried up tooten times using exponential retry algorithm.</span></span> <span data-ttu-id="fc237-327">Eftersom `first-fast-retry` anges toofalse, alla nya försök är ämne toohello exponsntial försök algoritm.</span><span class="sxs-lookup"><span data-stu-id="fc237-327">Since                    `first-fast-retry` is set toofalse, all retry attempts are subject toohello exponsntial retry algorithm.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-328">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-328">Elements</span></span>  
  
|<span data-ttu-id="fc237-329">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-329">Element</span></span>|<span data-ttu-id="fc237-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-330">Description</span></span>|<span data-ttu-id="fc237-331">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-332">retry</span><span class="sxs-lookup"><span data-stu-id="fc237-332">retry</span></span>|<span data-ttu-id="fc237-333">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-333">Root element.</span></span> <span data-ttu-id="fc237-334">Kan innehålla andra principer som dess underordnade element.</span><span class="sxs-lookup"><span data-stu-id="fc237-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="fc237-335">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-336">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-336">Attributes</span></span>  
  
|<span data-ttu-id="fc237-337">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-337">Attribute</span></span>|<span data-ttu-id="fc237-338">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-338">Description</span></span>|<span data-ttu-id="fc237-339">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-339">Required</span></span>|<span data-ttu-id="fc237-340">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-341">Villkor</span><span class="sxs-lookup"><span data-stu-id="fc237-341">condition</span></span>|<span data-ttu-id="fc237-342">En boolesk literal eller [uttryck](api-management-policy-expressions.md) anger om återförsök ska stoppas (`false`) eller fortsatte (`true`).</span><span class="sxs-lookup"><span data-stu-id="fc237-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="fc237-343">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-343">Yes</span></span>|<span data-ttu-id="fc237-344">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-344">N/A</span></span>|  
|<span data-ttu-id="fc237-345">Antal</span><span class="sxs-lookup"><span data-stu-id="fc237-345">count</span></span>|<span data-ttu-id="fc237-346">Ett positivt tal som anger hello maximalt antal återförsök tooattempt.</span><span class="sxs-lookup"><span data-stu-id="fc237-346">A positive number specifying hello maximum number of retries tooattempt.</span></span>|<span data-ttu-id="fc237-347">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-347">Yes</span></span>|<span data-ttu-id="fc237-348">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-348">N/A</span></span>|  
|<span data-ttu-id="fc237-349">interval</span><span class="sxs-lookup"><span data-stu-id="fc237-349">interval</span></span>|<span data-ttu-id="fc237-350">Ett positivt tal i sekunder att ange hello intervall mellan försök hello.</span><span class="sxs-lookup"><span data-stu-id="fc237-350">A positive number in seconds specifying hello wait interval between hello retry attempts.</span></span>|<span data-ttu-id="fc237-351">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-351">Yes</span></span>|<span data-ttu-id="fc237-352">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-352">N/A</span></span>|  
|<span data-ttu-id="fc237-353">Max-intervall</span><span class="sxs-lookup"><span data-stu-id="fc237-353">max-interval</span></span>|<span data-ttu-id="fc237-354">Ett positivt tal i sekunder att ange hello maximalt intervall mellan försök hello.</span><span class="sxs-lookup"><span data-stu-id="fc237-354">A positive number in seconds specifying hello maximum wait interval between hello retry attempts.</span></span> <span data-ttu-id="fc237-355">Det är används tooimplement en algoritm exponentiell försök igen.</span><span class="sxs-lookup"><span data-stu-id="fc237-355">It is used tooimplement an exponential retry algorithm.</span></span>|<span data-ttu-id="fc237-356">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-356">No</span></span>|<span data-ttu-id="fc237-357">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-357">N/A</span></span>|  
|<span data-ttu-id="fc237-358">delta</span><span class="sxs-lookup"><span data-stu-id="fc237-358">delta</span></span>|<span data-ttu-id="fc237-359">Ett positivt tal i sekunder att ange hello vänta intervall ökning.</span><span class="sxs-lookup"><span data-stu-id="fc237-359">A positive number in seconds specifying hello wait interval increment.</span></span> <span data-ttu-id="fc237-360">Det är används tooimplement hello linjär och exponentiella försök algoritmer.</span><span class="sxs-lookup"><span data-stu-id="fc237-360">It is used tooimplement hello linear and exponential retry algorithms.</span></span>|<span data-ttu-id="fc237-361">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-361">No</span></span>|<span data-ttu-id="fc237-362">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-362">N/A</span></span>|  
|<span data-ttu-id="fc237-363">första-fast-återförsök</span><span class="sxs-lookup"><span data-stu-id="fc237-363">first-fast-retry</span></span>|<span data-ttu-id="fc237-364">Om anges för `true` , hello första nytt försök utförs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="fc237-364">If set too                                   `true` , hello first retry attempt is performed immediately.</span></span>|<span data-ttu-id="fc237-365">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="fc237-366">När endast hello `interval` anges **fast** intervall för nya försök utförs.</span><span class="sxs-lookup"><span data-stu-id="fc237-366">When only hello `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="fc237-367">När endast hello `interval` och `delta` har angetts en **linjär** intervall försök algoritmen används, där väntetiden mellan försöken är beräknade bl.a hello följande formel - `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="fc237-367">When only hello `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according hello following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="fc237-368">När hello `interval`, `max-interval` och `delta` anges, **exponentiell** intervall försök algoritmen används där hello väntetiden mellan hello försök ökar exponentiellt från hello värdet för `interval`toohello värdet `max-interval` enligt följande toohello forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="fc237-368">When hello `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where hello wait time between hello retries is growing exponentially from hello value of `interval` toohello value `max-interval` according toohello following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="fc237-369">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-369">Usage</span></span>  
 <span data-ttu-id="fc237-370">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="fc237-370">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="fc237-371">Observera att ärvs underordnade principbegränsningar för användning av den här principen.</span><span class="sxs-lookup"><span data-stu-id="fc237-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="fc237-372">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-373">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-374"><a name="ReturnResponse"></a>Returnera svar</span><span class="sxs-lookup"><span data-stu-id="fc237-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="fc237-375">Hej `return-response` princip avbryter pipelinekörningen och returnerar ett standardvärde eller anpassade svar toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fc237-375">hello `return-response` policy aborts pipeline execution and returns either a default or custom response toohello caller.</span></span> <span data-ttu-id="fc237-376">Standard-svaret är `200 OK` med ingen brödtext.</span><span class="sxs-lookup"><span data-stu-id="fc237-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="fc237-377">Anpassade svar kan anges via en kontext variabel eller princip-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="fc237-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="fc237-378">När både tillhandahålls ändras hello svaret som ingår i hello kontexten variabeln vid hello principrapporter innan de returneras toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="fc237-378">When both are provided, hello response contained within hello context variable is modified by hello policy statements before being returned toohello caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-379">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-380">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="fc237-381">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-381">Elements</span></span>  
  
|<span data-ttu-id="fc237-382">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-382">Element</span></span>|<span data-ttu-id="fc237-383">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-383">Description</span></span>|<span data-ttu-id="fc237-384">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-385">returnera svar</span><span class="sxs-lookup"><span data-stu-id="fc237-385">return-response</span></span>|<span data-ttu-id="fc237-386">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-386">Root element.</span></span>|<span data-ttu-id="fc237-387">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-387">Yes</span></span>|  
|<span data-ttu-id="fc237-388">set-huvud</span><span class="sxs-lookup"><span data-stu-id="fc237-388">set-header</span></span>|<span data-ttu-id="fc237-389">En [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="fc237-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="fc237-390">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-390">No</span></span>|  
|<span data-ttu-id="fc237-391">Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="fc237-391">set-body</span></span>|<span data-ttu-id="fc237-392">En [set brödtext](api-management-transformation-policies.md#SetBody) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="fc237-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="fc237-393">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-393">No</span></span>|  
|<span data-ttu-id="fc237-394">Ange status</span><span class="sxs-lookup"><span data-stu-id="fc237-394">set-status</span></span>|<span data-ttu-id="fc237-395">En [Ange status](api-management-advanced-policies.md#SetStatus) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="fc237-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="fc237-396">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-397">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-397">Attributes</span></span>  
  
|<span data-ttu-id="fc237-398">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-398">Attribute</span></span>|<span data-ttu-id="fc237-399">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-399">Description</span></span>|<span data-ttu-id="fc237-400">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="fc237-401">svaret variabelnamn</span><span class="sxs-lookup"><span data-stu-id="fc237-401">response-variable-name</span></span>|<span data-ttu-id="fc237-402">hello namnet på hello kontexten variabel refereras från, till exempel en uppströms [-begäran om att skicka](api-management-advanced-policies.md#SendRequest) principen och som innehåller en `Response` objekt</span><span class="sxs-lookup"><span data-stu-id="fc237-402">hello name of hello context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="fc237-403">Valfri.</span><span class="sxs-lookup"><span data-stu-id="fc237-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-404">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-404">Usage</span></span>  
 <span data-ttu-id="fc237-405">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-405">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-406">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-407">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-408"><a name="SendOneWayRequest"></a>Skicka förfrågan om ett sätt</span><span class="sxs-lookup"><span data-stu-id="fc237-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="fc237-409">Hej `send-one-way-request` princip skickar hello tillhandahålls begäran toohello specificerat URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="fc237-409">hello `send-one-way-request` policy sends hello provided request toohello specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-410">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-411">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-411">Example</span></span>  
 <span data-ttu-id="fc237-412">Exempel principen visar ett exempel på hur hello `send-one-way-request` princip toosend ett meddelande tooa Slack chatt rum om hello HTTP-svarskoden är större än eller lika med too500.</span><span class="sxs-lookup"><span data-stu-id="fc237-412">This sample policy shows an example of using hello `send-one-way-request` policy toosend a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="fc237-413">Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="fc237-413">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-414">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-414">Elements</span></span>  
  
|<span data-ttu-id="fc237-415">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-415">Element</span></span>|<span data-ttu-id="fc237-416">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-416">Description</span></span>|<span data-ttu-id="fc237-417">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-418">en-sätt-begäran om att skicka</span><span class="sxs-lookup"><span data-stu-id="fc237-418">send-one-way-request</span></span>|<span data-ttu-id="fc237-419">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-419">Root element.</span></span>|<span data-ttu-id="fc237-420">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-420">Yes</span></span>|  
|<span data-ttu-id="fc237-421">URL: en</span><span class="sxs-lookup"><span data-stu-id="fc237-421">url</span></span>|<span data-ttu-id="fc237-422">hello-URL för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-422">hello URL of hello request.</span></span>|<span data-ttu-id="fc237-423">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="fc237-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="fc237-424">Metoden</span><span class="sxs-lookup"><span data-stu-id="fc237-424">method</span></span>|<span data-ttu-id="fc237-425">hello HTTP-metoden för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-425">hello HTTP method for hello request.</span></span>|<span data-ttu-id="fc237-426">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="fc237-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="fc237-427">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="fc237-427">header</span></span>|<span data-ttu-id="fc237-428">Huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-428">Request header.</span></span> <span data-ttu-id="fc237-429">Använda flera huvud-element för flera huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="fc237-430">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-430">No</span></span>|  
|<span data-ttu-id="fc237-431">Brödtext</span><span class="sxs-lookup"><span data-stu-id="fc237-431">body</span></span>|<span data-ttu-id="fc237-432">Hej begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="fc237-432">hello request body.</span></span>|<span data-ttu-id="fc237-433">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-434">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-434">Attributes</span></span>  
  
|<span data-ttu-id="fc237-435">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-435">Attribute</span></span>|<span data-ttu-id="fc237-436">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-436">Description</span></span>|<span data-ttu-id="fc237-437">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-437">Required</span></span>|<span data-ttu-id="fc237-438">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-439">mode = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-439">mode="string"</span></span>|<span data-ttu-id="fc237-440">Anger om detta är en ny begäran eller en kopia av hello aktuella begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-440">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="fc237-441">I utgående läge, läge = kopiera initieras inte hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="fc237-441">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="fc237-442">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-442">No</span></span>|<span data-ttu-id="fc237-443">Ny</span><span class="sxs-lookup"><span data-stu-id="fc237-443">New</span></span>|  
|<span data-ttu-id="fc237-444">namn</span><span class="sxs-lookup"><span data-stu-id="fc237-444">name</span></span>|<span data-ttu-id="fc237-445">Anger hello hello huvud toobe mängden.</span><span class="sxs-lookup"><span data-stu-id="fc237-445">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="fc237-446">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-446">Yes</span></span>|<span data-ttu-id="fc237-447">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-447">N/A</span></span>|  
|<span data-ttu-id="fc237-448">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="fc237-448">exists-action</span></span>|<span data-ttu-id="fc237-449">Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="fc237-449">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="fc237-450">Det här attributet måste ha något av följande värden hello.</span><span class="sxs-lookup"><span data-stu-id="fc237-450">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="fc237-451">-åsidosätt - ersätter hello värdet för befintliga hello-huvud.</span><span class="sxs-lookup"><span data-stu-id="fc237-451">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="fc237-452">-skip - ersätter inte hello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="fc237-452">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="fc237-453">-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="fc237-453">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="fc237-454">-delete - tar bort hello huvudet från hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-454">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="fc237-455">När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fc237-455">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="fc237-456">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-456">No</span></span>|<span data-ttu-id="fc237-457">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="fc237-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-458">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-458">Usage</span></span>  
 <span data-ttu-id="fc237-459">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-459">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-460">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-461">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-462"><a name="SendRequest"></a>Skicka förfrågan</span><span class="sxs-lookup"><span data-stu-id="fc237-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="fc237-463">Hej `send-request` princip skickar hello tillhandahålls begäran toohello angiven URL, väntar på längre än hello ange timeout-värde.</span><span class="sxs-lookup"><span data-stu-id="fc237-463">hello `send-request` policy sends hello provided request toohello specified URL, waiting no longer than hello set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-464">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-465">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-465">Example</span></span>  
 <span data-ttu-id="fc237-466">Det här exemplet visar enkelriktade tooverify en referens-token med en server för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="fc237-466">This example shows one way tooverify a reference token with an authorization server.</span></span> <span data-ttu-id="fc237-467">Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="fc237-467">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-468">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-468">Elements</span></span>  
  
|<span data-ttu-id="fc237-469">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-469">Element</span></span>|<span data-ttu-id="fc237-470">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-470">Description</span></span>|<span data-ttu-id="fc237-471">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-472">Skicka begäran</span><span class="sxs-lookup"><span data-stu-id="fc237-472">send-request</span></span>|<span data-ttu-id="fc237-473">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-473">Root element.</span></span>|<span data-ttu-id="fc237-474">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-474">Yes</span></span>|  
|<span data-ttu-id="fc237-475">URL: en</span><span class="sxs-lookup"><span data-stu-id="fc237-475">url</span></span>|<span data-ttu-id="fc237-476">hello-URL för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-476">hello URL of hello request.</span></span>|<span data-ttu-id="fc237-477">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="fc237-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="fc237-478">Metoden</span><span class="sxs-lookup"><span data-stu-id="fc237-478">method</span></span>|<span data-ttu-id="fc237-479">hello HTTP-metoden för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-479">hello HTTP method for hello request.</span></span>|<span data-ttu-id="fc237-480">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="fc237-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="fc237-481">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="fc237-481">header</span></span>|<span data-ttu-id="fc237-482">Huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-482">Request header.</span></span> <span data-ttu-id="fc237-483">Använda flera huvud-element för flera huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="fc237-484">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-484">No</span></span>|  
|<span data-ttu-id="fc237-485">Brödtext</span><span class="sxs-lookup"><span data-stu-id="fc237-485">body</span></span>|<span data-ttu-id="fc237-486">Hej begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="fc237-486">hello request body.</span></span>|<span data-ttu-id="fc237-487">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-488">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-488">Attributes</span></span>  
  
|<span data-ttu-id="fc237-489">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-489">Attribute</span></span>|<span data-ttu-id="fc237-490">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-490">Description</span></span>|<span data-ttu-id="fc237-491">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-491">Required</span></span>|<span data-ttu-id="fc237-492">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-493">mode = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-493">mode="string"</span></span>|<span data-ttu-id="fc237-494">Anger om detta är en ny begäran eller en kopia av hello aktuella begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-494">Determines whether this is a new request or a copy of hello current request.</span></span> <span data-ttu-id="fc237-495">I utgående läge, läge = kopiera initieras inte hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="fc237-495">In outbound mode, mode=copy does not initialize hello request body.</span></span>|<span data-ttu-id="fc237-496">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-496">No</span></span>|<span data-ttu-id="fc237-497">Ny</span><span class="sxs-lookup"><span data-stu-id="fc237-497">New</span></span>|  
|<span data-ttu-id="fc237-498">svaret variabelnamn = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-498">response-variable-name="string"</span></span>|<span data-ttu-id="fc237-499">Om den inte finns `context.Response` används.</span><span class="sxs-lookup"><span data-stu-id="fc237-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="fc237-500">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-500">No</span></span>|<span data-ttu-id="fc237-501">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-501">N/A</span></span>|  
|<span data-ttu-id="fc237-502">timeout = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="fc237-502">timeout="integer"</span></span>|<span data-ttu-id="fc237-503">hello timeout-intervall i sekunder innan anropet hello toohello URL misslyckas.</span><span class="sxs-lookup"><span data-stu-id="fc237-503">hello timeout interval in seconds before hello call toohello URL fails.</span></span>|<span data-ttu-id="fc237-504">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-504">No</span></span>|<span data-ttu-id="fc237-505">60</span><span class="sxs-lookup"><span data-stu-id="fc237-505">60</span></span>|  
|<span data-ttu-id="fc237-506">Ignorera fel</span><span class="sxs-lookup"><span data-stu-id="fc237-506">ignore-error</span></span>|<span data-ttu-id="fc237-507">Om true och hello-begäran resulterar i ett fel:</span><span class="sxs-lookup"><span data-stu-id="fc237-507">If true and hello request results in an error:</span></span><br /><br /> <span data-ttu-id="fc237-508">– Om svaret variabelnamn angavs innehåller ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="fc237-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="fc237-509">– Om svaret variabelnamn inte har angetts, kontext. Begäran kommer inte att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="fc237-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="fc237-510">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-510">No</span></span>|<span data-ttu-id="fc237-511">FALSKT</span><span class="sxs-lookup"><span data-stu-id="fc237-511">false</span></span>|  
|<span data-ttu-id="fc237-512">namn</span><span class="sxs-lookup"><span data-stu-id="fc237-512">name</span></span>|<span data-ttu-id="fc237-513">Anger hello hello huvud toobe mängden.</span><span class="sxs-lookup"><span data-stu-id="fc237-513">Specifies hello name of hello header toobe set.</span></span>|<span data-ttu-id="fc237-514">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-514">Yes</span></span>|<span data-ttu-id="fc237-515">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-515">N/A</span></span>|  
|<span data-ttu-id="fc237-516">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="fc237-516">exists-action</span></span>|<span data-ttu-id="fc237-517">Anger vilken åtgärd tootake när hello-sidhuvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="fc237-517">Specifies what action tootake when hello header is already specified.</span></span> <span data-ttu-id="fc237-518">Det här attributet måste ha något av följande värden hello.</span><span class="sxs-lookup"><span data-stu-id="fc237-518">This attribute must have one of hello following values.</span></span><br /><br /> <span data-ttu-id="fc237-519">-åsidosätt - ersätter hello värdet för befintliga hello-huvud.</span><span class="sxs-lookup"><span data-stu-id="fc237-519">-   override - replaces hello value of hello existing header.</span></span><br /><span data-ttu-id="fc237-520">-skip - ersätter inte hello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="fc237-520">-   skip - does not replace hello existing header value.</span></span><br /><span data-ttu-id="fc237-521">-Tillägg - lägger till hello värdet toohello befintliga huvudvärde.</span><span class="sxs-lookup"><span data-stu-id="fc237-521">-   append - appends hello value toohello existing header value.</span></span><br /><span data-ttu-id="fc237-522">-delete - tar bort hello huvudet från hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-522">-   delete - removes hello header from hello request.</span></span><br /><br /> <span data-ttu-id="fc237-523">När värdet för`override` ta med flera poster med hello samma namn resulterar i hello-huvud som set bl.a tooall poster (som visas flera gånger); endast listade värden anges i hello resultat.</span><span class="sxs-lookup"><span data-stu-id="fc237-523">When set too`override` enlisting multiple entries with hello same name results in hello header being set according tooall entries (which will be listed multiple times); only listed values will be set in hello result.</span></span>|<span data-ttu-id="fc237-524">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-524">No</span></span>|<span data-ttu-id="fc237-525">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="fc237-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-526">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-526">Usage</span></span>  
 <span data-ttu-id="fc237-527">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-527">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-528">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-529">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-530"><a name="SetHttpProxy"></a>Ange HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="fc237-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="fc237-531">Hej `proxy` principen kan du tooroute begäranden vidarebefordras toobackends via en HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="fc237-531">hello `proxy` policy allows you tooroute requests forwarded toobackends via an HTTP proxy.</span></span> <span data-ttu-id="fc237-532">Endast HTTP (HTTPS inte) stöds mellan hello gateway och hello proxy.</span><span class="sxs-lookup"><span data-stu-id="fc237-532">Only HTTP (not HTTPS) is supported between hello gateway and hello proxy.</span></span> <span data-ttu-id="fc237-533">Grundläggande och NTLM-autentisering.</span><span class="sxs-lookup"><span data-stu-id="fc237-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-534">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-535">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-535">Example</span></span>  
<span data-ttu-id="fc237-536">Observera hello användning av [egenskaper](api-management-howto-properties.md) som värden för hello användarnamn och lösenord tooavoid lagra känslig information i hello dokument.</span><span class="sxs-lookup"><span data-stu-id="fc237-536">Note hello use of [properties](api-management-howto-properties.md) as values of hello username and password tooavoid storing sensitive information in hello policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="fc237-537">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-537">Elements</span></span>  
  
|<span data-ttu-id="fc237-538">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-538">Element</span></span>|<span data-ttu-id="fc237-539">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-539">Description</span></span>|<span data-ttu-id="fc237-540">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-541">Proxy</span><span class="sxs-lookup"><span data-stu-id="fc237-541">proxy</span></span>|<span data-ttu-id="fc237-542">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="fc237-542">Root element</span></span>|<span data-ttu-id="fc237-543">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="fc237-544">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-544">Attributes</span></span>  
  
|<span data-ttu-id="fc237-545">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-545">Attribute</span></span>|<span data-ttu-id="fc237-546">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-546">Description</span></span>|<span data-ttu-id="fc237-547">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-547">Required</span></span>|<span data-ttu-id="fc237-548">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-549">URL = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-549">url="string"</span></span>|<span data-ttu-id="fc237-550">Proxy-URL i hello form av http://host:port.</span><span class="sxs-lookup"><span data-stu-id="fc237-550">Proxy URL in hello form of http://host:port.</span></span>|<span data-ttu-id="fc237-551">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-551">Yes</span></span>|<span data-ttu-id="fc237-552">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-552">N/A</span></span>|  
|<span data-ttu-id="fc237-553">UserName = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-553">username="string"</span></span>|<span data-ttu-id="fc237-554">Användarnamnet toobe används för autentisering med hello proxy.</span><span class="sxs-lookup"><span data-stu-id="fc237-554">Username toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="fc237-555">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-555">No</span></span>|<span data-ttu-id="fc237-556">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-556">N/A</span></span>|  
|<span data-ttu-id="fc237-557">lösenord = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-557">password="string"</span></span>|<span data-ttu-id="fc237-558">Lösenordet toobe används för autentisering med hello proxy.</span><span class="sxs-lookup"><span data-stu-id="fc237-558">Password toobe used for authentication with hello proxy.</span></span>|<span data-ttu-id="fc237-559">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-559">No</span></span>|<span data-ttu-id="fc237-560">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="fc237-561">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-561">Usage</span></span>  
 <span data-ttu-id="fc237-562">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-562">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-563">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="fc237-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="fc237-564">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="fc237-565"><a name="SetRequestMethod"></a>Set-metod för begäran</span><span class="sxs-lookup"><span data-stu-id="fc237-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="fc237-566">Hej `set-method` principen kan du toochange hello HTTP-frågemetoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="fc237-566">hello `set-method` policy allows you toochange hello HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-567">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-568">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-568">Example</span></span>  
 <span data-ttu-id="fc237-569">Det här exemplet princip som använder hello `set-method` princip visas ett exempel på skickas ett meddelande tooa Slack chatt-rum om hello HTTP-svarskoden är större än eller lika med too500.</span><span class="sxs-lookup"><span data-stu-id="fc237-569">This sample policy that uses hello `set-method` policy shows an example of sending a message tooa Slack chat room if hello HTTP response code is greater than or equal too500.</span></span> <span data-ttu-id="fc237-570">Mer information om det här exemplet finns [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="fc237-570">For more information on this sample, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-571">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-571">Elements</span></span>  
  
|<span data-ttu-id="fc237-572">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-572">Element</span></span>|<span data-ttu-id="fc237-573">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-573">Description</span></span>|<span data-ttu-id="fc237-574">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-575">set-metod</span><span class="sxs-lookup"><span data-stu-id="fc237-575">set-method</span></span>|<span data-ttu-id="fc237-576">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-576">Root element.</span></span> <span data-ttu-id="fc237-577">hello anger hello elementet hello HTTP-metod.</span><span class="sxs-lookup"><span data-stu-id="fc237-577">hello value of hello element specifies hello HTTP method.</span></span>|<span data-ttu-id="fc237-578">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-579">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-579">Usage</span></span>  
 <span data-ttu-id="fc237-580">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-580">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-581">**Avsnitt i princip:** inkommande, vid fel</span><span class="sxs-lookup"><span data-stu-id="fc237-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="fc237-582">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-583"><a name="SetStatus"></a>Ange statuskoden</span><span class="sxs-lookup"><span data-stu-id="fc237-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="fc237-584">Hej `set-status` principen anger hello HTTP-status kod toohello angivet värde.</span><span class="sxs-lookup"><span data-stu-id="fc237-584">hello `set-status` policy sets hello HTTP status code toohello specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-585">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-586">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-586">Example</span></span>  
 <span data-ttu-id="fc237-587">Det här exemplet illustrerar hur tooreturn 401 svar om hello autentiseringstoken är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="fc237-587">This example shows how tooreturn a 401 response if hello authorization token is invalid.</span></span> <span data-ttu-id="fc237-588">Mer information finns i [med externa tjänster från hello Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="fc237-588">For more information, see [Using external services from hello Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-589">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-589">Elements</span></span>  
  
|<span data-ttu-id="fc237-590">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-590">Element</span></span>|<span data-ttu-id="fc237-591">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-591">Description</span></span>|<span data-ttu-id="fc237-592">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-593">Ange status</span><span class="sxs-lookup"><span data-stu-id="fc237-593">set-status</span></span>|<span data-ttu-id="fc237-594">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-594">Root element.</span></span>|<span data-ttu-id="fc237-595">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-596">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-596">Attributes</span></span>  
  
|<span data-ttu-id="fc237-597">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-597">Attribute</span></span>|<span data-ttu-id="fc237-598">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-598">Description</span></span>|<span data-ttu-id="fc237-599">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-599">Required</span></span>|<span data-ttu-id="fc237-600">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-601">kod = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="fc237-601">code="integer"</span></span>|<span data-ttu-id="fc237-602">hello HTTP-status koden tooreturn.</span><span class="sxs-lookup"><span data-stu-id="fc237-602">hello HTTP status code tooreturn.</span></span>|<span data-ttu-id="fc237-603">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-603">Yes</span></span>|<span data-ttu-id="fc237-604">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-604">N/A</span></span>|  
|<span data-ttu-id="fc237-605">Orsak = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="fc237-605">reason="string"</span></span>|<span data-ttu-id="fc237-606">En beskrivning av hello orsak för att returnera hello-statuskod.</span><span class="sxs-lookup"><span data-stu-id="fc237-606">A description of hello reason for returning hello status code.</span></span>|<span data-ttu-id="fc237-607">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-607">Yes</span></span>|<span data-ttu-id="fc237-608">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-609">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-609">Usage</span></span>  
 <span data-ttu-id="fc237-610">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-610">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-611">**Avsnitt i princip:** utgående, backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-612">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="fc237-613"><a name="set-variable"></a>Ange variabel</span><span class="sxs-lookup"><span data-stu-id="fc237-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="fc237-614">Hej `set-variable` princip deklarerar en [kontexten](api-management-policy-expressions.md#ContextVariables) variabeln och tilldelar den ett värde som angetts via en [uttryck](api-management-policy-expressions.md) eller en teckensträng.</span><span class="sxs-lookup"><span data-stu-id="fc237-614">hello `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="fc237-615">Om hello uttryck innehåller ett litteralvärde som kommer att konverteras tooa sträng och hello Värdets typ hello blir `System.String`.</span><span class="sxs-lookup"><span data-stu-id="fc237-615">if hello expression contains a literal it will be converted tooa string and hello type of hello value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="fc237-616"><a name="set-variablePolicyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="fc237-617"><a name="set-variableExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="fc237-618">hello exemplet nedan visar en uppsättning variabeln princip i hello inkommande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fc237-618">hello following example demonstrates a set variable policy in hello inbound section.</span></span> <span data-ttu-id="fc237-619">Ange variabeln principen skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som anges tootrue om hello `User-Agent` begäran huvudet innehåller hello text `iPad` eller `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="fc237-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set tootrue if hello `User-Agent` request header contains hello text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="fc237-620">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-620">Elements</span></span>  
  
|<span data-ttu-id="fc237-621">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-621">Element</span></span>|<span data-ttu-id="fc237-622">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-622">Description</span></span>|<span data-ttu-id="fc237-623">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-624">Ange variabel</span><span class="sxs-lookup"><span data-stu-id="fc237-624">set-variable</span></span>|<span data-ttu-id="fc237-625">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-625">Root element.</span></span>|<span data-ttu-id="fc237-626">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-627">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-627">Attributes</span></span>  
  
|<span data-ttu-id="fc237-628">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-628">Attribute</span></span>|<span data-ttu-id="fc237-629">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-629">Description</span></span>|<span data-ttu-id="fc237-630">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="fc237-631">namn</span><span class="sxs-lookup"><span data-stu-id="fc237-631">name</span></span>|<span data-ttu-id="fc237-632">hello namnet på hello-variabeln.</span><span class="sxs-lookup"><span data-stu-id="fc237-632">hello name of hello variable.</span></span>|<span data-ttu-id="fc237-633">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-633">Yes</span></span>|  
|<span data-ttu-id="fc237-634">värde</span><span class="sxs-lookup"><span data-stu-id="fc237-634">value</span></span>|<span data-ttu-id="fc237-635">hello värdet för hello variabel.</span><span class="sxs-lookup"><span data-stu-id="fc237-635">hello value of hello variable.</span></span> <span data-ttu-id="fc237-636">Detta kan vara ett uttryck eller ett litteralvärde.</span><span class="sxs-lookup"><span data-stu-id="fc237-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="fc237-637">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-638">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-638">Usage</span></span>  
 <span data-ttu-id="fc237-639">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-639">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-640">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-641">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="fc237-642"><a name="set-variableAllowedTypes"></a>Tillåtna typer</span><span class="sxs-lookup"><span data-stu-id="fc237-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="fc237-643">Uttryck som används i hello `set-variable` princip måste returnera hello följande grundläggande typer.</span><span class="sxs-lookup"><span data-stu-id="fc237-643">Expressions used in hello `set-variable` policy must return one of hello following basic types.</span></span>  
  
-   <span data-ttu-id="fc237-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="fc237-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="fc237-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="fc237-645">System.SByte</span></span>  
  
-   <span data-ttu-id="fc237-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="fc237-646">System.Byte</span></span>  
  
-   <span data-ttu-id="fc237-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="fc237-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="fc237-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="fc237-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="fc237-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="fc237-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="fc237-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="fc237-650">System.Int16</span></span>  
  
-   <span data-ttu-id="fc237-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="fc237-651">System.Int32</span></span>  
  
-   <span data-ttu-id="fc237-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="fc237-652">System.Int64</span></span>  
  
-   <span data-ttu-id="fc237-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="fc237-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="fc237-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="fc237-654">System.Single</span></span>  
  
-   <span data-ttu-id="fc237-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="fc237-655">System.Double</span></span>  
  
-   <span data-ttu-id="fc237-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="fc237-656">System.Guid</span></span>  
  
-   <span data-ttu-id="fc237-657">System.String</span><span class="sxs-lookup"><span data-stu-id="fc237-657">System.String</span></span>  
  
-   <span data-ttu-id="fc237-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="fc237-658">System.Char</span></span>  
  
-   <span data-ttu-id="fc237-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="fc237-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="fc237-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fc237-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="fc237-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="fc237-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="fc237-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="fc237-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="fc237-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="fc237-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="fc237-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="fc237-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="fc237-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="fc237-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="fc237-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="fc237-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="fc237-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="fc237-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="fc237-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="fc237-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="fc237-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="fc237-669">System.Single?</span></span>  
  
-   <span data-ttu-id="fc237-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="fc237-670">System.Double?</span></span>  
  
-   <span data-ttu-id="fc237-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="fc237-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="fc237-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="fc237-672">System.String?</span></span>  
  
-   <span data-ttu-id="fc237-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="fc237-673">System.Char?</span></span>  
  
-   <span data-ttu-id="fc237-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="fc237-674">System.DateTime?</span></span>  

##  <span data-ttu-id="fc237-675"><a name="Trace"></a>Spårning</span><span class="sxs-lookup"><span data-stu-id="fc237-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="fc237-676">Hej `trace` princip lägger till en sträng i hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="fc237-676">hello             `trace` policy adds a string into hello [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="fc237-677">hello princip körs endast när spårning utlöses, d.v.s. `Ocp-Apim-Trace` huvud är närvarande och ange för`true` och `Ocp-Apim-Subscription-Key` begärandehuvudet finns och innehåller en giltig nyckel som är associerad med hello-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="fc237-677">hello policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set too`true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with hello admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-678">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="fc237-679">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-679">Elements</span></span>  
  
|<span data-ttu-id="fc237-680">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-680">Element</span></span>|<span data-ttu-id="fc237-681">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-681">Description</span></span>|<span data-ttu-id="fc237-682">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-683">Spårning</span><span class="sxs-lookup"><span data-stu-id="fc237-683">trace</span></span>|<span data-ttu-id="fc237-684">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-684">Root element.</span></span>|<span data-ttu-id="fc237-685">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-686">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-686">Attributes</span></span>  
  
|<span data-ttu-id="fc237-687">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-687">Attribute</span></span>|<span data-ttu-id="fc237-688">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-688">Description</span></span>|<span data-ttu-id="fc237-689">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-689">Required</span></span>|<span data-ttu-id="fc237-690">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-691">Källa</span><span class="sxs-lookup"><span data-stu-id="fc237-691">source</span></span>|<span data-ttu-id="fc237-692">Sträng literal meningsfulla toohello trace viewer och att ange hello källan för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fc237-692">String literal meaningful toohello trace viewer and specifying hello source of hello message.</span></span>|<span data-ttu-id="fc237-693">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-693">Yes</span></span>|<span data-ttu-id="fc237-694">Saknas</span><span class="sxs-lookup"><span data-stu-id="fc237-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-695">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-695">Usage</span></span>  
 <span data-ttu-id="fc237-696">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="fc237-696">This policy can be used in hello following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="fc237-697">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="fc237-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="fc237-698">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="fc237-699"><a name="Wait"></a>Vänta</span><span class="sxs-lookup"><span data-stu-id="fc237-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="fc237-700">Hej `wait` principen Kör sina direkt underordnade principer parallellt och väntar på alla eller en av dess omedelbara underordnade principer toocomplete innan den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="fc237-700">hello `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies toocomplete before it completes.</span></span> <span data-ttu-id="fc237-701">hello vänta princip kan ha sina direkt underordnade principer [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), och [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer.</span><span class="sxs-lookup"><span data-stu-id="fc237-701">hello wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="fc237-702">Principframställning</span><span class="sxs-lookup"><span data-stu-id="fc237-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="fc237-703">Exempel</span><span class="sxs-lookup"><span data-stu-id="fc237-703">Example</span></span>  
 <span data-ttu-id="fc237-704">I följande exempel hello finns två `choose` principer som direkt underordnade principer för hello `wait` princip.</span><span class="sxs-lookup"><span data-stu-id="fc237-704">In hello following example there are two `choose` policies as immediate child policies of hello `wait` policy.</span></span> <span data-ttu-id="fc237-705">Var och en av dessa `choose` principer körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="fc237-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="fc237-706">Varje `choose` princip försöker tooretrieve cachelagrade värde.</span><span class="sxs-lookup"><span data-stu-id="fc237-706">Each `choose` policy attempts tooretrieve a cached value.</span></span> <span data-ttu-id="fc237-707">Om det finns en cache-miss, kallas tooprovide hello värde med en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="fc237-707">If there is a cache miss, a backend service is called tooprovide hello value.</span></span> <span data-ttu-id="fc237-708">I det här exemplet hello `wait` principen inte slutföras förrän alla dess omedelbara underordnade principer slutföra eftersom hello `for` attribut har angetts för`all`.</span><span class="sxs-lookup"><span data-stu-id="fc237-708">In this example hello `wait` policy does not complete until all of its immediate child policies complete, because hello `for` attribute is set too`all`.</span></span>   <span data-ttu-id="fc237-709">I det här exemplet hello kontexten variabler (`execute-branch-one`, `value-one`, `execute-branch-two`, och `value-two`) deklareras utanför hello omfattning exempel principen.</span><span class="sxs-lookup"><span data-stu-id="fc237-709">In this example hello context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of hello scope of this example policy.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="fc237-710">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-710">Elements</span></span>  
  
|<span data-ttu-id="fc237-711">Element</span><span class="sxs-lookup"><span data-stu-id="fc237-711">Element</span></span>|<span data-ttu-id="fc237-712">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-712">Description</span></span>|<span data-ttu-id="fc237-713">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="fc237-714">Vänta</span><span class="sxs-lookup"><span data-stu-id="fc237-714">wait</span></span>|<span data-ttu-id="fc237-715">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="fc237-715">Root element.</span></span> <span data-ttu-id="fc237-716">Kan innehålla som underordnade element endast `send-request`, `cache-lookup-value`, och `choose` principer.</span><span class="sxs-lookup"><span data-stu-id="fc237-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="fc237-717">Ja</span><span class="sxs-lookup"><span data-stu-id="fc237-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="fc237-718">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-718">Attributes</span></span>  
  
|<span data-ttu-id="fc237-719">Attribut</span><span class="sxs-lookup"><span data-stu-id="fc237-719">Attribute</span></span>|<span data-ttu-id="fc237-720">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fc237-720">Description</span></span>|<span data-ttu-id="fc237-721">Krävs</span><span class="sxs-lookup"><span data-stu-id="fc237-721">Required</span></span>|<span data-ttu-id="fc237-722">Standard</span><span class="sxs-lookup"><span data-stu-id="fc237-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="fc237-723">för</span><span class="sxs-lookup"><span data-stu-id="fc237-723">for</span></span>|<span data-ttu-id="fc237-724">Avgör om hello `wait` princip väntar på att alla direkt underordnade principer toobe slutförts eller bara en.</span><span class="sxs-lookup"><span data-stu-id="fc237-724">Determines whether hello `wait` policy waits for all immediate child policies toobe completed or just one.</span></span> <span data-ttu-id="fc237-725">Tillåtna värden är:</span><span class="sxs-lookup"><span data-stu-id="fc237-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="fc237-726">-   `all`-Vänta tills alla direkt underordnade principer toocomplete</span><span class="sxs-lookup"><span data-stu-id="fc237-726">-   `all` - wait for all immediate child policies toocomplete</span></span><br /><span data-ttu-id="fc237-727">-alla - vänta tills alla direkt underordnade princip toocomplete.</span><span class="sxs-lookup"><span data-stu-id="fc237-727">-   any - wait for any immediate child policy toocomplete.</span></span> <span data-ttu-id="fc237-728">När hello första omedelbara underordnade principen är klar hello `wait` princip har slutförts och körningen av alla andra principer för omedelbart underordnade avslutas.</span><span class="sxs-lookup"><span data-stu-id="fc237-728">Once hello first immediate child policy has completed, hello `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="fc237-729">Nej</span><span class="sxs-lookup"><span data-stu-id="fc237-729">No</span></span>|<span data-ttu-id="fc237-730">Alla</span><span class="sxs-lookup"><span data-stu-id="fc237-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="fc237-731">Användning</span><span class="sxs-lookup"><span data-stu-id="fc237-731">Usage</span></span>  
 <span data-ttu-id="fc237-732">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="fc237-732">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="fc237-733">**Avsnitt i princip:** inkommande, utgående backend</span><span class="sxs-lookup"><span data-stu-id="fc237-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="fc237-734">**Princip för scope:**alla scope</span><span class="sxs-lookup"><span data-stu-id="fc237-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="fc237-735">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fc237-735">Next steps</span></span>
<span data-ttu-id="fc237-736">Arbeta med principer, Läs mer:</span><span class="sxs-lookup"><span data-stu-id="fc237-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="fc237-737">Principer för i API-hantering</span><span class="sxs-lookup"><span data-stu-id="fc237-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="fc237-738">Principuttryck</span><span class="sxs-lookup"><span data-stu-id="fc237-738">Policy expressions</span></span>](api-management-policy-expressions.md)
