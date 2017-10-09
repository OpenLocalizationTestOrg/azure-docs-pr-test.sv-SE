---
title: aaaSave rapporter i Azure Power BI Embedded | Microsoft Docs
description: "Lär dig hur toosave rapporter i Power BI embedded. Detta kräver har rätt behörigheter i ordning toowork."
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
ms.openlocfilehash: 984537ce1ce1afc787d6c6c9f61ae8d6226d1171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="save-reports-in-power-bi-embedded"></a>Spara rapporter i Power BI Embedded

Lär dig hur toosave rapporter i Power BI embedded. Detta kräver har rätt behörigheter i ordning toowork.

Du kan redigera befintliga rapporter och spara dem i Power BI Embedded. Du kan också skapa en ny rapport och spara som en ny rapport toocreate en.

I ordning toosave en rapport måste du först toocreate en token för för hello specifika rapporten med rätt hello-scope:

* tooenable spara Report.ReadWrite omfång måste anges
* tooenable Spara som, Report.Read och Workspace.Report.Copy scope krävs
* tooenable spara och spara som, Report.ReadWrite och Workspace.Report.Copy requierd

Respektive i ordning tooenable hello rätt save/spara måste som knappar Arkiv-menyn tooprovide hello Högerklicka behörighet i hello bädda in konfiguration när du Embed hello rapporten:

* modeller. Permissions.ReadWrite
* modeller. Permissions.Copy
* modeller. Permissions.All

> [!NOTE]
> Åtkomst-token måste också hello lämpliga omfattningar. Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Bädda in rapporten i redigeringsläge

Vi du vill säger tooEmbed en rapport i redigeringsläge i din app, toodo bara skicka hello rätt egenskaper i konfiguration Embed och anropa powerbi.embed(). Du behöver toosupply behörigheter och en viewMode i ordning toosee hello spara och spara som knappar i redigeringsläget. Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Till exempel i JavaScript:

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used toodescribe hello what and how tooembed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference toohello embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed hello report and display it within hello div container.
    var report = powerbi.embed(reportContainer, config);
```

En rapport ska nu vara inbäddat i din app i redigeringsläge.

## <a name="save-report"></a>Spara rapporten

När Embbeding hello rapporten i redigeringsläge med hello höger token och behörigheter kan du spara hello rapport från hello Arkiv-menyn eller javascript:

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Spara som

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
> Endast när *Spara som* är en ny rapport skapas. När hello spara visas hello arbetsytan fortfarande hello gamla rapporten i Redigera läge och inte hello ny rapport. Du behöver tooembed hello ny rapport som har skapats. Detta kräver en ny åtkomsttoken som har skapats per rapport.

Du måste sedan tooload hello ny rapport efter en *Spara som*. Detta är liknande tooembedding rapport.

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

## <a name="see-also"></a>Se även

[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Skapa en ny rapport från en datauppsättning](power-bi-embedded-create-report-from-dataset.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)

