---
title: aaaAzure AD v2 JS SPA interaktiv installation - introduktion | Microsoft Docs
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a><span data-ttu-id="393c2-103">Anropa hello Microsoft Graph API från ett JavaScript enda sidan program (SPA)</span><span class="sxs-lookup"><span data-stu-id="393c2-103">Call hello Microsoft Graph API from a JavaScript Single Page Application (SPA)</span></span>

<span data-ttu-id="393c2-104">Den här guiden visar hur en JavaScript enda sidan program (SPA) kan logga in personliga arbete och skolkonton, hämta en åtkomst-token och anropa hello Microsoft Graph API eller andra API: er som kräver åtkomst-token från hello Azure Active Directory v2 slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="393c2-104">This guide demonstrates how a JavaScript Single Page Application (SPA) can sign in personal, work and school accounts, get an access token, and call hello Microsoft Graph API or other APIs that require access tokens from hello Azure Active Directory v2 endpoint.</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="393c2-105">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="393c2-105">How this guide works</span></span>

![Så här fungerar hello sample-appen som genererats av den här guiden](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="393c2-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="393c2-107">More Information</span></span>

<span data-ttu-id="393c2-108">hello exempelprogram som skapats av den här guiden kan en JavaScript SPA tooquery hello Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="393c2-108">hello sample application created by this guide enables a JavaScript SPA tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="393c2-109">I det här scenariot är när en användare loggar in, en åtkomst-token begärda och tillagda tooHTTP begäranden via hello authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="393c2-109">For this scenario, after a user signs-in, an access token is requested and added tooHTTP requests via hello authorization header.</span></span> <span data-ttu-id="393c2-110">Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="393c2-110">Token acquisition and renewal are handled by hello Microsoft Authentication Library (MSAL).</span></span>

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a><span data-ttu-id="393c2-111">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="393c2-111">Libraries</span></span>

<span data-ttu-id="393c2-112">Den här guiden använder hello följande bibliotek:</span><span class="sxs-lookup"><span data-stu-id="393c2-112">This guide uses hello following library:</span></span>

|<span data-ttu-id="393c2-113">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="393c2-113">Library</span></span>|<span data-ttu-id="393c2-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="393c2-114">Description</span></span>|
|---|---|
|[<span data-ttu-id="393c2-115">msal.js</span><span class="sxs-lookup"><span data-stu-id="393c2-115">msal.js</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js)|<span data-ttu-id="393c2-116">Microsoft-Autentiseringsbibliotek för JavaScript-förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="393c2-116">Microsoft Authentication Library for JavaScript Preview</span></span>|

> [!NOTE]
> <span data-ttu-id="393c2-117">*msal.js* mål hello *Azure Active Directory v2 endpoint* -som aktiverar personliga, skolan och arbetsrelaterade konton toosign i och hämta token.</span><span class="sxs-lookup"><span data-stu-id="393c2-117">*msal.js* targets hello *Azure Active Directory v2 endpoint* - which enables personal, school and work accounts toosign in and acquire tokens.</span></span> <span data-ttu-id="393c2-118">Hej *Azure Active Directory v2 endpoint* har [vissa begränsningar](..\active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="393c2-118">hello *Azure Active Directory v2 endpoint* has [some limitations](..\active-directory-v2-limitations.md).</span></span> <span data-ttu-id="393c2-119">Om du bara vill skolan och arbetsrelaterade konton använder *adal.js* och hello *V1 endpoint*.</span><span class="sxs-lookup"><span data-stu-id="393c2-119">If you are interested only in school and work accounts, use *adal.js* and hello *V1 endpoint*.</span></span> <span data-ttu-id="393c2-120">toounderstand skillnaderna mellan hello v1 och v2 slutpunkter läsa hello [v1 v2 jämförelse](..\active-directory-v2-compare.md).</span><span class="sxs-lookup"><span data-stu-id="393c2-120">toounderstand differences between hello v1 and v2 endpoints read hello [v1-v2 comparison](..\active-directory-v2-compare.md).</span></span>

<!--end-collapse-->
