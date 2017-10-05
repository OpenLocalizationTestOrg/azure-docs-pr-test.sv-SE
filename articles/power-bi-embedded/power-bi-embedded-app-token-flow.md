---
title: Autentisering och auktorisering med Power BI Embedded
description: Autentisering och auktorisering med Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 93027f0f5467e0b21c47bbcbc84c67cdd50b26b8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a><span data-ttu-id="7c1f1-103">Autentisering och auktorisering med Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="7c1f1-103">Authenticating and authorizing with Power BI Embedded</span></span>

<span data-ttu-id="7c1f1-104">Power BI Embedded tjänsten använder **nycklar** och **Apptoken** för autentisering och auktorisering, i stället för att explicit slutanvändarens autentisering.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-104">The Power BI Embedded service uses **Keys** and **App Tokens** for authentication and authorization, instead of explicit end-user authentication.</span></span> <span data-ttu-id="7c1f1-105">I den här modellen hanterar programmet autentisering och auktorisering för dina slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-105">In this model, your application manages authentication and authorization for your end-users.</span></span> <span data-ttu-id="7c1f1-106">Vid behov, din app skapar och skickar Apptoken som talar om vår tjänst för att återge den begärda rapporten.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-106">When necessary, your app creates and sends the App Tokens that tells our service to render the requested report.</span></span> <span data-ttu-id="7c1f1-107">Den här designen kräver inte din app att använda Azure Active Directory för autentisering och auktorisering, även om du fortfarande kan.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-107">This design doesn't require your app to use Azure Active Directory for user authentication and authorization, although you still can.</span></span>

## <a name="two-ways-to-authenticate"></a><span data-ttu-id="7c1f1-108">Två sätt att autentisera</span><span class="sxs-lookup"><span data-stu-id="7c1f1-108">Two ways to authenticate</span></span>

<span data-ttu-id="7c1f1-109">**Nyckeln** -du kan använda nycklar för alla Power BI Embedded REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-109">**Key** -  You can use keys for all Power BI Embedded REST API calls.</span></span> <span data-ttu-id="7c1f1-110">Nycklarna kan hittas i den **Azure-portalen** genom att klicka på **alla inställningar** och sedan **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-110">The keys can be found in the **Azure portal** by clicking on **All settings** and then **Access keys**.</span></span> <span data-ttu-id="7c1f1-111">Behandla alltid din nyckel som om det vore ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-111">Always treat your key as if it were a password.</span></span> <span data-ttu-id="7c1f1-112">Dessa nycklar har behörighet att göra REST API: er anropa för en viss arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-112">These keys have permissions to make any REST API call on a particular workspace collection.</span></span>

<span data-ttu-id="7c1f1-113">Om du vill använda en nyckel på ett REST-anrop, lägger du till följande authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="7c1f1-113">To use a key on a REST call, add the following authorization header:</span></span>            

    Authorization: AppKey {your key}

<span data-ttu-id="7c1f1-114">**Apptoken** -apptoken som används för inbäddning begäranden.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-114">**App token** - App tokens are used for all embedding requests.</span></span> <span data-ttu-id="7c1f1-115">De har utformats för att köra klientsidan, så att de är begränsad till en enstaka rapport och det är bäst att ange en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-115">They’re designed to be run client-side, so they're restricted to a single report and it’s best practice to set an expiration time.</span></span>

<span data-ttu-id="7c1f1-116">Apptoken är en JWT (JSON Web Token) som är signerat av någon av dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-116">App tokens are a JWT (JSON Web Token) that is signed by one of your keys.</span></span>

<span data-ttu-id="7c1f1-117">App-token kan innehålla följande krav:</span><span class="sxs-lookup"><span data-stu-id="7c1f1-117">Your app token can contain the following claims:</span></span>

| <span data-ttu-id="7c1f1-118">Begär</span><span class="sxs-lookup"><span data-stu-id="7c1f1-118">Claim</span></span> | <span data-ttu-id="7c1f1-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7c1f1-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7c1f1-120">**ver**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-120">**ver**</span></span> |<span data-ttu-id="7c1f1-121">Versionen av app-token.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-121">The version of the app token.</span></span> <span data-ttu-id="7c1f1-122">0.2.0 är den aktuella versionen.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-122">0.2.0 is the current version.</span></span> |
| <span data-ttu-id="7c1f1-123">**eller**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-123">**aud**</span></span> |<span data-ttu-id="7c1f1-124">Den avsedda mottagaren av token.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-124">The intended recipient of the token.</span></span> <span data-ttu-id="7c1f1-125">Power BI Embedded användas: ”https://analysis.windows.net/powerbi/api”.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-125">For Power BI Embedded use: “https://analysis.windows.net/powerbi/api”.</span></span> |
| <span data-ttu-id="7c1f1-126">**ISS**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-126">**iss**</span></span> |<span data-ttu-id="7c1f1-127">En sträng som anger programmet som utfärdade token.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-127">A string indicating the application which issued the token.</span></span> |
| <span data-ttu-id="7c1f1-128">**typ**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-128">**type**</span></span> |<span data-ttu-id="7c1f1-129">Typen av apptoken som håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-129">The type of app token which is being created.</span></span> <span data-ttu-id="7c1f1-130">Aktuella den enda typ som stöds är **bädda in**.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-130">Current the only supported type is **embed**.</span></span> |
| <span data-ttu-id="7c1f1-131">**nätverkskonfiguration**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-131">**wcn**</span></span> |<span data-ttu-id="7c1f1-132">Arbetsytan samlingsnamn token utfärdas för.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-132">Workspace collection name the token is being issued for.</span></span> |
| <span data-ttu-id="7c1f1-133">**WID**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-133">**wid**</span></span> |<span data-ttu-id="7c1f1-134">Arbetsyte-ID token utfärdas för.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-134">Workspace ID the token is being issued for.</span></span> |
| <span data-ttu-id="7c1f1-135">**RID**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-135">**rid**</span></span> |<span data-ttu-id="7c1f1-136">ID token utfärdas för en rapport.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-136">Report ID the token is being issued for.</span></span> |
| <span data-ttu-id="7c1f1-137">**användarnamnet** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-137">**username** (optional)</span></span> |<span data-ttu-id="7c1f1-138">Används med RLS. är detta en sträng som kan hjälpa dig att identifiera användaren när du använder RLS regler.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-138">Used with RLS, this is a string that can help identify the user when applying RLS rules.</span></span> |
| <span data-ttu-id="7c1f1-139">**roller** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-139">**roles** (optional)</span></span> |<span data-ttu-id="7c1f1-140">En sträng som innehåller rollerna kan välja vid tillämpning av säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-140">A string containing the roles to select when applying Row Level Security rules.</span></span> <span data-ttu-id="7c1f1-141">Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-141">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="7c1f1-142">**SCP** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-142">**scp** (optional)</span></span> |<span data-ttu-id="7c1f1-143">En sträng som innehåller behörigheter scope.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-143">A string containing the permissions scopes.</span></span> <span data-ttu-id="7c1f1-144">Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-144">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="7c1f1-145">**EXP** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-145">**exp** (optional)</span></span> |<span data-ttu-id="7c1f1-146">Anger den tidpunkt då token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-146">Indicates the time in which the token will expire.</span></span> <span data-ttu-id="7c1f1-147">Dessa ska skickas i som Unix tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-147">These should be passed in as Unix timestamps.</span></span> |
| <span data-ttu-id="7c1f1-148">**NBF** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-148">**nbf** (optional)</span></span> |<span data-ttu-id="7c1f1-149">Anger den tidpunkt som startar token är giltig.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-149">Indicates the time in which the token starts being valid.</span></span> <span data-ttu-id="7c1f1-150">Dessa ska skickas i som Unix tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-150">These should be passed in as Unix timestamps.</span></span> |

<span data-ttu-id="7c1f1-151">Ett exempel apptoken ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="7c1f1-151">A sample app token will look like this:</span></span>

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

<span data-ttu-id="7c1f1-152">När avkodas, ser den ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="7c1f1-152">When decoded, it will look something like this:</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

<span data-ttu-id="7c1f1-153">Det finns metoder i SDK: er som gör det enklare att skapa apptokens.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-153">There are methods available within the SDKs that make creation of apptokens easier.</span></span> <span data-ttu-id="7c1f1-154">Till exempel för .NET du titta på den [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klass och [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoder.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-154">For example, for .NET you can look at the [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) class and the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methods.</span></span>

<span data-ttu-id="7c1f1-155">För .NET-SDK kan du gå till [scope](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span><span class="sxs-lookup"><span data-stu-id="7c1f1-155">For the .NET SDK, you can refer to [Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span></span>

## <a name="scopes"></a><span data-ttu-id="7c1f1-156">Scope</span><span class="sxs-lookup"><span data-stu-id="7c1f1-156">Scopes</span></span>

<span data-ttu-id="7c1f1-157">När du använder Embed token, kanske du vill begränsa användningen av du ger åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-157">When using Embed tokens, you may want to restrict usage of the resources you give access to.</span></span> <span data-ttu-id="7c1f1-158">Därför kan du generera en token med begränsade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-158">For this reason, you can generate a token with scoped permissions.</span></span>

<span data-ttu-id="7c1f1-159">Följande är tillgängliga scope för Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-159">The following are the available scopes for Power BI Embedded.</span></span>

|<span data-ttu-id="7c1f1-160">Omfång</span><span class="sxs-lookup"><span data-stu-id="7c1f1-160">Scope</span></span>|<span data-ttu-id="7c1f1-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7c1f1-161">Description</span></span>|
|---|---|
|<span data-ttu-id="7c1f1-162">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-162">Dataset.Read</span></span>|<span data-ttu-id="7c1f1-163">Ger behörighet att läsa den angivna datamängden.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-163">Provides permission to read the specified dataset.</span></span>|
|<span data-ttu-id="7c1f1-164">Dataset.Write</span><span class="sxs-lookup"><span data-stu-id="7c1f1-164">Dataset.Write</span></span>|<span data-ttu-id="7c1f1-165">Ger behörighet att skriva till den angivna datamängden.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-165">Provides permission to write to the specified dataset.</span></span>|
|<span data-ttu-id="7c1f1-166">Dataset.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="7c1f1-166">Dataset.ReadWrite</span></span>|<span data-ttu-id="7c1f1-167">Ger behörighet att läsa och skriva till datamängden som är angiven.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-167">Provides permission to read and write to the specificed dataset.</span></span>|
|<span data-ttu-id="7c1f1-168">Report.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-168">Report.Read</span></span>|<span data-ttu-id="7c1f1-169">Ger behörighet att visa den angivna rapporten.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-169">Provides permission to view the specified report.</span></span>|
|<span data-ttu-id="7c1f1-170">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="7c1f1-170">Report.ReadWrite</span></span>|<span data-ttu-id="7c1f1-171">Ger behörighet att visa och redigera den angivna rapporten.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-171">Provides permission to view and edit the specified report.</span></span>|
|<span data-ttu-id="7c1f1-172">Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="7c1f1-172">Workspace.Report.Create</span></span>|<span data-ttu-id="7c1f1-173">Ger behörighet att skapa en ny rapport på den angivna arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-173">Provides permission to create a new report within the specified workspace.</span></span>|
|<span data-ttu-id="7c1f1-174">Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="7c1f1-174">Workspace.Report.Copy</span></span>|<span data-ttu-id="7c1f1-175">Ger behörighet att klona en befintlig rapport på den angivna arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-175">Provides permission to clone an existing report within the specified workspace.</span></span>|

<span data-ttu-id="7c1f1-176">Du kan ange flera scope med ett blanksteg mellan scope som liknar följande.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-176">You can supply multiple scopes by using a space between the scopes like the following.</span></span>

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

<span data-ttu-id="7c1f1-177">**Nödvändiga anspråk - scope**</span><span class="sxs-lookup"><span data-stu-id="7c1f1-177">**Required claims - scopes**</span></span>

<span data-ttu-id="7c1f1-178">SCP: {scopesClaim} scopesClaim kan vara en sträng eller en matris med strängar som påpekas tillåtna behörigheter till arbetsytan resurser (rapport, Dataset, etc.)</span><span class="sxs-lookup"><span data-stu-id="7c1f1-178">scp: {scopesClaim} scopesClaim can be either a string or array of strings, noting the allowed permissions to workspace resources (Report, Dataset, etc.)</span></span>

<span data-ttu-id="7c1f1-179">Avkodade token, med scope som definierats, skulle se ut ungefär så här.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-179">A decoded token, with scopes defined, would look similar to the following.</span></span>

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a><span data-ttu-id="7c1f1-180">Åtgärder och scope</span><span class="sxs-lookup"><span data-stu-id="7c1f1-180">Operations and scopes</span></span>

|<span data-ttu-id="7c1f1-181">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="7c1f1-181">Operation</span></span>|<span data-ttu-id="7c1f1-182">Målresurs</span><span class="sxs-lookup"><span data-stu-id="7c1f1-182">Target resource</span></span>|<span data-ttu-id="7c1f1-183">Token behörigheter</span><span class="sxs-lookup"><span data-stu-id="7c1f1-183">Token permissions</span></span>|
|---|---|---|
|<span data-ttu-id="7c1f1-184">Skapa en ny rapport utifrån en datamängd för (i minnet).</span><span class="sxs-lookup"><span data-stu-id="7c1f1-184">Create (in-memory) a new report based on a dataset.</span></span>|<span data-ttu-id="7c1f1-185">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="7c1f1-185">Dataset</span></span>|<span data-ttu-id="7c1f1-186">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-186">Dataset.Read</span></span>|
|<span data-ttu-id="7c1f1-187">Skapa en ny rapport utifrån en datamängd för (i minnet) och spara rapporten.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-187">Create (in-memory) a new report based on a dataset and save the report.</span></span>|<span data-ttu-id="7c1f1-188">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="7c1f1-188">Dataset</span></span>|<span data-ttu-id="7c1f1-189">* Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-189">* Dataset.Read</span></span><br><span data-ttu-id="7c1f1-190">* Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="7c1f1-190">* Workspace.Report.Create</span></span>|
|<span data-ttu-id="7c1f1-191">Visa och utforska/Redigera (i minnet) en befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-191">View and explore/edit (in-memory) an existing report.</span></span> <span data-ttu-id="7c1f1-192">Report.Read innebär Dataset.Read.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-192">Report.Read implies Dataset.Read.</span></span> <span data-ttu-id="7c1f1-193">Detta tillåter inte behörighet för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-193">This will not allow permissions to save edits.</span></span>|<span data-ttu-id="7c1f1-194">Rapport</span><span class="sxs-lookup"><span data-stu-id="7c1f1-194">Report</span></span>|<span data-ttu-id="7c1f1-195">Report.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-195">Report.Read</span></span>|
|<span data-ttu-id="7c1f1-196">Redigera och spara en befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-196">Edit and save an existing report.</span></span>|<span data-ttu-id="7c1f1-197">Rapport</span><span class="sxs-lookup"><span data-stu-id="7c1f1-197">Report</span></span>|<span data-ttu-id="7c1f1-198">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="7c1f1-198">Report.ReadWrite</span></span>|
|<span data-ttu-id="7c1f1-199">Spara en kopia av en rapport (Spara som).</span><span class="sxs-lookup"><span data-stu-id="7c1f1-199">Save a copy of a report (Save As).</span></span>|<span data-ttu-id="7c1f1-200">Rapport</span><span class="sxs-lookup"><span data-stu-id="7c1f1-200">Report</span></span>|<span data-ttu-id="7c1f1-201">* Report.Read</span><span class="sxs-lookup"><span data-stu-id="7c1f1-201">* Report.Read</span></span><br><span data-ttu-id="7c1f1-202">* Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="7c1f1-202">* Workspace.Report.Copy</span></span>|

## <a name="heres-how-the-flow-works"></a><span data-ttu-id="7c1f1-203">Här är hur flödet fungerar</span><span class="sxs-lookup"><span data-stu-id="7c1f1-203">Here's how the flow works</span></span>
1. <span data-ttu-id="7c1f1-204">Kopiera API-nycklar till ditt program.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-204">Copy the API keys to your application.</span></span> <span data-ttu-id="7c1f1-205">Du kan hämta nycklarna i **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-205">You can get the keys in **Azure Portal**.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. <span data-ttu-id="7c1f1-206">Token Assert ett anspråk och har en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-206">Token asserts a claim and has an expiration time.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. <span data-ttu-id="7c1f1-207">Token hämtar signeras med ett API-åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-207">Token gets signed with an API access keys.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. <span data-ttu-id="7c1f1-208">Användarförfrågningar om att visa en rapport.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-208">User requests to view a report.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. <span data-ttu-id="7c1f1-209">Token verifieras med en API-åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-209">Token is validated with an API access keys.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. <span data-ttu-id="7c1f1-210">Power BI Embedded skickar en rapport till användaren.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-210">Power BI Embedded sends a report to user.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

<span data-ttu-id="7c1f1-211">Efter **Power BI Embedded** skickar en rapport till användaren, användaren kan visa rapporten i din egen app.</span><span class="sxs-lookup"><span data-stu-id="7c1f1-211">After **Power BI Embedded** sends a report to the user, the user can view the report in your custom app.</span></span> <span data-ttu-id="7c1f1-212">Till exempel om du har importerat den [analysera försäljning Data PBIX exempel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), exempelwebbapp skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7c1f1-212">For example, if you imported the [Analyzing Sales Data PBIX sample](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), the sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a><span data-ttu-id="7c1f1-213">Se även</span><span class="sxs-lookup"><span data-stu-id="7c1f1-213">See Also</span></span>

[<span data-ttu-id="7c1f1-214">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="7c1f1-214">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="7c1f1-215">Komma igång med Microsoft Power BI Embedded exemplet</span><span class="sxs-lookup"><span data-stu-id="7c1f1-215">Get started with Microsoft Power BI Embedded sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="7c1f1-216">Vanliga scenarier för Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="7c1f1-216">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="7c1f1-217">Kom igång med Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="7c1f1-217">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)  
[<span data-ttu-id="7c1f1-218">PowerBI CSharp Git Repo</span><span class="sxs-lookup"><span data-stu-id="7c1f1-218">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
<span data-ttu-id="7c1f1-219">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="7c1f1-219">More questions?</span></span> [<span data-ttu-id="7c1f1-220">Försök med Power BI Community</span><span class="sxs-lookup"><span data-stu-id="7c1f1-220">Try the Power BI Community</span></span>](http://community.powerbi.com/)

