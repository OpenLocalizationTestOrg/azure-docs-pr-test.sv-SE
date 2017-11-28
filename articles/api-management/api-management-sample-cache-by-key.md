---
title: Anpassade cachelagring i Azure API Management
description: "Lär dig att cachelagra objekt som nyckel i Azure API Management"
services: api-management
documentationcenter: 
author: darrelmiller
manager: erikre
editor: 
ms.assetid: 772bc8dd-5cda-41c4-95bf-b9f6f052bc85
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: f5d5f44e34fbcd122a10be0ca5b3001760c4c64d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="c9597-103">Anpassade cachelagring i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="c9597-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="c9597-104">Azure API Management-tjänsten har inbyggt stöd för [HTTP-svar cachelagring](api-management-howto-cache.md) med resurs-URL som nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9597-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using the resource URL as the key.</span></span> <span data-ttu-id="c9597-105">Nyckeln kan ändras av huvuden för begäran med hjälp av den `vary-by` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="c9597-105">The key can be modified by request headers using the `vary-by` properties.</span></span> <span data-ttu-id="c9597-106">Detta är användbart för cachelagring av hela HTTP-svar (aka garantier), men ibland är det praktiskt att cache bara en del av en representation.</span><span class="sxs-lookup"><span data-stu-id="c9597-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful to just cache a portion of a representation.</span></span> <span data-ttu-id="c9597-107">Den nya [cache-sökning-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) och [cache-lagra-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) principer ger möjlighet att lagra och hämta godtyckliga delar av data från i principdefinitioner.</span><span class="sxs-lookup"><span data-stu-id="c9597-107">The new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide the ability to store and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="c9597-108">Den här möjligheten också tillför värde till den tidigare introducerades [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) principen eftersom du kan nu cachelagra svar från externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="c9597-108">This ability also adds value to the previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="c9597-109">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="c9597-109">Architecture</span></span>
<span data-ttu-id="c9597-110">API Management-tjänsten använder en delad per klient cache så att när du skalar upp till flera enheter som du kommer fortfarande att få åtkomst till samma cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="c9597-110">API Management service uses a shared per-tenant data cache so that, as you scale up to multiple units you will still get access to the same cached data.</span></span> <span data-ttu-id="c9597-111">Men när du arbetar med en distribution i flera regioner finns oberoende cacheminnen inom vart och ett av regionerna.</span><span class="sxs-lookup"><span data-stu-id="c9597-111">However, when working with a multi-region deployment there are independent caches within each of the regions.</span></span> <span data-ttu-id="c9597-112">På grund av detta är det viktigt att inte behandla cacheminnet som ett datalager, där det är den enda källan för vissa information.</span><span class="sxs-lookup"><span data-stu-id="c9597-112">Due to this, it is important to not treat the cache as a data store, where it is the only source of some piece of information.</span></span> <span data-ttu-id="c9597-113">Om du har och senare har valt att dra nytta av flera regioner distributionen, kan sedan kunder med användare som reser förlora åtkomsten till den cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="c9597-113">If you did, and later decided to take advantage of the multi-region deployment, then customers with users that travel may lose access to that cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="c9597-114">Cachelagring av fragment</span><span class="sxs-lookup"><span data-stu-id="c9597-114">Fragment caching</span></span>
<span data-ttu-id="c9597-115">Det finns vissa fall där svar som returneras innehåller en del av data som är dyrt att avgöra och ännu är den senaste för en rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="c9597-115">There are certain cases where responses being returned contain some portion of data that is expensive to determine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="c9597-116">Exempelvis bör du en tjänst som skapats av ett flygbolag som innehåller information som rör svarta reservationer, svarta status osv. Om användaren är medlem i flygbolagen punkter programmet, skulle de också ha information om deras aktuella status och hittills utfört ackumulerade.</span><span class="sxs-lookup"><span data-stu-id="c9597-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If the user is a member of the airlines points program, they would also have information relating to their current status and mileage accumulated.</span></span> <span data-ttu-id="c9597-117">Den här användaren-relaterad information lagras i ett annat system, men kan det vara önskvärt att inkludera den i svar om svarta status och -reservationer som returneras.</span><span class="sxs-lookup"><span data-stu-id="c9597-117">This user-related information might be stored in a different system, but it may be desirable to include it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="c9597-118">Detta kan göras med hjälp av en process som kallas cachelagring av fragment.</span><span class="sxs-lookup"><span data-stu-id="c9597-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="c9597-119">Den primära representationen kan returneras från den ursprungliga servern med hjälp av någon typ av token som indikerar om användarrelaterade-information som ska infogas.</span><span class="sxs-lookup"><span data-stu-id="c9597-119">The primary representation can be returned from the origin server using some kind of token to indicate where the user-related information is to be inserted.</span></span> 

<span data-ttu-id="c9597-120">Överväg följande JSON-svar från en serverdel API.</span><span class="sxs-lookup"><span data-stu-id="c9597-120">Consider the following JSON response from a backend API.</span></span>

```json
{
  "airline" : "Air Canada",
  "flightno" : "871",
  "status" : "ontime",
  "gate" : "B40",
  "terminal" : "2A",
  "userprofile" : "$userprofile$"
}  
```

<span data-ttu-id="c9597-121">Och sekundära resursen på `/userprofile/{userid}` ser ut som att,</span><span class="sxs-lookup"><span data-stu-id="c9597-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="c9597-122">Vi behöver identifiera som användaren för att avgöra lämpliga användarinformationen att inkludera.</span><span class="sxs-lookup"><span data-stu-id="c9597-122">In order to determine the appropriate user information to include, we need to identify who the end user is.</span></span> <span data-ttu-id="c9597-123">Den här mekanismen är implementering beroende.</span><span class="sxs-lookup"><span data-stu-id="c9597-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="c9597-124">Jag använder exempelvis den `Subject` anspråk för en `JWT` token.</span><span class="sxs-lookup"><span data-stu-id="c9597-124">As an example, I am using the `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="c9597-125">Vi sparar du den `enduserid` värde i en kontext variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="c9597-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="c9597-126">Nästa steg är att avgöra om en tidigare begäran redan har hämtats användarinformationen och lagras i cachen.</span><span class="sxs-lookup"><span data-stu-id="c9597-126">The next step is to determine if a previous request has already retrieved the user information and stored it in the cache.</span></span> <span data-ttu-id="c9597-127">För den här vi använder den `cache-lookup-value` princip.</span><span class="sxs-lookup"><span data-stu-id="c9597-127">For this we use the `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="c9597-128">Om det finns ingen post i cacheminnet som motsvarar värdet för nyckeln och sedan Nej `userprofile` kontexten variabeln kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="c9597-128">If there is no entry in the cache that corresponds to the key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="c9597-129">Vi kontrollerar att en sökning med hjälp av den `choose` styr flödet för principen.</span><span class="sxs-lookup"><span data-stu-id="c9597-129">We check the success of the lookup using the `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="c9597-130">Om den `userprofile` kontexten variabel finns inte och vi kommer att göra en HTTP-begäran för att hämta den.</span><span class="sxs-lookup"><span data-stu-id="c9597-130">If the `userprofile` context variable doesn’t exist, then we are going to have to make an HTTP request to retrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points to the profile for the current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="c9597-131">Vi använder den `enduserid` att konstruera profil användarresurs URL-adress.</span><span class="sxs-lookup"><span data-stu-id="c9597-131">We use the `enduserid` to construct the URL to the user profile resource.</span></span> <span data-ttu-id="c9597-132">När vi har svaret vi hämtar brödtext utanför svaret och lagra den tillbaka till en variabel i kontexten.</span><span class="sxs-lookup"><span data-stu-id="c9597-132">Once we have the response, we can pull the body text out of the response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="c9597-133">För att undvika oss att göra denna HTTP-begäran igen, när samma användare gör en annan begäran, lagrar vi användarens profil i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="c9597-133">To avoid us having to make this HTTP request again, when the same user makes another request, we can store the user profile in the cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="c9597-134">Värdet lagras i cachen med exakt samma nyckel som vi ursprungligen gjordes ett försök att hämta den med.</span><span class="sxs-lookup"><span data-stu-id="c9597-134">We store the value in the cache using the exact same key that we originally attempted to retrieve it with.</span></span> <span data-ttu-id="c9597-135">Den varaktighet som vi vill lagra värdet ska baseras på hur ofta uppdateras ändringar och hur feltoleranta användare till inaktuell information.</span><span class="sxs-lookup"><span data-stu-id="c9597-135">The duration that we choose to store the value should be based on how often the information changes and how tolerant users are to out of date information.</span></span> 

<span data-ttu-id="c9597-136">Det är viktigt att tänka på att hämtas från cachen är fortfarande ett out-of-process, nätverksbegäran och eventuellt fortfarande lägga till flera millisekunder begäran.</span><span class="sxs-lookup"><span data-stu-id="c9597-136">It is important to realize that retrieving from the cache is still an out-of-process, network request and potentially can still add tens of milliseconds to the request.</span></span> <span data-ttu-id="c9597-137">Fördelarna kommer när du fastställer användarens profilinformation tar betydligt längre tid än den på grund av behöva databas frågor eller samlar in information från flera-servrar.</span><span class="sxs-lookup"><span data-stu-id="c9597-137">The benefits come when determining the user profile information takes significantly longer than that due to needing to do database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="c9597-138">Det sista steget i processen är att uppdatera returnerade svar med vår användarens profilinformation.</span><span class="sxs-lookup"><span data-stu-id="c9597-138">The final step in the process is to update the returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="c9597-139">Jag har valt att inkludera citattecken som en del av token så att även om Ersätt inte inträffar svaret var fortfarande giltig JSON.</span><span class="sxs-lookup"><span data-stu-id="c9597-139">I chose to include the quotation marks as part of the token so that even when the replace doesn’t occur, the response was still valid JSON.</span></span> <span data-ttu-id="c9597-140">Detta var främst underlättar felsökning.</span><span class="sxs-lookup"><span data-stu-id="c9597-140">This was primarily to make debugging easier.</span></span>

<span data-ttu-id="c9597-141">När du kombinerar de här stegen tillsammans är slutresultatet en princip som ser ut som på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="c9597-141">Once you combine all these steps together, the end result is a policy that looks like the following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in the cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request to get user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points to the profile for the current end-user -->
                    <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>

                <!-- Store response body in context variable -->
                <set-variable
                  name="userprofile"
                  value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                <!-- Store result in cache -->
                <cache-store-value
                  key="@("userprofile-" + context.Variables["enduserid"])"
                  value="@((string)context.Variables["userprofile"])"
                  duration="100000" />
            </when>
        </choose>
        <base />
    </inbound>
    <outbound>
        <!-- Update response body with user profile-->
        <find-and-replace
              from='"$userprofile$"'
              to="@((string)context.Variables["userprofile"])" />
        <base />
    </outbound>
</policies>
```

<span data-ttu-id="c9597-142">Den här cachelagring metoden används främst på webbplatser där HTML består på serversidan så att den kan återges som en enda sida.</span><span class="sxs-lookup"><span data-stu-id="c9597-142">This caching approach is primarily used in web sites where HTML is composed on the server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="c9597-143">Men det kan också vara användbart i API: er där klienten kan göra sida HTTP cachelagring eller bör inte att placera det ansvaret på klienten.</span><span class="sxs-lookup"><span data-stu-id="c9597-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not to put that responsibility on the client.</span></span>

<span data-ttu-id="c9597-144">Den här samma typ av cachelagring av fragment kan även utföras på backend-webbservrar som använder en Redis cache server, men använder API Management-tjänsten för att utföra arbetet är användbart när cachelagrade fragment kommer från olika-servrar än primärt svar.</span><span class="sxs-lookup"><span data-stu-id="c9597-144">This same kind of fragment caching can also be done on the backend web servers using a Redis caching server, however, using the API Management service to perform this work is useful when the cached fragments are coming from different back-ends than the primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="c9597-145">Transparent versionshantering</span><span class="sxs-lookup"><span data-stu-id="c9597-145">Transparent versioning</span></span>
<span data-ttu-id="c9597-146">Det är vanligt för flera olika implementering versioner av en API som stöds vid någon tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="c9597-146">It is common practice for multiple different implementation versions of an API to be supported at any one time.</span></span> <span data-ttu-id="c9597-147">Detta kanske är att stödja olika miljöer, t.ex. dev, testa, produktion, eller det kan vara att stödja äldre versioner av API: et för ange tid för API-konsumenter att migrera till nyare versioner.</span><span class="sxs-lookup"><span data-stu-id="c9597-147">This is perhaps to support different environments, like dev, test, production, etc, or it may be to support older versions of the API to give time for API consumers to migrate to newer versions.</span></span> 

<span data-ttu-id="c9597-148">Ett sätt att hantera detta i stället för att klienten utvecklare kan ändra URL-adresser från `/v1/customers` till `/v2/customers` är att spara i konsumentens profildata vilken version av API: et de för närvarande vill använda och anropa lämpliga backend-URL.</span><span class="sxs-lookup"><span data-stu-id="c9597-148">One approach to handling this instead of requiring client developers to change the URLs from `/v1/customers` to `/v2/customers` is to store in the consumer’s profile data which version of the API they currently wish to use and call the appropriate backend URL.</span></span> <span data-ttu-id="c9597-149">Det är nödvändigt att fråga några konfigurationsdata för att fastställa rätt backend-URL att anropa för en viss klient.</span><span class="sxs-lookup"><span data-stu-id="c9597-149">In order to determine the correct backend URL to call for a particular client, it is necessary to query some configuration data.</span></span> <span data-ttu-id="c9597-150">Vi kan minimera prestandaförsämring med att göra den här sökningen cachelagrar konfigurationsinformationen.</span><span class="sxs-lookup"><span data-stu-id="c9597-150">By caching this configuration data, we can minimize the performance penalty of doing this lookup.</span></span>

<span data-ttu-id="c9597-151">Det första steget är att avgöra den identifierare som används för att konfigurera den önskade versionen.</span><span class="sxs-lookup"><span data-stu-id="c9597-151">The first step is to determine the identifier used to configure the desired version.</span></span> <span data-ttu-id="c9597-152">I det här exemplet jag har valt versionen till prenumerationen produktnyckeln.</span><span class="sxs-lookup"><span data-stu-id="c9597-152">In this example, I chose to associate the version to the product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="c9597-153">Sedan gör vi en cache-sökning för att se om det redan har hämtat den önskade klientversionen.</span><span class="sxs-lookup"><span data-stu-id="c9597-153">We then do a cache lookup to see if we already have retrieved the desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="c9597-154">Sedan kontrollerar vi du om det inte att hitta det i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="c9597-154">Then we check to see if we did not find it in the cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="c9597-155">Om vi inte har vi gå och hämta den.</span><span class="sxs-lookup"><span data-stu-id="c9597-155">If we didn’t then we go and retrieve it.</span></span>

```xml
<send-request
    mode="new"
    response-variable-name="clientconfiguresponse"
    timeout="10"
    ignore-error="true">
            <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
            <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="c9597-156">Extrahera brödtext för svar från svaret.</span><span class="sxs-lookup"><span data-stu-id="c9597-156">Extract the response body text from the response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="c9597-157">Lagra den tillbaka i cacheminnet för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="c9597-157">Store it back in the cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="c9597-158">Och slutligen uppdatera backend-URL: en för att välja versionen av tjänsten som önskas av klienten.</span><span class="sxs-lookup"><span data-stu-id="c9597-158">And finally update the back-end URL to select the version of the service desired by the client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="c9597-159">Principen helt är som följer.</span><span class="sxs-lookup"><span data-stu-id="c9597-159">The completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in the cache, make a request for it and store it -->
    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">
            <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
            </send-request>
            <!-- Store response body in context variable -->
            <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
            <!-- Store result in cache -->
            <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
        </when>
    </choose>
    <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
</inbound>
```

<span data-ttu-id="c9597-160">Om du aktiverar API konsumenter att transparent styra vilken backend-version som används av klienter utan att behöva uppdatera och distribuera klienter är en smidig lösning som åtgärdar problem med många API-versioner.</span><span class="sxs-lookup"><span data-stu-id="c9597-160">Enabling API consumers to transparently control which backend version is being accessed by clients without having to update and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="c9597-161">Klientisolering</span><span class="sxs-lookup"><span data-stu-id="c9597-161">Tenant Isolation</span></span>
<span data-ttu-id="c9597-162">I distributioner av större och flera innehavare skapa vissa företag separata grupper för klienter på olika distributioner av backend-maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c9597-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="c9597-163">Detta minimerar antalet kunder som påverkas av maskinvaruproblem på serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c9597-163">This minimizes the number of customers who are impacted by a hardware issue on the backend.</span></span> <span data-ttu-id="c9597-164">Det gör också nya programvaruversioner återställas i etapper.</span><span class="sxs-lookup"><span data-stu-id="c9597-164">It also enables new software versions to be rolled out in stages.</span></span> <span data-ttu-id="c9597-165">Vi är den här backend-arkitekturen transparent för API-konsumenter.</span><span class="sxs-lookup"><span data-stu-id="c9597-165">Ideally this backend architecture should be transparent to API consumers.</span></span> <span data-ttu-id="c9597-166">Detta kan ske på ett liknande sätt att transparent versionshantering eftersom den är baserad på samma teknik manipulera backend-URL med konfigurationstillstånd per API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c9597-166">This can be achieved in a similar way to transparent versioning because it is based on the same technique of manipulating the backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="c9597-167">I stället för att returnera en önskade versionen av API: et för varje prenumeration nyckel, returneras en identifierare som gäller en klient för gruppen tilldelade maskinvara.</span><span class="sxs-lookup"><span data-stu-id="c9597-167">Instead of returning a preferred version of the API for each subscription key, you would return an identifier that relates a tenant to the assigned hardware group.</span></span> <span data-ttu-id="c9597-168">Identifierare kan användas för att konstruera lämpliga backend-URL.</span><span class="sxs-lookup"><span data-stu-id="c9597-168">That identifier can be used to construct the appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="c9597-169">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c9597-169">Summary</span></span>
<span data-ttu-id="c9597-170">Friheten att använda Azure API management-cache för att lagra alla slags data möjliggör effektiv åtkomst till konfigurationsdata som kan påverka hur en inkommande begäran bearbetas.</span><span class="sxs-lookup"><span data-stu-id="c9597-170">The freedom to use the Azure API management cache for storing any kind of data enables efficient access to configuration data that can affect the way an inbound request is processed.</span></span> <span data-ttu-id="c9597-171">Det kan också användas för att lagra datafragment som kan utöka svar som returnerades från en serverdel API.</span><span class="sxs-lookup"><span data-stu-id="c9597-171">It can also be used to store data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9597-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9597-172">Next steps</span></span>
<span data-ttu-id="c9597-173">Ge oss din feedback i Disqus-tråden för det här avsnittet om det finns andra scenarier som dessa principer har aktiverats för dig, eller om det finns scenarier du vill uppnå men inte är för närvarande går.</span><span class="sxs-lookup"><span data-stu-id="c9597-173">Please give us your feedback in the Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like to achieve but do not feel are currently possible.</span></span>

