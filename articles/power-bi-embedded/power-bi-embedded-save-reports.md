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
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="85be3-104">Spara rapporter i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="85be3-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="85be3-105">Lär dig hur toosave rapporter i Power BI embedded.</span><span class="sxs-lookup"><span data-stu-id="85be3-105">Learn how toosave reports within Power BI embedded.</span></span> <span data-ttu-id="85be3-106">Detta kräver har rätt behörigheter i ordning toowork.</span><span class="sxs-lookup"><span data-stu-id="85be3-106">This requires proper permissions in order toowork successfully.</span></span>

<span data-ttu-id="85be3-107">Du kan redigera befintliga rapporter och spara dem i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="85be3-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="85be3-108">Du kan också skapa en ny rapport och spara som en ny rapport toocreate en.</span><span class="sxs-lookup"><span data-stu-id="85be3-108">You can also create a new report and save as a new report toocreate one.</span></span>

<span data-ttu-id="85be3-109">I ordning toosave en rapport måste du först toocreate en token för för hello specifika rapporten med rätt hello-scope:</span><span class="sxs-lookup"><span data-stu-id="85be3-109">In order toosave a report you first need toocreate a token for hello specific report with hello right scopes:</span></span>

* <span data-ttu-id="85be3-110">tooenable spara Report.ReadWrite omfång måste anges</span><span class="sxs-lookup"><span data-stu-id="85be3-110">tooenable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="85be3-111">tooenable Spara som, Report.Read och Workspace.Report.Copy scope krävs</span><span class="sxs-lookup"><span data-stu-id="85be3-111">tooenable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="85be3-112">tooenable spara och spara som, Report.ReadWrite och Workspace.Report.Copy requierd</span><span class="sxs-lookup"><span data-stu-id="85be3-112">tooenable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="85be3-113">Respektive i ordning tooenable hello rätt save/spara måste som knappar Arkiv-menyn tooprovide hello Högerklicka behörighet i hello bädda in konfiguration när du Embed hello rapporten:</span><span class="sxs-lookup"><span data-stu-id="85be3-113">Respectively in order tooenable hello right save/save as buttons in file menu you need tooprovide hello right permission in hello Embed configuration when you Embed hello report:</span></span>

* <span data-ttu-id="85be3-114">modeller. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="85be3-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="85be3-115">modeller. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="85be3-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="85be3-116">modeller. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="85be3-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="85be3-117">Åtkomst-token måste också hello lämpliga omfattningar.</span><span class="sxs-lookup"><span data-stu-id="85be3-117">Your access token also needs hello appropriate scopes.</span></span> <span data-ttu-id="85be3-118">Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="85be3-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="85be3-119">Bädda in rapporten i redigeringsläge</span><span class="sxs-lookup"><span data-stu-id="85be3-119">Embed report in edit mode</span></span>

<span data-ttu-id="85be3-120">Vi du vill säger tooEmbed en rapport i redigeringsläge i din app, toodo bara skicka hello rätt egenskaper i konfiguration Embed och anropa powerbi.embed().</span><span class="sxs-lookup"><span data-stu-id="85be3-120">Let's say you want tooEmbed a report in edit mode inside your app, toodo so just pass hello right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="85be3-121">Du behöver toosupply behörigheter och en viewMode i ordning toosee hello spara och spara som knappar i redigeringsläget.</span><span class="sxs-lookup"><span data-stu-id="85be3-121">You will need toosupply permissions and a viewMode in order toosee hello save and save as buttons when in edit mode.</span></span> <span data-ttu-id="85be3-122">Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="85be3-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="85be3-123">Till exempel i JavaScript:</span><span class="sxs-lookup"><span data-stu-id="85be3-123">For example in JavaScript:</span></span>

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

<span data-ttu-id="85be3-124">En rapport ska nu vara inbäddat i din app i redigeringsläge.</span><span class="sxs-lookup"><span data-stu-id="85be3-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="85be3-125">Spara rapporten</span><span class="sxs-lookup"><span data-stu-id="85be3-125">Save report</span></span>

<span data-ttu-id="85be3-126">När Embbeding hello rapporten i redigeringsläge med hello höger token och behörigheter kan du spara hello rapport från hello Arkiv-menyn eller javascript:</span><span class="sxs-lookup"><span data-stu-id="85be3-126">After Embbeding hello report in edit mode with hello right token and permissions you can save hello report from hello file menu or from javascript:</span></span>

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="85be3-127">Spara som</span><span class="sxs-lookup"><span data-stu-id="85be3-127">Save as</span></span>

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
> <span data-ttu-id="85be3-128">Endast när *Spara som* är en ny rapport skapas.</span><span class="sxs-lookup"><span data-stu-id="85be3-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="85be3-129">När hello spara visas hello arbetsytan fortfarande hello gamla rapporten i Redigera läge och inte hello ny rapport.</span><span class="sxs-lookup"><span data-stu-id="85be3-129">After hello save, hello canvas is still showing hello old report in edit mode and not hello new report.</span></span> <span data-ttu-id="85be3-130">Du behöver tooembed hello ny rapport som har skapats.</span><span class="sxs-lookup"><span data-stu-id="85be3-130">You will need tooembed hello new report that was created.</span></span> <span data-ttu-id="85be3-131">Detta kräver en ny åtkomsttoken som har skapats per rapport.</span><span class="sxs-lookup"><span data-stu-id="85be3-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="85be3-132">Du måste sedan tooload hello ny rapport efter en *Spara som*.</span><span class="sxs-lookup"><span data-stu-id="85be3-132">You will then need tooload hello new report after a *save as*.</span></span> <span data-ttu-id="85be3-133">Detta är liknande tooembedding rapport.</span><span class="sxs-lookup"><span data-stu-id="85be3-133">This is similar tooembedding any report.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="85be3-134">Se även</span><span class="sxs-lookup"><span data-stu-id="85be3-134">See also</span></span>

[<span data-ttu-id="85be3-135">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="85be3-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="85be3-136">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="85be3-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="85be3-137">Skapa en ny rapport från en datauppsättning</span><span class="sxs-lookup"><span data-stu-id="85be3-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="85be3-138">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="85be3-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="85be3-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="85be3-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="85be3-140">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="85be3-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="85be3-141">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="85be3-141">More questions?</span></span> [<span data-ttu-id="85be3-142">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="85be3-142">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

