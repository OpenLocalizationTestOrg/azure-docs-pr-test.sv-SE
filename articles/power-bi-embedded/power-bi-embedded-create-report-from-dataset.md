---
title: "aaaCreate en ny rapport från en datamängd i Azure Power BI Embedded | Microsoft Docs"
description: "Power BI Embedded rapporter kan nu skapas från en datamängd i ditt eget program."
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
ms.openlocfilehash: 41a0a52e4c833313f495bb5ff14749203fef9b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>Skapa en ny rapport från en datamängd i Power BI Embedded

Power BI Embedded rapporter kan nu skapas från en datamängd i ditt eget program. 

hello autentiseringsmetod liknar toothat av rapporten bäddas in. Den baseras på åtkomst-token som är specifika tooa dataset. Token som används för PowerBI.com har utfärdats av Azure Active Directory (AAD) och Power BI Embedded token utfärdas av din egen tjänst.

När createing en inbäddad rapport hello är token som utfärdats för en specifik dataset. Token som ska associeras med hello bädda in URL: en på hello samma elementet tooensure varje har en unik token. Ordna toocreate en inbäddad rapport *Dataset.Read och Workspace.Report.Create* omfång måste anges i hello åtkomst-token.

## <a name="create-access-token-needed-toocreate-new-report"></a>Skapa ny rapport för åtkomst-token nödvändiga toocreate

Power BI Embedded använder bädda in token, som är HMAC signerade JSON Web token. hello token är signerade med hello snabbtangent från din Azure Power BI Embedded arbetsytesamling. Bädda in tokens som standard, har använt tooprovide läsa bara komma åt tooa rapporten tooembed till ett program. Bädda in token utfärdas för en viss rapport och ska vara associerat med en embed-URL.

Åtkomst-token ska skapas på hello-servern som hello snabbtangenter är används toosign/kryptera hello-token. Mer information om hur toocreate en åtkomst-token finns [Authenticating och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md). Du kan också granska hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod. Här är ett exempel på hur det skulle se ut med hello .NET SDK för Power BI.

I det här exemplet har vi vårt datauppsättnings-id som vi vill toocreat hello nya rapporten i. Vi behöver också tooadd hello scope för *Dataset.Read och Workspace.Report.Create*.

Hej *PowerBIToken klassen* kräver att du installerar hello [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-paketet installation**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-kod**

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Skapa en ny tom rapport

I ordning toocreate en ny rapport hello skapar konfigurationen måste anges. Detta ska inbegripa hello åtkomst-token, hello embedURL och hello datasetID som vi vill toocreate hello rapporten mot. Detta kräver att du installerar hello nuget [Power BI JavaScript paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). Hej embedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.

> [!NOTE]
> Du kan använda hello [JavaScript rapporten bäddas in provet](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funktioner. Det ger även kodexempel för hello olika åtgärder som är tillgängliga.

**NuGet-paketet installation**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-kod**

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Anropar *powerbi.createReport()* gör början i redigeringsläge visas inom hello *div* element.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Spara nya rapporter

hello rapporten faktiskt skapas inte förrän du anropar hello **Spara som** igen. Detta kan göras från Arkiv-menyn eller JavaScript.

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> En ny rapport skapas efter **Spara som** anropas. När du har sparat visas hello arbetsytan fortfarande hello dataset i Redigera läge och inte hello rapporten. Du behöver tooreload hello ny rapport sätt som andra rapporter.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a>Läsa in hello ny rapport

I ordning toointeract med hello ny rapport måste tooembed i hello samma sätt som programmet hello bäddar in en vanlig rapport, betydelse, en ny token utfärdas specifikt för hello ny rapport och sedan anropa hello bädda in metod.

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a>Automatisera spara och läsa in i en ny rapport med hello ”spara” händelse

I ordning tooautomate hello processen att ”spara som” och läser in hello ny rapport kan du använda hello ”spara” händelse. Den här händelsen utlöses när hello spara åtgärden är klar och den returnerar ett Json-objekt som innehåller hello nya reportId, rapportnamnet, hello gamla reportId (om sådan fanns) och om hello utfördes saveAs eller spara.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

tooAutomate hello process som du kan lyssna på hello ”spara” händelse tar hello nya reportId, skapa nya hello-token och bädda in hello ny rapport med den.

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints tooLog window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that hello application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide hello wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
```

## <a name="see-also"></a>Se även

[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Spara rapporter](power-bi-embedded-save-reports.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[Power BI JavaScript-paket](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)
