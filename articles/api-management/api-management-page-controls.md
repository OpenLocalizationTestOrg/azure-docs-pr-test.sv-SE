---
title: aaaAzure API Management kontroller | Microsoft Docs
description: "Läs mer om hello kontroller kan användas i developer portal mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 03e0ac8d-64ff-4e9a-b029-d7be14fb31e3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 1a16a6fce197c0a2e14807ac21e81a9a73b515b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-page-controls"></a><span data-ttu-id="f18e8-103">Kontroller Azure API Management</span><span class="sxs-lookup"><span data-stu-id="f18e8-103">Azure API Management page controls</span></span>
<span data-ttu-id="f18e8-104">Azure API Management ger hello följande kontroller för användning i hello developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-104">Azure API Management provides hello following controls for use in hello developer portal templates.</span></span>  
  
 <span data-ttu-id="f18e8-105">toouse en kontroll placeras hello önskad plats i mallen för hello developer portal.</span><span class="sxs-lookup"><span data-stu-id="f18e8-105">toouse a control, place it in hello desired location in hello developer portal template.</span></span> <span data-ttu-id="f18e8-106">Vissa kontroller, till exempel hello [app åtgärder](#app-actions) styra har parametrar, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="f18e8-106">Some controls, such as hello [app-actions](#app-actions) control, have parameters, as shown in hello following example.</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
 <span data-ttu-id="f18e8-107">hello värden för hello parametrar skickas i som en del av hello datamodellen för hello mallen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-107">hello values for hello parameters are passed in as part of hello data model for hello template.</span></span> <span data-ttu-id="f18e8-108">I de flesta fall kan du endast klistra in hello som exempel för varje kontroll för den toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="f18e8-108">In most cases, you can simply paste in hello provided example for each control for it toowork correctly.</span></span> <span data-ttu-id="f18e8-109">Mer information om parametervärden hello ser hello modellen dataavsnittet för varje mall som en kontroll får användas.</span><span class="sxs-lookup"><span data-stu-id="f18e8-109">For more information on hello parameter values, you can see hello data model section for each template in which a control may be used.</span></span>  
  
 <span data-ttu-id="f18e8-110">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="f18e8-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
## <a name="developer-portal-template-page-controls"></a><span data-ttu-id="f18e8-111">Developer portal mallen kontroller</span><span class="sxs-lookup"><span data-stu-id="f18e8-111">Developer portal template page controls</span></span>  
  
-   [<span data-ttu-id="f18e8-112">App-åtgärder</span><span class="sxs-lookup"><span data-stu-id="f18e8-112">app-actions</span></span>](#app-actions)  
  
-   [<span data-ttu-id="f18e8-113">Basic-inloggning</span><span class="sxs-lookup"><span data-stu-id="f18e8-113">basic-signin</span></span>](#basic-signin)  
  
-   [<span data-ttu-id="f18e8-114">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="f18e8-114">paging-control</span></span>](#paging-control)  
  
-   [<span data-ttu-id="f18e8-115">providers</span><span class="sxs-lookup"><span data-stu-id="f18e8-115">providers</span></span>](#providers)  
  
-   [<span data-ttu-id="f18e8-116">Sök-kontroll</span><span class="sxs-lookup"><span data-stu-id="f18e8-116">search-control</span></span>](#search-control)  
  
-   [<span data-ttu-id="f18e8-117">registrering</span><span class="sxs-lookup"><span data-stu-id="f18e8-117">sign-up</span></span>](#sign-up)  
  
-   [<span data-ttu-id="f18e8-118">prenumerera på knappen</span><span class="sxs-lookup"><span data-stu-id="f18e8-118">subscribe-button</span></span>](#subscribe-button)  
  
-   [<span data-ttu-id="f18e8-119">Avbryt prenumeration</span><span class="sxs-lookup"><span data-stu-id="f18e8-119">subscription-cancel</span></span>](#subscription-cancel)  
  
##  <span data-ttu-id="f18e8-120"><a name="app-actions"></a>App-åtgärder</span><span class="sxs-lookup"><span data-stu-id="f18e8-120"><a name="app-actions"></a> app-actions</span></span>  
 <span data-ttu-id="f18e8-121">Hej `app-actions` kontrollen innehåller ett användargränssnitt för att interagera med program på hello användaren profilsida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-121">hello `app-actions` control provides a user interface for interacting with applications on hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="f18e8-122">![App- &#45; åtgärder kontrollen](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app åtgärder kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-122">![app&#45;actions control](./media/api-management-page-controls/APIM-app-actions-control.png "APIM app-actions control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-123">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-123">Usage</span></span>  
  
```xml  
<app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-124">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-124">Parameters</span></span>  
  
|<span data-ttu-id="f18e8-125">Parameter</span><span class="sxs-lookup"><span data-stu-id="f18e8-125">Parameter</span></span>|<span data-ttu-id="f18e8-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f18e8-126">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="f18e8-127">appId</span><span class="sxs-lookup"><span data-stu-id="f18e8-127">appId</span></span>|<span data-ttu-id="f18e8-128">hello-id för hello program.</span><span class="sxs-lookup"><span data-stu-id="f18e8-128">hello id of hello application.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-129">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-129">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-130">Hej `app-actions` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-130">hello `app-actions` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-131">Program</span><span class="sxs-lookup"><span data-stu-id="f18e8-131">Applications</span></span>](api-management-user-profile-templates.md#Applications)  
  
##  <span data-ttu-id="f18e8-132"><a name="basic-signin"></a>Basic-inloggning</span><span class="sxs-lookup"><span data-stu-id="f18e8-132"><a name="basic-signin"></a> basic-signin</span></span>  
 <span data-ttu-id="f18e8-133">Hej `basic-signin` kontrollen innehåller en kontroll för att samla användare logga in informationen i hello inloggningssidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-133">hello `basic-signin` control provides a control for collecting user sign in information in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="f18e8-134">![grundläggande &#45; inloggning kontrollen](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic signin-kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-134">![basic&#45;signin control](./media/api-management-page-controls/APIM-basic-signin-control.png "APIM basic-signin control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-135">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-135">Usage</span></span>  
  
```xml  
<basic-SignIn></basic-SignIn>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-136">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-136">Parameters</span></span>  
 <span data-ttu-id="f18e8-137">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-137">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-138">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-138">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-139">Hej `basic-signin` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-139">hello `basic-signin` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-140">Logga in</span><span class="sxs-lookup"><span data-stu-id="f18e8-140">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="f18e8-141"><a name="paging-control"></a>växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="f18e8-141"><a name="paging-control"></a> paging-control</span></span>  
 <span data-ttu-id="f18e8-142">Hej `paging-control` tillhandahåller funktioner för sidindelning på developer portal sidor som visar en lista med objekt.</span><span class="sxs-lookup"><span data-stu-id="f18e8-142">hello `paging-control` provides paging functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="f18e8-143">![växling kontrollen](./media/api-management-page-controls/APIM-paging-control.png "APIM sidindelning kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-143">![paging control](./media/api-management-page-controls/APIM-paging-control.png "APIM paging control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-144">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-144">Usage</span></span>  
  
```xml  
<paging-control></paging-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-145">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-145">Parameters</span></span>  
 <span data-ttu-id="f18e8-146">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-146">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-147">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-147">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-148">Hej `paging-control` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-148">hello `paging-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-149">API-lista</span><span class="sxs-lookup"><span data-stu-id="f18e8-149">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="f18e8-150">Problemet lista</span><span class="sxs-lookup"><span data-stu-id="f18e8-150">Issue list</span></span>](api-management-issue-templates.md#IssueList)  
  
-   [<span data-ttu-id="f18e8-151">Produktlista</span><span class="sxs-lookup"><span data-stu-id="f18e8-151">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="f18e8-152"><a name="providers"></a>providers</span><span class="sxs-lookup"><span data-stu-id="f18e8-152"><a name="providers"></a> providers</span></span>  
 <span data-ttu-id="f18e8-153">Hej `providers` kontrollen innehåller en kontroll för val av autentiseringsproviders i hello inloggningssidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-153">hello `providers` control provides a control for selection of authentication providers in hello sign in page in hello developer portal.</span></span>  
  
 <span data-ttu-id="f18e8-154">![providers kontrollen](./media/api-management-page-controls/APIM-providers-control.png "APIM providers kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-154">![providers control](./media/api-management-page-controls/APIM-providers-control.png "APIM providers control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-155">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-155">Usage</span></span>  
  
```xml  
<providers></providers>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-156">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-156">Parameters</span></span>  
 <span data-ttu-id="f18e8-157">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-157">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-158">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-158">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-159">Hej `providers` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-159">hello `providers` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-160">Logga in</span><span class="sxs-lookup"><span data-stu-id="f18e8-160">Sign in</span></span>](api-management-page-templates.md#SignIn)  
  
##  <span data-ttu-id="f18e8-161"><a name="search-control"></a>Sök-kontroll</span><span class="sxs-lookup"><span data-stu-id="f18e8-161"><a name="search-control"></a> search-control</span></span>  
 <span data-ttu-id="f18e8-162">Hej `search-control` tillhandahåller funktioner för sökning på developer portal sidor som visar en lista med objekt.</span><span class="sxs-lookup"><span data-stu-id="f18e8-162">hello `search-control` provides search functionality on developer portal pages that display a list of items.</span></span>  
  
 <span data-ttu-id="f18e8-163">![Sök kontrollen](./media/api-management-page-controls/APIM-search-control.png "APIM sökkontrollen")</span><span class="sxs-lookup"><span data-stu-id="f18e8-163">![search control](./media/api-management-page-controls/APIM-search-control.png "APIM search control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-164">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-164">Usage</span></span>  
  
```xml  
<search-control></search-control>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-165">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-165">Parameters</span></span>  
 <span data-ttu-id="f18e8-166">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-166">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-167">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-167">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-168">Hej `search-control` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-168">hello `search-control` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-169">API-lista</span><span class="sxs-lookup"><span data-stu-id="f18e8-169">API list</span></span>](api-management-api-templates.md#APIList)  
  
-   [<span data-ttu-id="f18e8-170">Produktlista</span><span class="sxs-lookup"><span data-stu-id="f18e8-170">Product list</span></span>](api-management-product-templates.md#ProductList)  
  
##  <span data-ttu-id="f18e8-171"><a name="sign-up"></a>registrering</span><span class="sxs-lookup"><span data-stu-id="f18e8-171"><a name="sign-up"></a> sign-up</span></span>  
 <span data-ttu-id="f18e8-172">Hej `sign-up` kontrollen innehåller en kontroll för att samla in användarens profilinformation i hello-registrering i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-172">hello `sign-up` control provides a control for collecting user profile information in hello sign up page in hello developer portal.</span></span>  
  
 <span data-ttu-id="f18e8-173">![signera &#45; upp kontrollen](./media/api-management-page-controls/APIM-sign-up-control.png "APIM anmälan kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-173">![sign&#45;up control](./media/api-management-page-controls/APIM-sign-up-control.png "APIM sign-up control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-174">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-174">Usage</span></span>  
  
```xml  
<sign-up></sign-up>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-175">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-175">Parameters</span></span>  
 <span data-ttu-id="f18e8-176">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-176">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-177">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-177">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-178">Hej `sign-up` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-178">hello `sign-up` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-179">Registrera sig</span><span class="sxs-lookup"><span data-stu-id="f18e8-179">Sign up</span></span>](api-management-page-templates.md#SignUp)  
  
##  <span data-ttu-id="f18e8-180"><a name="subscribe-button"></a>prenumerera på knappen</span><span class="sxs-lookup"><span data-stu-id="f18e8-180"><a name="subscribe-button"></a> subscribe-button</span></span>  
 <span data-ttu-id="f18e8-181">Hej `subscribe-button` innehåller en kontroll för att prenumerera på en användare tooa produkt.</span><span class="sxs-lookup"><span data-stu-id="f18e8-181">hello `subscribe-button` provides a control for subscribing a user tooa product.</span></span>  
  
 <span data-ttu-id="f18e8-182">![prenumerera &#45; knappkontrollen](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM prenumerera knappen kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-182">![subscribe&#45;button control](./media/api-management-page-controls/APIM-subscribe-button-control.png "APIM subscribe-button control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-183">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-183">Usage</span></span>  
  
```xml  
<subscribe-button></subscribe-button>  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-184">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-184">Parameters</span></span>  
 <span data-ttu-id="f18e8-185">Ingen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-185">None.</span></span>  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-186">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-186">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-187">Hej `subscribe-button` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-187">hello `subscribe-button` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-188">Produkten</span><span class="sxs-lookup"><span data-stu-id="f18e8-188">Product</span></span>](api-management-product-templates.md#Product)  
  
##  <span data-ttu-id="f18e8-189"><a name="subscription-cancel"></a>Avbryt prenumeration</span><span class="sxs-lookup"><span data-stu-id="f18e8-189"><a name="subscription-cancel"></a> subscription-cancel</span></span>  
 <span data-ttu-id="f18e8-190">Hej `subscription-cancel` kontrollen innehåller en kontroll för att avbryta en prenumeration tooa produkt i hello användaren profilsida i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="f18e8-190">hello `subscription-cancel` control provides a control for cancelling a subscription tooa product in hello user profile page in hello developer portal.</span></span>  
  
 <span data-ttu-id="f18e8-191">![prenumerationen &#45; Avbryt kontrollen](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM Avbryt prenumeration kontroll")</span><span class="sxs-lookup"><span data-stu-id="f18e8-191">![subscription&#45;cancel control](./media/api-management-page-controls/APIM-subscription-cancel-control.png "APIM subscription-cancel control")</span></span>  
  
### <a name="usage"></a><span data-ttu-id="f18e8-192">Användning</span><span class="sxs-lookup"><span data-stu-id="f18e8-192">Usage</span></span>  
  
```xml  
<subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }">  
</subscription-cancel>  
  
```  
  
### <a name="parameters"></a><span data-ttu-id="f18e8-193">Parametrar</span><span class="sxs-lookup"><span data-stu-id="f18e8-193">Parameters</span></span>  
  
|<span data-ttu-id="f18e8-194">Parameter</span><span class="sxs-lookup"><span data-stu-id="f18e8-194">Parameter</span></span>|<span data-ttu-id="f18e8-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f18e8-195">Description</span></span>|  
|---------------|-----------------|  
|<span data-ttu-id="f18e8-196">subscriptionId</span><span class="sxs-lookup"><span data-stu-id="f18e8-196">subscriptionId</span></span>|<span data-ttu-id="f18e8-197">hello-id för hello prenumeration toocancel.</span><span class="sxs-lookup"><span data-stu-id="f18e8-197">hello id of hello subscription toocancel.</span></span>|  
|<span data-ttu-id="f18e8-198">CancelUrl</span><span class="sxs-lookup"><span data-stu-id="f18e8-198">cancelUrl</span></span>|<span data-ttu-id="f18e8-199">hello prenumeration avbryta URL.</span><span class="sxs-lookup"><span data-stu-id="f18e8-199">hello subscription cancel URL.</span></span>|  
  
### <a name="developer-portal-templates"></a><span data-ttu-id="f18e8-200">Developer portal mallar</span><span class="sxs-lookup"><span data-stu-id="f18e8-200">Developer portal templates</span></span>  
 <span data-ttu-id="f18e8-201">Hej `subscription-cancel` kontrollen får användas i hello följande developer portal mallar.</span><span class="sxs-lookup"><span data-stu-id="f18e8-201">hello `subscription-cancel` control may be used in hello following developer portal templates.</span></span>  
  
-   [<span data-ttu-id="f18e8-202">Produkten</span><span class="sxs-lookup"><span data-stu-id="f18e8-202">Product</span></span>](api-management-product-templates.md#Product)

## <a name="next-steps"></a><span data-ttu-id="f18e8-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f18e8-203">Next steps</span></span>
<span data-ttu-id="f18e8-204">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f18e8-204">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
