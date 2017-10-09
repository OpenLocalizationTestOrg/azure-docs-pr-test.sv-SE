---
title: aaaCustom cachelagring i Azure API Management
description: "Lär dig hur toocache objekt som nyckel i Azure API Management"
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
ms.openlocfilehash: 681380743c8c96af5d0a8e25948a8c0663e9fd35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-caching-in-azure-api-management"></a><span data-ttu-id="b2055-103">Anpassade cachelagring i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="b2055-103">Custom caching in Azure API Management</span></span>
<span data-ttu-id="b2055-104">Azure API Management-tjänsten har inbyggt stöd för [HTTP-svar cachelagring](api-management-howto-cache.md) med hello resurs-URL som hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="b2055-104">Azure API Management service has built-in support for [HTTP response caching](api-management-howto-cache.md) using hello resource URL as hello key.</span></span> <span data-ttu-id="b2055-105">hello nyckel kan ändras av huvuden för begäran med hjälp av hello `vary-by` egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b2055-105">hello key can be modified by request headers using hello `vary-by` properties.</span></span> <span data-ttu-id="b2055-106">Detta är användbart för cachelagring av hela HTTP-svar (aka garantier), men ibland är det användbart toojust cache en del av en representation.</span><span class="sxs-lookup"><span data-stu-id="b2055-106">This is useful for caching entire HTTP responses (aka representations), but sometimes it is useful toojust cache a portion of a representation.</span></span> <span data-ttu-id="b2055-107">hello nya [cache-sökning-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) och [cache-lagra-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) principer ger hello möjlighet toostore och hämta godtyckliga delar av data från i principdefinitioner.</span><span class="sxs-lookup"><span data-stu-id="b2055-107">hello new [cache-lookup-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) and [cache-store-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) policies provide hello ability toostore and retrieve arbitrary pieces of data from within policy definitions.</span></span> <span data-ttu-id="b2055-108">Den här möjligheten lägger också till värdet toohello tidigare introduceras [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) principen eftersom du kan nu cachelagra svar från externa tjänster.</span><span class="sxs-lookup"><span data-stu-id="b2055-108">This ability also adds value toohello previously introduced [send-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) policy because you can now cache responses from external services.</span></span>

## <a name="architecture"></a><span data-ttu-id="b2055-109">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="b2055-109">Architecture</span></span>
<span data-ttu-id="b2055-110">API Management-tjänsten använder en delad per klient cache så att när du skalar toomultiple enheter kommer fortfarande att åtkomst toohello samma cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="b2055-110">API Management service uses a shared per-tenant data cache so that, as you scale up toomultiple units you will still get access toohello same cached data.</span></span> <span data-ttu-id="b2055-111">Men när du arbetar med en distribution i flera regioner finns oberoende cacheminnen inom varje hello regioner.</span><span class="sxs-lookup"><span data-stu-id="b2055-111">However, when working with a multi-region deployment there are independent caches within each of hello regions.</span></span> <span data-ttu-id="b2055-112">På grund av toothis, är det viktigt toonot behandla hello cache som ett datalager, där det är hello enda källan för vissa information.</span><span class="sxs-lookup"><span data-stu-id="b2055-112">Due toothis, it is important toonot treat hello cache as a data store, where it is hello only source of some piece of information.</span></span> <span data-ttu-id="b2055-113">Om du har och senare valt tootake nytta av hello flera regioner distribution, kan kunder med användare som reser förlora åtkomst toothat cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="b2055-113">If you did, and later decided tootake advantage of hello multi-region deployment, then customers with users that travel may lose access toothat cached data.</span></span>

## <a name="fragment-caching"></a><span data-ttu-id="b2055-114">Cachelagring av fragment</span><span class="sxs-lookup"><span data-stu-id="b2055-114">Fragment caching</span></span>
<span data-ttu-id="b2055-115">Det finns vissa fall där svar som returneras innehåller en del av data som är dyr toodetermine och ännu är den senaste för en rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="b2055-115">There are certain cases where responses being returned contain some portion of data that is expensive toodetermine and yet remains fresh for a reasonable amount of time.</span></span> <span data-ttu-id="b2055-116">Exempelvis bör du en tjänst som skapats av ett flygbolag som innehåller information som rör svarta reservationer, svarta status osv. Om hello användaren är medlem av hello flygbolagen punkter program, skulle de också ha uppgifter om aktuell status för tootheir och hittills utfört ackumulerade.</span><span class="sxs-lookup"><span data-stu-id="b2055-116">As an example, consider a service built by an airline that provides information relating flight reservations, flight status, etc. If hello user is a member of hello airlines points program, they would also have information relating tootheir current status and mileage accumulated.</span></span> <span data-ttu-id="b2055-117">Den här användaren-relaterad information lagras i ett annat system, men kan det vara önskvärt tooinclude i svar returnerades om svarta status och reservationer.</span><span class="sxs-lookup"><span data-stu-id="b2055-117">This user-related information might be stored in a different system, but it may be desirable tooinclude it in responses returned about flight status and reservations.</span></span> <span data-ttu-id="b2055-118">Detta kan göras med hjälp av en process som kallas cachelagring av fragment.</span><span class="sxs-lookup"><span data-stu-id="b2055-118">This can be done using a process called fragment caching.</span></span> <span data-ttu-id="b2055-119">hello kan primära representation returneras från hello ursprungsservern med hjälp av någon typ av token tooindicate där hello användarrelaterad information är toobe infogas.</span><span class="sxs-lookup"><span data-stu-id="b2055-119">hello primary representation can be returned from hello origin server using some kind of token tooindicate where hello user-related information is toobe inserted.</span></span> 

<span data-ttu-id="b2055-120">Överväg att hello följande JSON-svar från en serverdel API.</span><span class="sxs-lookup"><span data-stu-id="b2055-120">Consider hello following JSON response from a backend API.</span></span>

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

<span data-ttu-id="b2055-121">Och sekundära resursen på `/userprofile/{userid}` ser ut som att,</span><span class="sxs-lookup"><span data-stu-id="b2055-121">And secondary resource at `/userprofile/{userid}` that looks like,</span></span>

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

<span data-ttu-id="b2055-122">I ordning toodetermine hello lämpliga användaren information tooinclude måste vi tooidentify som hello slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="b2055-122">In order toodetermine hello appropriate user information tooinclude, we need tooidentify who hello end user is.</span></span> <span data-ttu-id="b2055-123">Den här mekanismen är implementering beroende.</span><span class="sxs-lookup"><span data-stu-id="b2055-123">This mechanism is implementation dependent.</span></span> <span data-ttu-id="b2055-124">Exempelvis jag använder hello `Subject` anspråk för en `JWT` token.</span><span class="sxs-lookup"><span data-stu-id="b2055-124">As an example, I am using hello `Subject` claim of a `JWT` token.</span></span> 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

<span data-ttu-id="b2055-125">Vi sparar du den `enduserid` värde i en kontext variabel för senare användning.</span><span class="sxs-lookup"><span data-stu-id="b2055-125">We store this `enduserid` value in a context variable for later use.</span></span> <span data-ttu-id="b2055-126">hello nästa steg är toodetermine om en tidigare begäran har redan hämtats hello användarinformation och lagras i hello cache.</span><span class="sxs-lookup"><span data-stu-id="b2055-126">hello next step is toodetermine if a previous request has already retrieved hello user information and stored it in hello cache.</span></span> <span data-ttu-id="b2055-127">Detta vi använder hello `cache-lookup-value` princip.</span><span class="sxs-lookup"><span data-stu-id="b2055-127">For this we use hello `cache-lookup-value` policy.</span></span>

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

<span data-ttu-id="b2055-128">Om det finns ingen post i hello cacheminnet som motsvarar toohello nyckelvärdet sedan Nej `userprofile` kontexten variabeln kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="b2055-128">If there is no entry in hello cache that corresponds toohello key value, then no `userprofile` context variable will be created.</span></span> <span data-ttu-id="b2055-129">Vi kontrollerar hello lyckats hello sökning med hello `choose` styr flödet för principen.</span><span class="sxs-lookup"><span data-stu-id="b2055-129">We check hello success of hello lookup using hello `choose` control flow policy.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

<span data-ttu-id="b2055-130">Om hello `userprofile` kontexten variabeln inte finns så kan vi gå toohave toomake en HTTP-begäran tooretrieve den.</span><span class="sxs-lookup"><span data-stu-id="b2055-130">If hello `userprofile` context variable doesn’t exist, then we are going toohave toomake an HTTP request tooretrieve it.</span></span>

```xml
<send-request
  mode="new"
  response-variable-name="userprofileresponse"
  timeout="10"
  ignore-error="true">

  <!-- Build a URL that points toohello profile for hello current end-user -->
  <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
      (string)context.Variables["enduserid"]).AbsoluteUri)
  </set-url>
  <set-method>GET</set-method>
</send-request>
```

<span data-ttu-id="b2055-131">Vi använder hello `enduserid` tooconstruct hello URL toohello profil användarresurs.</span><span class="sxs-lookup"><span data-stu-id="b2055-131">We use hello `enduserid` tooconstruct hello URL toohello user profile resource.</span></span> <span data-ttu-id="b2055-132">När vi har hello svar, vi hämtar hello brödtext utanför hello svar och lagra den tillbaka till en variabel i kontexten.</span><span class="sxs-lookup"><span data-stu-id="b2055-132">Once we have hello response, we can pull hello body text out of hello response and store it back into a context variable.</span></span>

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

<span data-ttu-id="b2055-133">tooavoid oss med toomake HTTP-begäran igen när hello samma användare gör en annan begäran vi kan lagra hello användarprofil i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="b2055-133">tooavoid us having toomake this HTTP request again, when hello same user makes another request, we can store hello user profile in hello cache.</span></span>

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

<span data-ttu-id="b2055-134">Vi lagra hello värde i hello cacheminne med hjälp av hello exakt samma nyckel vi ursprungligen försökte tooretrieve med.</span><span class="sxs-lookup"><span data-stu-id="b2055-134">We store hello value in hello cache using hello exact same key that we originally attempted tooretrieve it with.</span></span> <span data-ttu-id="b2055-135">hello ska varaktighet som vi väljer toostore hello värde baseras på hur ofta hello ändringar och hur feltoleranta användare är tooout av datuminformation.</span><span class="sxs-lookup"><span data-stu-id="b2055-135">hello duration that we choose toostore hello value should be based on how often hello information changes and how tolerant users are tooout of date information.</span></span> 

<span data-ttu-id="b2055-136">Det är viktigt toorealize att hämtas från cachen hello fortfarande är en out-of-process, nätverksbegäran och potentiellt kan du lägga till flera millisekunder toohello begäran.</span><span class="sxs-lookup"><span data-stu-id="b2055-136">It is important toorealize that retrieving from hello cache is still an out-of-process, network request and potentially can still add tens of milliseconds toohello request.</span></span> <span data-ttu-id="b2055-137">hello fördelar kommer när avgörande hello användarens profilinformation tar betydligt längre tid än den på grund av tooneeding toodo databasfrågor eller samlar in information från flera-servrar.</span><span class="sxs-lookup"><span data-stu-id="b2055-137">hello benefits come when determining hello user profile information takes significantly longer than that due tooneeding toodo database queries or aggregate information from multiple back-ends.</span></span>

<span data-ttu-id="b2055-138">hello är sista steget i processen för hello tooupdate hello returnerade svar med vår användarens profilinformation.</span><span class="sxs-lookup"><span data-stu-id="b2055-138">hello final step in hello process is tooupdate hello returned response with our user profile information.</span></span>

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

<span data-ttu-id="b2055-139">Jag har valt tooinclude hello citattecken som en del av hello token så att även om hello Ersätt inte inträffar hello svar är fortfarande giltig JSON.</span><span class="sxs-lookup"><span data-stu-id="b2055-139">I chose tooinclude hello quotation marks as part of hello token so that even when hello replace doesn’t occur, hello response was still valid JSON.</span></span> <span data-ttu-id="b2055-140">Detta var främst toomake felsökning.</span><span class="sxs-lookup"><span data-stu-id="b2055-140">This was primarily toomake debugging easier.</span></span>

<span data-ttu-id="b2055-141">När du kombinerar de här stegen tillsammans är hello slutresultatet en princip som ser ut som hello efter.</span><span class="sxs-lookup"><span data-stu-id="b2055-141">Once you combine all these steps together, hello end result is a policy that looks like hello following one.</span></span>

```xml
<policies>
    <inbound>
        <!-- How you determine user identity is application dependent -->
        <set-variable
          name="enduserid"
          value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

        <!--Look for userprofile for this user in hello cache -->
        <cache-lookup-value
          key="@("userprofile-" + context.Variables["enduserid"])"
          variable-name="userprofile" />

        <!-- If we don’t find it in hello cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                <!-- Make HTTP request tooget user profile -->
                <send-request
                  mode="new"
                  response-variable-name="userprofileresponse"
                  timeout="10"
                  ignore-error="true">

                   <!-- Build a URL that points toohello profile for hello current end-user -->
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

<span data-ttu-id="b2055-142">Den här cachelagring metoden används främst på webbplatser där HTML består på serversidan hello så att den kan återges som en enda sida.</span><span class="sxs-lookup"><span data-stu-id="b2055-142">This caching approach is primarily used in web sites where HTML is composed on hello server side so that it can be rendered as a single page.</span></span> <span data-ttu-id="b2055-143">Men det kan också vara användbart i API: er där klienten kan göra sida HTTP cachelagring eller bör inte tooput detta ansvar på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="b2055-143">However, it can also be useful in APIs where clients cannot do client side HTTP caching or it is desirable not tooput that responsibility on hello client.</span></span>

<span data-ttu-id="b2055-144">Den här samma typ av cachelagring av fragment kan även utföras på hello backend webbservrar med hjälp av en Redis cache server, men hello API Management-tjänsten tooperform detta arbete är användbara när hello cachelagras fragment kommer från olika-servrar än hello primära svar.</span><span class="sxs-lookup"><span data-stu-id="b2055-144">This same kind of fragment caching can also be done on hello backend web servers using a Redis caching server, however, using hello API Management service tooperform this work is useful when hello cached fragments are coming from different back-ends than hello primary responses.</span></span>

## <a name="transparent-versioning"></a><span data-ttu-id="b2055-145">Transparent versionshantering</span><span class="sxs-lookup"><span data-stu-id="b2055-145">Transparent versioning</span></span>
<span data-ttu-id="b2055-146">Det är vanligt för flera olika implementering versioner av en API-toobe stöd åt gången.</span><span class="sxs-lookup"><span data-stu-id="b2055-146">It is common practice for multiple different implementation versions of an API toobe supported at any one time.</span></span> <span data-ttu-id="b2055-147">Detta är kanske toosupport olika miljöer, t.ex. dev, testa, produktion, eller kanske toosupport äldre versioner av hello API toogive tid för API konsumenter toomigrate toonewer versioner.</span><span class="sxs-lookup"><span data-stu-id="b2055-147">This is perhaps toosupport different environments, like dev, test, production, etc, or it may be toosupport older versions of hello API toogive time for API consumers toomigrate toonewer versions.</span></span> 

<span data-ttu-id="b2055-148">En metod som toohandling detta i stället för att kräva att utvecklare klienten toochange hello URL från `/v1/customers` för`/v2/customers` är toostore i hello konsumenten profildata vilken version av hello API de för närvarande vill toouse och anropa hello lämpliga backend-URL.</span><span class="sxs-lookup"><span data-stu-id="b2055-148">One approach toohandling this instead of requiring client developers toochange hello URLs from `/v1/customers` too`/v2/customers` is toostore in hello consumer’s profile data which version of hello API they currently wish toouse and call hello appropriate backend URL.</span></span> <span data-ttu-id="b2055-149">I ordning toodetermine hello rätt backend URL toocall för en viss klient, är det nödvändigt tooquery några konfigurationsdata.</span><span class="sxs-lookup"><span data-stu-id="b2055-149">In order toodetermine hello correct backend URL toocall for a particular client, it is necessary tooquery some configuration data.</span></span> <span data-ttu-id="b2055-150">Vi kan minimera hello prestandaförsämring med att göra den här sökningen cachelagrar data för den här.</span><span class="sxs-lookup"><span data-stu-id="b2055-150">By caching this configuration data, we can minimize hello performance penalty of doing this lookup.</span></span>

<span data-ttu-id="b2055-151">hello första steget är toodetermine hello identifierare används tooconfigure hello önskade versionen.</span><span class="sxs-lookup"><span data-stu-id="b2055-151">hello first step is toodetermine hello identifier used tooconfigure hello desired version.</span></span> <span data-ttu-id="b2055-152">I det här exemplet väljer jag tooassociate hello version toohello prenumeration produktnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b2055-152">In this example, I chose tooassociate hello version toohello product subscription key.</span></span> 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

<span data-ttu-id="b2055-153">Vi gör en cache-sökning toosee om redan har hämtats hello önskade klientversionen.</span><span class="sxs-lookup"><span data-stu-id="b2055-153">We then do a cache lookup toosee if we already have retrieved hello desired client version.</span></span>

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

<span data-ttu-id="b2055-154">Vi kontrollera sedan toosee om vi inte hittade den i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="b2055-154">Then we check toosee if we did not find it in hello cache.</span></span>

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

<span data-ttu-id="b2055-155">Om vi inte har vi gå och hämta den.</span><span class="sxs-lookup"><span data-stu-id="b2055-155">If we didn’t then we go and retrieve it.</span></span>

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

<span data-ttu-id="b2055-156">Extrahera hello brödtext för svar från hello svar.</span><span class="sxs-lookup"><span data-stu-id="b2055-156">Extract hello response body text from hello response.</span></span>

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

<span data-ttu-id="b2055-157">Lagra den tillbaka i hello cache för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="b2055-157">Store it back in hello cache for future use.</span></span>

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

<span data-ttu-id="b2055-158">Och slutligen uppdatera hello backend-URL: en tooselect hello versionen av hello önskad av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="b2055-158">And finally update hello back-end URL tooselect hello version of hello service desired by hello client.</span></span>

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

<span data-ttu-id="b2055-159">hello är helt princip som följer.</span><span class="sxs-lookup"><span data-stu-id="b2055-159">hello completely policy is as follows.</span></span>

```xml
<inbound>
    <base />
    <set-variable name="clientid" value="@(context.Subscription.Key)" />
    <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

    <!-- If we don’t find it in hello cache, make a request for it and store it -->
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

<span data-ttu-id="b2055-160">Att aktivera API konsumenter tootransparently kontroll vilken backend-version som används av klienter utan att behöva tooupdate och distribuera om klienterna är en smidig lösning som åtgärdar problem med många API-versioner.</span><span class="sxs-lookup"><span data-stu-id="b2055-160">Enabling API consumers tootransparently control which backend version is being accessed by clients without having tooupdate and redeploy clients is a elegant solution that addresses many API versioning concerns.</span></span>

## <a name="tenant-isolation"></a><span data-ttu-id="b2055-161">Klientisolering</span><span class="sxs-lookup"><span data-stu-id="b2055-161">Tenant Isolation</span></span>
<span data-ttu-id="b2055-162">I distributioner av större och flera innehavare skapa vissa företag separata grupper för klienter på olika distributioner av backend-maskinvara.</span><span class="sxs-lookup"><span data-stu-id="b2055-162">In larger, multi-tenant deployments some companies create separate groups of tenants on distinct deployments of backend hardware.</span></span> <span data-ttu-id="b2055-163">Detta minimerar hello antal kunder som påverkas av maskinvaruproblem på hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="b2055-163">This minimizes hello number of customers who are impacted by a hardware issue on hello backend.</span></span> <span data-ttu-id="b2055-164">Ny programvara versioner toobe distribuerat i steg kan också.</span><span class="sxs-lookup"><span data-stu-id="b2055-164">It also enables new software versions toobe rolled out in stages.</span></span> <span data-ttu-id="b2055-165">Vi är den här arkitekturen för serverdelen transparent tooAPI konsumenter.</span><span class="sxs-lookup"><span data-stu-id="b2055-165">Ideally this backend architecture should be transparent tooAPI consumers.</span></span> <span data-ttu-id="b2055-166">Detta kan ske på ett liknande sätt tootransparent versionshantering eftersom den är baserad på hello samma teknik manipulera hello backend-URL med konfigurationstillstånd per API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="b2055-166">This can be achieved in a similar way tootransparent versioning because it is based on hello same technique of manipulating hello backend URL using configuration state per API key.</span></span>  

<span data-ttu-id="b2055-167">I stället för att returnera en önskad version av hello API för varje prenumeration nyckel, returneras en identifierare som gäller en klient toohello tilldelade maskinvarugrupp.</span><span class="sxs-lookup"><span data-stu-id="b2055-167">Instead of returning a preferred version of hello API for each subscription key, you would return an identifier that relates a tenant toohello assigned hardware group.</span></span> <span data-ttu-id="b2055-168">Identifierare kan vara används tooconstruct hello lämpliga backend-URL.</span><span class="sxs-lookup"><span data-stu-id="b2055-168">That identifier can be used tooconstruct hello appropriate backend URL.</span></span>

## <a name="summary"></a><span data-ttu-id="b2055-169">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b2055-169">Summary</span></span>
<span data-ttu-id="b2055-170">hello frihet toouse hello Azure API management cache för att lagra alla typer av data gör att effektiv åtkomst tooconfiguration data som kan påverka hello sätt en inkommande begäran bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b2055-170">hello freedom toouse hello Azure API management cache for storing any kind of data enables efficient access tooconfiguration data that can affect hello way an inbound request is processed.</span></span> <span data-ttu-id="b2055-171">Det kan också vara används toostore datafragment som kan utöka svar som returnerades från en serverdel API.</span><span class="sxs-lookup"><span data-stu-id="b2055-171">It can also be used toostore data fragments that can augment responses, returned from a backend API.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2055-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2055-172">Next steps</span></span>
<span data-ttu-id="b2055-173">Ge oss din feedback i hello Disqus-tråden för det här avsnittet om det finns andra scenarier att dessa principer har aktiverats för dig, eller om det finns scenarier du vill tooachieve men inte är är för närvarande är möjliga.</span><span class="sxs-lookup"><span data-stu-id="b2055-173">Please give us your feedback in hello Disqus thread for this topic if there are other scenarios that these policies have enabled for you, or if there are scenarios you would like tooachieve but do not feel are currently possible.</span></span>

