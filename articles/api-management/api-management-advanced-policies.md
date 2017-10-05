---
title: Azure API Management avancerade principer | Microsoft Docs
description: "Läs mer om de avancerade principerna som är tillgängligt för användning i Azure API Management."
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
ms.openlocfilehash: 0c65ac74316421a0258f01143baa25ffecb5be3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="api-management-advanced-policies"></a><span data-ttu-id="4077f-103">API Management avancerade principer</span><span class="sxs-lookup"><span data-stu-id="4077f-103">API Management advanced policies</span></span>
<span data-ttu-id="4077f-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="4077f-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="4077f-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="4077f-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="4077f-106"><a name="AdvancedPolicies"></a>Avancerade principer</span><span class="sxs-lookup"><span data-stu-id="4077f-106"><a name="AdvancedPolicies"></a> Advanced policies</span></span>  
  
-   <span data-ttu-id="4077f-107">[Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) - villkorligt gäller principrapporter baserat på resultatet av utvärderingen av boolesk [uttryck](api-management-policy-expressions.md).</span><span class="sxs-lookup"><span data-stu-id="4077f-107">[Control flow](api-management-advanced-policies.md#choose) - Conditionally applies policy statements based on the results of the evaluation of Boolean [expressions](api-management-policy-expressions.md).</span></span>  
  
-   <span data-ttu-id="4077f-108">[Vidarebefordra begäran](#ForwardRequest) -vidarebefordrar begäran till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4077f-108">[Forward request](#ForwardRequest) - Forwards the request to the backend service.</span></span>

-   <span data-ttu-id="4077f-109">[Begränsa samtidighet](#LimitConcurrency) -förhindrar omslutna principer från att köras med mer än det angivna antalet begäranden i taget.</span><span class="sxs-lookup"><span data-stu-id="4077f-109">[Limit concurrency](#LimitConcurrency) - Prevents enclosed policies from executing by more than the specified number of requests at a time.</span></span>
  
-   <span data-ttu-id="4077f-110">[Loggen till Event Hub](#log-to-eventhub) -skickar meddelanden i det angivna formatet till en Händelsehubb som definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="4077f-110">[Log to Event Hub](#log-to-eventhub) - Sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> 

-   <span data-ttu-id="4077f-111">[Mock svar](#mock-response) -avbryts körningen i pipeline och returnerar svaret mocked direkt till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-111">[Mock response](#mock-response) - Aborts pipeline execution and returns a mocked response directly to the caller.</span></span>
  
-   <span data-ttu-id="4077f-112">[Försök](#Retry) -återförsök körningen av de slutna principrapporter om och tills villkoret är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="4077f-112">[Retry](#Retry) - Retries execution of the enclosed policy statements, if and until the condition is met.</span></span> <span data-ttu-id="4077f-113">Körningen upprepas med de angivna intervall och upp till angivet antal nya försök.</span><span class="sxs-lookup"><span data-stu-id="4077f-113">Execution will repeat at the specified time intervals and up to the specified retry count.</span></span>  
  
-   <span data-ttu-id="4077f-114">[Returnera svar](#ReturnResponse) -avbryts körningen i pipeline och returnerar det angivna svaret direkt till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-114">[Return response](#ReturnResponse) - Aborts pipeline execution and returns the specified response directly to the caller.</span></span> 
  
-   <span data-ttu-id="4077f-115">[Skicka förfrågan om enkelriktade](#SendOneWayRequest) -skickar en begäran till den angivna URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="4077f-115">[Send one way request](#SendOneWayRequest) - Sends a request to the specified URL without waiting for a response.</span></span>  
  
-   <span data-ttu-id="4077f-116">[Skicka förfrågan](#SendRequest) -skickar en begäran till angiven URL.</span><span class="sxs-lookup"><span data-stu-id="4077f-116">[Send request](#SendRequest) - Sends a request to the specified URL.</span></span>  

-   <span data-ttu-id="4077f-117">[Ange HTTP-proxy](#SetHttpProxy) -låter dig vägen vidarebefordrade begäranden via en HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="4077f-117">[Set HTTP proxy](#SetHttpProxy) - Allows you to route forwarded requests via an HTTP proxy.</span></span>  

-   <span data-ttu-id="4077f-118">[Ange metoden](#SetRequestMethod) -kan du ändra HTTP-metoden för en begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-118">[Set request method](#SetRequestMethod) - Allows you to change the HTTP method for a request.</span></span>  
  
-   <span data-ttu-id="4077f-119">[Ange statuskoden](#SetStatus) -ändrar HTTP-statuskoden till det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-119">[Set status code](#SetStatus) - Changes the HTTP status code to the specified value.</span></span>  
  
-   <span data-ttu-id="4077f-120">[Ange variabel](api-management-advanced-policies.md#set-variable) -kvarstår ett värde i en namngiven [kontexten](api-management-policy-expressions.md#ContextVariables) variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="4077f-120">[Set variable](api-management-advanced-policies.md#set-variable) - Persists a value in a named [context](api-management-policy-expressions.md#ContextVariables) variable for later access.</span></span>  

-   <span data-ttu-id="4077f-121">[Spåra](#Trace) -lägger till en sträng i den [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="4077f-121">[Trace](#Trace) - Adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span>  
  
-   <span data-ttu-id="4077f-122">[Vänta](#Wait) -väntar för omslutna [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), eller [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer för att slutföra innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="4077f-122">[Wait](#Wait) - Waits for enclosed [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), or [Control flow](api-management-advanced-policies.md#choose) policies to complete before proceeding.</span></span>  
  
##  <span data-ttu-id="4077f-123"><a name="choose"></a>Kontrollflöde</span><span class="sxs-lookup"><span data-stu-id="4077f-123"><a name="choose"></a> Control flow</span></span>  
 <span data-ttu-id="4077f-124">Den `choose` principen gäller omslutna princip uttryck baserat på resultatet av utvärderingen av booleska uttryck som liknar en if-then-else eller växel konstruera i ett programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="4077f-124">The `choose` policy applies enclosed policy statements based on the outcome of evaluation of Boolean expressions, similar to an if-then-else or a switch construct in a programming language.</span></span>  
  
###  <span data-ttu-id="4077f-125"><a name="ChoosePolicyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-125"><a name="ChoosePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 <span data-ttu-id="4077f-126">Princip för åtkomstkontroll flödet måste innehålla minst ett `<when/>` element.</span><span class="sxs-lookup"><span data-stu-id="4077f-126">The control flow policy must contain at least one `<when/>` element.</span></span> <span data-ttu-id="4077f-127">Den `<otherwise/>` element är valfria.</span><span class="sxs-lookup"><span data-stu-id="4077f-127">The `<otherwise/>` element is optional.</span></span> <span data-ttu-id="4077f-128">Villkoren i `<when/>` element utvärderas i ordning efter deras utseende i principen.</span><span class="sxs-lookup"><span data-stu-id="4077f-128">Conditions in `<when/>` elements are evaluated in order of their appearance within the policy.</span></span> <span data-ttu-id="4077f-129">Principen instruktion(er) omgiven första `<when/>` element med villkoret attribut är lika med `true` tillämpas.</span><span class="sxs-lookup"><span data-stu-id="4077f-129">Policy statement(s) enclosed within the first `<when/>` element with condition attribute equals `true` will be applied.</span></span> <span data-ttu-id="4077f-130">Principer omgiven av `<otherwise/>` element, i förekommande fall, kommer att tillämpas om alla av den `<when/>` elementattribut för villkoret är `false`.</span><span class="sxs-lookup"><span data-stu-id="4077f-130">Policies enclosed within the `<otherwise/>` element, if present, will be applied if all of the `<when/>` element condition attributes are `false`.</span></span>  
  
### <a name="examples"></a><span data-ttu-id="4077f-131">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-131">Examples</span></span>  
  
####  <span data-ttu-id="4077f-132"><a name="ChooseExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-132"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="4077f-133">I följande exempel visas en [ange variabel](api-management-advanced-policies.md#set-variable) principen och två principer för åtkomstkontroll flödet.</span><span class="sxs-lookup"><span data-stu-id="4077f-133">The following example demonstrates a [set-variable](api-management-advanced-policies.md#set-variable) policy and two control flow policies.</span></span>  
  
 <span data-ttu-id="4077f-134">Ange variabeln princip finns i avsnittet inkommande och skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som har angetts till true om den `User-Agent` begäran huvudet innehåller texten `iPad` eller `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="4077f-134">The set variable policy is in the inbound section and creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
 <span data-ttu-id="4077f-135">Den första kontroll flödet principen finns i avsnittet inkommande och villkorligt gäller en av två [ange frågesträngparametern](api-management-transformation-policies.md#SetQueryStringParameter) principer beroende på värdet för den `isMobile` kontexten variabeln.</span><span class="sxs-lookup"><span data-stu-id="4077f-135">The first control flow policy is also in the inbound section, and conditionally applies one of two [Set query string parameter](api-management-transformation-policies.md#SetQueryStringParameter) policies depending on the value of the `isMobile` context variable.</span></span>  
  
 <span data-ttu-id="4077f-136">Den andra kontrollen flödet i avsnittet utgående och villkorligt gäller den [konvertera XML till JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) principen när `isMobile` är inställd på `true`.</span><span class="sxs-lookup"><span data-stu-id="4077f-136">The second control flow policy is in the outbound section and conditionally applies the [Convert XML to JSON](api-management-transformation-policies.md#ConvertXMLtoJSON) policy when `isMobile` is set to `true`.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="4077f-137">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-137">Example</span></span>  
 <span data-ttu-id="4077f-138">Det här exemplet illustrerar hur du utför innehållsfiltrering genom att ta bort dataelement från svar togs emot från serverdelstjänsten när du använder den `Starter` produkten.</span><span class="sxs-lookup"><span data-stu-id="4077f-138">This example shows how to perform content filtering by removing data elements from the response received from the backend service when using the `Starter` product.</span></span> <span data-ttu-id="4077f-139">En demonstration av hur du konfigurerar och använder den här principen finns [moln omfattar avsnitt 177: mer API Management-funktioner med Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) och spola framåt till 34:30.</span><span class="sxs-lookup"><span data-stu-id="4077f-139">For a demonstration of configuring and using this policy, see [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) and fast-forward to 34:30.</span></span> <span data-ttu-id="4077f-140">Börja med 31:50 att se en översikt över [mörkt Sky prognos API: N](https://developer.forecast.io/) används för den här demon.</span><span class="sxs-lookup"><span data-stu-id="4077f-140">Start  at 31:50 to see an overview of [The Dark Sky Forecast API](https://developer.forecast.io/) used for this demo.</span></span>  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-141">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-141">Elements</span></span>  
  
|<span data-ttu-id="4077f-142">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-142">Element</span></span>|<span data-ttu-id="4077f-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-143">Description</span></span>|<span data-ttu-id="4077f-144">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-144">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-145">Välj</span><span class="sxs-lookup"><span data-stu-id="4077f-145">choose</span></span>|<span data-ttu-id="4077f-146">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-146">Root element.</span></span>|<span data-ttu-id="4077f-147">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-147">Yes</span></span>|  
|<span data-ttu-id="4077f-148">När</span><span class="sxs-lookup"><span data-stu-id="4077f-148">when</span></span>|<span data-ttu-id="4077f-149">De villkor du vill använda för den `if` eller `ifelse` delar av den `choose` princip.</span><span class="sxs-lookup"><span data-stu-id="4077f-149">The condition to use for the `if` or `ifelse` parts of the `choose` policy.</span></span> <span data-ttu-id="4077f-150">Om den `choose` princip har flera `when` avsnitt, de utvärderas i turordning.</span><span class="sxs-lookup"><span data-stu-id="4077f-150">If the `choose` policy has multiple `when` sections, they are evaluated sequentially.</span></span> <span data-ttu-id="4077f-151">En gång i `condition` av ett när element beräknas till `true`, ingen ytterligare `when` villkor utvärderas.</span><span class="sxs-lookup"><span data-stu-id="4077f-151">Once the `condition` of a when element evaluates to `true`, no further `when` conditions are evaluated.</span></span>|<span data-ttu-id="4077f-152">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-152">Yes</span></span>|  
|<span data-ttu-id="4077f-153">Annars</span><span class="sxs-lookup"><span data-stu-id="4077f-153">otherwise</span></span>|<span data-ttu-id="4077f-154">Innehåller princip fragment som ska användas om ingen av de `when` villkor utvärderas till `true`.</span><span class="sxs-lookup"><span data-stu-id="4077f-154">Contains the policy snippet to be used if none of the `when` conditions evaluate to `true`.</span></span>|<span data-ttu-id="4077f-155">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-155">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-156">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-156">Attributes</span></span>  
  
|<span data-ttu-id="4077f-157">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-157">Attribute</span></span>|<span data-ttu-id="4077f-158">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-158">Description</span></span>|<span data-ttu-id="4077f-159">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-159">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="4077f-160">villkor = ”booleskt uttryck &#124; Booleskt konstant ”</span><span class="sxs-lookup"><span data-stu-id="4077f-160">condition="Boolean expression &#124; Boolean constant"</span></span>|<span data-ttu-id="4077f-161">Booleska uttryck eller en konstant som utvärderas när den innehållande `when` Principframställning utvärderas.</span><span class="sxs-lookup"><span data-stu-id="4077f-161">The Boolean expression or constant to evaluated when the containing `when` policy statement is evaluated.</span></span>|<span data-ttu-id="4077f-162">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-162">Yes</span></span>|  
  
###  <span data-ttu-id="4077f-163"><a name="ChooseUsage"></a>Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-163"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="4077f-164">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-164">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-165">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-165">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-166">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-166">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-167"><a name="ForwardRequest"></a>Vidarebefordra begäran</span><span class="sxs-lookup"><span data-stu-id="4077f-167"><a name="ForwardRequest"></a> Forward request</span></span>  
 <span data-ttu-id="4077f-168">Den `forward-request` princip vidarebefordrar inkommande begäran till backend-tjänst som anges i begäran [kontexten](api-management-policy-expressions.md#ContextVariables).</span><span class="sxs-lookup"><span data-stu-id="4077f-168">The `forward-request` policy forwards the incoming request to the backend service specified in the request [context](api-management-policy-expressions.md#ContextVariables).</span></span> <span data-ttu-id="4077f-169">URL: en för backend-tjänsten har angetts i API [inställningar](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) och kan ändras med hjälp av den [ange serverdelstjänst](api-management-transformation-policies.md) princip.</span><span class="sxs-lookup"><span data-stu-id="4077f-169">The backend service URL is specified in the API  [settings](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) and can be changed using the [set backend service](api-management-transformation-policies.md) policy.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4077f-170">Tar bort den här grupprincipresultat i begäran inte vidarebefordras till backend-tjänsten och principer i avsnittet utgående utvärderas omedelbart vid slutförande av principer i avsnittet inkommande.</span><span class="sxs-lookup"><span data-stu-id="4077f-170">Removing this policy results in the request not being forwarded to the backend service and the policies in the outbound section are evaluated immediately upon the successful completion of the policies in the inbound section.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-171">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-171">Policy statement</span></span>  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a><span data-ttu-id="4077f-172">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-172">Examples</span></span>  
  
#### <a name="example"></a><span data-ttu-id="4077f-173">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-173">Example</span></span>  
 <span data-ttu-id="4077f-174">Följande princip för API-nivå vidarebefordras alla begäranden till backend-tjänsten med en timeout på 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="4077f-174">The following API level policy forwards all requests to the backend service with a timeout interval of 60 seconds.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="4077f-175">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-175">Example</span></span>  
 <span data-ttu-id="4077f-176">Använder den här åtgärden säkerhetsnivå för den `base` element ska ärva backend-principen från överordnat nivån API-scope.</span><span class="sxs-lookup"><span data-stu-id="4077f-176">This operation level policy uses the `base` element to inherit the backend policy from the parent API level scope.</span></span>  
  
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
  
#### <a name="example"></a><span data-ttu-id="4077f-177">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-177">Example</span></span>  
 <span data-ttu-id="4077f-178">Den här åtgärden säkerhetsnivå för uttryckligen vidarebefordrar alla begäranden till backend-tjänsten med en tidsgräns på 120 och ärver inte överordnat nivån backend API-princip.</span><span class="sxs-lookup"><span data-stu-id="4077f-178">This operation level policy explicitly forwards all requests to the backend service with a timeout of 120 and does not inherit the parent API level backend policy.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a><span data-ttu-id="4077f-179">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-179">Example</span></span>  
 <span data-ttu-id="4077f-180">Den här åtgärden säkerhetsnivå för vidarebefordrar inte begäranden till backend-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4077f-180">This operation level policy does not forward requests to the backend service.</span></span>  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-181">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-181">Elements</span></span>  
  
|<span data-ttu-id="4077f-182">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-182">Element</span></span>|<span data-ttu-id="4077f-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-183">Description</span></span>|<span data-ttu-id="4077f-184">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-184">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-185">vidarebefordra begäran</span><span class="sxs-lookup"><span data-stu-id="4077f-185">forward-request</span></span>|<span data-ttu-id="4077f-186">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-186">Root element.</span></span>|<span data-ttu-id="4077f-187">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-187">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-188">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-188">Attributes</span></span>  
  
|<span data-ttu-id="4077f-189">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-189">Attribute</span></span>|<span data-ttu-id="4077f-190">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-190">Description</span></span>|<span data-ttu-id="4077f-191">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-191">Required</span></span>|<span data-ttu-id="4077f-192">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-192">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-193">timeout = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="4077f-193">timeout="integer"</span></span>|<span data-ttu-id="4077f-194">Det går inte att timeoutintervall i sekunder innan anropet till serverdelstjänsten.</span><span class="sxs-lookup"><span data-stu-id="4077f-194">The timeout interval in seconds before the call to the backend service fails.</span></span>|<span data-ttu-id="4077f-195">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-195">No</span></span>|<span data-ttu-id="4077f-196">Ingen tidsgräns</span><span class="sxs-lookup"><span data-stu-id="4077f-196">No timeout</span></span>|  
|<span data-ttu-id="4077f-197">Följ omdirigeringar = ”true &#124; FALSE ”</span><span class="sxs-lookup"><span data-stu-id="4077f-197">follow-redirects="true &#124; false"</span></span>|<span data-ttu-id="4077f-198">Anger huruvida omdirigeringar från serverdelstjänsten följt av gateway eller returneras till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-198">Specifies whether redirects from the backend service are followed by the gateway or returned to the caller.</span></span>|<span data-ttu-id="4077f-199">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-199">No</span></span>|<span data-ttu-id="4077f-200">FALSKT</span><span class="sxs-lookup"><span data-stu-id="4077f-200">false</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-201">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-201">Usage</span></span>  
 <span data-ttu-id="4077f-202">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-202">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-203">**Avsnitt i princip:** backend</span><span class="sxs-lookup"><span data-stu-id="4077f-203">**Policy sections:** backend</span></span>  
  
-   <span data-ttu-id="4077f-204">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-204">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-205"><a name="LimitConcurrency"></a>Gränsen för samtidighet</span><span class="sxs-lookup"><span data-stu-id="4077f-205"><a name="LimitConcurrency"></a> Limit concurrency</span></span>  
 <span data-ttu-id="4077f-206">Den `limit-concurrency` princip förhindrar att omslutna principer körning av fler än det angivna antalet förfrågningar vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="4077f-206">The `limit-concurrency` policy prevents enclosed policies from executing by more than the specified number of requests at a given time.</span></span> <span data-ttu-id="4077f-207">På överskrider tröskeln läggs nya begäranden till en kö tills maximala kölängden uppnås.</span><span class="sxs-lookup"><span data-stu-id="4077f-207">Upon exceeding the threshold, new requests are added to a queue, until the maximum queue length is achieved.</span></span> <span data-ttu-id="4077f-208">När kön uttömning misslyckas nya begäranden omedelbart.</span><span class="sxs-lookup"><span data-stu-id="4077f-208">Upon queue exhaustion, new requests will fail immediately.</span></span>
  
###  <span data-ttu-id="4077f-209"><a name="LimitConcurrencyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-209"><a name="LimitConcurrencyStatement"></a> Policy statement</span></span>  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a><span data-ttu-id="4077f-210">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-210">Examples</span></span>  
  
####  <span data-ttu-id="4077f-211"><a name="ChooseExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-211"><a name="ChooseExample"></a> Example</span></span>  
 <span data-ttu-id="4077f-212">Exemplet nedan visar hur du begränsar antalet begäranden som vidarebefordras till en serverdel baserat på värdet för en variabel i kontexten.</span><span class="sxs-lookup"><span data-stu-id="4077f-212">The following example demonstrates how to limit number of requests forwarded to a backend based on the value of a context variable.</span></span>
 
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

### <a name="elements"></a><span data-ttu-id="4077f-213">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-213">Elements</span></span>  
  
|<span data-ttu-id="4077f-214">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-214">Element</span></span>|<span data-ttu-id="4077f-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-215">Description</span></span>|<span data-ttu-id="4077f-216">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-216">Required</span></span>|  
|-------------|-----------------|--------------|    
|<span data-ttu-id="4077f-217">gränsen för samtidighet</span><span class="sxs-lookup"><span data-stu-id="4077f-217">limit-concurrency</span></span>|<span data-ttu-id="4077f-218">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-218">Root element.</span></span>|<span data-ttu-id="4077f-219">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-219">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-220">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-220">Attributes</span></span>  
  
|<span data-ttu-id="4077f-221">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-221">Attribute</span></span>|<span data-ttu-id="4077f-222">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-222">Description</span></span>|<span data-ttu-id="4077f-223">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-223">Required</span></span>|<span data-ttu-id="4077f-224">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-224">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="4077f-225">key</span><span class="sxs-lookup"><span data-stu-id="4077f-225">key</span></span>|<span data-ttu-id="4077f-226">En sträng.</span><span class="sxs-lookup"><span data-stu-id="4077f-226">A string.</span></span> <span data-ttu-id="4077f-227">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="4077f-227">Expression allowed.</span></span> <span data-ttu-id="4077f-228">Anger samtidighet scope.</span><span class="sxs-lookup"><span data-stu-id="4077f-228">Specifies the concurrency scope.</span></span> <span data-ttu-id="4077f-229">Kan delas av flera principer.</span><span class="sxs-lookup"><span data-stu-id="4077f-229">Can be shared by multiple policies.</span></span>|<span data-ttu-id="4077f-230">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-230">Yes</span></span>|<span data-ttu-id="4077f-231">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-231">N/A</span></span>|  
|<span data-ttu-id="4077f-232">Max antal</span><span class="sxs-lookup"><span data-stu-id="4077f-232">max-count</span></span>|<span data-ttu-id="4077f-233">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="4077f-233">An integer.</span></span> <span data-ttu-id="4077f-234">Anger maximalt antal begäranden som tillåts att ange principen.</span><span class="sxs-lookup"><span data-stu-id="4077f-234">Specifies a maximum number of requests that are allowed to enter the policy.</span></span>|<span data-ttu-id="4077f-235">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-235">Yes</span></span>|<span data-ttu-id="4077f-236">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-236">N/A</span></span>|  
|<span data-ttu-id="4077f-237">Timeout</span><span class="sxs-lookup"><span data-stu-id="4077f-237">timeout</span></span>|<span data-ttu-id="4077f-238">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="4077f-238">An integer.</span></span> <span data-ttu-id="4077f-239">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="4077f-239">Expression allowed.</span></span> <span data-ttu-id="4077f-240">Anger antalet sekunder som en begäran ska vänta med att ange ett scope innan åtgärden misslyckas med ”403 för många begäranden”</span><span class="sxs-lookup"><span data-stu-id="4077f-240">Specifies the number of seconds a request should wait to enter a scope before failing with "403 Too Many Requests"</span></span>|<span data-ttu-id="4077f-241">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-241">No</span></span>|<span data-ttu-id="4077f-242">Infinity</span><span class="sxs-lookup"><span data-stu-id="4077f-242">Infinity</span></span>|  
|<span data-ttu-id="4077f-243">Max Kölängd</span><span class="sxs-lookup"><span data-stu-id="4077f-243">max-queue-length</span></span>|<span data-ttu-id="4077f-244">Ett heltal.</span><span class="sxs-lookup"><span data-stu-id="4077f-244">An integer.</span></span> <span data-ttu-id="4077f-245">Uttryck tillåts.</span><span class="sxs-lookup"><span data-stu-id="4077f-245">Expression allowed.</span></span> <span data-ttu-id="4077f-246">Anger den maximala längden.</span><span class="sxs-lookup"><span data-stu-id="4077f-246">Specifies the maximum queue length.</span></span> <span data-ttu-id="4077f-247">Inkommande begäranden försök att ange den här principen kommer att avslutas med ”403 för många begäranden” omedelbart när kön är slut.</span><span class="sxs-lookup"><span data-stu-id="4077f-247">Incoming requests trying to enter this policy will be terminated with “403 Too Many Requests” immediately when the queue is exhausted.</span></span>|<span data-ttu-id="4077f-248">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-248">No</span></span>|<span data-ttu-id="4077f-249">Infinity</span><span class="sxs-lookup"><span data-stu-id="4077f-249">Infinity</span></span>|  
  
###  <span data-ttu-id="4077f-250"><a name="ChooseUsage"></a>Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-250"><a name="ChooseUsage"></a> Usage</span></span>  
 <span data-ttu-id="4077f-251">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-251">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-252">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-252">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-253">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-253">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="4077f-254"><a name="log-to-eventhub"></a>Loggen till Händelsehubb</span><span class="sxs-lookup"><span data-stu-id="4077f-254"><a name="log-to-eventhub"></a> Log to Event Hub</span></span>  
 <span data-ttu-id="4077f-255">Den `log-to-eventhub` princip skickar meddelanden i det angivna formatet till en Händelsehubb som definieras av en loggaren entitet.</span><span class="sxs-lookup"><span data-stu-id="4077f-255">The `log-to-eventhub` policy sends messages in the specified format to an Event Hub defined by a Logger entity.</span></span> <span data-ttu-id="4077f-256">Som namnet antyder används principen för att spara valda begäran eller svar omständighetsinformation för analys online eller offline.</span><span class="sxs-lookup"><span data-stu-id="4077f-256">As its name implies, the policy is used for saving selected request or response context information for online or offline analysis.</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="4077f-257">Stegvisa instruktioner om hur du konfigurerar en händelsehubb och loggning av händelser, se [så logghändelser API-hantering med Händelsehubbar](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span><span class="sxs-lookup"><span data-stu-id="4077f-257">For a step-by-step guide on configuring an event hub and logging events, see [How to log API Management events with Azure Event Hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-258">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-258">Policy statement</span></span>  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-259">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-259">Example</span></span>  
 <span data-ttu-id="4077f-260">Valfri sträng kan användas som värde som ska loggas i Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="4077f-260">Any string can be used as the value to be logged in Event Hubs.</span></span> <span data-ttu-id="4077f-261">I det här exemplet datum och tid, tjänstnamn för distribution, förfrågnings-id, ip-adress och åtgärdsnamn för alla inkommande samtal loggas till händelsehubben loggaren registrerats med den `contoso-logger` id.</span><span class="sxs-lookup"><span data-stu-id="4077f-261">In this example the date and time, deployment service name, request id, ip address, and operation name for all inbound calls are logged to the event hub Logger registered with the `contoso-logger` id.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-262">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-262">Elements</span></span>  
  
|<span data-ttu-id="4077f-263">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-263">Element</span></span>|<span data-ttu-id="4077f-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-264">Description</span></span>|<span data-ttu-id="4077f-265">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-265">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-266">loggen till eventhub</span><span class="sxs-lookup"><span data-stu-id="4077f-266">log-to-eventhub</span></span>|<span data-ttu-id="4077f-267">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-267">Root element.</span></span> <span data-ttu-id="4077f-268">Värdet för det här elementet är sträng att logga in till din event hub.</span><span class="sxs-lookup"><span data-stu-id="4077f-268">The value of this element is the string to log to your event hub.</span></span>|<span data-ttu-id="4077f-269">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-269">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-270">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-270">Attributes</span></span>  
  
|<span data-ttu-id="4077f-271">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-271">Attribute</span></span>|<span data-ttu-id="4077f-272">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-272">Description</span></span>|<span data-ttu-id="4077f-273">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-273">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="4077f-274">loggaren-id</span><span class="sxs-lookup"><span data-stu-id="4077f-274">logger-id</span></span>|<span data-ttu-id="4077f-275">Id för loggaren registrerats API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4077f-275">The id of the Logger registered with your API Management service.</span></span>|<span data-ttu-id="4077f-276">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-276">Yes</span></span>|  
|<span data-ttu-id="4077f-277">partitions-id</span><span class="sxs-lookup"><span data-stu-id="4077f-277">partition-id</span></span>|<span data-ttu-id="4077f-278">Anger index för partitionen som meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="4077f-278">Specifies the index of the partition where messages are sent.</span></span>|<span data-ttu-id="4077f-279">Valfri.</span><span class="sxs-lookup"><span data-stu-id="4077f-279">Optional.</span></span> <span data-ttu-id="4077f-280">Det här attributet kan inte användas om `partition-key` används.</span><span class="sxs-lookup"><span data-stu-id="4077f-280">This attribute may not be used if `partition-key` is used.</span></span>|  
|<span data-ttu-id="4077f-281">Partitionsnyckeln</span><span class="sxs-lookup"><span data-stu-id="4077f-281">partition-key</span></span>|<span data-ttu-id="4077f-282">Anger det värde som används för tilldelning av partitionen när meddelanden skickas.</span><span class="sxs-lookup"><span data-stu-id="4077f-282">Specifies the value used for partition assignment when messages are sent.</span></span>|<span data-ttu-id="4077f-283">Valfri.</span><span class="sxs-lookup"><span data-stu-id="4077f-283">Optional.</span></span> <span data-ttu-id="4077f-284">Det här attributet kan inte användas om `partition-id` används.</span><span class="sxs-lookup"><span data-stu-id="4077f-284">This attribute may not be used if `partition-id` is used.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-285">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-285">Usage</span></span>  
 <span data-ttu-id="4077f-286">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-286">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-287">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-287">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-288">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-288">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="4077f-289"><a name="mock-response"></a>Fingerad svar</span><span class="sxs-lookup"><span data-stu-id="4077f-289"><a name="mock-response"></a> Mock response</span></span>  
<span data-ttu-id="4077f-290">Den `mock-response`, som namn innebär, används för att mock API: er och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="4077f-290">The `mock-response`, as the name implies, is used to mock APIs and operations.</span></span> <span data-ttu-id="4077f-291">Den normala pipelinekörningen avbryts och returnerar ett mocked svar till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-291">It aborts normal pipeline execution and returns a mocked response to the caller.</span></span> <span data-ttu-id="4077f-292">Principen försöker alltid returnera svar för högsta återgivning.</span><span class="sxs-lookup"><span data-stu-id="4077f-292">The policy always tries to return responses of highest fidelity.</span></span> <span data-ttu-id="4077f-293">Den föredrar svar innehåll exempel, när det finns tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="4077f-293">It prefers response content examples, whenever available.</span></span> <span data-ttu-id="4077f-294">Den genererar exempel svar från scheman, när scheman har angetts och exempel finns inte.</span><span class="sxs-lookup"><span data-stu-id="4077f-294">It generates sample responses from schemas, when schemas are provided and examples are not.</span></span> <span data-ttu-id="4077f-295">Om varken exempel eller scheman hittas returneras svar med inget innehåll.</span><span class="sxs-lookup"><span data-stu-id="4077f-295">If neither examples or schemas are found, responses with no content are returned.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-296">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-296">Policy statement</span></span>  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a><span data-ttu-id="4077f-297">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-297">Examples</span></span>  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-298">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-298">Elements</span></span>  
  
|<span data-ttu-id="4077f-299">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-299">Element</span></span>|<span data-ttu-id="4077f-300">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-300">Description</span></span>|<span data-ttu-id="4077f-301">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-301">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-302">mock-svar</span><span class="sxs-lookup"><span data-stu-id="4077f-302">mock-response</span></span>|<span data-ttu-id="4077f-303">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-303">Root element.</span></span>|<span data-ttu-id="4077f-304">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-304">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-305">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-305">Attributes</span></span>  
  
|<span data-ttu-id="4077f-306">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-306">Attribute</span></span>|<span data-ttu-id="4077f-307">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-307">Description</span></span>|<span data-ttu-id="4077f-308">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-308">Required</span></span>|<span data-ttu-id="4077f-309">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-309">Default</span></span>|  
|---------------|-----------------|--------------|--------------|  
|<span data-ttu-id="4077f-310">statuskod</span><span class="sxs-lookup"><span data-stu-id="4077f-310">status-code</span></span>|<span data-ttu-id="4077f-311">Anger Svarets statuskod och används för att välja motsvarande exempel eller schema.</span><span class="sxs-lookup"><span data-stu-id="4077f-311">Specifies response status code and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="4077f-312">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-312">No</span></span>|<span data-ttu-id="4077f-313">200</span><span class="sxs-lookup"><span data-stu-id="4077f-313">200</span></span>|  
|<span data-ttu-id="4077f-314">innehållstyp</span><span class="sxs-lookup"><span data-stu-id="4077f-314">content-type</span></span>|<span data-ttu-id="4077f-315">Anger `Content-Type` svar huvudets värde och används för att välja motsvarande exempel eller schema.</span><span class="sxs-lookup"><span data-stu-id="4077f-315">Specifies `Content-Type` response header value and is used to select corresponding example or schema.</span></span>|<span data-ttu-id="4077f-316">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-316">No</span></span>|<span data-ttu-id="4077f-317">Ingen</span><span class="sxs-lookup"><span data-stu-id="4077f-317">None</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-318">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-318">Usage</span></span>  
 <span data-ttu-id="4077f-319">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-319">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-320">**Avsnitt i princip:** inkommande, utgående, vid fel</span><span class="sxs-lookup"><span data-stu-id="4077f-320">**Policy sections:** inbound, outbound, on-error</span></span>  
  
-   <span data-ttu-id="4077f-321">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-321">**Policy scopes:** all scopes</span></span>

##  <span data-ttu-id="4077f-322"><a name="Retry"></a>Försök igen</span><span class="sxs-lookup"><span data-stu-id="4077f-322"><a name="Retry"></a> Retry</span></span>  
 <span data-ttu-id="4077f-323">Den `retry` princip kör dess underordnade principer en gång och sedan försöker körningen tills för och försök igen `condition` blir `false` eller försök `count` är slut.</span><span class="sxs-lookup"><span data-stu-id="4077f-323">The             `retry` policy executes its child policies once and then retries their execution until the retry `condition` becomes            `false` or retry            `count` is exhausted.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-324">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-324">Policy statement</span></span>  
  
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
  
### <a name="example"></a><span data-ttu-id="4077f-325">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-325">Example</span></span>  
 <span data-ttu-id="4077f-326">I följande exempelbegäran försöks forewarding upp till tio gånger med exponentiell försök algoritm.</span><span class="sxs-lookup"><span data-stu-id="4077f-326">In the following example request forewarding is retried up to ten times using exponential retry algorithm.</span></span> <span data-ttu-id="4077f-327">Eftersom `first-fast-retry` har angetts till false, alla nya försök regleras av algoritmen exponsntial försök igen.</span><span class="sxs-lookup"><span data-stu-id="4077f-327">Since                    `first-fast-retry` is set to false, all retry attempts are subject to the exponsntial retry algorithm.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-328">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-328">Elements</span></span>  
  
|<span data-ttu-id="4077f-329">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-329">Element</span></span>|<span data-ttu-id="4077f-330">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-330">Description</span></span>|<span data-ttu-id="4077f-331">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-331">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-332">Försök igen</span><span class="sxs-lookup"><span data-stu-id="4077f-332">retry</span></span>|<span data-ttu-id="4077f-333">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-333">Root element.</span></span> <span data-ttu-id="4077f-334">Kan innehålla andra principer som dess underordnade element.</span><span class="sxs-lookup"><span data-stu-id="4077f-334">May contain any other policies as its child elements.</span></span>|<span data-ttu-id="4077f-335">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-335">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-336">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-336">Attributes</span></span>  
  
|<span data-ttu-id="4077f-337">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-337">Attribute</span></span>|<span data-ttu-id="4077f-338">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-338">Description</span></span>|<span data-ttu-id="4077f-339">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-339">Required</span></span>|<span data-ttu-id="4077f-340">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-340">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-341">Villkor</span><span class="sxs-lookup"><span data-stu-id="4077f-341">condition</span></span>|<span data-ttu-id="4077f-342">En boolesk literal eller [uttryck](api-management-policy-expressions.md) anger om återförsök ska stoppas (`false`) eller fortsatte (`true`).</span><span class="sxs-lookup"><span data-stu-id="4077f-342">A boolean literal or [expression](api-management-policy-expressions.md) specifying if retries should be stopped (`false`) or continued (`true`).</span></span>|<span data-ttu-id="4077f-343">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-343">Yes</span></span>|<span data-ttu-id="4077f-344">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-344">N/A</span></span>|  
|<span data-ttu-id="4077f-345">Antal</span><span class="sxs-lookup"><span data-stu-id="4077f-345">count</span></span>|<span data-ttu-id="4077f-346">Ett positivt tal som anger det maximala antalet försök att försöka.</span><span class="sxs-lookup"><span data-stu-id="4077f-346">A positive number specifying the maximum number of retries to attempt.</span></span>|<span data-ttu-id="4077f-347">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-347">Yes</span></span>|<span data-ttu-id="4077f-348">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-348">N/A</span></span>|  
|<span data-ttu-id="4077f-349">intervall</span><span class="sxs-lookup"><span data-stu-id="4077f-349">interval</span></span>|<span data-ttu-id="4077f-350">Ett positivt tal i sekunder som anger vänta intervall mellan det nya försöket försöker.</span><span class="sxs-lookup"><span data-stu-id="4077f-350">A positive number in seconds specifying the wait interval between the retry attempts.</span></span>|<span data-ttu-id="4077f-351">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-351">Yes</span></span>|<span data-ttu-id="4077f-352">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-352">N/A</span></span>|  
|<span data-ttu-id="4077f-353">Max-intervall</span><span class="sxs-lookup"><span data-stu-id="4077f-353">max-interval</span></span>|<span data-ttu-id="4077f-354">Ett positivt tal i sekunder som anger maximalt vänta mellan nya försök.</span><span class="sxs-lookup"><span data-stu-id="4077f-354">A positive number in seconds specifying the maximum wait interval between the retry attempts.</span></span> <span data-ttu-id="4077f-355">Den används för att implementera en algoritm exponentiell försök igen.</span><span class="sxs-lookup"><span data-stu-id="4077f-355">It is used to implement an exponential retry algorithm.</span></span>|<span data-ttu-id="4077f-356">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-356">No</span></span>|<span data-ttu-id="4077f-357">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-357">N/A</span></span>|  
|<span data-ttu-id="4077f-358">delta</span><span class="sxs-lookup"><span data-stu-id="4077f-358">delta</span></span>|<span data-ttu-id="4077f-359">Ett positivt tal i sekunder som anger att vänta intervall ökning.</span><span class="sxs-lookup"><span data-stu-id="4077f-359">A positive number in seconds specifying the wait interval increment.</span></span> <span data-ttu-id="4077f-360">Används för att implementera linjär och exponentiella retry-algoritmer.</span><span class="sxs-lookup"><span data-stu-id="4077f-360">It is used to implement the linear and exponential retry algorithms.</span></span>|<span data-ttu-id="4077f-361">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-361">No</span></span>|<span data-ttu-id="4077f-362">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-362">N/A</span></span>|  
|<span data-ttu-id="4077f-363">första-fast-återförsök</span><span class="sxs-lookup"><span data-stu-id="4077f-363">first-fast-retry</span></span>|<span data-ttu-id="4077f-364">Om värdet `true` , första nytt försök utförs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="4077f-364">If set to                                    `true` , the first retry attempt is performed immediately.</span></span>|<span data-ttu-id="4077f-365">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-365">No</span></span>|`false`|  
  
> [!NOTE]
>  <span data-ttu-id="4077f-366">När bara den `interval` anges **fast** intervall för nya försök utförs.</span><span class="sxs-lookup"><span data-stu-id="4077f-366">When only the `interval` is specified, **fixed** interval retries are performed.</span></span>  
>  <span data-ttu-id="4077f-367">När bara den `interval` och `delta` har angetts en **linjär** intervall försök algoritmen används, där väntetiden mellan försök beräknas enligt följande formel - `interval + (count - 1)*delta`.</span><span class="sxs-lookup"><span data-stu-id="4077f-367">When only the `interval` and `delta` are specified, a **linear** interval retry algorithm is used, where wait time between retries is calculated according the following formula - `interval + (count - 1)*delta`.</span></span>  
>  <span data-ttu-id="4077f-368">När den `interval`, `max-interval` och `delta` anges, **exponentiell** intervall försök algoritmen används där väntetiden mellan försöken ökar exponentiellt från värdet för `interval` till värdet `max-interval` enligt följande forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span><span class="sxs-lookup"><span data-stu-id="4077f-368">When the `interval`, `max-interval` and `delta` are specified, **exponential** interval retry algorithm is applied, where the wait time between the retries is growing exponentially from the value of `interval` to the value `max-interval` according to the following forumula - `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.</span></span>  
  
### <a name="usage"></a><span data-ttu-id="4077f-369">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-369">Usage</span></span>  
 <span data-ttu-id="4077f-370">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="4077f-370">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span> <span data-ttu-id="4077f-371">Observera att ärvs underordnade principbegränsningar för användning av den här principen.</span><span class="sxs-lookup"><span data-stu-id="4077f-371">Note that child policy usage restrictions will be inherited by this policy.</span></span>  
  
-   <span data-ttu-id="4077f-372">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-372">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-373">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-373">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-374"><a name="ReturnResponse"></a>Returnera svar</span><span class="sxs-lookup"><span data-stu-id="4077f-374"><a name="ReturnResponse"></a> Return response</span></span>  
 <span data-ttu-id="4077f-375">Den `return-response` principen avbryter pipelinekörningen och returnerar ett standardvärde eller anpassade svar till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-375">The `return-response` policy aborts pipeline execution and returns either a default or custom response to the caller.</span></span> <span data-ttu-id="4077f-376">Standard-svaret är `200 OK` med ingen brödtext.</span><span class="sxs-lookup"><span data-stu-id="4077f-376">Default response is `200 OK` with no body.</span></span> <span data-ttu-id="4077f-377">Anpassade svar kan anges via en kontext variabel eller princip-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="4077f-377">Custom response can be specified via a context variable or policy statements.</span></span> <span data-ttu-id="4077f-378">När både tillhandahålls ändras svaret finns i kontexten variabeln av principen instruktionerna innan de returneras till anroparen.</span><span class="sxs-lookup"><span data-stu-id="4077f-378">When both are provided, the response contained within the context variable is modified by the policy statements before being returned to the caller.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-379">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-379">Policy statement</span></span>  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-380">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-380">Example</span></span>  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-381">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-381">Elements</span></span>  
  
|<span data-ttu-id="4077f-382">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-382">Element</span></span>|<span data-ttu-id="4077f-383">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-383">Description</span></span>|<span data-ttu-id="4077f-384">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-384">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-385">returnera svar</span><span class="sxs-lookup"><span data-stu-id="4077f-385">return-response</span></span>|<span data-ttu-id="4077f-386">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-386">Root element.</span></span>|<span data-ttu-id="4077f-387">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-387">Yes</span></span>|  
|<span data-ttu-id="4077f-388">set-huvud</span><span class="sxs-lookup"><span data-stu-id="4077f-388">set-header</span></span>|<span data-ttu-id="4077f-389">En [set-huvudet](api-management-transformation-policies.md#SetHTTPheader) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="4077f-389">A [set-header](api-management-transformation-policies.md#SetHTTPheader) policy statement.</span></span>|<span data-ttu-id="4077f-390">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-390">No</span></span>|  
|<span data-ttu-id="4077f-391">Ange brödtext</span><span class="sxs-lookup"><span data-stu-id="4077f-391">set-body</span></span>|<span data-ttu-id="4077f-392">En [set brödtext](api-management-transformation-policies.md#SetBody) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="4077f-392">A [set-body](api-management-transformation-policies.md#SetBody) policy statement.</span></span>|<span data-ttu-id="4077f-393">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-393">No</span></span>|  
|<span data-ttu-id="4077f-394">Ange status</span><span class="sxs-lookup"><span data-stu-id="4077f-394">set-status</span></span>|<span data-ttu-id="4077f-395">En [Ange status](api-management-advanced-policies.md#SetStatus) princip-satsen.</span><span class="sxs-lookup"><span data-stu-id="4077f-395">A [set-status](api-management-advanced-policies.md#SetStatus) policy statement.</span></span>|<span data-ttu-id="4077f-396">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-396">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-397">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-397">Attributes</span></span>  
  
|<span data-ttu-id="4077f-398">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-398">Attribute</span></span>|<span data-ttu-id="4077f-399">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-399">Description</span></span>|<span data-ttu-id="4077f-400">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-400">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="4077f-401">svaret variabelnamn</span><span class="sxs-lookup"><span data-stu-id="4077f-401">response-variable-name</span></span>|<span data-ttu-id="4077f-402">Namnet på variabeln kontexten refereras från, till exempel en uppströms [-begäran om att skicka](api-management-advanced-policies.md#SendRequest) principen och som innehåller en `Response` objekt</span><span class="sxs-lookup"><span data-stu-id="4077f-402">The name of the context variable referenced from, for example, an upstream [send-request](api-management-advanced-policies.md#SendRequest) policy and containing a `Response` object</span></span>|<span data-ttu-id="4077f-403">Valfri.</span><span class="sxs-lookup"><span data-stu-id="4077f-403">Optional.</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-404">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-404">Usage</span></span>  
 <span data-ttu-id="4077f-405">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-405">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-406">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-406">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-407">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-407">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-408"><a name="SendOneWayRequest"></a>Skicka förfrågan om ett sätt</span><span class="sxs-lookup"><span data-stu-id="4077f-408"><a name="SendOneWayRequest"></a> Send one way request</span></span>  
 <span data-ttu-id="4077f-409">Den `send-one-way-request` princip angivna begäran skickas till den angivna URL: en utan att vänta på ett svar.</span><span class="sxs-lookup"><span data-stu-id="4077f-409">The `send-one-way-request` policy sends the provided request to the specified URL without waiting for a response.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-410">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-410">Policy statement</span></span>  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-411">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-411">Example</span></span>  
 <span data-ttu-id="4077f-412">Exempel principen visar ett exempel på hur du använder den `send-one-way-request` princip för att skicka ett meddelande till en Slack chatt-rummet om HTTP-svarskoden är större än eller lika med 500.</span><span class="sxs-lookup"><span data-stu-id="4077f-412">This sample policy shows an example of using the `send-one-way-request` policy to send a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="4077f-413">Mer information om det här exemplet finns [med externa tjänster från Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="4077f-413">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-414">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-414">Elements</span></span>  
  
|<span data-ttu-id="4077f-415">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-415">Element</span></span>|<span data-ttu-id="4077f-416">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-416">Description</span></span>|<span data-ttu-id="4077f-417">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-417">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-418">en-sätt-begäran om att skicka</span><span class="sxs-lookup"><span data-stu-id="4077f-418">send-one-way-request</span></span>|<span data-ttu-id="4077f-419">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-419">Root element.</span></span>|<span data-ttu-id="4077f-420">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-420">Yes</span></span>|  
|<span data-ttu-id="4077f-421">URL: en</span><span class="sxs-lookup"><span data-stu-id="4077f-421">url</span></span>|<span data-ttu-id="4077f-422">URL för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-422">The URL of the request.</span></span>|<span data-ttu-id="4077f-423">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="4077f-423">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="4077f-424">Metoden</span><span class="sxs-lookup"><span data-stu-id="4077f-424">method</span></span>|<span data-ttu-id="4077f-425">HTTP-metod för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-425">The HTTP method for the request.</span></span>|<span data-ttu-id="4077f-426">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="4077f-426">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="4077f-427">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="4077f-427">header</span></span>|<span data-ttu-id="4077f-428">Huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-428">Request header.</span></span> <span data-ttu-id="4077f-429">Använda flera huvud-element för flera huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-429">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="4077f-430">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-430">No</span></span>|  
|<span data-ttu-id="4077f-431">Brödtext</span><span class="sxs-lookup"><span data-stu-id="4077f-431">body</span></span>|<span data-ttu-id="4077f-432">Begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="4077f-432">The request body.</span></span>|<span data-ttu-id="4077f-433">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-433">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-434">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-434">Attributes</span></span>  
  
|<span data-ttu-id="4077f-435">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-435">Attribute</span></span>|<span data-ttu-id="4077f-436">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-436">Description</span></span>|<span data-ttu-id="4077f-437">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-437">Required</span></span>|<span data-ttu-id="4077f-438">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-438">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-439">mode = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-439">mode="string"</span></span>|<span data-ttu-id="4077f-440">Anger om detta är en ny begäran eller en kopia av den aktuella begäranden.</span><span class="sxs-lookup"><span data-stu-id="4077f-440">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="4077f-441">I utgående läge, läge = kopiera initieras inte begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="4077f-441">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="4077f-442">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-442">No</span></span>|<span data-ttu-id="4077f-443">Ny</span><span class="sxs-lookup"><span data-stu-id="4077f-443">New</span></span>|  
|<span data-ttu-id="4077f-444">namn</span><span class="sxs-lookup"><span data-stu-id="4077f-444">name</span></span>|<span data-ttu-id="4077f-445">Anger namnet på rubriken anges.</span><span class="sxs-lookup"><span data-stu-id="4077f-445">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="4077f-446">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-446">Yes</span></span>|<span data-ttu-id="4077f-447">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-447">N/A</span></span>|  
|<span data-ttu-id="4077f-448">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="4077f-448">exists-action</span></span>|<span data-ttu-id="4077f-449">Anger vilken åtgärd som ska vidtas när huvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="4077f-449">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="4077f-450">Det här attributet måste ha något av följande värden.</span><span class="sxs-lookup"><span data-stu-id="4077f-450">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="4077f-451">-åsidosätt - ersätter värdet för befintliga-huvud.</span><span class="sxs-lookup"><span data-stu-id="4077f-451">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="4077f-452">-skip - ersätter inte det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-452">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="4077f-453">-Tillägg - lägger till värdet på det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-453">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="4077f-454">-delete - tar bort huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-454">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="4077f-455">Om värdet är `override` ta med flera poster med samma namn resulterar i sidhuvudet har angetts enligt alla poster (som visas flera gånger); endast listade värden anges i resultatet.</span><span class="sxs-lookup"><span data-stu-id="4077f-455">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="4077f-456">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-456">No</span></span>|<span data-ttu-id="4077f-457">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="4077f-457">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-458">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-458">Usage</span></span>  
 <span data-ttu-id="4077f-459">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-459">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-460">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-460">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-461">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-461">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-462"><a name="SendRequest"></a>Skicka förfrågan</span><span class="sxs-lookup"><span data-stu-id="4077f-462"><a name="SendRequest"></a> Send request</span></span>  
 <span data-ttu-id="4077f-463">Den `send-request` princip angivna begäran skickas till angiven URL, väntar på längre än ange timeout-värdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-463">The `send-request` policy sends the provided request to the specified URL, waiting no longer than the set timeout value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-464">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-464">Policy statement</span></span>  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-465">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-465">Example</span></span>  
 <span data-ttu-id="4077f-466">Det här exemplet illustrerar ett sätt att kontrollera en referens token med en server för auktorisering.</span><span class="sxs-lookup"><span data-stu-id="4077f-466">This example shows one way to verify a reference token with an authorization server.</span></span> <span data-ttu-id="4077f-467">Mer information om det här exemplet finns [med externa tjänster från Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="4077f-467">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-468">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-468">Elements</span></span>  
  
|<span data-ttu-id="4077f-469">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-469">Element</span></span>|<span data-ttu-id="4077f-470">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-470">Description</span></span>|<span data-ttu-id="4077f-471">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-471">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-472">Skicka begäran</span><span class="sxs-lookup"><span data-stu-id="4077f-472">send-request</span></span>|<span data-ttu-id="4077f-473">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-473">Root element.</span></span>|<span data-ttu-id="4077f-474">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-474">Yes</span></span>|  
|<span data-ttu-id="4077f-475">URL: en</span><span class="sxs-lookup"><span data-stu-id="4077f-475">url</span></span>|<span data-ttu-id="4077f-476">URL för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-476">The URL of the request.</span></span>|<span data-ttu-id="4077f-477">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="4077f-477">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="4077f-478">Metoden</span><span class="sxs-lookup"><span data-stu-id="4077f-478">method</span></span>|<span data-ttu-id="4077f-479">HTTP-metod för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-479">The HTTP method for the request.</span></span>|<span data-ttu-id="4077f-480">Inga om läge = kopian. Annars Ja.</span><span class="sxs-lookup"><span data-stu-id="4077f-480">No if mode=copy; otherwise yes.</span></span>|  
|<span data-ttu-id="4077f-481">sidhuvud</span><span class="sxs-lookup"><span data-stu-id="4077f-481">header</span></span>|<span data-ttu-id="4077f-482">Huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-482">Request header.</span></span> <span data-ttu-id="4077f-483">Använda flera huvud-element för flera huvuden för begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-483">Use multiple header elements for multiple request headers.</span></span>|<span data-ttu-id="4077f-484">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-484">No</span></span>|  
|<span data-ttu-id="4077f-485">Brödtext</span><span class="sxs-lookup"><span data-stu-id="4077f-485">body</span></span>|<span data-ttu-id="4077f-486">Begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="4077f-486">The request body.</span></span>|<span data-ttu-id="4077f-487">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-487">No</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-488">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-488">Attributes</span></span>  
  
|<span data-ttu-id="4077f-489">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-489">Attribute</span></span>|<span data-ttu-id="4077f-490">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-490">Description</span></span>|<span data-ttu-id="4077f-491">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-491">Required</span></span>|<span data-ttu-id="4077f-492">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-492">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-493">mode = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-493">mode="string"</span></span>|<span data-ttu-id="4077f-494">Anger om detta är en ny begäran eller en kopia av den aktuella begäranden.</span><span class="sxs-lookup"><span data-stu-id="4077f-494">Determines whether this is a new request or a copy of the current request.</span></span> <span data-ttu-id="4077f-495">I utgående läge, läge = kopiera initieras inte begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="4077f-495">In outbound mode, mode=copy does not initialize the request body.</span></span>|<span data-ttu-id="4077f-496">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-496">No</span></span>|<span data-ttu-id="4077f-497">Ny</span><span class="sxs-lookup"><span data-stu-id="4077f-497">New</span></span>|  
|<span data-ttu-id="4077f-498">svaret variabelnamn = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-498">response-variable-name="string"</span></span>|<span data-ttu-id="4077f-499">Om den inte finns `context.Response` används.</span><span class="sxs-lookup"><span data-stu-id="4077f-499">If not present, `context.Response` is used.</span></span>|<span data-ttu-id="4077f-500">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-500">No</span></span>|<span data-ttu-id="4077f-501">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-501">N/A</span></span>|  
|<span data-ttu-id="4077f-502">timeout = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="4077f-502">timeout="integer"</span></span>|<span data-ttu-id="4077f-503">Det går inte att timeout-intervall i sekunder innan anropet till URL: en.</span><span class="sxs-lookup"><span data-stu-id="4077f-503">The timeout interval in seconds before the call to the URL fails.</span></span>|<span data-ttu-id="4077f-504">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-504">No</span></span>|<span data-ttu-id="4077f-505">60</span><span class="sxs-lookup"><span data-stu-id="4077f-505">60</span></span>|  
|<span data-ttu-id="4077f-506">Ignorera fel</span><span class="sxs-lookup"><span data-stu-id="4077f-506">ignore-error</span></span>|<span data-ttu-id="4077f-507">Om true, och begäran resulterar i ett fel:</span><span class="sxs-lookup"><span data-stu-id="4077f-507">If true and the request results in an error:</span></span><br /><br /> <span data-ttu-id="4077f-508">– Om svaret variabelnamn angavs innehåller ett null-värde.</span><span class="sxs-lookup"><span data-stu-id="4077f-508">-   If response-variable-name was specified it will contain a null value.</span></span><br /><span data-ttu-id="4077f-509">– Om svaret variabelnamn inte har angetts, kontext. Begäran kommer inte att uppdateras.</span><span class="sxs-lookup"><span data-stu-id="4077f-509">-   If response-variable-name was not specified, context.Request will not be updated.</span></span>|<span data-ttu-id="4077f-510">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-510">No</span></span>|<span data-ttu-id="4077f-511">FALSKT</span><span class="sxs-lookup"><span data-stu-id="4077f-511">false</span></span>|  
|<span data-ttu-id="4077f-512">namn</span><span class="sxs-lookup"><span data-stu-id="4077f-512">name</span></span>|<span data-ttu-id="4077f-513">Anger namnet på rubriken anges.</span><span class="sxs-lookup"><span data-stu-id="4077f-513">Specifies the name of the header to be set.</span></span>|<span data-ttu-id="4077f-514">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-514">Yes</span></span>|<span data-ttu-id="4077f-515">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-515">N/A</span></span>|  
|<span data-ttu-id="4077f-516">Det finns åtgärd</span><span class="sxs-lookup"><span data-stu-id="4077f-516">exists-action</span></span>|<span data-ttu-id="4077f-517">Anger vilken åtgärd som ska vidtas när huvudet har redan angetts.</span><span class="sxs-lookup"><span data-stu-id="4077f-517">Specifies what action to take when the header is already specified.</span></span> <span data-ttu-id="4077f-518">Det här attributet måste ha något av följande värden.</span><span class="sxs-lookup"><span data-stu-id="4077f-518">This attribute must have one of the following values.</span></span><br /><br /> <span data-ttu-id="4077f-519">-åsidosätt - ersätter värdet för befintliga-huvud.</span><span class="sxs-lookup"><span data-stu-id="4077f-519">-   override - replaces the value of the existing header.</span></span><br /><span data-ttu-id="4077f-520">-skip - ersätter inte det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-520">-   skip - does not replace the existing header value.</span></span><br /><span data-ttu-id="4077f-521">-Tillägg - lägger till värdet på det befintliga huvudvärdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-521">-   append - appends the value to the existing header value.</span></span><br /><span data-ttu-id="4077f-522">-delete - tar bort huvudet i begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-522">-   delete - removes the header from the request.</span></span><br /><br /> <span data-ttu-id="4077f-523">Om värdet är `override` ta med flera poster med samma namn resulterar i sidhuvudet har angetts enligt alla poster (som visas flera gånger); endast listade värden anges i resultatet.</span><span class="sxs-lookup"><span data-stu-id="4077f-523">When set to `override` enlisting multiple entries with the same name results in the header being set according to all entries (which will be listed multiple times); only listed values will be set in the result.</span></span>|<span data-ttu-id="4077f-524">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-524">No</span></span>|<span data-ttu-id="4077f-525">åsidosätt</span><span class="sxs-lookup"><span data-stu-id="4077f-525">override</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-526">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-526">Usage</span></span>  
 <span data-ttu-id="4077f-527">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-527">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-528">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-528">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-529">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-529">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-530"><a name="SetHttpProxy"></a>Ange HTTP-proxy</span><span class="sxs-lookup"><span data-stu-id="4077f-530"><a name="SetHttpProxy"></a> Set HTTP proxy</span></span>  
 <span data-ttu-id="4077f-531">Den `proxy` principen kan du på väg begäranden vidarebefordras till serverdelar via en HTTP-proxy.</span><span class="sxs-lookup"><span data-stu-id="4077f-531">The `proxy` policy allows you to route requests forwarded to backends via an HTTP proxy.</span></span> <span data-ttu-id="4077f-532">Endast HTTP (HTTPS inte) stöds mellan gatewayen och proxyn.</span><span class="sxs-lookup"><span data-stu-id="4077f-532">Only HTTP (not HTTPS) is supported between the gateway and the proxy.</span></span> <span data-ttu-id="4077f-533">Grundläggande och NTLM-autentisering.</span><span class="sxs-lookup"><span data-stu-id="4077f-533">Basic and NTLM authentication only.</span></span>
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-534">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-534">Policy statement</span></span>  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-535">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-535">Example</span></span>  
<span data-ttu-id="4077f-536">Observera användningen av [egenskaper](api-management-howto-properties.md) som värden för användarnamn och lösenord för att undvika att lagra känslig information i dokumentets princip.</span><span class="sxs-lookup"><span data-stu-id="4077f-536">Note the use of [properties](api-management-howto-properties.md) as values of the username and password to avoid storing sensitive information in the policy document.</span></span>  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-537">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-537">Elements</span></span>  
  
|<span data-ttu-id="4077f-538">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-538">Element</span></span>|<span data-ttu-id="4077f-539">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-539">Description</span></span>|<span data-ttu-id="4077f-540">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-540">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-541">Proxy</span><span class="sxs-lookup"><span data-stu-id="4077f-541">proxy</span></span>|<span data-ttu-id="4077f-542">Rotelementet</span><span class="sxs-lookup"><span data-stu-id="4077f-542">Root element</span></span>|<span data-ttu-id="4077f-543">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-543">Yes</span></span>|  

### <a name="attributes"></a><span data-ttu-id="4077f-544">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-544">Attributes</span></span>  
  
|<span data-ttu-id="4077f-545">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-545">Attribute</span></span>|<span data-ttu-id="4077f-546">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-546">Description</span></span>|<span data-ttu-id="4077f-547">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-547">Required</span></span>|<span data-ttu-id="4077f-548">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-548">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-549">URL = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-549">url="string"</span></span>|<span data-ttu-id="4077f-550">Proxy-URL i form av http://host:port.</span><span class="sxs-lookup"><span data-stu-id="4077f-550">Proxy URL in the form of http://host:port.</span></span>|<span data-ttu-id="4077f-551">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-551">Yes</span></span>|<span data-ttu-id="4077f-552">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-552">N/A</span></span>|  
|<span data-ttu-id="4077f-553">UserName = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-553">username="string"</span></span>|<span data-ttu-id="4077f-554">Användarnamnet som ska användas för autentisering med proxyservern.</span><span class="sxs-lookup"><span data-stu-id="4077f-554">Username to be used for authentication with the proxy.</span></span>|<span data-ttu-id="4077f-555">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-555">No</span></span>|<span data-ttu-id="4077f-556">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-556">N/A</span></span>|  
|<span data-ttu-id="4077f-557">lösenord = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-557">password="string"</span></span>|<span data-ttu-id="4077f-558">Lösenordet som ska användas för autentisering med proxyservern.</span><span class="sxs-lookup"><span data-stu-id="4077f-558">Password to be used for authentication with the proxy.</span></span>|<span data-ttu-id="4077f-559">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-559">No</span></span>|<span data-ttu-id="4077f-560">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-560">N/A</span></span>|  

### <a name="usage"></a><span data-ttu-id="4077f-561">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-561">Usage</span></span>  
 <span data-ttu-id="4077f-562">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-562">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-563">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="4077f-563">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="4077f-564">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-564">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="4077f-565"><a name="SetRequestMethod"></a>Set-metod för begäran</span><span class="sxs-lookup"><span data-stu-id="4077f-565"><a name="SetRequestMethod"></a> Set request method</span></span>  
 <span data-ttu-id="4077f-566">Den `set-method` principen kan du ändra metoden HTTP-begäran för en begäran.</span><span class="sxs-lookup"><span data-stu-id="4077f-566">The `set-method` policy allows you to change the HTTP request method for a request.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-567">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-567">Policy statement</span></span>  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-568">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-568">Example</span></span>  
 <span data-ttu-id="4077f-569">Det här exemplet princip som använder den `set-method` principen visas ett exempel på ett meddelande skickades till en Slack chatt-rum om HTTP-svarskoden är större än eller lika med 500.</span><span class="sxs-lookup"><span data-stu-id="4077f-569">This sample policy that uses the `set-method` policy shows an example of sending a message to a Slack chat room if the HTTP response code is greater than or equal to 500.</span></span> <span data-ttu-id="4077f-570">Mer information om det här exemplet finns [med externa tjänster från Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span><span class="sxs-lookup"><span data-stu-id="4077f-570">For more information on this sample, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-571">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-571">Elements</span></span>  
  
|<span data-ttu-id="4077f-572">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-572">Element</span></span>|<span data-ttu-id="4077f-573">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-573">Description</span></span>|<span data-ttu-id="4077f-574">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-574">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-575">set-metod</span><span class="sxs-lookup"><span data-stu-id="4077f-575">set-method</span></span>|<span data-ttu-id="4077f-576">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-576">Root element.</span></span> <span data-ttu-id="4077f-577">Värdet för elementet anger HTTP-metoden.</span><span class="sxs-lookup"><span data-stu-id="4077f-577">The value of the element specifies the HTTP method.</span></span>|<span data-ttu-id="4077f-578">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-578">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-579">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-579">Usage</span></span>  
 <span data-ttu-id="4077f-580">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-580">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-581">**Avsnitt i princip:** inkommande, vid fel</span><span class="sxs-lookup"><span data-stu-id="4077f-581">**Policy sections:** inbound, on-error</span></span>  
  
-   <span data-ttu-id="4077f-582">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-582">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-583"><a name="SetStatus"></a>Ange statuskoden</span><span class="sxs-lookup"><span data-stu-id="4077f-583"><a name="SetStatus"></a> Set status code</span></span>  
 <span data-ttu-id="4077f-584">Den `set-status` principen anger HTTP-statuskoden med det angivna värdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-584">The `set-status` policy sets the HTTP status code to the specified value.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-585">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-585">Policy statement</span></span>  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-586">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-586">Example</span></span>  
 <span data-ttu-id="4077f-587">Det här exemplet illustrerar hur du skickar tillbaka ett 401 svar om autentiseringstoken är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="4077f-587">This example shows how to return a 401 response if the authorization token is invalid.</span></span> <span data-ttu-id="4077f-588">Mer information finns i [med externa tjänster från Azure API Management-tjänsten](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span><span class="sxs-lookup"><span data-stu-id="4077f-588">For more information, see [Using external services from the Azure API Management service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-589">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-589">Elements</span></span>  
  
|<span data-ttu-id="4077f-590">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-590">Element</span></span>|<span data-ttu-id="4077f-591">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-591">Description</span></span>|<span data-ttu-id="4077f-592">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-592">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-593">Ange status</span><span class="sxs-lookup"><span data-stu-id="4077f-593">set-status</span></span>|<span data-ttu-id="4077f-594">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-594">Root element.</span></span>|<span data-ttu-id="4077f-595">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-595">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-596">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-596">Attributes</span></span>  
  
|<span data-ttu-id="4077f-597">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-597">Attribute</span></span>|<span data-ttu-id="4077f-598">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-598">Description</span></span>|<span data-ttu-id="4077f-599">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-599">Required</span></span>|<span data-ttu-id="4077f-600">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-600">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-601">kod = ”heltal”</span><span class="sxs-lookup"><span data-stu-id="4077f-601">code="integer"</span></span>|<span data-ttu-id="4077f-602">HTTP-statuskoden ska returneras.</span><span class="sxs-lookup"><span data-stu-id="4077f-602">The HTTP status code to return.</span></span>|<span data-ttu-id="4077f-603">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-603">Yes</span></span>|<span data-ttu-id="4077f-604">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-604">N/A</span></span>|  
|<span data-ttu-id="4077f-605">Orsak = ”sträng”</span><span class="sxs-lookup"><span data-stu-id="4077f-605">reason="string"</span></span>|<span data-ttu-id="4077f-606">En beskrivning av orsaken för att returnera statuskoden.</span><span class="sxs-lookup"><span data-stu-id="4077f-606">A description of the reason for returning the status code.</span></span>|<span data-ttu-id="4077f-607">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-607">Yes</span></span>|<span data-ttu-id="4077f-608">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-608">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-609">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-609">Usage</span></span>  
 <span data-ttu-id="4077f-610">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-610">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-611">**Avsnitt i princip:** utgående, backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-611">**Policy sections:** outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-612">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-612">**Policy scopes:** all scopes</span></span>  

##  <span data-ttu-id="4077f-613"><a name="set-variable"></a>Ange variabel</span><span class="sxs-lookup"><span data-stu-id="4077f-613"><a name="set-variable"></a> Set variable</span></span>  
 <span data-ttu-id="4077f-614">Den `set-variable` princip deklarerar en [kontexten](api-management-policy-expressions.md#ContextVariables) variabeln och tilldelar den ett värde som angetts via en [uttryck](api-management-policy-expressions.md) eller en teckensträng.</span><span class="sxs-lookup"><span data-stu-id="4077f-614">The `set-variable` policy declares a [context](api-management-policy-expressions.md#ContextVariables) variable and assigns it a value specified via an [expression](api-management-policy-expressions.md) or a string literal.</span></span> <span data-ttu-id="4077f-615">Om uttrycket innehåller ett litteralvärde kommer att konverteras till en sträng och värdet ska vara `System.String`.</span><span class="sxs-lookup"><span data-stu-id="4077f-615">if the expression contains a literal it will be converted to a string and the type of the value will be `System.String`.</span></span>  
  
###  <span data-ttu-id="4077f-616"><a name="set-variablePolicyStatement"></a>Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-616"><a name="set-variablePolicyStatement"></a> Policy statement</span></span>  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <span data-ttu-id="4077f-617"><a name="set-variableExample"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-617"><a name="set-variableExample"></a> Example</span></span>  
 <span data-ttu-id="4077f-618">I följande exempel visas en uppsättning variabeln princip i avsnittet inkommande.</span><span class="sxs-lookup"><span data-stu-id="4077f-618">The following example demonstrates a set variable policy in the inbound section.</span></span> <span data-ttu-id="4077f-619">Ange variabeln principen skapar en `isMobile` booleskt [kontexten](api-management-policy-expressions.md#ContextVariables) variabel som har angetts till true om den `User-Agent` begäran huvudet innehåller texten `iPad` eller `iPhone`.</span><span class="sxs-lookup"><span data-stu-id="4077f-619">This set variable policy creates an `isMobile` Boolean [context](api-management-policy-expressions.md#ContextVariables) variable that is set to true if the `User-Agent` request header contains the text `iPad` or `iPhone`.</span></span>  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-620">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-620">Elements</span></span>  
  
|<span data-ttu-id="4077f-621">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-621">Element</span></span>|<span data-ttu-id="4077f-622">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-622">Description</span></span>|<span data-ttu-id="4077f-623">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-623">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-624">Ange variabel</span><span class="sxs-lookup"><span data-stu-id="4077f-624">set-variable</span></span>|<span data-ttu-id="4077f-625">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-625">Root element.</span></span>|<span data-ttu-id="4077f-626">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-626">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-627">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-627">Attributes</span></span>  
  
|<span data-ttu-id="4077f-628">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-628">Attribute</span></span>|<span data-ttu-id="4077f-629">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-629">Description</span></span>|<span data-ttu-id="4077f-630">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-630">Required</span></span>|  
|---------------|-----------------|--------------|  
|<span data-ttu-id="4077f-631">namn</span><span class="sxs-lookup"><span data-stu-id="4077f-631">name</span></span>|<span data-ttu-id="4077f-632">Namnet på variabeln.</span><span class="sxs-lookup"><span data-stu-id="4077f-632">The name of the variable.</span></span>|<span data-ttu-id="4077f-633">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-633">Yes</span></span>|  
|<span data-ttu-id="4077f-634">värde</span><span class="sxs-lookup"><span data-stu-id="4077f-634">value</span></span>|<span data-ttu-id="4077f-635">Värdet för variabeln.</span><span class="sxs-lookup"><span data-stu-id="4077f-635">The value of the variable.</span></span> <span data-ttu-id="4077f-636">Detta kan vara ett uttryck eller ett litteralvärde.</span><span class="sxs-lookup"><span data-stu-id="4077f-636">This can be an expression or a literal value.</span></span>|<span data-ttu-id="4077f-637">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-637">Yes</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-638">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-638">Usage</span></span>  
 <span data-ttu-id="4077f-639">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-639">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-640">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-640">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-641">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-641">**Policy scopes:** all scopes</span></span>  
  
###  <span data-ttu-id="4077f-642"><a name="set-variableAllowedTypes"></a>Tillåtna typer</span><span class="sxs-lookup"><span data-stu-id="4077f-642"><a name="set-variableAllowedTypes"></a> Allowed types</span></span>  
 <span data-ttu-id="4077f-643">Uttryck som används i den `set-variable` principen måste returnera ett av följande grundläggande typer.</span><span class="sxs-lookup"><span data-stu-id="4077f-643">Expressions used in the `set-variable` policy must return one of the following basic types.</span></span>  
  
-   <span data-ttu-id="4077f-644">System.Boolean</span><span class="sxs-lookup"><span data-stu-id="4077f-644">System.Boolean</span></span>  
  
-   <span data-ttu-id="4077f-645">System.SByte</span><span class="sxs-lookup"><span data-stu-id="4077f-645">System.SByte</span></span>  
  
-   <span data-ttu-id="4077f-646">System.Byte</span><span class="sxs-lookup"><span data-stu-id="4077f-646">System.Byte</span></span>  
  
-   <span data-ttu-id="4077f-647">System.UInt16</span><span class="sxs-lookup"><span data-stu-id="4077f-647">System.UInt16</span></span>  
  
-   <span data-ttu-id="4077f-648">System.UInt32</span><span class="sxs-lookup"><span data-stu-id="4077f-648">System.UInt32</span></span>  
  
-   <span data-ttu-id="4077f-649">System.UInt64</span><span class="sxs-lookup"><span data-stu-id="4077f-649">System.UInt64</span></span>  
  
-   <span data-ttu-id="4077f-650">System.Int16</span><span class="sxs-lookup"><span data-stu-id="4077f-650">System.Int16</span></span>  
  
-   <span data-ttu-id="4077f-651">System.Int32</span><span class="sxs-lookup"><span data-stu-id="4077f-651">System.Int32</span></span>  
  
-   <span data-ttu-id="4077f-652">System.Int64</span><span class="sxs-lookup"><span data-stu-id="4077f-652">System.Int64</span></span>  
  
-   <span data-ttu-id="4077f-653">System.Decimal</span><span class="sxs-lookup"><span data-stu-id="4077f-653">System.Decimal</span></span>  
  
-   <span data-ttu-id="4077f-654">System.Single</span><span class="sxs-lookup"><span data-stu-id="4077f-654">System.Single</span></span>  
  
-   <span data-ttu-id="4077f-655">System.Double</span><span class="sxs-lookup"><span data-stu-id="4077f-655">System.Double</span></span>  
  
-   <span data-ttu-id="4077f-656">System.Guid</span><span class="sxs-lookup"><span data-stu-id="4077f-656">System.Guid</span></span>  
  
-   <span data-ttu-id="4077f-657">System.String</span><span class="sxs-lookup"><span data-stu-id="4077f-657">System.String</span></span>  
  
-   <span data-ttu-id="4077f-658">System.Char</span><span class="sxs-lookup"><span data-stu-id="4077f-658">System.Char</span></span>  
  
-   <span data-ttu-id="4077f-659">System.DateTime</span><span class="sxs-lookup"><span data-stu-id="4077f-659">System.DateTime</span></span>  
  
-   <span data-ttu-id="4077f-660">System.TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4077f-660">System.TimeSpan</span></span>  
  
-   <span data-ttu-id="4077f-661">System.Byte?</span><span class="sxs-lookup"><span data-stu-id="4077f-661">System.Byte?</span></span>  
  
-   <span data-ttu-id="4077f-662">System.UInt16?</span><span class="sxs-lookup"><span data-stu-id="4077f-662">System.UInt16?</span></span>  
  
-   <span data-ttu-id="4077f-663">System.UInt32?</span><span class="sxs-lookup"><span data-stu-id="4077f-663">System.UInt32?</span></span>  
  
-   <span data-ttu-id="4077f-664">System.UInt64?</span><span class="sxs-lookup"><span data-stu-id="4077f-664">System.UInt64?</span></span>  
  
-   <span data-ttu-id="4077f-665">System.Int16?</span><span class="sxs-lookup"><span data-stu-id="4077f-665">System.Int16?</span></span>  
  
-   <span data-ttu-id="4077f-666">System.Int32?</span><span class="sxs-lookup"><span data-stu-id="4077f-666">System.Int32?</span></span>  
  
-   <span data-ttu-id="4077f-667">System.Int64?</span><span class="sxs-lookup"><span data-stu-id="4077f-667">System.Int64?</span></span>  
  
-   <span data-ttu-id="4077f-668">System.Decimal?</span><span class="sxs-lookup"><span data-stu-id="4077f-668">System.Decimal?</span></span>  
  
-   <span data-ttu-id="4077f-669">System.Single?</span><span class="sxs-lookup"><span data-stu-id="4077f-669">System.Single?</span></span>  
  
-   <span data-ttu-id="4077f-670">System.Double?</span><span class="sxs-lookup"><span data-stu-id="4077f-670">System.Double?</span></span>  
  
-   <span data-ttu-id="4077f-671">System.Guid?</span><span class="sxs-lookup"><span data-stu-id="4077f-671">System.Guid?</span></span>  
  
-   <span data-ttu-id="4077f-672">System.String?</span><span class="sxs-lookup"><span data-stu-id="4077f-672">System.String?</span></span>  
  
-   <span data-ttu-id="4077f-673">System.Char?</span><span class="sxs-lookup"><span data-stu-id="4077f-673">System.Char?</span></span>  
  
-   <span data-ttu-id="4077f-674">System.DateTime?</span><span class="sxs-lookup"><span data-stu-id="4077f-674">System.DateTime?</span></span>  

##  <span data-ttu-id="4077f-675"><a name="Trace"></a>Spårning</span><span class="sxs-lookup"><span data-stu-id="4077f-675"><a name="Trace"></a> Trace</span></span>  
 <span data-ttu-id="4077f-676">Den `trace` princip lägger till en sträng i den [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) utdata.</span><span class="sxs-lookup"><span data-stu-id="4077f-676">The             `trace` policy adds a string into the [API Inspector](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) output.</span></span> <span data-ttu-id="4077f-677">Principen körs endast när spårning utlöses, d.v.s. `Ocp-Apim-Trace` huvud är närvarande och ange att `true` och `Ocp-Apim-Subscription-Key` begärandehuvudet finns och innehåller en giltig nyckel som är associerade med administratörskontot.</span><span class="sxs-lookup"><span data-stu-id="4077f-677">The policy will execute only when tracing is triggered, i.e. `Ocp-Apim-Trace` request header is present and set to `true` and `Ocp-Apim-Subscription-Key` request header is present and holds a valid key associated with the admin account.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-678">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-678">Policy statement</span></span>  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a><span data-ttu-id="4077f-679">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-679">Elements</span></span>  
  
|<span data-ttu-id="4077f-680">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-680">Element</span></span>|<span data-ttu-id="4077f-681">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-681">Description</span></span>|<span data-ttu-id="4077f-682">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-682">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-683">Spårning</span><span class="sxs-lookup"><span data-stu-id="4077f-683">trace</span></span>|<span data-ttu-id="4077f-684">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-684">Root element.</span></span>|<span data-ttu-id="4077f-685">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-685">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-686">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-686">Attributes</span></span>  
  
|<span data-ttu-id="4077f-687">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-687">Attribute</span></span>|<span data-ttu-id="4077f-688">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-688">Description</span></span>|<span data-ttu-id="4077f-689">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-689">Required</span></span>|<span data-ttu-id="4077f-690">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-690">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-691">Källa</span><span class="sxs-lookup"><span data-stu-id="4077f-691">source</span></span>|<span data-ttu-id="4077f-692">Stränglitteral meningsfulla för visningsprogram och ange källan för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="4077f-692">String literal meaningful to the trace viewer and specifying the source of the message.</span></span>|<span data-ttu-id="4077f-693">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-693">Yes</span></span>|<span data-ttu-id="4077f-694">Saknas</span><span class="sxs-lookup"><span data-stu-id="4077f-694">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-695">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-695">Usage</span></span>  
 <span data-ttu-id="4077f-696">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span><span class="sxs-lookup"><span data-stu-id="4077f-696">This policy can be used in the following policy                    [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and                   [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .</span></span>  
  
-   <span data-ttu-id="4077f-697">**Avsnitt i princip:** inkommande, utgående backend fel</span><span class="sxs-lookup"><span data-stu-id="4077f-697">**Policy sections:** inbound, outbound, backend, on-error</span></span>  
  
-   <span data-ttu-id="4077f-698">**Princip för scope:** alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-698">**Policy scopes:** all scopes</span></span>  
  
##  <span data-ttu-id="4077f-699"><a name="Wait"></a>Vänta</span><span class="sxs-lookup"><span data-stu-id="4077f-699"><a name="Wait"></a> Wait</span></span>  
 <span data-ttu-id="4077f-700">Den `wait` principen Kör sina direkt underordnade principer parallellt och väntar på att alla eller en av dess omedelbara underordnade principer ska slutföras innan den slutförs.</span><span class="sxs-lookup"><span data-stu-id="4077f-700">The `wait` policy executes its immediate child policies in parallel, and waits for either all or one of its immediate child policies to complete before it completes.</span></span> <span data-ttu-id="4077f-701">Vänta principen kan ha sina direkt underordnade principer [begäran om att skicka](api-management-advanced-policies.md#SendRequest), [hämta värdet från cache](api-management-caching-policies.md#GetFromCacheByKey), och [Åtkomstkontrollflödet](api-management-advanced-policies.md#choose) principer.</span><span class="sxs-lookup"><span data-stu-id="4077f-701">The wait policy can have as its immediate child policies [Send request](api-management-advanced-policies.md#SendRequest), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey), and [Control flow](api-management-advanced-policies.md#choose) policies.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="4077f-702">Principframställning</span><span class="sxs-lookup"><span data-stu-id="4077f-702">Policy statement</span></span>  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a><span data-ttu-id="4077f-703">Exempel</span><span class="sxs-lookup"><span data-stu-id="4077f-703">Example</span></span>  
 <span data-ttu-id="4077f-704">I följande exempel finns två `choose` principer som direkt underordnade principer för den `wait` princip.</span><span class="sxs-lookup"><span data-stu-id="4077f-704">In the following example there are two `choose` policies as immediate child policies of the `wait` policy.</span></span> <span data-ttu-id="4077f-705">Var och en av dessa `choose` principer körs parallellt.</span><span class="sxs-lookup"><span data-stu-id="4077f-705">Each of these `choose` policies executes in parallel.</span></span> <span data-ttu-id="4077f-706">Varje `choose` princip försöker hämta ett cachelagrade värde.</span><span class="sxs-lookup"><span data-stu-id="4077f-706">Each `choose` policy attempts to retrieve a cached value.</span></span> <span data-ttu-id="4077f-707">Om det finns en cache-miss, kallas en backend-tjänst för att ange värdet.</span><span class="sxs-lookup"><span data-stu-id="4077f-707">If there is a cache miss, a backend service is called to provide the value.</span></span> <span data-ttu-id="4077f-708">I det här exemplet i `wait` principen inte slutföras förrän alla dess omedelbara underordnade principer slutföras eftersom den `for` -attributet är inställt på `all`.</span><span class="sxs-lookup"><span data-stu-id="4077f-708">In this example the `wait` policy does not complete until all of its immediate child policies complete, because the `for` attribute is set to `all`.</span></span>   <span data-ttu-id="4077f-709">I det här exemplet variablerna kontext (`execute-branch-one`, `value-one`, `execute-branch-two`, och `value-two`) deklareras utanför omfånget för den här principen exempel.</span><span class="sxs-lookup"><span data-stu-id="4077f-709">In this example the context variables (`execute-branch-one`, `value-one`, `execute-branch-two`, and `value-two`) are declared outside of the scope of this example policy.</span></span>  
  
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
  
### <a name="elements"></a><span data-ttu-id="4077f-710">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-710">Elements</span></span>  
  
|<span data-ttu-id="4077f-711">Element</span><span class="sxs-lookup"><span data-stu-id="4077f-711">Element</span></span>|<span data-ttu-id="4077f-712">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-712">Description</span></span>|<span data-ttu-id="4077f-713">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-713">Required</span></span>|  
|-------------|-----------------|--------------|  
|<span data-ttu-id="4077f-714">Vänta</span><span class="sxs-lookup"><span data-stu-id="4077f-714">wait</span></span>|<span data-ttu-id="4077f-715">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="4077f-715">Root element.</span></span> <span data-ttu-id="4077f-716">Kan innehålla som underordnade element endast `send-request`, `cache-lookup-value`, och `choose` principer.</span><span class="sxs-lookup"><span data-stu-id="4077f-716">May contain as child elements only `send-request`, `cache-lookup-value`, and `choose` policies.</span></span>|<span data-ttu-id="4077f-717">Ja</span><span class="sxs-lookup"><span data-stu-id="4077f-717">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="4077f-718">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-718">Attributes</span></span>  
  
|<span data-ttu-id="4077f-719">Attribut</span><span class="sxs-lookup"><span data-stu-id="4077f-719">Attribute</span></span>|<span data-ttu-id="4077f-720">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4077f-720">Description</span></span>|<span data-ttu-id="4077f-721">Krävs</span><span class="sxs-lookup"><span data-stu-id="4077f-721">Required</span></span>|<span data-ttu-id="4077f-722">Standard</span><span class="sxs-lookup"><span data-stu-id="4077f-722">Default</span></span>|  
|---------------|-----------------|--------------|-------------|  
|<span data-ttu-id="4077f-723">för</span><span class="sxs-lookup"><span data-stu-id="4077f-723">for</span></span>|<span data-ttu-id="4077f-724">Anger om den `wait` principen väntar på att alla direkt underordnade principer ska slutföras eller bara en.</span><span class="sxs-lookup"><span data-stu-id="4077f-724">Determines whether the `wait` policy waits for all immediate child policies to be completed or just one.</span></span> <span data-ttu-id="4077f-725">Tillåtna värden är:</span><span class="sxs-lookup"><span data-stu-id="4077f-725">Allowed values are:</span></span><br /><br /> <span data-ttu-id="4077f-726">-   `all`-Vänta tills alla direkt underordnade principer ska slutföras</span><span class="sxs-lookup"><span data-stu-id="4077f-726">-   `all` - wait for all immediate child policies to complete</span></span><br /><span data-ttu-id="4077f-727">-alla - vänta tills alla direkt underordnade principen att slutföra.</span><span class="sxs-lookup"><span data-stu-id="4077f-727">-   any - wait for any immediate child policy to complete.</span></span> <span data-ttu-id="4077f-728">När den första omedelbara underordnade principen är klar, den `wait` principen har slutförts och körningen av alla andra principer för omedelbart underordnade avslutas.</span><span class="sxs-lookup"><span data-stu-id="4077f-728">Once the first immediate child policy has completed, the `wait` policy completes and execution of any other immediate child policies is terminated.</span></span>|<span data-ttu-id="4077f-729">Nej</span><span class="sxs-lookup"><span data-stu-id="4077f-729">No</span></span>|<span data-ttu-id="4077f-730">Alla</span><span class="sxs-lookup"><span data-stu-id="4077f-730">all</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="4077f-731">Användning</span><span class="sxs-lookup"><span data-stu-id="4077f-731">Usage</span></span>  
 <span data-ttu-id="4077f-732">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="4077f-732">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="4077f-733">**Avsnitt i princip:** inkommande, utgående backend</span><span class="sxs-lookup"><span data-stu-id="4077f-733">**Policy sections:** inbound, outbound, backend</span></span>  
  
-   <span data-ttu-id="4077f-734">**Princip för scope:**alla scope</span><span class="sxs-lookup"><span data-stu-id="4077f-734">**Policy scopes:**all scopes</span></span>  
  
## <a name="next-steps"></a><span data-ttu-id="4077f-735">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4077f-735">Next steps</span></span>
<span data-ttu-id="4077f-736">Arbeta med principer, Läs mer:</span><span class="sxs-lookup"><span data-stu-id="4077f-736">For more information working with policies, see:</span></span>
-   [<span data-ttu-id="4077f-737">Principer för i API-hantering</span><span class="sxs-lookup"><span data-stu-id="4077f-737">Policies in API Management</span></span>](api-management-howto-policies.md) 
-   [<span data-ttu-id="4077f-738">Principuttryck</span><span class="sxs-lookup"><span data-stu-id="4077f-738">Policy expressions</span></span>](api-management-policy-expressions.md)
