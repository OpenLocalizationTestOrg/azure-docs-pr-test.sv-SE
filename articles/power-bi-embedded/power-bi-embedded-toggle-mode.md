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
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a><span data-ttu-id="9950e-103">Växla mellan att visa och redigera läge för rapporter i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="9950e-103">Toggle between view and edit mode for reports in Power BI Embedded</span></span>

<span data-ttu-id="9950e-104">Lär dig hur tootoggle mellan visa och redigera läge för dina rapporter i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="9950e-104">Learn how tootoggle between view and edit mode for your reports within Power BI Embedded.</span></span>

## <a name="creating-an-access-token"></a><span data-ttu-id="9950e-105">Skapa en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="9950e-105">Creating an access token</span></span>

<span data-ttu-id="9950e-106">Du behöver toocreate ett åtkomsttoken som ger dig hello möjlighet tooboth visa och redigera en rapport.</span><span class="sxs-lookup"><span data-stu-id="9950e-106">You will need toocreate an access token that gives you hello ability tooboth view and edit a report.</span></span> <span data-ttu-id="9950e-107">tooedit och spara en rapport måste hello **Report.ReadWrite** token behörighet.</span><span class="sxs-lookup"><span data-stu-id="9950e-107">tooedit and save a report, you will need hello **Report.ReadWrite** token permission.</span></span> <span data-ttu-id="9950e-108">Mer information finns i [Authenticating och auktorisera i Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="9950e-108">For more information, see [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9950e-109">Detta kan du tooedit och spara ändringarna tooan befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="9950e-109">This will allow you tooedit and save changes tooan existing report.</span></span> <span data-ttu-id="9950e-110">Om du vill också hello-funktionen stöder **Spara som**, behöver du toosupply ytterligare behörighet.</span><span class="sxs-lookup"><span data-stu-id="9950e-110">If you would also like hello function of supporting **Save As**, you will need toosupply additional permissions.</span></span> <span data-ttu-id="9950e-111">Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="9950e-111">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a><span data-ttu-id="9950e-112">Bädda in konfiguration</span><span class="sxs-lookup"><span data-stu-id="9950e-112">Embed configuration</span></span>

<span data-ttu-id="9950e-113">Du behöver toosupply behörigheter och en viewMode i ordning toosee hello spara knappen i redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="9950e-113">You will need toosupply permissions and a viewMode in order toosee hello save button when in edit mode.</span></span> <span data-ttu-id="9950e-114">Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="9950e-114">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="9950e-115">Till exempel i JavaScript:</span><span class="sxs-lookup"><span data-stu-id="9950e-115">For example in JavaScript:</span></span>

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

<span data-ttu-id="9950e-116">Detta visar tooembed hello rapporten i läget baserat på **viewMode** har angetts för**modeller. ViewMode.View**.</span><span class="sxs-lookup"><span data-stu-id="9950e-116">This will indicate tooembed hello report in view mode based on **viewMode** being set too**models.ViewMode.View**.</span></span>

## <a name="view-mode"></a><span data-ttu-id="9950e-117">Läget för</span><span class="sxs-lookup"><span data-stu-id="9950e-117">View mode</span></span>

<span data-ttu-id="9950e-118">Du kan använda följande JavaScript tooswitch i läget för hello om du är i redigeringsläge.</span><span class="sxs-lookup"><span data-stu-id="9950e-118">You can use hello following JavaScript tooswitch into view mode, if you are in edit mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooview mode.
report.switchMode("view");

```

## <a name="edit-mode"></a><span data-ttu-id="9950e-119">Redigeringsläge</span><span class="sxs-lookup"><span data-stu-id="9950e-119">Edit mode</span></span>

<span data-ttu-id="9950e-120">Du kan använda hello följande JavaScript tooswitch i redigeringsläge, om du är i vyn läge.</span><span class="sxs-lookup"><span data-stu-id="9950e-120">You can use hello following JavaScript tooswitch into edit mode, if you are in view mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooedit mode.
report.switchMode("edit");

```

## <a name="see-also"></a><span data-ttu-id="9950e-121">Se även</span><span class="sxs-lookup"><span data-stu-id="9950e-121">See also</span></span>

[<span data-ttu-id="9950e-122">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="9950e-122">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="9950e-123">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="9950e-123">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="9950e-124">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="9950e-124">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="9950e-125">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="9950e-125">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="9950e-126">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="9950e-126">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="9950e-127">PowerBI CSharp Git Repo</span><span class="sxs-lookup"><span data-stu-id="9950e-127">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="9950e-128">PowerBI-nod Git Repo</span><span class="sxs-lookup"><span data-stu-id="9950e-128">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="9950e-129">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="9950e-129">More questions?</span></span> [<span data-ttu-id="9950e-130">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="9950e-130">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
