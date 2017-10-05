---
title: Azure Active Directory v2.0-slutpunkten | Microsoft Docs
description: "En introduktion till att skapa appar med både Account och Azure Active Directory-inloggning."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1e286044fb1a1b367fcac2dc14c47f68d5ed120d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="f4e56-103">Logga in Account & Azure AD-användare i en enda app</span><span class="sxs-lookup"><span data-stu-id="f4e56-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="f4e56-104">Tidigare var en app-utvecklare som ville stöder både personliga Microsoft-konton och fungerar konton från Azure Active Directory krävs för att integrera med två separata system.</span><span class="sxs-lookup"><span data-stu-id="f4e56-104">In the past, an app developer who wanted to support both personal Microsoft accounts and work accounts from Azure Active Directory was required to integrate with two separate systems.</span></span>  <span data-ttu-id="f4e56-105">Den **Azure AD v2.0-slutpunkten** introducerar en ny autentisering-API-version som du kan logga in båda typer av konton med hjälp av en enkel integrering.</span><span class="sxs-lookup"><span data-stu-id="f4e56-105">The **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you to sign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="f4e56-106">Appar som använder v2.0-slutpunkten kan också använda REST API: er från den [Microsoft Graph](https://graph.microsoft.io) med hjälp av någon typ av konto.</span><span class="sxs-lookup"><span data-stu-id="f4e56-106">Apps that use the v2.0 endpoint can also consume REST APIs from the [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="f4e56-107">Komma igång</span><span class="sxs-lookup"><span data-stu-id="f4e56-107">Getting Started</span></span>
<span data-ttu-id="f4e56-108">Välj din favoritplattform i listan nedan för att skapa en app med våra bibliotek med öppen källkod och ramverk.</span><span class="sxs-lookup"><span data-stu-id="f4e56-108">Choose your favorite platform from the following list to build an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="f4e56-109">Du kan också använda vår dokumentation för OAuth 2.0 & OpenID Connect-protokollet för att skicka och ta emot protokollmeddelanden direkt utan att använda ett auth-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="f4e56-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation to send & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="f4e56-110">Senaste nytt</span><span class="sxs-lookup"><span data-stu-id="f4e56-110">What's New</span></span>
<span data-ttu-id="f4e56-111">Den här informationen kan vara användbart i förstå vad & vad är inte möjlig med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="f4e56-111">The information here will be useful in understanding what is & what isn't possible with the v2.0 endpoint.</span></span>

* <span data-ttu-id="f4e56-112">Lär dig mer om den [typer av appar som du kan skapa med v2.0-slutpunkten](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="f4e56-112">Learn about the [types of apps you can build with the v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="f4e56-113">Förstå de [begränsningar, restriktioner och villkor](active-directory-v2-limitations.md) med v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="f4e56-113">Understand the [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with the v2.0 endpoint.</span></span>
* <span data-ttu-id="f4e56-114">Checka ut den här översikten för v2.0-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="f4e56-114">Check out this overview video for the v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="f4e56-115">Referens</span><span class="sxs-lookup"><span data-stu-id="f4e56-115">Reference</span></span>
<span data-ttu-id="f4e56-116">Dessa länkar kan vara användbart för att utforska plattform i mer detalj:</span><span class="sxs-lookup"><span data-stu-id="f4e56-116">These links will be useful for exploring the platform in depth:</span></span>

* [<span data-ttu-id="f4e56-117">Referens för v2.0-protokollet</span><span class="sxs-lookup"><span data-stu-id="f4e56-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="f4e56-118">Referens för v2.0-Token</span><span class="sxs-lookup"><span data-stu-id="f4e56-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="f4e56-119">Referens för v2.0-bibliotek</span><span class="sxs-lookup"><span data-stu-id="f4e56-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="f4e56-120">Scope och medgivande i v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="f4e56-120">Scopes and Consent in the v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="f4e56-121">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="f4e56-121">The Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="f4e56-122">Hjälp och support</span><span class="sxs-lookup"><span data-stu-id="f4e56-122">Help & Support</span></span>
<span data-ttu-id="f4e56-123">Det här är ett bra sätt att få hjälp med utveckling i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f4e56-123">These are the best places to get help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="f4e56-124">Stackspill`azure-active-directory` och `adal`taggar</span><span class="sxs-lookup"><span data-stu-id="f4e56-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="f4e56-125">Feedback om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f4e56-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="f4e56-126">Om du bara behöver logga in arbets- och skolkonton konton från Azure Active Directory ska du starta med vår [Azure AD-guide för utvecklare](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f4e56-126">If you only need to sign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="f4e56-127">V2.0-slutpunkten är avsedd att användas av utvecklare som uttryckligen måste du logga in personliga Microsoft-konton.</span><span class="sxs-lookup"><span data-stu-id="f4e56-127">The v2.0 endpoint is intended for use by developers who explicitly need to sign in Microsoft personal accounts.</span></span>

