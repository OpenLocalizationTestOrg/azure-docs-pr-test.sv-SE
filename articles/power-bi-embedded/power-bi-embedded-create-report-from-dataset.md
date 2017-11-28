---
title: "Skapa en ny rapport från en datamängd i Azure Power BI Embedded | Microsoft Docs"
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
ms.openlocfilehash: 457f53aa76059dbb2faed6b264102f1f59b9918a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="9e6f6-103">Skapa en ny rapport från en datamängd i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="9e6f6-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="9e6f6-104">Power BI Embedded rapporter kan nu skapas från en datamängd i ditt eget program.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="9e6f6-105">Autentiseringsmetoden som liknar att av rapporten bäddas in.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-105">The authentication method is similar to that of report embed.</span></span> <span data-ttu-id="9e6f6-106">Den baseras på åtkomst-token som är specifika för en dataset.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-106">It is based on access tokens that are specific to a dataset.</span></span> <span data-ttu-id="9e6f6-107">Token som används för PowerBI.com har utfärdats av Azure Active Directory (AAD) och Power BI Embedded token utfärdas av din egen tjänst.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="9e6f6-108">När är createing en inbäddad rapport token som utfärdas för en specifik dataset.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-108">When createing an Embedded report, the tokens issued are for a specific dataset.</span></span> <span data-ttu-id="9e6f6-109">Token ska vara associerat med bädda in URL: en i samma element för var och en har en unik token.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-109">Tokens should be associated with the embed URL on the same element to ensure each has a unique token.</span></span> <span data-ttu-id="9e6f6-110">För att skapa en inbäddad rapport *Dataset.Read och Workspace.Report.Create* omfång måste anges i den åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-110">In order to create an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in the access token.</span></span>

## <a name="create-access-token-needed-to-create-new-report"></a><span data-ttu-id="9e6f6-111">Skapa åtkomsttoken som behövs för att skapa ny rapport</span><span class="sxs-lookup"><span data-stu-id="9e6f6-111">Create access token needed to create new report</span></span>

<span data-ttu-id="9e6f6-112">Power BI Embedded använder bädda in token, som är HMAC signerade JSON Web token.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="9e6f6-113">Token som signerats med åtkomst till nyckeln från din Azure Power BI Embedded arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-113">The tokens are signed with the access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="9e6f6-114">Bädda in tokens som standard, som används för skrivskyddat läge till en rapport som ska bäddas in i ett program.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-114">Embed tokens, by default, are used to provide read only access to a report to embed into an application.</span></span> <span data-ttu-id="9e6f6-115">Bädda in token utfärdas för en viss rapport och ska vara associerat med en embed-URL.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="9e6f6-116">Åtkomst-token ska skapas på servern som åtkomstnycklarna används för att logga/kryptera token.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-116">Access tokens should be created on the server as the access keys are used to sign/encrypt the tokens.</span></span> <span data-ttu-id="9e6f6-117">Information om hur du skapar en åtkomst-token finns [Authenticating och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="9e6f6-117">For information on how to create an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="9e6f6-118">Du kan också granska den [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-118">You can also review the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="9e6f6-119">Här är ett exempel på hur det skulle se ut med .NET SDK för Power BI.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-119">Here is an example of what this would look like using the .NET SDK for Power BI.</span></span>

<span data-ttu-id="9e6f6-120">I det här exemplet har vi vårt datauppsättnings-id som vi vill skap den nya rapporten i.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-120">In this example, we have our dataset id that we want to creat the new report on.</span></span> <span data-ttu-id="9e6f6-121">Vi behöver lägga till scope för *Dataset.Read och Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-121">We also need to add the scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="9e6f6-122">Den *PowerBIToken klassen* kräver att du installerar den [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="9e6f6-122">The *PowerBIToken class* requires that you install the [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="9e6f6-123">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="9e6f6-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="9e6f6-124">**C#-kod**</span><span class="sxs-lookup"><span data-stu-id="9e6f6-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="9e6f6-125">Skapa en ny tom rapport</span><span class="sxs-lookup"><span data-stu-id="9e6f6-125">Create a new blank report</span></span>

<span data-ttu-id="9e6f6-126">Skapa konfiguration måste anges för att skapa en ny rapport.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-126">In order to create a new report, the create configuration should be provided.</span></span> <span data-ttu-id="9e6f6-127">Detta ska inkludera den åtkomst-token och embedURL datasetID som vi vill skapa rapporten mot.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-127">This should include the access token, the embedURL and the datasetID that we want to create the report against.</span></span> <span data-ttu-id="9e6f6-128">Detta kräver att du installerar nuget [Power BI JavaScript paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="9e6f6-128">This requires that you install the nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="9e6f6-129">EmbedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-129">The embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="9e6f6-130">Du kan använda den [JavaScript rapporten bäddas in provet](https://microsoft.github.io/PowerBI-JavaScript/demo/) att testa funktionen.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-130">You can use the [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) to test functionality.</span></span> <span data-ttu-id="9e6f6-131">Det ger även kodexempel för olika åtgärder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-131">It also gives code examples for the different operations that are available.</span></span>

<span data-ttu-id="9e6f6-132">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="9e6f6-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="9e6f6-133">**JavaScript-kod**</span><span class="sxs-lookup"><span data-stu-id="9e6f6-133">**JavaScript code**</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

<span data-ttu-id="9e6f6-134">Anropar *powerbi.createReport()* gör början i redigeringsläge visas inom den *div* element.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within the *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="9e6f6-135">Spara nya rapporter</span><span class="sxs-lookup"><span data-stu-id="9e6f6-135">Save new reports</span></span>

<span data-ttu-id="9e6f6-136">Rapporten faktiskt skapas inte förrän du anropar den **Spara som** igen.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-136">The report will not actually be created until you call the **save as** operation.</span></span> <span data-ttu-id="9e6f6-137">Detta kan göras från Arkiv-menyn eller JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-137">This can be done from file menu or from JavaScript.</span></span>

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
> <span data-ttu-id="9e6f6-138">En ny rapport skapas efter **Spara som** anropas.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="9e6f6-139">När du har sparat visas arbetsytan fortfarande datauppsättningen i redigeringsläge och inte i rapporten.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-139">After you save, the canvas will still show the dataset in edit mode and not the report.</span></span> <span data-ttu-id="9e6f6-140">Du måste du uppdatera den nya rapporten sätt som andra rapporter.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-140">You will need to reload the new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a><span data-ttu-id="9e6f6-141">Läsa in den nya rapporten</span><span class="sxs-lookup"><span data-stu-id="9e6f6-141">Load the new report</span></span>

<span data-ttu-id="9e6f6-142">För att kunna interagera med den nya rapporten som du behöver bädda in den på samma sätt som programmet bäddar in en vanlig rapport, vilket innebär att en ny token utfärdas specifikt för den nya rapporten och sedan anropa metoden Bädda in.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-142">In order to interact with the new report you need to embed it in the same way the application embeds a regular report, meaning, a new token must be issued specifically for the new report and then call the embed method.</span></span>

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

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a><span data-ttu-id="9e6f6-143">Automatisera spara och läsa in i en ny rapport med händelsen ”sparade”</span><span class="sxs-lookup"><span data-stu-id="9e6f6-143">Automate save and load of a new report using the "saved" event</span></span>

<span data-ttu-id="9e6f6-144">För att automatisera processen med ”Spara som” i och läser in den nya rapporten kan du använda ”sparade” händelsen.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-144">In order to automate the process of "save as" and then loading the new report you can make use of the "saved" Event.</span></span> <span data-ttu-id="9e6f6-145">Den här händelsen utlöses när spara åtgärden har slutförts och returnerar ett Json-objekt som innehåller nya reportId, rapportnamnet, gamla reportId (om sådan fanns) och om åtgärden saveAs eller spara.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-145">This event is fired when the save operation is complete and it returns a Json object containing the new reportId, report name, the old reportId(if there was one) and if the operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="9e6f6-146">För att automatisera processen du lyssna på händelsen ”sparade”, tar den nya reportId, skapa en ny token och bädda in den nya rapporten med den.</span><span class="sxs-lookup"><span data-stu-id="9e6f6-146">To Automate the process you can listen on the "saved" event, take the new reportId, create the new token and embed the new report with it.</span></span>

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
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

## <a name="see-also"></a><span data-ttu-id="9e6f6-147">Se även</span><span class="sxs-lookup"><span data-stu-id="9e6f6-147">See also</span></span>

[<span data-ttu-id="9e6f6-148">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="9e6f6-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="9e6f6-149">Spara rapporter</span><span class="sxs-lookup"><span data-stu-id="9e6f6-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="9e6f6-150">Bädda in en rapport</span><span class="sxs-lookup"><span data-stu-id="9e6f6-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="9e6f6-151">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="9e6f6-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="9e6f6-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="9e6f6-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="9e6f6-153">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="9e6f6-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="9e6f6-154">Power BI Core NuGut paketet</span><span class="sxs-lookup"><span data-stu-id="9e6f6-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="9e6f6-155">Power BI JavaScript-paket</span><span class="sxs-lookup"><span data-stu-id="9e6f6-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="9e6f6-156">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="9e6f6-156">More questions?</span></span> [<span data-ttu-id="9e6f6-157">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="9e6f6-157">Try the Power BI Community</span></span>](http://community.powerbi.com/)