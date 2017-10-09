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
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="e7206-103">Skapa en ny rapport från en datamängd i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e7206-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="e7206-104">Power BI Embedded rapporter kan nu skapas från en datamängd i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="e7206-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="e7206-105">hello autentiseringsmetod liknar toothat av rapporten bäddas in.</span><span class="sxs-lookup"><span data-stu-id="e7206-105">hello authentication method is similar toothat of report embed.</span></span> <span data-ttu-id="e7206-106">Den baseras på åtkomst-token som är specifika tooa dataset.</span><span class="sxs-lookup"><span data-stu-id="e7206-106">It is based on access tokens that are specific tooa dataset.</span></span> <span data-ttu-id="e7206-107">Token som används för PowerBI.com har utfärdats av Azure Active Directory (AAD) och Power BI Embedded token utfärdas av din egen tjänst.</span><span class="sxs-lookup"><span data-stu-id="e7206-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="e7206-108">När createing en inbäddad rapport hello är token som utfärdats för en specifik dataset.</span><span class="sxs-lookup"><span data-stu-id="e7206-108">When createing an Embedded report, hello tokens issued are for a specific dataset.</span></span> <span data-ttu-id="e7206-109">Token som ska associeras med hello bädda in URL: en på hello samma elementet tooensure varje har en unik token.</span><span class="sxs-lookup"><span data-stu-id="e7206-109">Tokens should be associated with hello embed URL on hello same element tooensure each has a unique token.</span></span> <span data-ttu-id="e7206-110">Ordna toocreate en inbäddad rapport *Dataset.Read och Workspace.Report.Create* omfång måste anges i hello åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="e7206-110">In order toocreate an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in hello access token.</span></span>

## <a name="create-access-token-needed-toocreate-new-report"></a><span data-ttu-id="e7206-111">Skapa ny rapport för åtkomst-token nödvändiga toocreate</span><span class="sxs-lookup"><span data-stu-id="e7206-111">Create access token needed toocreate new report</span></span>

<span data-ttu-id="e7206-112">Power BI Embedded använder bädda in token, som är HMAC signerade JSON Web token.</span><span class="sxs-lookup"><span data-stu-id="e7206-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="e7206-113">hello token är signerade med hello snabbtangent från din Azure Power BI Embedded arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="e7206-113">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="e7206-114">Bädda in tokens som standard, har använt tooprovide läsa bara komma åt tooa rapporten tooembed till ett program.</span><span class="sxs-lookup"><span data-stu-id="e7206-114">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="e7206-115">Bädda in token utfärdas för en viss rapport och ska vara associerat med en embed-URL.</span><span class="sxs-lookup"><span data-stu-id="e7206-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="e7206-116">Åtkomst-token ska skapas på hello-servern som hello snabbtangenter är används toosign/kryptera hello-token.</span><span class="sxs-lookup"><span data-stu-id="e7206-116">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="e7206-117">Mer information om hur toocreate en åtkomst-token finns [Authenticating och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="e7206-117">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="e7206-118">Du kan också granska hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod.</span><span class="sxs-lookup"><span data-stu-id="e7206-118">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="e7206-119">Här är ett exempel på hur det skulle se ut med hello .NET SDK för Power BI.</span><span class="sxs-lookup"><span data-stu-id="e7206-119">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="e7206-120">I det här exemplet har vi vårt datauppsättnings-id som vi vill toocreat hello nya rapporten i.</span><span class="sxs-lookup"><span data-stu-id="e7206-120">In this example, we have our dataset id that we want toocreat hello new report on.</span></span> <span data-ttu-id="e7206-121">Vi behöver också tooadd hello scope för *Dataset.Read och Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="e7206-121">We also need tooadd hello scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="e7206-122">Hej *PowerBIToken klassen* kräver att du installerar hello [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="e7206-122">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="e7206-123">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="e7206-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="e7206-124">**C#-kod**</span><span class="sxs-lookup"><span data-stu-id="e7206-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="e7206-125">Skapa en ny tom rapport</span><span class="sxs-lookup"><span data-stu-id="e7206-125">Create a new blank report</span></span>

<span data-ttu-id="e7206-126">I ordning toocreate en ny rapport hello skapar konfigurationen måste anges.</span><span class="sxs-lookup"><span data-stu-id="e7206-126">In order toocreate a new report, hello create configuration should be provided.</span></span> <span data-ttu-id="e7206-127">Detta ska inbegripa hello åtkomst-token, hello embedURL och hello datasetID som vi vill toocreate hello rapporten mot.</span><span class="sxs-lookup"><span data-stu-id="e7206-127">This should include hello access token, hello embedURL and hello datasetID that we want toocreate hello report against.</span></span> <span data-ttu-id="e7206-128">Detta kräver att du installerar hello nuget [Power BI JavaScript paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="e7206-128">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="e7206-129">Hej embedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="e7206-129">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="e7206-130">Du kan använda hello [JavaScript rapporten bäddas in provet](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funktioner.</span><span class="sxs-lookup"><span data-stu-id="e7206-130">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="e7206-131">Det ger även kodexempel för hello olika åtgärder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="e7206-131">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="e7206-132">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="e7206-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="e7206-133">**JavaScript-kod**</span><span class="sxs-lookup"><span data-stu-id="e7206-133">**JavaScript code**</span></span>

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

<span data-ttu-id="e7206-134">Anropar *powerbi.createReport()* gör början i redigeringsläge visas inom hello *div* element.</span><span class="sxs-lookup"><span data-stu-id="e7206-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within hello *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="e7206-135">Spara nya rapporter</span><span class="sxs-lookup"><span data-stu-id="e7206-135">Save new reports</span></span>

<span data-ttu-id="e7206-136">hello rapporten faktiskt skapas inte förrän du anropar hello **Spara som** igen.</span><span class="sxs-lookup"><span data-stu-id="e7206-136">hello report will not actually be created until you call hello **save as** operation.</span></span> <span data-ttu-id="e7206-137">Detta kan göras från Arkiv-menyn eller JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e7206-137">This can be done from file menu or from JavaScript.</span></span>

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
> <span data-ttu-id="e7206-138">En ny rapport skapas efter **Spara som** anropas.</span><span class="sxs-lookup"><span data-stu-id="e7206-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="e7206-139">När du har sparat visas hello arbetsytan fortfarande hello dataset i Redigera läge och inte hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="e7206-139">After you save, hello canvas will still show hello dataset in edit mode and not hello report.</span></span> <span data-ttu-id="e7206-140">Du behöver tooreload hello ny rapport sätt som andra rapporter.</span><span class="sxs-lookup"><span data-stu-id="e7206-140">You will need tooreload hello new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a><span data-ttu-id="e7206-141">Läsa in hello ny rapport</span><span class="sxs-lookup"><span data-stu-id="e7206-141">Load hello new report</span></span>

<span data-ttu-id="e7206-142">I ordning toointeract med hello ny rapport måste tooembed i hello samma sätt som programmet hello bäddar in en vanlig rapport, betydelse, en ny token utfärdas specifikt för hello ny rapport och sedan anropa hello bädda in metod.</span><span class="sxs-lookup"><span data-stu-id="e7206-142">In order toointeract with hello new report you need tooembed it in hello same way hello application embeds a regular report, meaning, a new token must be issued specifically for hello new report and then call hello embed method.</span></span>

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

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a><span data-ttu-id="e7206-143">Automatisera spara och läsa in i en ny rapport med hello ”spara” händelse</span><span class="sxs-lookup"><span data-stu-id="e7206-143">Automate save and load of a new report using hello "saved" event</span></span>

<span data-ttu-id="e7206-144">I ordning tooautomate hello processen att ”spara som” och läser in hello ny rapport kan du använda hello ”spara” händelse.</span><span class="sxs-lookup"><span data-stu-id="e7206-144">In order tooautomate hello process of "save as" and then loading hello new report you can make use of hello "saved" Event.</span></span> <span data-ttu-id="e7206-145">Den här händelsen utlöses när hello spara åtgärden är klar och den returnerar ett Json-objekt som innehåller hello nya reportId, rapportnamnet, hello gamla reportId (om sådan fanns) och om hello utfördes saveAs eller spara.</span><span class="sxs-lookup"><span data-stu-id="e7206-145">This event is fired when hello save operation is complete and it returns a Json object containing hello new reportId, report name, hello old reportId(if there was one) and if hello operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="e7206-146">tooAutomate hello process som du kan lyssna på hello ”spara” händelse tar hello nya reportId, skapa nya hello-token och bädda in hello ny rapport med den.</span><span class="sxs-lookup"><span data-stu-id="e7206-146">tooAutomate hello process you can listen on hello "saved" event, take hello new reportId, create hello new token and embed hello new report with it.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="e7206-147">Se även</span><span class="sxs-lookup"><span data-stu-id="e7206-147">See also</span></span>

[<span data-ttu-id="e7206-148">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="e7206-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="e7206-149">Spara rapporter</span><span class="sxs-lookup"><span data-stu-id="e7206-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="e7206-150">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="e7206-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="e7206-151">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="e7206-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="e7206-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="e7206-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="e7206-153">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="e7206-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="e7206-154">Power BI Core NuGut paketet</span><span class="sxs-lookup"><span data-stu-id="e7206-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="e7206-155">Power BI JavaScript-paket</span><span class="sxs-lookup"><span data-stu-id="e7206-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="e7206-156">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="e7206-156">More questions?</span></span> [<span data-ttu-id="e7206-157">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="e7206-157">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
