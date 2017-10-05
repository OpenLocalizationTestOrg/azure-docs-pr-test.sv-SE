---
title: "Växla mellan att visa och redigera läge för rapporter i Azure Power BI Embedded | Microsoft Docs"
description: "Lär dig mer om att växla mellan att visa och redigera läge för rapporter i Power BI Embedded."
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
ms.openlocfilehash: f73bf05b41523a5833cc9366fb84cb7021b4b7a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a><span data-ttu-id="83ee7-103">Växla mellan att visa och redigera läge för rapporter i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="83ee7-103">Toggle between view and edit mode for reports in Power BI Embedded</span></span>

<span data-ttu-id="83ee7-104">Lär dig mer om att växla mellan att visa och redigera läge för rapporter i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="83ee7-104">Learn how to toggle between view and edit mode for your reports within Power BI Embedded.</span></span>

## <a name="creating-an-access-token"></a><span data-ttu-id="83ee7-105">Skapa en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="83ee7-105">Creating an access token</span></span>

<span data-ttu-id="83ee7-106">Du behöver skapa ett åtkomsttoken som ger dig möjlighet att både visa och redigera en rapport.</span><span class="sxs-lookup"><span data-stu-id="83ee7-106">You will need to create an access token that gives you the ability to both view and edit a report.</span></span> <span data-ttu-id="83ee7-107">Om du vill redigera och spara en rapport, måste den **Report.ReadWrite** token behörighet.</span><span class="sxs-lookup"><span data-stu-id="83ee7-107">To edit and save a report, you will need the **Report.ReadWrite** token permission.</span></span> <span data-ttu-id="83ee7-108">Mer information finns i [Authenticating och auktorisera i Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="83ee7-108">For more information, see [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

> [!NOTE]
> <span data-ttu-id="83ee7-109">Detta kan du redigera och spara ändringar i en befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="83ee7-109">This will allow you to edit and save changes to an existing report.</span></span> <span data-ttu-id="83ee7-110">Om du vill också att funktionen stödja **Spara som**, måste du ange ytterligare behörighet.</span><span class="sxs-lookup"><span data-stu-id="83ee7-110">If you would also like the function of supporting **Save As**, you will need to supply additional permissions.</span></span> <span data-ttu-id="83ee7-111">Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="83ee7-111">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a><span data-ttu-id="83ee7-112">Bädda in konfiguration</span><span class="sxs-lookup"><span data-stu-id="83ee7-112">Embed configuration</span></span>

<span data-ttu-id="83ee7-113">Du måste ange behörigheter och en viewMode för att visa spara knappen i redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="83ee7-113">You will need to supply permissions and a viewMode in order to see the save button when in edit mode.</span></span> <span data-ttu-id="83ee7-114">Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="83ee7-114">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="83ee7-115">Till exempel i JavaScript:</span><span class="sxs-lookup"><span data-stu-id="83ee7-115">For example in JavaScript:</span></span>

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
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

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

<span data-ttu-id="83ee7-116">Detta visar att bädda in rapporten i läget baserat på **viewMode** anges till **modeller. ViewMode.View**.</span><span class="sxs-lookup"><span data-stu-id="83ee7-116">This will indicate to embed the report in view mode based on **viewMode** being set to **models.ViewMode.View**.</span></span>

## <a name="view-mode"></a><span data-ttu-id="83ee7-117">Läget för</span><span class="sxs-lookup"><span data-stu-id="83ee7-117">View mode</span></span>

<span data-ttu-id="83ee7-118">Du kan använda följande JavaScript för att växla till läget, om du är i redigeringsläge.</span><span class="sxs-lookup"><span data-stu-id="83ee7-118">You can use the following JavaScript to switch into view mode, if you are in edit mode.</span></span>

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a><span data-ttu-id="83ee7-119">Redigeringsläge</span><span class="sxs-lookup"><span data-stu-id="83ee7-119">Edit mode</span></span>

<span data-ttu-id="83ee7-120">Du kan använda följande JavaScript för att växla till redigeringsläget, om du är i läget.</span><span class="sxs-lookup"><span data-stu-id="83ee7-120">You can use the following JavaScript to switch into edit mode, if you are in view mode.</span></span>

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a><span data-ttu-id="83ee7-121">Se även</span><span class="sxs-lookup"><span data-stu-id="83ee7-121">See also</span></span>

[<span data-ttu-id="83ee7-122">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="83ee7-122">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="83ee7-123">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="83ee7-123">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="83ee7-124">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="83ee7-124">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="83ee7-125">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="83ee7-125">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="83ee7-126">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="83ee7-126">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="83ee7-127">PowerBI CSharp Git Repo</span><span class="sxs-lookup"><span data-stu-id="83ee7-127">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="83ee7-128">PowerBI-nod Git Repo</span><span class="sxs-lookup"><span data-stu-id="83ee7-128">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="83ee7-129">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="83ee7-129">More questions?</span></span> [<span data-ttu-id="83ee7-130">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="83ee7-130">Try the Power BI Community</span></span>](http://community.powerbi.com/)
