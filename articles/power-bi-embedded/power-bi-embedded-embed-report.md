---
title: aaaEmbed en rapport i Azure Power BI Embedded | Microsoft Docs
description: "Lär dig hur tooembed en rapport som är i Power BI Embedded till programmet."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: f25344bbd0b9c092ef19da04d0b455d453b426a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="embed-a-report-in-power-bi-embedded"></a>Bädda in en rapport i Power BI Embedded

Lär dig hur tooembed en rapport som är i Power BI Embedded till programmet.

Vi ska titta på hur tooactually bädda in en rapport i ditt program. Detta under förutsättning att du redan har en rapport som finns i en arbetsyta i din arbetsytesamling. Om du inte gjort det steget ännu, se [Kom igång med Power BI Embedded](power-bi-embedded-get-started.md).

Du kan använda hello .NET (C#) eller Node.js SDK, tillsammans med JavaScript, tooeasily skapa ditt program med Power BI Embedded. 

## <a name="using-hello-access-keys-toouse-rest-apis"></a>Med hjälp av hello åtkomst nycklar toouse REST API: er

Du kan överföra hello åtkomstnyckel som du kan hämta från hello Azure portal för en viss arbetsytesamling i ordning toocall hello REST API. Mer information finns i [Kom igång med Power BI Embedded](power-bi-embedded-get-started.md).

## <a name="get-a-report-id"></a>Hämta en rapport-id

Varje åtkomsttoken är baserad på en rapport. Du behöver tooget hello rapport-id för hello rapport som du vill tooembed angivna. Detta kan göras baserat på anrop toohello [hämta rapporter](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API. Detta returnerar hello rapport-id och hello bädda in URL: en. Detta kan göras med hjälp av hello Power BI .NET SDK eller anropa hello REST API direkt.

### <a name="using-hello-power-bi-net-sdk"></a>Med hjälp av hello Power BI .NET SDK

När du använder hello .NET SDK behöver toocreate token autentiseringsuppgifter som är baserad på hello åtkomstnyckel som du får från hello Azure-portalen. Detta kräver att du installerar hello [Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi).

**NuGet-paketet installation**

```
Install-Package Microsoft.PowerBI.Api
```

**C#-kod**

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a>Direkt anropar hello REST API

```
System.Net.WebRequest request = System.Net.WebRequest.Create("https://api.powerbi.com/v1.0/collections/{collectionName}/workspaces/{workspaceId}/Reports") as System.Net.HttpWebRequest;

request.Method = "GET";
request.ContentLength = 0;
request.Headers.Add("Authorization", String.Format("AppKey {0}", accessToken.Value));

using (var response = request.GetResponse() as System.Net.HttpWebResponse)
{
    using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
    {
        // Work with JSON response tooget hello report you want toowork with.
    }

}
```

## <a name="create-an-access-token"></a>Skapa en åtkomst-token

Power BI Embedded använder bädda in token, som är HMAC signerade JSON Web token. hello token är signerade med hello snabbtangent från din Azure Power BI Embedded arbetsytesamling. Bädda in tokens som standard, har använt tooprovide läsa bara komma åt tooa rapporten tooembed till ett program. Bädda in token utfärdas för en viss rapport och ska vara associerat med en embed-URL.

Åtkomst-token ska skapas på hello-servern som hello snabbtangenter är används toosign/kryptera hello-token. Mer information om hur toocreate en åtkomst-token finns [Authenticating och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md). Du kan också granska hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod. Här är ett exempel på hur det skulle se ut med hello .NET SDK för Power BI.

Du använder hello rapport-id som du hämtade tidigare. När hello bädda in token har skapats kan du sedan använda hello viktiga toogenerate hello åtkomsttoken som du kan använda hello javascript ur. Hej *PowerBIToken klassen* kräver att du installerar hello [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-paketet installation**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-kod**

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a>Lägga till behörigheten scope tooembed token

När du använder Embed token kan kanske du vill toorestrict användning av hello-resurser som du ger åtkomst till. Därför kan du generera en token med begränsade behörigheter. Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes)

## <a name="embed-using-javascript"></a>Bädda in med hjälp av JavaScript

När du har hello åtkomst-token och hello rapport-id, kan vi bädda in hello rapport med hjälp av JavaScript. Detta kräver att du installerar hello nuget [Power BI JavaScript paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). Hej embedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Du kan använda hello [JavaScript rapporten bäddas in provet](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funktioner. Det ger även kodexempel för hello olika åtgärder som är tillgängliga.

**NuGet-paketet installation**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-kod**

```
<script src="/scripts/powerbi.js"></script>
<div id="reportContainer"></div>

var embedConfiguration = {
    type: 'report',
    accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed'
};

var $reportContainer = $('#reportContainer');
var report = powerbi.embed($reportContainer.get(0), embedConfiguration);
```

### <a name="set-hello-size-of-embedded-elements"></a>Ange hello storleken på inbäddade element

hello rapporten bäddas automatiskt baserat på hello storleken på dess behållare. toooverride hello standardstorlek hello bäddar in helt enkelt lägga till en CSS-klassen attribut eller infogade format för bredd och höjd.

## <a name="see-also"></a>Se även

[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI JavaScript-paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
[Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-nod Git Repo](https://github.com/Microsoft/PowerBI-Node)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)
