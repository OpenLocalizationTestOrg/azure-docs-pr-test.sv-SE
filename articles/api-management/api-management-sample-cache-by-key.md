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
# <a name="custom-caching-in-azure-api-management"></a>Anpassade cachelagring i Azure API Management
Azure API Management-tjänsten har inbyggt stöd för [HTTP-svar cachelagring](api-management-howto-cache.md) med hello resurs-URL som hello nyckel. hello nyckel kan ändras av huvuden för begäran med hjälp av hello `vary-by` egenskaper. Detta är användbart för cachelagring av hela HTTP-svar (aka garantier), men ibland är det användbart toojust cache en del av en representation. hello nya [cache-sökning-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) och [cache-lagra-value](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) principer ger hello möjlighet toostore och hämta godtyckliga delar av data från i principdefinitioner. Den här möjligheten lägger också till värdet toohello tidigare introduceras [-begäran om att skicka](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) principen eftersom du kan nu cachelagra svar från externa tjänster.

## <a name="architecture"></a>Arkitektur
API Management-tjänsten använder en delad per klient cache så att när du skalar toomultiple enheter kommer fortfarande att åtkomst toohello samma cachelagrade data. Men när du arbetar med en distribution i flera regioner finns oberoende cacheminnen inom varje hello regioner. På grund av toothis, är det viktigt toonot behandla hello cache som ett datalager, där det är hello enda källan för vissa information. Om du har och senare valt tootake nytta av hello flera regioner distribution, kan kunder med användare som reser förlora åtkomst toothat cachelagrade data.

## <a name="fragment-caching"></a>Cachelagring av fragment
Det finns vissa fall där svar som returneras innehåller en del av data som är dyr toodetermine och ännu är den senaste för en rimlig tid. Exempelvis bör du en tjänst som skapats av ett flygbolag som innehåller information som rör svarta reservationer, svarta status osv. Om hello användaren är medlem av hello flygbolagen punkter program, skulle de också ha uppgifter om aktuell status för tootheir och hittills utfört ackumulerade. Den här användaren-relaterad information lagras i ett annat system, men kan det vara önskvärt tooinclude i svar returnerades om svarta status och reservationer. Detta kan göras med hjälp av en process som kallas cachelagring av fragment. hello kan primära representation returneras från hello ursprungsservern med hjälp av någon typ av token tooindicate där hello användarrelaterad information är toobe infogas. 

Överväg att hello följande JSON-svar från en serverdel API.

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

Och sekundära resursen på `/userprofile/{userid}` ser ut som att,

```json
{ "username" : "Bob Smith", "Status" : "Gold" }
```

I ordning toodetermine hello lämpliga användaren information tooinclude måste vi tooidentify som hello slutanvändaren. Den här mekanismen är implementering beroende. Exempelvis jag använder hello `Subject` anspråk för en `JWT` token. 

```xml
<set-variable
  name="enduserid"
  value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />
```

Vi sparar du den `enduserid` värde i en kontext variabel för senare användning. hello nästa steg är toodetermine om en tidigare begäran har redan hämtats hello användarinformation och lagras i hello cache. Detta vi använder hello `cache-lookup-value` princip.

```xml
<cache-lookup-value
key="@("userprofile-" + context.Variables["enduserid"])"
variable-name="userprofile" />
```

Om det finns ingen post i hello cacheminnet som motsvarar toohello nyckelvärdet sedan Nej `userprofile` kontexten variabeln kommer att skapas. Vi kontrollerar hello lyckats hello sökning med hello `choose` styr flödet för principen.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("userprofile"))">
        <!-- If hello userprofile context variable doesn’t exist, make an HTTP request tooretrieve it.  -->
    </when>
</choose>
```

Om hello `userprofile` kontexten variabeln inte finns så kan vi gå toohave toomake en HTTP-begäran tooretrieve den.

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

Vi använder hello `enduserid` tooconstruct hello URL toohello profil användarresurs. När vi har hello svar, vi hämtar hello brödtext utanför hello svar och lagra den tillbaka till en variabel i kontexten.

```xml
<set-variable
    name="userprofile"
    value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />
```

tooavoid oss med toomake HTTP-begäran igen när hello samma användare gör en annan begäran vi kan lagra hello användarprofil i hello-cachen.

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />
```

Vi lagra hello värde i hello cacheminne med hjälp av hello exakt samma nyckel vi ursprungligen försökte tooretrieve med. hello ska varaktighet som vi väljer toostore hello värde baseras på hur ofta hello ändringar och hur feltoleranta användare är tooout av datuminformation. 

Det är viktigt toorealize att hämtas från cachen hello fortfarande är en out-of-process, nätverksbegäran och potentiellt kan du lägga till flera millisekunder toohello begäran. hello fördelar kommer när avgörande hello användarens profilinformation tar betydligt längre tid än den på grund av tooneeding toodo databasfrågor eller samlar in information från flera-servrar.

hello är sista steget i processen för hello tooupdate hello returnerade svar med vår användarens profilinformation.

```xml
<!-- Update response body with user profile-->
<find-and-replace
    from='"$userprofile$"'
    to="@((string)context.Variables["userprofile"])" />
```

Jag har valt tooinclude hello citattecken som en del av hello token så att även om hello Ersätt inte inträffar hello svar är fortfarande giltig JSON. Detta var främst toomake felsökning.

När du kombinerar de här stegen tillsammans är hello slutresultatet en princip som ser ut som hello efter.

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

Den här cachelagring metoden används främst på webbplatser där HTML består på serversidan hello så att den kan återges som en enda sida. Men det kan också vara användbart i API: er där klienten kan göra sida HTTP cachelagring eller bör inte tooput detta ansvar på hello-klienten.

Den här samma typ av cachelagring av fragment kan även utföras på hello backend webbservrar med hjälp av en Redis cache server, men hello API Management-tjänsten tooperform detta arbete är användbara när hello cachelagras fragment kommer från olika-servrar än hello primära svar.

## <a name="transparent-versioning"></a>Transparent versionshantering
Det är vanligt för flera olika implementering versioner av en API-toobe stöd åt gången. Detta är kanske toosupport olika miljöer, t.ex. dev, testa, produktion, eller kanske toosupport äldre versioner av hello API toogive tid för API konsumenter toomigrate toonewer versioner. 

En metod som toohandling detta i stället för att kräva att utvecklare klienten toochange hello URL från `/v1/customers` för`/v2/customers` är toostore i hello konsumenten profildata vilken version av hello API de för närvarande vill toouse och anropa hello lämpliga backend-URL. I ordning toodetermine hello rätt backend URL toocall för en viss klient, är det nödvändigt tooquery några konfigurationsdata. Vi kan minimera hello prestandaförsämring med att göra den här sökningen cachelagrar data för den här.

hello första steget är toodetermine hello identifierare används tooconfigure hello önskade versionen. I det här exemplet väljer jag tooassociate hello version toohello prenumeration produktnyckeln. 

```xml
<set-variable name="clientid" value="@(context.Subscription.Key)" />
```

Vi gör en cache-sökning toosee om redan har hämtats hello önskade klientversionen.

```xml
<cache-lookup-value
key="@("clientversion-" + context.Variables["clientid"])"
variable-name="clientversion" />
```

Vi kontrollera sedan toosee om vi inte hittade den i hello-cachen.

```xml
<choose>
    <when condition="@(!context.Variables.ContainsKey("clientversion"))">
```

Om vi inte har vi gå och hämta den.

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

Extrahera hello brödtext för svar från hello svar.

```xml
<set-variable
      name="clientversion"
      value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
```

Lagra den tillbaka i hello cache för framtida användning.

```xml
<cache-store-value
      key="@("clientversion-" + context.Variables["clientid"])"
      value="@((string)context.Variables["clientversion"])"
      duration="100000" />
```

Och slutligen uppdatera hello backend-URL: en tooselect hello versionen av hello önskad av hello-klienten.

```xml
<set-backend-service
      base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
```

hello är helt princip som följer.

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

Att aktivera API konsumenter tootransparently kontroll vilken backend-version som används av klienter utan att behöva tooupdate och distribuera om klienterna är en smidig lösning som åtgärdar problem med många API-versioner.

## <a name="tenant-isolation"></a>Klientisolering
I distributioner av större och flera innehavare skapa vissa företag separata grupper för klienter på olika distributioner av backend-maskinvara. Detta minimerar hello antal kunder som påverkas av maskinvaruproblem på hello serverdel. Ny programvara versioner toobe distribuerat i steg kan också. Vi är den här arkitekturen för serverdelen transparent tooAPI konsumenter. Detta kan ske på ett liknande sätt tootransparent versionshantering eftersom den är baserad på hello samma teknik manipulera hello backend-URL med konfigurationstillstånd per API-nyckel.  

I stället för att returnera en önskad version av hello API för varje prenumeration nyckel, returneras en identifierare som gäller en klient toohello tilldelade maskinvarugrupp. Identifierare kan vara används tooconstruct hello lämpliga backend-URL.

## <a name="summary"></a>Sammanfattning
hello frihet toouse hello Azure API management cache för att lagra alla typer av data gör att effektiv åtkomst tooconfiguration data som kan påverka hello sätt en inkommande begäran bearbetas. Det kan också vara används toostore datafragment som kan utöka svar som returnerades från en serverdel API.

## <a name="next-steps"></a>Nästa steg
Ge oss din feedback i hello Disqus-tråden för det här avsnittet om det finns andra scenarier att dessa principer har aktiverats för dig, eller om det finns scenarier du vill tooachieve men inte är är för närvarande är möjliga.

