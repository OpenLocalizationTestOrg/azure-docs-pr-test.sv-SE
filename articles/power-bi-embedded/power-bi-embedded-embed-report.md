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
# <a name="embed-a-report-in-power-bi-embedded"></a><span data-ttu-id="c30de-103">Bädda in en rapport i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="c30de-103">Embed a report in Power BI Embedded</span></span>

<span data-ttu-id="c30de-104">Lär dig hur tooembed en rapport som är i Power BI Embedded till programmet.</span><span class="sxs-lookup"><span data-stu-id="c30de-104">Learn how tooembed a report that is in Power BI Embedded into your application.</span></span>

<span data-ttu-id="c30de-105">Vi ska titta på hur tooactually bädda in en rapport i ditt program.</span><span class="sxs-lookup"><span data-stu-id="c30de-105">We will look at how tooactually embed a report into your application.</span></span> <span data-ttu-id="c30de-106">Detta under förutsättning att du redan har en rapport som finns i en arbetsyta i din arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="c30de-106">This is assuming you already have a report that exists within a workspace in your workspace collection.</span></span> <span data-ttu-id="c30de-107">Om du inte gjort det steget ännu, se [Kom igång med Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c30de-107">If you haven't done that step yet, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

<span data-ttu-id="c30de-108">Du kan använda hello .NET (C#) eller Node.js SDK, tillsammans med JavaScript, tooeasily skapa ditt program med Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="c30de-108">You can use hello .NET (C#) or Node.js SDK, along with JavaScript, tooeasily build your application with Power BI Embedded.</span></span> 

## <a name="using-hello-access-keys-toouse-rest-apis"></a><span data-ttu-id="c30de-109">Med hjälp av hello åtkomst nycklar toouse REST API: er</span><span class="sxs-lookup"><span data-stu-id="c30de-109">Using hello access keys toouse REST APIs</span></span>

<span data-ttu-id="c30de-110">Du kan överföra hello åtkomstnyckel som du kan hämta från hello Azure portal för en viss arbetsytesamling i ordning toocall hello REST API.</span><span class="sxs-lookup"><span data-stu-id="c30de-110">In order toocall hello REST API, you can pass hello access key which you can get from hello Azure portal for a given workspace collection.</span></span> <span data-ttu-id="c30de-111">Mer information finns i [Kom igång med Power BI Embedded](power-bi-embedded-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c30de-111">For more information, see [Get started with Power BI Embedded](power-bi-embedded-get-started.md).</span></span>

## <a name="get-a-report-id"></a><span data-ttu-id="c30de-112">Hämta en rapport-id</span><span class="sxs-lookup"><span data-stu-id="c30de-112">Get a report id</span></span>

<span data-ttu-id="c30de-113">Varje åtkomsttoken är baserad på en rapport.</span><span class="sxs-lookup"><span data-stu-id="c30de-113">Every access token is based on a report.</span></span> <span data-ttu-id="c30de-114">Du behöver tooget hello rapport-id för hello rapport som du vill tooembed angivna.</span><span class="sxs-lookup"><span data-stu-id="c30de-114">You will need tooget hello given report id for hello report that you want tooembed.</span></span> <span data-ttu-id="c30de-115">Detta kan göras baserat på anrop toohello [hämta rapporter](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span><span class="sxs-lookup"><span data-stu-id="c30de-115">This can be done based on calls toohello [Get Reports](https://msdn.microsoft.com/library/azure/mt711510.aspx) REST API.</span></span> <span data-ttu-id="c30de-116">Detta returnerar hello rapport-id och hello bädda in URL: en.</span><span class="sxs-lookup"><span data-stu-id="c30de-116">This will return hello report id and hello embed url.</span></span> <span data-ttu-id="c30de-117">Detta kan göras med hjälp av hello Power BI .NET SDK eller anropa hello REST API direkt.</span><span class="sxs-lookup"><span data-stu-id="c30de-117">This can be done using hello Power BI .NET SDK or calling hello REST API directly.</span></span>

### <a name="using-hello-power-bi-net-sdk"></a><span data-ttu-id="c30de-118">Med hjälp av hello Power BI .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c30de-118">Using hello Power BI .NET SDK</span></span>

<span data-ttu-id="c30de-119">När du använder hello .NET SDK behöver toocreate token autentiseringsuppgifter som är baserad på hello åtkomstnyckel som du får från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c30de-119">When using hello .NET SDK, you will need toocreate a token credential which is based on hello access key you get from hello Azure portal.</span></span> <span data-ttu-id="c30de-120">Detta kräver att du installerar hello [Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi).</span><span class="sxs-lookup"><span data-stu-id="c30de-120">This requires that you install hello [Power BI API NuGet package](https://www.nuget.org/profiles/powerbi).</span></span>

<span data-ttu-id="c30de-121">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="c30de-121">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Api
```

<span data-ttu-id="c30de-122">**C#-kod**</span><span class="sxs-lookup"><span data-stu-id="c30de-122">**C# code**</span></span>

```
using Microsoft.PowerBI.Api.V1;
using Microsoft.Rest;

var credentials = new TokenCredentials("{access key}", "AppKey");
var client = new PowerBIClient(credentials);
client.BaseUri = new Uri(https://api.powerbi.com);

var reports = (IList<Report>)client.Reports.GetReports(workspaceCollectionName, workspaceId).Value;

// Select hello report that you want toowork with from hello collection of reports.
```

### <a name="calling-hello-rest-api-directly"></a><span data-ttu-id="c30de-123">Direkt anropar hello REST API</span><span class="sxs-lookup"><span data-stu-id="c30de-123">Calling hello REST API directly</span></span>

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

## <a name="create-an-access-token"></a><span data-ttu-id="c30de-124">Skapa en åtkomst-token</span><span class="sxs-lookup"><span data-stu-id="c30de-124">Create an access token</span></span>

<span data-ttu-id="c30de-125">Power BI Embedded använder bädda in token, som är HMAC signerade JSON Web token.</span><span class="sxs-lookup"><span data-stu-id="c30de-125">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="c30de-126">hello token är signerade med hello snabbtangent från din Azure Power BI Embedded arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="c30de-126">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="c30de-127">Bädda in tokens som standard, har använt tooprovide läsa bara komma åt tooa rapporten tooembed till ett program.</span><span class="sxs-lookup"><span data-stu-id="c30de-127">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="c30de-128">Bädda in token utfärdas för en viss rapport och ska vara associerat med en embed-URL.</span><span class="sxs-lookup"><span data-stu-id="c30de-128">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="c30de-129">Åtkomst-token ska skapas på hello-servern som hello snabbtangenter är används toosign/kryptera hello-token.</span><span class="sxs-lookup"><span data-stu-id="c30de-129">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="c30de-130">Mer information om hur toocreate en åtkomst-token finns [Authenticating och auktorisera med Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="c30de-130">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="c30de-131">Du kan också granska hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metod.</span><span class="sxs-lookup"><span data-stu-id="c30de-131">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="c30de-132">Här är ett exempel på hur det skulle se ut med hello .NET SDK för Power BI.</span><span class="sxs-lookup"><span data-stu-id="c30de-132">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="c30de-133">Du använder hello rapport-id som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c30de-133">You will use hello report id that you retrieved earlier.</span></span> <span data-ttu-id="c30de-134">När hello bädda in token har skapats kan du sedan använda hello viktiga toogenerate hello åtkomsttoken som du kan använda hello javascript ur.</span><span class="sxs-lookup"><span data-stu-id="c30de-134">Once hello embed token is created, you will then use hello access key toogenerate hello token which you can use from hello javascript perspective.</span></span> <span data-ttu-id="c30de-135">Hej *PowerBIToken klassen* kräver att du installerar hello [Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="c30de-135">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="c30de-136">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="c30de-136">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="c30de-137">**C#-kod**</span><span class="sxs-lookup"><span data-stu-id="c30de-137">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername, roles and scopes are optional.
embedToken = PowerBIToken.CreateReportEmbedToken(workspaceCollectionName, workspaceId, reportId, rlsUsername, roles, scopes);

var token = embedToken.Generate("{access key}");
```

### <a name="adding-permission-scopes-tooembed-tokens"></a><span data-ttu-id="c30de-138">Lägga till behörigheten scope tooembed token</span><span class="sxs-lookup"><span data-stu-id="c30de-138">Adding permission scopes tooembed tokens</span></span>

<span data-ttu-id="c30de-139">När du använder Embed token kan kanske du vill toorestrict användning av hello-resurser som du ger åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="c30de-139">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="c30de-140">Därför kan du generera en token med begränsade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="c30de-140">For this reason, you can generate a token with scoped permissions.</span></span> <span data-ttu-id="c30de-141">Mer information finns i [scope](power-bi-embedded-app-token-flow.md#scopes)</span><span class="sxs-lookup"><span data-stu-id="c30de-141">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes)</span></span>

## <a name="embed-using-javascript"></a><span data-ttu-id="c30de-142">Bädda in med hjälp av JavaScript</span><span class="sxs-lookup"><span data-stu-id="c30de-142">Embed using JavaScript</span></span>

<span data-ttu-id="c30de-143">När du har hello åtkomst-token och hello rapport-id, kan vi bädda in hello rapport med hjälp av JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c30de-143">After you have hello access token and hello report id, we can embed hello report using JavaScript.</span></span> <span data-ttu-id="c30de-144">Detta kräver att du installerar hello nuget [Power BI JavaScript paketet](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="c30de-144">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="c30de-145">Hej embedUrl blir https://embedded.powerbi.com/appTokenReportEmbed.</span><span class="sxs-lookup"><span data-stu-id="c30de-145">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="c30de-146">Du kan använda hello [JavaScript rapporten bäddas in provet](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funktioner.</span><span class="sxs-lookup"><span data-stu-id="c30de-146">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="c30de-147">Det ger även kodexempel för hello olika åtgärder som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="c30de-147">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="c30de-148">**NuGet-paketet installation**</span><span class="sxs-lookup"><span data-stu-id="c30de-148">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="c30de-149">**JavaScript-kod**</span><span class="sxs-lookup"><span data-stu-id="c30de-149">**JavaScript code**</span></span>

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

### <a name="set-hello-size-of-embedded-elements"></a><span data-ttu-id="c30de-150">Ange hello storleken på inbäddade element</span><span class="sxs-lookup"><span data-stu-id="c30de-150">Set hello size of embedded elements</span></span>

<span data-ttu-id="c30de-151">hello rapporten bäddas automatiskt baserat på hello storleken på dess behållare.</span><span class="sxs-lookup"><span data-stu-id="c30de-151">hello report will automatically be embedded based on hello size of its container.</span></span> <span data-ttu-id="c30de-152">toooverride hello standardstorlek hello bäddar in helt enkelt lägga till en CSS-klassen attribut eller infogade format för bredd och höjd.</span><span class="sxs-lookup"><span data-stu-id="c30de-152">toooverride hello default size of hello embeds simply add a CSS class attribute or inline styles for width & height.</span></span>

## <a name="see-also"></a><span data-ttu-id="c30de-153">Se även</span><span class="sxs-lookup"><span data-stu-id="c30de-153">See also</span></span>

[<span data-ttu-id="c30de-154">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="c30de-154">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="c30de-155">Autentisering och auktorisering i Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="c30de-155">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="c30de-156">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="c30de-156">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="c30de-157">Inbäddat exempel med JavaScript</span><span class="sxs-lookup"><span data-stu-id="c30de-157">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="c30de-158">Power BI JavaScript-paket</span><span class="sxs-lookup"><span data-stu-id="c30de-158">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="c30de-159">[Power BI API NuGet-paketet](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut paketet](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span><span class="sxs-lookup"><span data-stu-id="c30de-159">[Power BI API NuGet package](https://www.nuget.org/profiles/powerbi)
[Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)</span></span>  
[<span data-ttu-id="c30de-160">PowerBI CSharp Git Repo</span><span class="sxs-lookup"><span data-stu-id="c30de-160">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="c30de-161">PowerBI-nod Git Repo</span><span class="sxs-lookup"><span data-stu-id="c30de-161">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="c30de-162">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="c30de-162">More questions?</span></span> [<span data-ttu-id="c30de-163">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="c30de-163">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
