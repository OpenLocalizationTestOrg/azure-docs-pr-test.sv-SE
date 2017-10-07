---
title: aaaAuthenticating och auktorisera med Power BI Embedded
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
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a><span data-ttu-id="91550-103">Autentisering och auktorisering med Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="91550-103">Authenticating and authorizing with Power BI Embedded</span></span>

<span data-ttu-id="91550-104">hello Power BI Embedded tjänsten använder **nycklar** och **Apptoken** för autentisering och auktorisering, i stället för att explicit slutanvändarens autentisering.</span><span class="sxs-lookup"><span data-stu-id="91550-104">hello Power BI Embedded service uses **Keys** and **App Tokens** for authentication and authorization, instead of explicit end-user authentication.</span></span> <span data-ttu-id="91550-105">I den här modellen hanterar programmet autentisering och auktorisering för dina slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="91550-105">In this model, your application manages authentication and authorization for your end-users.</span></span> <span data-ttu-id="91550-106">Vid behov, din app skapar och skickar hello Apptoken som talar om vår tjänst toorender hello begärda rapporten.</span><span class="sxs-lookup"><span data-stu-id="91550-106">When necessary, your app creates and sends hello App Tokens that tells our service toorender hello requested report.</span></span> <span data-ttu-id="91550-107">Den här designen kräver din app toouse Azure Active Directory för autentisering och auktorisering, även om du fortfarande kan.</span><span class="sxs-lookup"><span data-stu-id="91550-107">This design doesn't require your app toouse Azure Active Directory for user authentication and authorization, although you still can.</span></span>

## <a name="two-ways-tooauthenticate"></a><span data-ttu-id="91550-108">Två sätt tooauthenticate</span><span class="sxs-lookup"><span data-stu-id="91550-108">Two ways tooauthenticate</span></span>

<span data-ttu-id="91550-109">**Nyckeln** -du kan använda nycklar för alla Power BI Embedded REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="91550-109">**Key** -  You can use keys for all Power BI Embedded REST API calls.</span></span> <span data-ttu-id="91550-110">hello nycklar finns i hello **Azure-portalen** genom att klicka på **alla inställningar** och sedan **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="91550-110">hello keys can be found in hello **Azure portal** by clicking on **All settings** and then **Access keys**.</span></span> <span data-ttu-id="91550-111">Behandla alltid din nyckel som om det vore ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="91550-111">Always treat your key as if it were a password.</span></span> <span data-ttu-id="91550-112">Dessa nycklar har behörighet toomake REST API: er anropa för en viss arbetsytesamling.</span><span class="sxs-lookup"><span data-stu-id="91550-112">These keys have permissions toomake any REST API call on a particular workspace collection.</span></span>

<span data-ttu-id="91550-113">toouse en nyckel på ett REST-anrop, Lägg till hello följande authorization-huvud:</span><span class="sxs-lookup"><span data-stu-id="91550-113">toouse a key on a REST call, add hello following authorization header:</span></span>            

    Authorization: AppKey {your key}

<span data-ttu-id="91550-114">**Apptoken** -apptoken som används för inbäddning begäranden.</span><span class="sxs-lookup"><span data-stu-id="91550-114">**App token** - App tokens are used for all embedding requests.</span></span> <span data-ttu-id="91550-115">De är utformad toobe kör klientsidan så att de är begränsade tooa enkel rapport och det är bästa praxis tooset en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="91550-115">They’re designed toobe run client-side, so they're restricted tooa single report and it’s best practice tooset an expiration time.</span></span>

<span data-ttu-id="91550-116">Apptoken är en JWT (JSON Web Token) som är signerat av någon av dina nycklar.</span><span class="sxs-lookup"><span data-stu-id="91550-116">App tokens are a JWT (JSON Web Token) that is signed by one of your keys.</span></span>

<span data-ttu-id="91550-117">App-token kan innehålla hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="91550-117">Your app token can contain hello following claims:</span></span>

| <span data-ttu-id="91550-118">Begär</span><span class="sxs-lookup"><span data-stu-id="91550-118">Claim</span></span> | <span data-ttu-id="91550-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91550-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="91550-120">**ver**</span><span class="sxs-lookup"><span data-stu-id="91550-120">**ver**</span></span> |<span data-ttu-id="91550-121">hello version av hello app-token.</span><span class="sxs-lookup"><span data-stu-id="91550-121">hello version of hello app token.</span></span> <span data-ttu-id="91550-122">0.2.0 är hello aktuella versionen.</span><span class="sxs-lookup"><span data-stu-id="91550-122">0.2.0 is hello current version.</span></span> |
| <span data-ttu-id="91550-123">**eller**</span><span class="sxs-lookup"><span data-stu-id="91550-123">**aud**</span></span> |<span data-ttu-id="91550-124">hello avsedda mottagaren av hello-token.</span><span class="sxs-lookup"><span data-stu-id="91550-124">hello intended recipient of hello token.</span></span> <span data-ttu-id="91550-125">Power BI Embedded användas: ”https://analysis.windows.net/powerbi/api”.</span><span class="sxs-lookup"><span data-stu-id="91550-125">For Power BI Embedded use: “https://analysis.windows.net/powerbi/api”.</span></span> |
| <span data-ttu-id="91550-126">**ISS**</span><span class="sxs-lookup"><span data-stu-id="91550-126">**iss**</span></span> |<span data-ttu-id="91550-127">En sträng som anger hello-program som har utfärdat hello-token.</span><span class="sxs-lookup"><span data-stu-id="91550-127">A string indicating hello application which issued hello token.</span></span> |
| <span data-ttu-id="91550-128">**typ**</span><span class="sxs-lookup"><span data-stu-id="91550-128">**type**</span></span> |<span data-ttu-id="91550-129">hello typ av apptoken som håller på att skapas.</span><span class="sxs-lookup"><span data-stu-id="91550-129">hello type of app token which is being created.</span></span> <span data-ttu-id="91550-130">Den aktuella hello stöds endast typen är **bädda in**.</span><span class="sxs-lookup"><span data-stu-id="91550-130">Current hello only supported type is **embed**.</span></span> |
| <span data-ttu-id="91550-131">**nätverkskonfiguration**</span><span class="sxs-lookup"><span data-stu-id="91550-131">**wcn**</span></span> |<span data-ttu-id="91550-132">Arbetsytan samling namn hello token utfärdas för.</span><span class="sxs-lookup"><span data-stu-id="91550-132">Workspace collection name hello token is being issued for.</span></span> |
| <span data-ttu-id="91550-133">**WID**</span><span class="sxs-lookup"><span data-stu-id="91550-133">**wid**</span></span> |<span data-ttu-id="91550-134">Arbetsytan ID hello token utfärdas för.</span><span class="sxs-lookup"><span data-stu-id="91550-134">Workspace ID hello token is being issued for.</span></span> |
| <span data-ttu-id="91550-135">**RID**</span><span class="sxs-lookup"><span data-stu-id="91550-135">**rid**</span></span> |<span data-ttu-id="91550-136">ID hello token utfärdas för en rapport.</span><span class="sxs-lookup"><span data-stu-id="91550-136">Report ID hello token is being issued for.</span></span> |
| <span data-ttu-id="91550-137">**användarnamnet** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="91550-137">**username** (optional)</span></span> |<span data-ttu-id="91550-138">Används med RLS. är detta en sträng som kan hjälpa dig att identifiera hello användare när du använder RLS regler.</span><span class="sxs-lookup"><span data-stu-id="91550-138">Used with RLS, this is a string that can help identify hello user when applying RLS rules.</span></span> |
| <span data-ttu-id="91550-139">**roller** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="91550-139">**roles** (optional)</span></span> |<span data-ttu-id="91550-140">En sträng som innehåller hello roller tooselect när du använder regler för säkerhet på radnivå.</span><span class="sxs-lookup"><span data-stu-id="91550-140">A string containing hello roles tooselect when applying Row Level Security rules.</span></span> <span data-ttu-id="91550-141">Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris.</span><span class="sxs-lookup"><span data-stu-id="91550-141">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="91550-142">**SCP** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="91550-142">**scp** (optional)</span></span> |<span data-ttu-id="91550-143">En sträng som innehåller hello behörigheter scope.</span><span class="sxs-lookup"><span data-stu-id="91550-143">A string containing hello permissions scopes.</span></span> <span data-ttu-id="91550-144">Om skicka fler än en roll ska de skickas som en förekomster av textsträngen-matris.</span><span class="sxs-lookup"><span data-stu-id="91550-144">If passing more than one role, they should be passed as a sting array.</span></span> |
| <span data-ttu-id="91550-145">**EXP** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="91550-145">**exp** (optional)</span></span> |<span data-ttu-id="91550-146">Anger hello tidpunkt i vilka hello token upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="91550-146">Indicates hello time in which hello token will expire.</span></span> <span data-ttu-id="91550-147">Dessa ska skickas i som Unix tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="91550-147">These should be passed in as Unix timestamps.</span></span> |
| <span data-ttu-id="91550-148">**NBF** (valfritt)</span><span class="sxs-lookup"><span data-stu-id="91550-148">**nbf** (optional)</span></span> |<span data-ttu-id="91550-149">Anger hello tidpunkt i vilka hello startar token är giltig.</span><span class="sxs-lookup"><span data-stu-id="91550-149">Indicates hello time in which hello token starts being valid.</span></span> <span data-ttu-id="91550-150">Dessa ska skickas i som Unix tidsstämplar.</span><span class="sxs-lookup"><span data-stu-id="91550-150">These should be passed in as Unix timestamps.</span></span> |

<span data-ttu-id="91550-151">Ett exempel apptoken ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="91550-151">A sample app token will look like this:</span></span>

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

<span data-ttu-id="91550-152">När avkodas, ser den ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="91550-152">When decoded, it will look something like this:</span></span>

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

<span data-ttu-id="91550-153">Det finns metoder i hello SDK: er som gör det enklare att skapa apptokens.</span><span class="sxs-lookup"><span data-stu-id="91550-153">There are methods available within hello SDKs that make creation of apptokens easier.</span></span> <span data-ttu-id="91550-154">Till exempel för .NET titta på hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) klass och hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metoder.</span><span class="sxs-lookup"><span data-stu-id="91550-154">For example, for .NET you can look at hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) class and hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) methods.</span></span>

<span data-ttu-id="91550-155">För hello .NET SDK, kan du läsa för[scope](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span><span class="sxs-lookup"><span data-stu-id="91550-155">For hello .NET SDK, you can refer too[Scopes](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).</span></span>

## <a name="scopes"></a><span data-ttu-id="91550-156">Scope</span><span class="sxs-lookup"><span data-stu-id="91550-156">Scopes</span></span>

<span data-ttu-id="91550-157">När du använder Embed token kan kanske du vill toorestrict användning av hello-resurser som du ger åtkomst till.</span><span class="sxs-lookup"><span data-stu-id="91550-157">When using Embed tokens, you may want toorestrict usage of hello resources you give access to.</span></span> <span data-ttu-id="91550-158">Därför kan du generera en token med begränsade behörigheter.</span><span class="sxs-lookup"><span data-stu-id="91550-158">For this reason, you can generate a token with scoped permissions.</span></span>

<span data-ttu-id="91550-159">hello följande är hello tillgängliga scope för Power BI Embedded.</span><span class="sxs-lookup"><span data-stu-id="91550-159">hello following are hello available scopes for Power BI Embedded.</span></span>

|<span data-ttu-id="91550-160">Omfång</span><span class="sxs-lookup"><span data-stu-id="91550-160">Scope</span></span>|<span data-ttu-id="91550-161">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="91550-161">Description</span></span>|
|---|---|
|<span data-ttu-id="91550-162">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="91550-162">Dataset.Read</span></span>|<span data-ttu-id="91550-163">Ger behörighet tooread hello angetts dataset.</span><span class="sxs-lookup"><span data-stu-id="91550-163">Provides permission tooread hello specified dataset.</span></span>|
|<span data-ttu-id="91550-164">Dataset.Write</span><span class="sxs-lookup"><span data-stu-id="91550-164">Dataset.Write</span></span>|<span data-ttu-id="91550-165">Ger behörighet toowrite toohello angetts dataset.</span><span class="sxs-lookup"><span data-stu-id="91550-165">Provides permission toowrite toohello specified dataset.</span></span>|
|<span data-ttu-id="91550-166">Dataset.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="91550-166">Dataset.ReadWrite</span></span>|<span data-ttu-id="91550-167">Ger behörighet tooread och skriva toohello angiven dataset.</span><span class="sxs-lookup"><span data-stu-id="91550-167">Provides permission tooread and write toohello specificed dataset.</span></span>|
|<span data-ttu-id="91550-168">Report.Read</span><span class="sxs-lookup"><span data-stu-id="91550-168">Report.Read</span></span>|<span data-ttu-id="91550-169">Ger behörighet tooview hello angivna rapporten.</span><span class="sxs-lookup"><span data-stu-id="91550-169">Provides permission tooview hello specified report.</span></span>|
|<span data-ttu-id="91550-170">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="91550-170">Report.ReadWrite</span></span>|<span data-ttu-id="91550-171">Ger behörighet tooview och redigera hello angivna rapporten.</span><span class="sxs-lookup"><span data-stu-id="91550-171">Provides permission tooview and edit hello specified report.</span></span>|
|<span data-ttu-id="91550-172">Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="91550-172">Workspace.Report.Create</span></span>|<span data-ttu-id="91550-173">Ger behörighet toocreate en ny rapport i hello angivna arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="91550-173">Provides permission toocreate a new report within hello specified workspace.</span></span>|
|<span data-ttu-id="91550-174">Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="91550-174">Workspace.Report.Copy</span></span>|<span data-ttu-id="91550-175">Ger behörighet tooclone en befintlig rapport i hello angivna arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="91550-175">Provides permission tooclone an existing report within hello specified workspace.</span></span>|

<span data-ttu-id="91550-176">Du kan ange flera scope med ett blanksteg mellan hello scope som hello nedan.</span><span class="sxs-lookup"><span data-stu-id="91550-176">You can supply multiple scopes by using a space between hello scopes like hello following.</span></span>

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

<span data-ttu-id="91550-177">**Nödvändiga anspråk - scope**</span><span class="sxs-lookup"><span data-stu-id="91550-177">**Required claims - scopes**</span></span>

<span data-ttu-id="91550-178">SCP: {scopesClaim} scopesClaim kan vara en sträng eller en matris med strängar som påpekas hello behörigheter tooworkspace resurser (rapport, Dataset, etc.)</span><span class="sxs-lookup"><span data-stu-id="91550-178">scp: {scopesClaim} scopesClaim can be either a string or array of strings, noting hello allowed permissions tooworkspace resources (Report, Dataset, etc.)</span></span>

<span data-ttu-id="91550-179">En avkodade token med scope som definierats, skulle se ut ungefär toohello följande.</span><span class="sxs-lookup"><span data-stu-id="91550-179">A decoded token, with scopes defined, would look similar toohello following.</span></span>

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

### <a name="operations-and-scopes"></a><span data-ttu-id="91550-180">Åtgärder och scope</span><span class="sxs-lookup"><span data-stu-id="91550-180">Operations and scopes</span></span>

|<span data-ttu-id="91550-181">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="91550-181">Operation</span></span>|<span data-ttu-id="91550-182">Målresurs</span><span class="sxs-lookup"><span data-stu-id="91550-182">Target resource</span></span>|<span data-ttu-id="91550-183">Token behörigheter</span><span class="sxs-lookup"><span data-stu-id="91550-183">Token permissions</span></span>|
|---|---|---|
|<span data-ttu-id="91550-184">Skapa en ny rapport utifrån en datamängd för (i minnet).</span><span class="sxs-lookup"><span data-stu-id="91550-184">Create (in-memory) a new report based on a dataset.</span></span>|<span data-ttu-id="91550-185">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="91550-185">Dataset</span></span>|<span data-ttu-id="91550-186">Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="91550-186">Dataset.Read</span></span>|
|<span data-ttu-id="91550-187">Skapa en ny rapport utifrån en datamängd för (i minnet) och spara hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="91550-187">Create (in-memory) a new report based on a dataset and save hello report.</span></span>|<span data-ttu-id="91550-188">Datauppsättning</span><span class="sxs-lookup"><span data-stu-id="91550-188">Dataset</span></span>|<span data-ttu-id="91550-189">* Dataset.Read</span><span class="sxs-lookup"><span data-stu-id="91550-189">* Dataset.Read</span></span><br><span data-ttu-id="91550-190">* Workspace.Report.Create</span><span class="sxs-lookup"><span data-stu-id="91550-190">* Workspace.Report.Create</span></span>|
|<span data-ttu-id="91550-191">Visa och utforska/Redigera (i minnet) en befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="91550-191">View and explore/edit (in-memory) an existing report.</span></span> <span data-ttu-id="91550-192">Report.Read innebär Dataset.Read.</span><span class="sxs-lookup"><span data-stu-id="91550-192">Report.Read implies Dataset.Read.</span></span> <span data-ttu-id="91550-193">Detta tillåter inte behörigheter toosave redigeringar.</span><span class="sxs-lookup"><span data-stu-id="91550-193">This will not allow permissions toosave edits.</span></span>|<span data-ttu-id="91550-194">Rapport</span><span class="sxs-lookup"><span data-stu-id="91550-194">Report</span></span>|<span data-ttu-id="91550-195">Report.Read</span><span class="sxs-lookup"><span data-stu-id="91550-195">Report.Read</span></span>|
|<span data-ttu-id="91550-196">Redigera och spara en befintlig rapport.</span><span class="sxs-lookup"><span data-stu-id="91550-196">Edit and save an existing report.</span></span>|<span data-ttu-id="91550-197">Rapport</span><span class="sxs-lookup"><span data-stu-id="91550-197">Report</span></span>|<span data-ttu-id="91550-198">Report.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="91550-198">Report.ReadWrite</span></span>|
|<span data-ttu-id="91550-199">Spara en kopia av en rapport (Spara som).</span><span class="sxs-lookup"><span data-stu-id="91550-199">Save a copy of a report (Save As).</span></span>|<span data-ttu-id="91550-200">Rapport</span><span class="sxs-lookup"><span data-stu-id="91550-200">Report</span></span>|<span data-ttu-id="91550-201">* Report.Read</span><span class="sxs-lookup"><span data-stu-id="91550-201">* Report.Read</span></span><br><span data-ttu-id="91550-202">* Workspace.Report.Copy</span><span class="sxs-lookup"><span data-stu-id="91550-202">* Workspace.Report.Copy</span></span>|

## <a name="heres-how-hello-flow-works"></a><span data-ttu-id="91550-203">Här är hur hello flödet fungerar</span><span class="sxs-lookup"><span data-stu-id="91550-203">Here's how hello flow works</span></span>
1. <span data-ttu-id="91550-204">Kopiera hello API nycklar tooyour-program.</span><span class="sxs-lookup"><span data-stu-id="91550-204">Copy hello API keys tooyour application.</span></span> <span data-ttu-id="91550-205">Du kan hämta hello nycklar i **Azure Portal**.</span><span class="sxs-lookup"><span data-stu-id="91550-205">You can get hello keys in **Azure Portal**.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. <span data-ttu-id="91550-206">Token Assert ett anspråk och har en förfallotid.</span><span class="sxs-lookup"><span data-stu-id="91550-206">Token asserts a claim and has an expiration time.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. <span data-ttu-id="91550-207">Token hämtar signeras med ett API-åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="91550-207">Token gets signed with an API access keys.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. <span data-ttu-id="91550-208">Användaren begär tooview en rapport.</span><span class="sxs-lookup"><span data-stu-id="91550-208">User requests tooview a report.</span></span>
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. <span data-ttu-id="91550-209">Token verifieras med en API-åtkomstnycklar.</span><span class="sxs-lookup"><span data-stu-id="91550-209">Token is validated with an API access keys.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. <span data-ttu-id="91550-210">Power BI Embedded skickar en rapport toouser.</span><span class="sxs-lookup"><span data-stu-id="91550-210">Power BI Embedded sends a report toouser.</span></span>
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

<span data-ttu-id="91550-211">Efter **Power BI Embedded** skickar en toohello användare, hello användare kan visa hello rapport i din egen app.</span><span class="sxs-lookup"><span data-stu-id="91550-211">After **Power BI Embedded** sends a report toohello user, hello user can view hello report in your custom app.</span></span> <span data-ttu-id="91550-212">Till exempel om du har importerat hello [analysera försäljning Data PBIX exempel](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello exempelwebbapp skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="91550-212">For example, if you imported hello [Analyzing Sales Data PBIX sample](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello sample web app would look like this:</span></span>

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a><span data-ttu-id="91550-213">Se även</span><span class="sxs-lookup"><span data-stu-id="91550-213">See Also</span></span>

[<span data-ttu-id="91550-214">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="91550-214">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="91550-215">Komma igång med Microsoft Power BI Embedded exemplet</span><span class="sxs-lookup"><span data-stu-id="91550-215">Get started with Microsoft Power BI Embedded sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="91550-216">Vanliga scenarier för Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="91550-216">Common Microsoft Power BI Embedded scenarios</span></span>](power-bi-embedded-scenarios.md)  
[<span data-ttu-id="91550-217">Kom igång med Microsoft Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="91550-217">Get started with Microsoft Power BI Embedded</span></span>](power-bi-embedded-get-started.md)  
[<span data-ttu-id="91550-218">PowerBI CSharp Git Repo</span><span class="sxs-lookup"><span data-stu-id="91550-218">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
<span data-ttu-id="91550-219">Fler frågor?</span><span class="sxs-lookup"><span data-stu-id="91550-219">More questions?</span></span> [<span data-ttu-id="91550-220">Försök hello Power BI-communityn</span><span class="sxs-lookup"><span data-stu-id="91550-220">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

