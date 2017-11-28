---
title: Azure AD v2 JS SPA interaktiv installation - introduktion | Microsoft Docs
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
ms.openlocfilehash: 3d195d0d67f8f82c9450ffd93767917698addee3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a><span data-ttu-id="cb11e-103">Anropa Microsoft Graph API från ett JavaScript Single-Page Application (SPA)</span><span class="sxs-lookup"><span data-stu-id="cb11e-103">Call the Microsoft Graph API from a JavaScript Single Page Application (SPA)</span></span>

<span data-ttu-id="cb11e-104">Den här guiden visar hur en JavaScript enda sidan program (SPA) kan logga in personliga arbete och skolkonton, hämta en åtkomst-token och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från slutpunkten för Azure Active Directory-v2.</span><span class="sxs-lookup"><span data-stu-id="cb11e-104">This guide demonstrates how a JavaScript Single Page Application (SPA) can sign in personal, work and school accounts, get an access token, and call the Microsoft Graph API or other APIs that require access tokens from the Azure Active Directory v2 endpoint.</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="cb11e-105">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="cb11e-105">How this guide works</span></span>

![Så här fungerar sample-appen som genererats av den här guiden](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="cb11e-107">Mer information</span><span class="sxs-lookup"><span data-stu-id="cb11e-107">More Information</span></span>

<span data-ttu-id="cb11e-108">Det exempelprogram som skapats av den här guiden kan en JavaScript-SPA frågar Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="cb11e-108">The sample application created by this guide enables a JavaScript SPA to query the Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="cb11e-109">För det här scenariot, när en användare loggar in, begärt en åtkomst-token och lägga till HTTP-förfrågningar via authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="cb11e-109">For this scenario, after a user signs-in, an access token is requested and added to HTTP requests via the authorization header.</span></span> <span data-ttu-id="cb11e-110">Token förvärv och förnyelse hanteras av Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="cb11e-110">Token acquisition and renewal are handled by the Microsoft Authentication Library (MSAL).</span></span>

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a><span data-ttu-id="cb11e-111">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="cb11e-111">Libraries</span></span>

<span data-ttu-id="cb11e-112">Den här guiden använder följande biblioteket:</span><span class="sxs-lookup"><span data-stu-id="cb11e-112">This guide uses the following library:</span></span>

|<span data-ttu-id="cb11e-113">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="cb11e-113">Library</span></span>|<span data-ttu-id="cb11e-114">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cb11e-114">Description</span></span>|
|---|---|
|[<span data-ttu-id="cb11e-115">msal.js</span><span class="sxs-lookup"><span data-stu-id="cb11e-115">msal.js</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js)|<span data-ttu-id="cb11e-116">Microsoft-Autentiseringsbibliotek för JavaScript-förhandsgranskning</span><span class="sxs-lookup"><span data-stu-id="cb11e-116">Microsoft Authentication Library for JavaScript Preview</span></span>|

> [!NOTE]
> <span data-ttu-id="cb11e-117">*msal.js* mål i *Azure Active Directory v2 endpoint* -vilket innebär att personliga, skolan och arbetsrelaterade konton att logga in och hämta token.</span><span class="sxs-lookup"><span data-stu-id="cb11e-117">*msal.js* targets the *Azure Active Directory v2 endpoint* - which enables personal, school and work accounts to sign in and acquire tokens.</span></span> <span data-ttu-id="cb11e-118">Den *Azure Active Directory v2 endpoint* har [vissa begränsningar](..\active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="cb11e-118">The *Azure Active Directory v2 endpoint* has [some limitations](..\active-directory-v2-limitations.md).</span></span> <span data-ttu-id="cb11e-119">Om du bara vill skolan och arbetsrelaterade konton använder *adal.js* och *V1 endpoint*.</span><span class="sxs-lookup"><span data-stu-id="cb11e-119">If you are interested only in school and work accounts, use *adal.js* and the *V1 endpoint*.</span></span> <span data-ttu-id="cb11e-120">Att förstå skillnaderna mellan v1 och v2-slutpunkter läsa den [v1 v2 jämförelse](..\active-directory-v2-compare.md).</span><span class="sxs-lookup"><span data-stu-id="cb11e-120">To understand differences between the v1 and v2 endpoints read the [v1-v2 comparison](..\active-directory-v2-compare.md).</span></span>

<!--end-collapse-->
