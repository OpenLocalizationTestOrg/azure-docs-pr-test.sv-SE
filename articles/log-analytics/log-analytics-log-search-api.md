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
# <a name="log-analytics-log-search-rest-api"></a>Logganalys logga Sök REST API
Den här guiden innehåller en grundläggande självstudier, inklusive exempel på hur du kan använda hello Log Analytics Sök REST API. Log Analytics är en del av hello Operations Management Suite (OMS).

> [!NOTE]
> Om ditt arbetsområde har uppgraderade toohello [nya Log Analytics-frågespråket](log-analytics-log-search-upgrade.md), och du bör fortsätta toouse hello äldre frågespråket hello loggen Sök API som beskrivs i den här artikeln.  Ett nytt API släpps för uppgraderade arbetsytor och hello dokumentationen uppdateras samtidigt. 

> [!NOTE]
> Logganalys kallades tidigare åtgärdsinformation, som det är därför hello-namnet som används i hello resursprovidern.
>
>

## <a name="overview-of-hello-log-search-rest-api"></a>Översikt över hello loggen Sök REST API
hello Log Analytics Sök REST API är RESTful och kan nås via hello Azure Resource Manager API. Den här artikeln innehåller exempel på att komma åt hello API [ARMClient](https://github.com/projectkudu/ARMClient), ett kommandoradsverktyg för öppen källkod som förenklar anropar hello Azure Resource Manager API. hello användning av ARMClient är en av många alternativ tooaccess hello Log Analytics Sök-API. Ett annat alternativ är toouse hello Azure PowerShell-modulen för OperationalInsights som innehåller cmdlets för att komma åt sökningen. Du kan använda hello Azure Resource Manager API toomake anrop tooOMS arbetsytor och utföra sökkommandon i dem med dessa verktyg. hello API matar ut sökresultat i JSON-format, vilket gör att du toouse hello sökresultat på många olika sätt programmässigt.

hello Azure Resource Manager kan användas en [-biblioteket för .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) och hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). toolearn granska fler hello länkade webbsidor.

> [!NOTE]
> Om du använder en aggregering som `|measure count()` eller `distinct`, varje anrop toosearch kan returnera upp till 500 000 poster. Sökningar som inte inkluderar en aggregering kommandot returnerar upp till 5 000 poster.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Grundläggande självstudierna för Log Analytics Sök REST API
### <a name="toouse-armclient"></a>toouse ARMClient
1. Installera [Chocolatey](https://chocolatey.org/), vilket är en öppen källa Package Manager för Windows. Öppna ett kommandotolksfönster som administratör och kör sedan följande kommando hello:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Installera ARMClient genom att köra följande kommando hello:

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a>tooperform en sökning med ARMClient
1. Logga in med ditt Microsoft-konto eller ditt arbets- eller skolkonto konto:

    ```
    armclient login
    ```

    En lyckad inloggning visas alla prenumerationer som är knutna toohello angivna konto:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Hämta hello Operations Management Suite arbetsytor:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    En lyckad Get-anrop skulle utdata alla arbetsytor knutna toohello prenumerationen:

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
3. Skapa Sök-variabeln:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Sökningen med din nya Sök-variabeln:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Logga Analytics Sök REST API-referens-exempel
hello som följande exempel visar hur du kan använda hello Sök-API.

### <a name="search---actionread"></a>Sök - åtgärd/läsning
**Exempel-Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Begäran:**

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
hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.

| **Egenskap** | **Beskrivning** |
| --- | --- |
| Upp |hello maximalt antal resultat tooreturn. |
| Markera |Innehåller före och efter parametrar, används vanligtvis för syntaxmarkering matchande fält |
| före |Prefix hello anges tooyour matchade strängfält. |
| post |Lägger till hello anges tooyour matchade strängfält. |
| DocumentDB |hello sökfråga används toocollect och returnera resultat. |
| start |hello början av hello tidsfönster som du vill att resultaten toobe hitta från. |
| End |hello slutet av hello tidsfönster som du vill resulterar toobe hitta från. |

**Svar:**

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

### <a name="searchid---actionread"></a>Söka / {ID} - åtgärd/läsning
**Begär hello innehållet i en sparad sökning:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Om hello sökningen med ”väntande” status, göras avsökningen hello uppdaterade resultat via den här API: T. Efter 6 min hello resultatet av hello sökningen kommer att tas bort från cachen hello och HTTP-rest ska returneras. Om hello inledande sökbegäran returnerar status ”lyckades” omedelbart, läggs inte toohello cache orsaka det här API-tooreturn http-rest om frågas hello resultat. hello innehållet i ett HTTP-200 resultat finns i hello samma format som hello inledande sökbegäran bara med de uppdaterade värdena.
>
>

### <a name="saved-searches"></a>Sparade sökningar
**Listan över sparade sökningar för begäran:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Metoder som stöds: GET PLACERA ta bort

Samlingen metoder som stöds: hämta

hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.

| Egenskap | Beskrivning |
| --- | --- |
| Id |hello Unik identifierare. |
| ETag |**Krävs för korrigeringsfilen**. Uppdateras av servern vid varje skrivning. Värdet måste vara lika toohello aktuella lagrade värdet eller ' *' tooupdate. 409 returneras för gamla/ogiltiga värden. |
| Properties.Query |**Krävs**. hello sökfråga. |
| properties.displayName |**Krävs**. hello användardefinierade visningsnamn hello frågan. |
| Properties.category |**Krävs**. hello användardefinierade kategori hello frågan. |

> [!NOTE]
> hello logganalys Sök API returnerar för närvarande användarskapade sparade sökningar när avsökas för sparade sökningar i en arbetsyta. hello API returnerar inte sparade sökningar som tillhandahålls av lösningar.
>
>

### <a name="create-saved-searches"></a>Skapa sparade sökningar
**Begäran:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> hello namn för alla sparade sökningar, scheman och åtgärder som skapats med hello Log Analytics API måste vara i gemener.

### <a name="delete-saved-searches"></a>Ta bort sparade sökningar
**Begäran:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Uppdatera sparade sökningar
 **Begäran:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadata - JSON endast
Här är ett sätt toosee hello-fält för alla typer av loggen för hello data som samlas in i din arbetsyta. Om du vill att du vet om hello händelsetyp har ett fält med namnet på datorn, är den här frågan enkelriktade toocheck.

**Begäran för fält:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Svar:**

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

hello i den följande tabellen beskrivs hello egenskaper som är tillgängliga.

| **Egenskap** | **Beskrivning** |
| --- | --- |
| namn |Fältnamn. |
| Visningsnamn |hello visningsnamn för hello-fältet. |
| typ |hello typ av hello fältvärdet. |
| facetable |Kombinationen av aktuella 'indexerad', ' lagras ' och 'aspekten' egenskaper. |
| Visa |Aktuella 'Visa-egenskapen. TRUE om fältet är synligt i sökningen. |
| ownerType |Minskade tooonly typer som hör tooonboarded IP. |

## <a name="optional-parameters"></a>Extraparametrar
hello följande information beskriver valfria parametrar finns tillgängliga.

### <a name="highlighting"></a>Markering
Hej ”Highlight” parametern är en valfri parameter som du kan använda toorequest hello Sök undersystemet innehåller en uppsättning markörer i sitt svar.

Markörerna ange hello start och sluta markerad text som matchar hello villkoren som angetts i sökfrågan.
Du kan ange hello start och avslutar markörer som används av sökterm toowrap hello markerat.

**Exempel sökfråga**

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

**Exempel resultat:**

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

Observera att hello föregående resultat innehåller ett felmeddelande som prefix och läggas till.

## <a name="computer-groups"></a>Datorgrupper
Datorgrupper är särskilda sparade sökningar som returnerar en uppsättning datorer.  Du kan använda en datorgrupp i andra frågor toolimit hello resultat toohello datorer i hello-gruppen.  En datorgrupp implementeras som en sparad sökning med en grupp-tagg med ett värde på datorn.

Följande är ett exempelsvar för en datorgrupp.

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

### <a name="retrieving-computer-groups"></a>Hämtar datorgrupper
tooretrieve en datorgrupp, Använd hello Get-metoden med hello grupp-ID.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Skapa eller uppdatera en datorgrupp
toocreate en datorgrupp använda hello Put-metoden med en unik sparad sökning-ID. Om du använder en befintlig dator-ID för gruppen har att en ändrats. När du skapar en datorgrupp i hello logganalys-portalen skapas hello-ID från hello grupp och namn.

hello-fråga som används för hello Gruppdefinition måste returnera en uppsättning datorer för hello grupp toofunction korrekt.  Vi rekommenderar att du avslutar frågan med `| Distinct Computer` tooensure hello rätt data returneras.

hello definition av hello sparad sökning måste innehålla en tagg med namnet grupp med ett värde på datorn för hello Sök toobe klassificeras som en datorgrupp.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Ta bort datorgrupper
toodelete en datorgrupp, Använd hello Delete-metoden med hello grupp-ID.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [logga sökningar](log-analytics-log-searches.md) toobuild frågor med anpassade fält för villkoret.
