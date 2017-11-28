---
title: aaaAzure Active Directory v2.0-slutpunkten | Microsoft Docs
description: "En introduktion toobuilding appar med både Account och Azure Active Directory-inloggning."
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
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="e1983-103">Logga in Account & Azure AD-användare i en enda app</span><span class="sxs-lookup"><span data-stu-id="e1983-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="e1983-104">I hello efter en app-utvecklare som vill toosupport både personliga Microsoft-konton och arbetskonton från Azure Active Directory har nödvändiga toointegrate med två separata system.</span><span class="sxs-lookup"><span data-stu-id="e1983-104">In hello past, an app developer who wanted toosupport both personal Microsoft accounts and work accounts from Azure Active Directory was required toointegrate with two separate systems.</span></span>  <span data-ttu-id="e1983-105">Hej **Azure AD v2.0-slutpunkten** introducerar en ny autentisering-API-version som du kan använda toosign i båda typerna av konton med hjälp av en enkel integrering.</span><span class="sxs-lookup"><span data-stu-id="e1983-105">hello **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you toosign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="e1983-106">Appar som använder hello v2.0-slutpunkten kan också använda REST API: er från hello [Microsoft Graph](https://graph.microsoft.io) med hjälp av någon typ av konto.</span><span class="sxs-lookup"><span data-stu-id="e1983-106">Apps that use hello v2.0 endpoint can also consume REST APIs from hello [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="e1983-107">Komma igång</span><span class="sxs-lookup"><span data-stu-id="e1983-107">Getting Started</span></span>
<span data-ttu-id="e1983-108">Välj din favoritplattform från följande lista toobuild hello en app med våra bibliotek med öppen källkod och ramverk.</span><span class="sxs-lookup"><span data-stu-id="e1983-108">Choose your favorite platform from hello following list toobuild an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="e1983-109">Du kan också använda vår OAuth 2.0 & OpenID Connect protokollet dokumentationen toosend och ta emot protokollmeddelanden direkt utan att använda ett auth-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="e1983-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation toosend & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="e1983-110">Senaste nytt</span><span class="sxs-lookup"><span data-stu-id="e1983-110">What's New</span></span>
<span data-ttu-id="e1983-111">hello information här kan vara användbart i förstå vad & vad är inte möjlig med hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e1983-111">hello information here will be useful in understanding what is & what isn't possible with hello v2.0 endpoint.</span></span>

* <span data-ttu-id="e1983-112">Lär dig mer om hello [typer av appar som du kan skapa med hello v2.0-slutpunkten](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="e1983-112">Learn about hello [types of apps you can build with hello v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="e1983-113">Förstå hello [begränsningar, restriktioner och villkor](active-directory-v2-limitations.md) med hello v2.0-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e1983-113">Understand hello [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with hello v2.0 endpoint.</span></span>
* <span data-ttu-id="e1983-114">Checka ut den här översikten för hello v2.0-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="e1983-114">Check out this overview video for hello v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="e1983-115">Referens</span><span class="sxs-lookup"><span data-stu-id="e1983-115">Reference</span></span>
<span data-ttu-id="e1983-116">Dessa länkar kan vara användbart för att utforska hello-plattformen i mer detalj:</span><span class="sxs-lookup"><span data-stu-id="e1983-116">These links will be useful for exploring hello platform in depth:</span></span>

* [<span data-ttu-id="e1983-117">Referens för v2.0-protokollet</span><span class="sxs-lookup"><span data-stu-id="e1983-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="e1983-118">Referens för v2.0-Token</span><span class="sxs-lookup"><span data-stu-id="e1983-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="e1983-119">Referens för v2.0-bibliotek</span><span class="sxs-lookup"><span data-stu-id="e1983-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="e1983-120">Scope och medgivande i hello v2.0-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="e1983-120">Scopes and Consent in hello v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="e1983-121">hello Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="e1983-121">hello Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="e1983-122">Hjälp och support</span><span class="sxs-lookup"><span data-stu-id="e1983-122">Help & Support</span></span>
<span data-ttu-id="e1983-123">Dessa är hello bästa platser tooget hjälp med att utveckla på Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e1983-123">These are hello best places tooget help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="e1983-124">Stackspill`azure-active-directory` och `adal`taggar</span><span class="sxs-lookup"><span data-stu-id="e1983-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="e1983-125">Feedback om Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e1983-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="e1983-126">Om du bara behöver toosign arbets- och skolkonton konton från Azure Active Directory ska du starta med vår [Azure AD-guide för utvecklare](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="e1983-126">If you only need toosign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="e1983-127">hello v2.0-slutpunkten är avsedd att användas av utvecklare som uttryckligen måste toosign i personliga Microsoft-konton.</span><span class="sxs-lookup"><span data-stu-id="e1983-127">hello v2.0 endpoint is intended for use by developers who explicitly need toosign in Microsoft personal accounts.</span></span>

