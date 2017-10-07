---
title: "aaaToggle mellan visa och redigera läge för rapporter i Azure Power BI Embedded | Microsoft Docs"
description: "Lär dig hur tootoggle mellan visa och redigera läge för dina rapporter i Power BI Embedded."
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
ms.openlocfilehash: c9e3da5f9ae74d221af650adebde7c9d83b38a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a>Växla mellan att visa och redigera läge för rapporter i Power BI Embedded

Lär dig hur tootoggle mellan visa och redigera läge för dina rapporter i Power BI Embedded.

## <a name="creating-an-access-token"></a>Skapa en åtkomst-token

Du behöver toocreate ett åtkomsttoken som ger dig hello möjlighet tooboth visa och redigera en rapport. tooedit och spara en rapport måste hello **Report.ReadWrite** token behörighet. Mer information finns i [Authenticating och auktorisera i Power BI Embedded](power-bi-embedded-app-token-flow.md).

> [!NOTE]
> Detta kan du tooedit och spara ändringarna tooan befintlig rapport. Om du vill också hello-funktionen stöder **Spara som**, behöver du toosupply ytterligare behörighet. Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Bädda in konfiguration

Du behöver toosupply behörigheter och en viewMode i ordning toosee hello spara knappen i redigeringsläget. Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

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
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
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

Detta visar tooembed hello rapporten i läget baserat på **viewMode** har angetts för**modeller. ViewMode.View**.

## <a name="view-mode"></a>Läget för

Du kan använda följande JavaScript tooswitch i läget för hello om du är i redigeringsläge.

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooview mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Redigeringsläge

Du kan använda hello följande JavaScript tooswitch i redigeringsläge, om du är i vyn läge.

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooedit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Se även

[Komma igång med exemplet](power-bi-embedded-get-started-sample.md)  
[Bädda in en rapport](power-bi-embedded-embed-report.md)  
[Autentisering och auktorisering i Power BI Embedded](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Inbäddat exempel med JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[PowerBI CSharp Git Repo](https://github.com/Microsoft/PowerBI-CSharp)  
[PowerBI-nod Git Repo](https://github.com/Microsoft/PowerBI-Node)  
Fler frågor? [Försök hello Power BI-communityn](http://community.powerbi.com/)
