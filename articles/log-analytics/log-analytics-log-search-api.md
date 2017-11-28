---
title: "aaaLog Analytics loggen Sök REST API | Microsoft Docs"
description: "Den här guiden innehåller en grundläggande genomgång som beskriver hur du kan använda hello logganalys söka REST API i hello Operations Management Suite (OMS) och det exempel som visar hur toouse hello kommandon."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: dafe5eeb8cc11a339f2cbf78cec657e344d87cac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-log-search-rest-api"></a><span data-ttu-id="0483b-103">Logganalys logga Sök REST API</span><span class="sxs-lookup"><span data-stu-id="0483b-103">Log Analytics log search REST API</span></span>
<span data-ttu-id="0483b-104">Den här guiden innehåller en grundläggande självstudier, inklusive exempel på hur du kan använda hello Log Analytics Sök REST API.</span><span class="sxs-lookup"><span data-stu-id="0483b-104">This guide provides a basic tutorial, including examples, of how you can use hello Log Analytics Search REST API.</span></span> <span data-ttu-id="0483b-105">Log Analytics är en del av hello Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="0483b-105">Log Analytics is part of hello Operations Management Suite (OMS).</span></span>

> [!NOTE]
> <span data-ttu-id="0483b-106">Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och du bör fortsätta toouse hello äldre frågespråket hello loggen Sök API som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0483b-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should continue toouse hello legacy query language with hello log search API as described in this article.</span></span>  <span data-ttu-id="0483b-107">Ett nytt API släpps för uppgraderade arbetsytor och hello dokumentationen uppdateras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="0483b-107">A new API will be released for upgraded workspaces, and hello documentation will be updated at that time.</span></span> 

> [!NOTE]
> <span data-ttu-id="0483b-108">Logganalys kallades tidigare åtgärdsinformation, som det är därför hello-namnet som används i hello resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="0483b-108">Log Analytics was previously called Operational Insights, which is why it is hello name used in hello resource provider.</span></span>
>
>

## <a name="overview-of-hello-log-search-rest-api"></a><span data-ttu-id="0483b-109">Översikt över hello loggen Sök REST API</span><span class="sxs-lookup"><span data-stu-id="0483b-109">Overview of hello Log Search REST API</span></span>
<span data-ttu-id="0483b-110">hello Log Analytics Sök REST API är RESTful och kan nås via hello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="0483b-110">hello Log Analytics Search REST API is RESTful and can be accessed via hello Azure Resource Manager API.</span></span> <span data-ttu-id="0483b-111">Den här artikeln innehåller exempel på att komma åt hello API [ARMClient](https://github.com/projectkudu/ARMClient), ett kommandoradsverktyg för öppen källkod som förenklar anropar hello Azure Resource Manager API.</span><span class="sxs-lookup"><span data-stu-id="0483b-111">This article provides examples of accessing hello API through [ARMClient](https://github.com/projectkudu/ARMClient), an open source command-line tool that simplifies invoking hello Azure Resource Manager API.</span></span> <span data-ttu-id="0483b-112">hello användning av ARMClient är en av många alternativ tooaccess hello Log Analytics Sök-API.</span><span class="sxs-lookup"><span data-stu-id="0483b-112">hello use of ARMClient is one of many options tooaccess hello Log Analytics Search API.</span></span> <span data-ttu-id="0483b-113">Ett annat alternativ är toouse hello Azure PowerShell-modulen för OperationalInsights som innehåller cmdlets för att komma åt sökningen.</span><span class="sxs-lookup"><span data-stu-id="0483b-113">Another option is toouse hello Azure PowerShell module for OperationalInsights, which includes cmdlets for accessing search.</span></span> <span data-ttu-id="0483b-114">Du kan använda hello Azure Resource Manager API toomake anrop tooOMS arbetsytor och utföra sökkommandon i dem med dessa verktyg.</span><span class="sxs-lookup"><span data-stu-id="0483b-114">With these tools, you can utilize hello Azure Resource Manager API toomake calls tooOMS workspaces and perform search commands within them.</span></span> <span data-ttu-id="0483b-115">hello API matar ut sökresultat i JSON-format, vilket gör att du toouse hello sökresultat på många olika sätt programmässigt.</span><span class="sxs-lookup"><span data-stu-id="0483b-115">hello API outputs search results in JSON format, allowing you toouse hello search results in many different ways programmatically.</span></span>

<span data-ttu-id="0483b-116">hello Azure Resource Manager kan användas en [-biblioteket för .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) och hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span><span class="sxs-lookup"><span data-stu-id="0483b-116">hello Azure Resource Manager can be used via a [Library for .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) and hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx).</span></span> <span data-ttu-id="0483b-117">toolearn granska fler hello länkade webbsidor.</span><span class="sxs-lookup"><span data-stu-id="0483b-117">toolearn more, review hello linked web pages.</span></span>

> [!NOTE]
> <span data-ttu-id="0483b-118">Om du använder en aggregering som `|measure count()` eller `distinct`, varje anrop toosearch kan returnera upp till 500 000 poster.</span><span class="sxs-lookup"><span data-stu-id="0483b-118">If you use an aggregation command such as `|measure count()` or `distinct`, each call toosearch can return upto 500,000 records.</span></span> <span data-ttu-id="0483b-119">Sökningar som inte inkluderar en aggregering kommandot returnerar upp till 5 000 poster.</span><span class="sxs-lookup"><span data-stu-id="0483b-119">Searches that do not include an aggregation command return upto 5,000 records.</span></span>
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a><span data-ttu-id="0483b-120">Grundläggande självstudierna för Log Analytics Sök REST API</span><span class="sxs-lookup"><span data-stu-id="0483b-120">Basic Log Analytics Search REST API tutorial</span></span>
### <a name="toouse-armclient"></a><span data-ttu-id="0483b-121">toouse ARMClient</span><span class="sxs-lookup"><span data-stu-id="0483b-121">toouse ARMClient</span></span>
1. <span data-ttu-id="0483b-122">Installera [Chocolatey](https://chocolatey.org/), vilket är en öppen källa Package Manager för Windows.</span><span class="sxs-lookup"><span data-stu-id="0483b-122">Install [Chocolatey](https://chocolatey.org/), which is an Open Source Package Manager for Windows.</span></span> <span data-ttu-id="0483b-123">Öppna ett kommandotolksfönster som administratör och kör sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0483b-123">Open a command prompt window as administrator and then run hello following command:</span></span>

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. <span data-ttu-id="0483b-124">Installera ARMClient genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="0483b-124">Install ARMClient by running hello following command:</span></span>

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a><span data-ttu-id="0483b-125">tooperform en sökning med ARMClient</span><span class="sxs-lookup"><span data-stu-id="0483b-125">tooperform a search using ARMClient</span></span>
1. <span data-ttu-id="0483b-126">Logga in med ditt Microsoft-konto eller ditt arbets- eller skolkonto konto:</span><span class="sxs-lookup"><span data-stu-id="0483b-126">Log in using your Microsoft account or your work or school account:</span></span>

    ```
    armclient login
    ```

    <span data-ttu-id="0483b-127">En lyckad inloggning visas alla prenumerationer som är knutna toohello angivna konto:</span><span class="sxs-lookup"><span data-stu-id="0483b-127">A successful login lists all subscriptions tied toohello given account:</span></span>

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. <span data-ttu-id="0483b-128">Hämta hello Operations Management Suite arbetsytor:</span><span class="sxs-lookup"><span data-stu-id="0483b-128">Get hello Operations Management Suite workspaces:</span></span>

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    <span data-ttu-id="0483b-129">En lyckad Get-anrop skulle utdata alla arbetsytor knutna toohello prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="0483b-129">A successful Get call would output all workspaces tied toohello subscription:</span></span>

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. <span data-ttu-id="0483b-130">Skapa Sök-variabeln:</span><span class="sxs-lookup"><span data-stu-id="0483b-130">Create your search variable:</span></span>

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. <span data-ttu-id="0483b-131">Sökningen med din nya Sök-variabeln:</span><span class="sxs-lookup"><span data-stu-id="0483b-131">Search using your new search variable:</span></span>

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a><span data-ttu-id="0483b-132">Logga Analytics Sök REST API-referens-exempel</span><span class="sxs-lookup"><span data-stu-id="0483b-132">Log Analytics Search REST API reference examples</span></span>
<span data-ttu-id="0483b-133">hello som följande exempel visar hur du kan använda hello Sök-API.</span><span class="sxs-lookup"><span data-stu-id="0483b-133">hello following examples show you how you can use hello Search API.</span></span>

### <a name="search---actionread"></a><span data-ttu-id="0483b-134">Sök - åtgärd/läsning</span><span class="sxs-lookup"><span data-stu-id="0483b-134">Search - Action/Read</span></span>
<span data-ttu-id="0483b-135">**Exempel-Url:**</span><span class="sxs-lookup"><span data-stu-id="0483b-135">**Sample Url:**</span></span>

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

<span data-ttu-id="0483b-136">**Begäran:**</span><span class="sxs-lookup"><span data-stu-id="0483b-136">**Request:**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
<span data-ttu-id="0483b-137">hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0483b-137">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="0483b-138">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="0483b-138">**Property**</span></span> | <span data-ttu-id="0483b-139">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0483b-139">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="0483b-140">Upp</span><span class="sxs-lookup"><span data-stu-id="0483b-140">top</span></span> |<span data-ttu-id="0483b-141">hello maximalt antal resultat tooreturn.</span><span class="sxs-lookup"><span data-stu-id="0483b-141">hello maximum number of results tooreturn.</span></span> |
| <span data-ttu-id="0483b-142">Markera</span><span class="sxs-lookup"><span data-stu-id="0483b-142">highlight</span></span> |<span data-ttu-id="0483b-143">Innehåller före och efter parametrar, används vanligtvis för syntaxmarkering matchande fält</span><span class="sxs-lookup"><span data-stu-id="0483b-143">Contains pre and post parameters, used usually for highlighting matching fields</span></span> |
| <span data-ttu-id="0483b-144">före</span><span class="sxs-lookup"><span data-stu-id="0483b-144">pre</span></span> |<span data-ttu-id="0483b-145">Prefix hello anges tooyour matchade strängfält.</span><span class="sxs-lookup"><span data-stu-id="0483b-145">Prefixes hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="0483b-146">post</span><span class="sxs-lookup"><span data-stu-id="0483b-146">post</span></span> |<span data-ttu-id="0483b-147">Lägger till hello anges tooyour matchade strängfält.</span><span class="sxs-lookup"><span data-stu-id="0483b-147">Appends hello given string tooyour matched fields.</span></span> |
| <span data-ttu-id="0483b-148">DocumentDB</span><span class="sxs-lookup"><span data-stu-id="0483b-148">query</span></span> |<span data-ttu-id="0483b-149">hello sökfråga används toocollect och returnera resultat.</span><span class="sxs-lookup"><span data-stu-id="0483b-149">hello search query used toocollect and return results.</span></span> |
| <span data-ttu-id="0483b-150">start</span><span class="sxs-lookup"><span data-stu-id="0483b-150">start</span></span> |<span data-ttu-id="0483b-151">hello början av hello tidsfönster som du vill att resultaten toobe hitta från.</span><span class="sxs-lookup"><span data-stu-id="0483b-151">hello beginning of hello time window you want results toobe found from.</span></span> |
| <span data-ttu-id="0483b-152">End</span><span class="sxs-lookup"><span data-stu-id="0483b-152">end</span></span> |<span data-ttu-id="0483b-153">hello slutet av hello tidsfönster som du vill resulterar toobe hitta från.</span><span class="sxs-lookup"><span data-stu-id="0483b-153">hello end of hello time window you want results toobe found from.</span></span> |

<span data-ttu-id="0483b-154">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="0483b-154">**Response:**</span></span>

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a><span data-ttu-id="0483b-155">Söka / {ID} - åtgärd/läsning</span><span class="sxs-lookup"><span data-stu-id="0483b-155">Search/{ID} - Action/Read</span></span>
<span data-ttu-id="0483b-156">**Begär hello innehållet i en sparad sökning:**</span><span class="sxs-lookup"><span data-stu-id="0483b-156">**Request hello contents of a Saved Search:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> <span data-ttu-id="0483b-157">Om hello sökningen med ”väntande” status, göras avsökningen hello uppdaterade resultat via den här API: T.</span><span class="sxs-lookup"><span data-stu-id="0483b-157">If hello search returns with a ‘Pending’ status, then polling hello updated results can be done through this API.</span></span> <span data-ttu-id="0483b-158">Efter 6 min hello resultatet av hello sökningen kommer att tas bort från cachen hello och HTTP-rest ska returneras.</span><span class="sxs-lookup"><span data-stu-id="0483b-158">After 6 min, hello result of hello search will be dropped from hello cache and HTTP Gone will be returned.</span></span> <span data-ttu-id="0483b-159">Om hello inledande sökbegäran returnerar status ”lyckades” omedelbart, läggs inte toohello cache orsaka det här API-tooreturn http-rest om frågas hello resultat.</span><span class="sxs-lookup"><span data-stu-id="0483b-159">If hello initial search request returns a ‘Successful’ status immediately, hello results are not added toohello cache causing this API tooreturn HTTP Gone if queried.</span></span> <span data-ttu-id="0483b-160">hello innehållet i ett HTTP-200 resultat finns i hello samma format som hello inledande sökbegäran bara med de uppdaterade värdena.</span><span class="sxs-lookup"><span data-stu-id="0483b-160">hello contents of an HTTP 200 result are in hello same format as hello initial search request just with updated values.</span></span>
>
>

### <a name="saved-searches"></a><span data-ttu-id="0483b-161">Sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="0483b-161">Saved searches</span></span>
<span data-ttu-id="0483b-162">**Listan över sparade sökningar för begäran:**</span><span class="sxs-lookup"><span data-stu-id="0483b-162">**Request List of Saved Searches:**</span></span>

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

<span data-ttu-id="0483b-163">Metoder som stöds: GET PLACERA ta bort</span><span class="sxs-lookup"><span data-stu-id="0483b-163">Supported methods: GET PUT DELETE</span></span>

<span data-ttu-id="0483b-164">Samlingen metoder som stöds: hämta</span><span class="sxs-lookup"><span data-stu-id="0483b-164">Supported collection methods: GET</span></span>

<span data-ttu-id="0483b-165">hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0483b-165">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="0483b-166">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0483b-166">Property</span></span> | <span data-ttu-id="0483b-167">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0483b-167">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0483b-168">Id</span><span class="sxs-lookup"><span data-stu-id="0483b-168">Id</span></span> |<span data-ttu-id="0483b-169">hello Unik identifierare.</span><span class="sxs-lookup"><span data-stu-id="0483b-169">hello unique identifier.</span></span> |
| <span data-ttu-id="0483b-170">ETag</span><span class="sxs-lookup"><span data-stu-id="0483b-170">Etag</span></span> |<span data-ttu-id="0483b-171">**Krävs för korrigeringsfilen**.</span><span class="sxs-lookup"><span data-stu-id="0483b-171">**Required for Patch**.</span></span> <span data-ttu-id="0483b-172">Uppdateras av servern vid varje skrivning.</span><span class="sxs-lookup"><span data-stu-id="0483b-172">Updated by server on each write.</span></span> <span data-ttu-id="0483b-173">Värdet måste vara lika toohello aktuella lagrade värdet eller ' *' tooupdate.</span><span class="sxs-lookup"><span data-stu-id="0483b-173">Value must be equal toohello current stored value or ‘*’ tooupdate.</span></span> <span data-ttu-id="0483b-174">409 returneras för gamla/ogiltiga värden.</span><span class="sxs-lookup"><span data-stu-id="0483b-174">409 returned for old/invalid values.</span></span> |
| <span data-ttu-id="0483b-175">Properties.Query</span><span class="sxs-lookup"><span data-stu-id="0483b-175">properties.query</span></span> |<span data-ttu-id="0483b-176">**Krävs**.</span><span class="sxs-lookup"><span data-stu-id="0483b-176">**Required**.</span></span> <span data-ttu-id="0483b-177">hello sökfråga.</span><span class="sxs-lookup"><span data-stu-id="0483b-177">hello search query.</span></span> |
| <span data-ttu-id="0483b-178">properties.displayName</span><span class="sxs-lookup"><span data-stu-id="0483b-178">properties.displayName</span></span> |<span data-ttu-id="0483b-179">**Krävs**.</span><span class="sxs-lookup"><span data-stu-id="0483b-179">**Required**.</span></span> <span data-ttu-id="0483b-180">hello användardefinierade visningsnamn hello frågan.</span><span class="sxs-lookup"><span data-stu-id="0483b-180">hello user-defined display name of hello query.</span></span> |
| <span data-ttu-id="0483b-181">Properties.category</span><span class="sxs-lookup"><span data-stu-id="0483b-181">properties.category</span></span> |<span data-ttu-id="0483b-182">**Krävs**.</span><span class="sxs-lookup"><span data-stu-id="0483b-182">**Required**.</span></span> <span data-ttu-id="0483b-183">hello användardefinierade kategori hello frågan.</span><span class="sxs-lookup"><span data-stu-id="0483b-183">hello user-defined category of hello query.</span></span> |

> [!NOTE]
> <span data-ttu-id="0483b-184">hello logganalys Sök API returnerar för närvarande användarskapade sparade sökningar när avsökas för sparade sökningar i en arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0483b-184">hello Log Analytics search API currently returns user-created saved searches when polled for saved searches in a workspace.</span></span> <span data-ttu-id="0483b-185">hello API returnerar inte sparade sökningar som tillhandahålls av lösningar.</span><span class="sxs-lookup"><span data-stu-id="0483b-185">hello API does not return saved searches provided by solutions.</span></span>
>
>

### <a name="create-saved-searches"></a><span data-ttu-id="0483b-186">Skapa sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="0483b-186">Create saved searches</span></span>
<span data-ttu-id="0483b-187">**Begäran:**</span><span class="sxs-lookup"><span data-stu-id="0483b-187">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> <span data-ttu-id="0483b-188">hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.</span><span class="sxs-lookup"><span data-stu-id="0483b-188">hello name for all saved searches, schedules, and actions created with hello Log Analytics API must be in lowercase.</span></span>

### <a name="delete-saved-searches"></a><span data-ttu-id="0483b-189">Ta bort sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="0483b-189">Delete saved searches</span></span>
<span data-ttu-id="0483b-190">**Begäran:**</span><span class="sxs-lookup"><span data-stu-id="0483b-190">**Request:**</span></span>

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a><span data-ttu-id="0483b-191">Uppdatera sparade sökningar</span><span class="sxs-lookup"><span data-stu-id="0483b-191">Update saved searches</span></span>
 <span data-ttu-id="0483b-192">**Begäran:**</span><span class="sxs-lookup"><span data-stu-id="0483b-192">**Request:**</span></span>

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a><span data-ttu-id="0483b-193">Metadata - JSON endast</span><span class="sxs-lookup"><span data-stu-id="0483b-193">Metadata - JSON only</span></span>
<span data-ttu-id="0483b-194">Här är ett sätt toosee hello-fält för alla typer av loggen för hello data som samlas in i din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="0483b-194">Here’s a way toosee hello fields for all log types for hello data collected in your workspace.</span></span> <span data-ttu-id="0483b-195">Om du vill att du vet om hello händelsetyp har ett fält med namnet på datorn, är den här frågan enkelriktade toocheck.</span><span class="sxs-lookup"><span data-stu-id="0483b-195">For example, if you want you know if hello Event type has a field named Computer, then this query is one way toocheck.</span></span>

<span data-ttu-id="0483b-196">**Begäran för fält:**</span><span class="sxs-lookup"><span data-stu-id="0483b-196">**Request for Fields:**</span></span>

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

<span data-ttu-id="0483b-197">**Svar:**</span><span class="sxs-lookup"><span data-stu-id="0483b-197">**Response:**</span></span>

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

<span data-ttu-id="0483b-198">hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0483b-198">hello following table describes hello properties that are available.</span></span>

| <span data-ttu-id="0483b-199">**Egenskap**</span><span class="sxs-lookup"><span data-stu-id="0483b-199">**Property**</span></span> | <span data-ttu-id="0483b-200">**Beskrivning**</span><span class="sxs-lookup"><span data-stu-id="0483b-200">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="0483b-201">namn</span><span class="sxs-lookup"><span data-stu-id="0483b-201">name</span></span> |<span data-ttu-id="0483b-202">Fältnamn.</span><span class="sxs-lookup"><span data-stu-id="0483b-202">Field name.</span></span> |
| <span data-ttu-id="0483b-203">Visningsnamn</span><span class="sxs-lookup"><span data-stu-id="0483b-203">displayName</span></span> |<span data-ttu-id="0483b-204">hello visningsnamn för hello-fältet.</span><span class="sxs-lookup"><span data-stu-id="0483b-204">hello display name of hello field.</span></span> |
| <span data-ttu-id="0483b-205">typ</span><span class="sxs-lookup"><span data-stu-id="0483b-205">type</span></span> |<span data-ttu-id="0483b-206">hello typ av hello fältvärdet.</span><span class="sxs-lookup"><span data-stu-id="0483b-206">hello Type of hello field value.</span></span> |
| <span data-ttu-id="0483b-207">facetable</span><span class="sxs-lookup"><span data-stu-id="0483b-207">facetable</span></span> |<span data-ttu-id="0483b-208">Kombinationen av aktuella 'indexerad', ' lagras ' och 'aspekten' egenskaper.</span><span class="sxs-lookup"><span data-stu-id="0483b-208">Combination of current ‘indexed’, ‘stored ‘and ‘facet’ properties.</span></span> |
| <span data-ttu-id="0483b-209">Visa</span><span class="sxs-lookup"><span data-stu-id="0483b-209">display</span></span> |<span data-ttu-id="0483b-210">Aktuella 'Visa-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0483b-210">Current ‘display’ property.</span></span> <span data-ttu-id="0483b-211">TRUE om fältet är synligt i sökningen.</span><span class="sxs-lookup"><span data-stu-id="0483b-211">True if field is visible in search.</span></span> |
| <span data-ttu-id="0483b-212">ownerType</span><span class="sxs-lookup"><span data-stu-id="0483b-212">ownerType</span></span> |<span data-ttu-id="0483b-213">Minskade tooonly typer som hör tooonboarded IP.</span><span class="sxs-lookup"><span data-stu-id="0483b-213">Reduced tooonly Types belonging tooonboarded IP’s.</span></span> |

## <a name="optional-parameters"></a><span data-ttu-id="0483b-214">Extraparametrar</span><span class="sxs-lookup"><span data-stu-id="0483b-214">Optional parameters</span></span>
<span data-ttu-id="0483b-215">hello följande information beskriver valfria parametrar finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0483b-215">hello following information describes optional parameters available.</span></span>

### <a name="highlighting"></a><span data-ttu-id="0483b-216">Markering</span><span class="sxs-lookup"><span data-stu-id="0483b-216">Highlighting</span></span>
<span data-ttu-id="0483b-217">Hej ”Highlight” parametern är en valfri parameter som du kan använda toorequest hello Sök undersystemet innehåller en uppsättning markörer i sitt svar.</span><span class="sxs-lookup"><span data-stu-id="0483b-217">hello “Highlight” parameter is an optional parameter you may use toorequest hello search subsystem include a set of markers in its response.</span></span>

<span data-ttu-id="0483b-218">Markörerna ange hello start och sluta markerad text som matchar hello villkoren som angetts i sökfrågan.</span><span class="sxs-lookup"><span data-stu-id="0483b-218">These markers indicate hello start and end highlighted text that matches hello terms provided in your search query.</span></span>
<span data-ttu-id="0483b-219">Du kan ange hello start och avslutar markörer som används av sökterm toowrap hello markerat.</span><span class="sxs-lookup"><span data-stu-id="0483b-219">You may specify hello start and end markers that are used by search toowrap hello highlighted term.</span></span>

<span data-ttu-id="0483b-220">**Exempel sökfråga**</span><span class="sxs-lookup"><span data-stu-id="0483b-220">**Example search query**</span></span>

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

<span data-ttu-id="0483b-221">**Exempel resultat:**</span><span class="sxs-lookup"><span data-stu-id="0483b-221">**Sample result:**</span></span>

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

<span data-ttu-id="0483b-222">Observera att hello föregående resultat innehåller ett felmeddelande som prefix och läggas till.</span><span class="sxs-lookup"><span data-stu-id="0483b-222">Notice that hello preceding result contains an error message that has been prefixed and appended.</span></span>

## <a name="computer-groups"></a><span data-ttu-id="0483b-223">Datorgrupper</span><span class="sxs-lookup"><span data-stu-id="0483b-223">Computer groups</span></span>
<span data-ttu-id="0483b-224">Datorgrupper är särskilda sparade sökningar som returnerar en uppsättning datorer.</span><span class="sxs-lookup"><span data-stu-id="0483b-224">Computer groups are special saved searches that return a set of computers.</span></span>  <span data-ttu-id="0483b-225">Du kan använda en datorgrupp i andra frågor toolimit hello resultat toohello datorer i hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="0483b-225">You can use a computer group in other queries toolimit hello results toohello computers in hello group.</span></span>  <span data-ttu-id="0483b-226">En datorgrupp implementeras som en sparad sökning med en grupp-tagg med ett värde på datorn.</span><span class="sxs-lookup"><span data-stu-id="0483b-226">A computer group is implemented as a saved search with a Group tag with a value of Computer.</span></span>

<span data-ttu-id="0483b-227">Följande är ett exempelsvar för en datorgrupp.</span><span class="sxs-lookup"><span data-stu-id="0483b-227">Following is a sample response for a computer group.</span></span>

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a><span data-ttu-id="0483b-228">Hämtar datorgrupper</span><span class="sxs-lookup"><span data-stu-id="0483b-228">Retrieving computer groups</span></span>
<span data-ttu-id="0483b-229">tooretrieve en datorgrupp, Använd hello Get-metoden med hello grupp-ID.</span><span class="sxs-lookup"><span data-stu-id="0483b-229">tooretrieve a computer group, use hello Get method with hello group ID.</span></span>

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a><span data-ttu-id="0483b-230">Skapa eller uppdatera en datorgrupp</span><span class="sxs-lookup"><span data-stu-id="0483b-230">Creating or updating a computer group</span></span>
<span data-ttu-id="0483b-231">toocreate en datorgrupp använda hello Put-metoden med en unik sparad sökning-ID.</span><span class="sxs-lookup"><span data-stu-id="0483b-231">toocreate a computer group, use hello Put method with a unique saved search ID.</span></span> <span data-ttu-id="0483b-232">Om du använder en befintlig dator-ID för gruppen har att en ändrats.</span><span class="sxs-lookup"><span data-stu-id="0483b-232">If you use an existing computer group ID, then that one is modified.</span></span> <span data-ttu-id="0483b-233">När du skapar en datorgrupp i hello logganalys-portalen skapas hello-ID från hello grupp och namn.</span><span class="sxs-lookup"><span data-stu-id="0483b-233">When you create a computer group in hello Log Analytics portal, then hello ID is created from hello group and name.</span></span>

<span data-ttu-id="0483b-234">hello-fråga som används för hello Gruppdefinition måste returnera en uppsättning datorer för hello grupp toofunction korrekt.</span><span class="sxs-lookup"><span data-stu-id="0483b-234">hello query used for hello group definition must return a set of computers for hello group toofunction properly.</span></span>  <span data-ttu-id="0483b-235">Vi rekommenderar att du avslutar frågan med `| Distinct Computer` tooensure hello rätt data returneras.</span><span class="sxs-lookup"><span data-stu-id="0483b-235">It's recommended that you end your query with `| Distinct Computer` tooensure hello correct data is returned.</span></span>

<span data-ttu-id="0483b-236">hello definition av hello sparad sökning måste innehålla en tagg med namnet grupp med ett värde på datorn för hello Sök toobe klassificeras som en datorgrupp.</span><span class="sxs-lookup"><span data-stu-id="0483b-236">hello definition of hello saved search must include a tag named Group with a value of Computer for hello search toobe classified as a computer group.</span></span>

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a><span data-ttu-id="0483b-237">Ta bort datorgrupper</span><span class="sxs-lookup"><span data-stu-id="0483b-237">Deleting computer groups</span></span>
<span data-ttu-id="0483b-238">toodelete en datorgrupp, Använd hello Delete-metoden med hello grupp-ID.</span><span class="sxs-lookup"><span data-stu-id="0483b-238">toodelete a computer group, use hello Delete method with hello group ID.</span></span>

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a><span data-ttu-id="0483b-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0483b-239">Next steps</span></span>
* <span data-ttu-id="0483b-240">Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toobuild frågor med anpassade fält för villkoret.</span><span class="sxs-lookup"><span data-stu-id="0483b-240">Learn about [log searches](log-analytics-log-searches.md) toobuild queries using custom fields for criteria.</span></span>
