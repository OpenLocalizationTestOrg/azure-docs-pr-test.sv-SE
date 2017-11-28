---
title: Spara rapporterna i Azure Power BI Embedded | Microsoft Docs
description: "Lär dig mer om att spara rapporter i Power BI embedded. Detta kräver att rätt behörighet för att fungera korrekt."
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
ms.openlocfilehash: ad895004cc2972f2ded81566186325a16d401151
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="1616a-104">Spara rapporter i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1616a-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="1616a-105">Lär dig mer om att spara rapporter i Power BI embedded.</span><span class="sxs-lookup"><span data-stu-id="1616a-105">Learn how to save reports within Power BI embedded.</span></span> <span data-ttu-id="1616a-106">Detta kräver att rätt behörighet för att fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="1616a-106">This requires proper permissions in order to work successfully.</span></span>

<span data-ttu-id="1616a-107">Du kan redigera befintliga rapporter och spara dem i Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="1616a-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="1616a-108">Du kan också skapa en ny rapport och spara som en ny rapport för att skapa en.</span><span class="sxs-lookup"><span data-stu-id="1616a-108">You can also create a new report and save as a new report to create one.</span></span>

<span data-ttu-id="1616a-109">Om du vill spara en rapport måste du först skapa en token för den valda rapporten med rätt scope:</span><span class="sxs-lookup"><span data-stu-id="1616a-109">In order to save a report you first need to create a token for the specific report with the right scopes:</span></span>

* <span data-ttu-id="1616a-110">Om du vill aktivera spara Report.ReadWrite krävs omfattning</span><span class="sxs-lookup"><span data-stu-id="1616a-110">To enable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="1616a-111">Om du vill aktivera Spara som, krävs Report.Read och Workspace.Report.Copy scope</span><span class="sxs-lookup"><span data-stu-id="1616a-111">To enable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="1616a-112">Om du vill aktivera spara och spara som, är Report.ReadWrite och Workspace.Report.Copy requierd</span><span class="sxs-lookup"><span data-stu-id="1616a-112">To enable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="1616a-113">Respektive för att aktivera höger save/Spara som knappar Arkiv-menyn som du behöver ge behörighet i konfiguration Embed när du bäddar in rapporten:</span><span class="sxs-lookup"><span data-stu-id="1616a-113">Respectively in order to enable the right save/save as buttons in file menu you need to provide the right permission in the Embed configuration when you Embed the report:</span></span>

* <span data-ttu-id="1616a-114">modeller. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="1616a-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="1616a-115">modeller. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="1616a-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="1616a-116">modeller. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="1616a-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="1616a-117">Åtkomst-token måste också lämpliga omfattningar.</span><span class="sxs-lookup"><span data-stu-id="1616a-117">Your access token also needs the appropriate scopes.</span></span> <span data-ttu-id="1616a-118">Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="1616a-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="1616a-119">Bädda in rapporten i redigeringsläge</span><span class="sxs-lookup"><span data-stu-id="1616a-119">Embed report in edit mode</span></span>

<span data-ttu-id="1616a-120">Anta att du vill bädda in en rapport i redigeringsläge i din app att bara skicka rätt egenskaperna i konfiguration Embed och anropa powerbi.embed().</span><span class="sxs-lookup"><span data-stu-id="1616a-120">Let's say you want to Embed a report in edit mode inside your app, to do so just pass the right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="1616a-121">Du måste ange behörigheter och en viewMode för att visa spara och spara som knappar när i redigeringsläge.</span><span class="sxs-lookup"><span data-stu-id="1616a-121">You will need to supply permissions and a viewMode in order to see the save and save as buttons when in edit mode.</span></span> <span data-ttu-id="1616a-122">Mer information finns i [bädda in konfigurationsinformation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="1616a-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="1616a-123">Till exempel i JavaScript:</span><span class="sxs-lookup"><span data-stu-id="1616a-123">For example in JavaScript:</span></span>

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
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
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

<span data-ttu-id="1616a-124">En rapport ska nu vara inbäddat i din app i redigeringsläge.</span><span class="sxs-lookup"><span data-stu-id="1616a-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="1616a-125">Spara rapporten</span><span class="sxs-lookup"><span data-stu-id="1616a-125">Save report</span></span>

<span data-ttu-id="1616a-126">Efter Embbeding rapporten i redigeringsläge med rätt token och behörigheter som du kan spara rapporten från Arkivmenyn eller javascript:</span><span class="sxs-lookup"><span data-stu-id="1616a-126">After Embbeding the report in edit mode with the right token and permissions you can save the report from the file menu or from javascript:</span></span>

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="1616a-127">Spara som</span><span class="sxs-lookup"><span data-stu-id="1616a-127">Save as</span></span>

```
// Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> <span data-ttu-id="1616a-128">Endast när *Spara som* är en ny rapport skapas.</span><span class="sxs-lookup"><span data-stu-id="1616a-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="1616a-129">När du spara visas arbetsytan fortfarande gamla rapporten i redigeringsläge och inte den nya rapporten.</span><span class="sxs-lookup"><span data-stu-id="1616a-129">After the save, the canvas is still showing the old report in edit mode and not the new report.</span></span> <span data-ttu-id="1616a-130">Du behöver att bädda in den nya rapporten skapades.</span><span class="sxs-lookup"><span data-stu-id="1616a-130">You will need to embed the new report that was created.</span></span> <span data-ttu-id="1616a-131">Detta kräver en ny åtkomsttoken som har skapats per rapport.</span><span class="sxs-lookup"><span data-stu-id="1616a-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="1616a-132">Sedan måste du läsa in den nya rapporten efter en *Spara som*.</span><span class="sxs-lookup"><span data-stu-id="1616a-132">You will then need to load the new report after a *save as*.</span></span> <span data-ttu-id="1616a-133">Detta liknar bädda in en rapport.</span><span class="sxs-lookup"><span data-stu-id="1616a-133">This is similar to embedding any report.</span></span>

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="see-also"></a><span data-ttu-id="1616a-134">Se även</span><span class="sxs-lookup"><span data-stu-id="1616a-134">See also</span></span>

[<span data-ttu-id="1616a-135">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="1616a-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="1616a-136">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="1616a-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="1616a-137">Skapa en ny rapport från en datauppsättning</span><span class="sxs-lookup"><span data-stu-id="1616a-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="1616a-138">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="1616a-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="1616a-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="1616a-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="1616a-140">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="1616a-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="1616a-141">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="1616a-141">More questions?</span></span> [<span data-ttu-id="1616a-142">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="1616a-142">Try the Power BI Community</span></span>](http://community.powerbi.com/)

