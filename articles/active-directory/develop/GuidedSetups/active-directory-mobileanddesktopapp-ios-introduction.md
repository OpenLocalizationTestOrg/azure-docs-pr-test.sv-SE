---
title: "aaaAzure AD v2 iOS komma igång - introduktion | Microsoft Docs"
description: "Hur iOS (Swift)-program kan anropa ett API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: f40aebbb75490912e533aecc7eedfb2b2dcd8c6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a><span data-ttu-id="6a45e-103">Anropa hello Microsoft Graph API från en iOS-app</span><span class="sxs-lookup"><span data-stu-id="6a45e-103">Call hello Microsoft Graph API from an iOS app</span></span>

<span data-ttu-id="6a45e-104">Den här guiden visar hur interna iOS-program (Swift) kan hämta en åtkomst-token och anropa hello Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6a45e-104">This guide demonstrates how a native iOS application (Swift) can get an access token and call hello Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="6a45e-105">Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6a45e-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>

> ### <a name="pre-requisites"></a><span data-ttu-id="6a45e-106">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="6a45e-106">Pre-requisites</span></span>
> - <span data-ttu-id="6a45e-107">XCode 8.x krävs för den här guiden.</span><span class="sxs-lookup"><span data-stu-id="6a45e-107">XCode 8.x is required for this guide.</span></span> <span data-ttu-id="6a45e-108">Du kan hämta XCode [härifrån](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode hämta URL").</span><span class="sxs-lookup"><span data-stu-id="6a45e-108">You can download XCode [from here](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").</span></span>
> - <span data-ttu-id="6a45e-109">[Carthage](https://github.com/Carthage/Carthage) för hantering av paketet</span><span class="sxs-lookup"><span data-stu-id="6a45e-109">[Carthage](https://github.com/Carthage/Carthage) for package management</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="6a45e-110">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="6a45e-110">How this guide works</span></span>

![Hur den här guiden fungerar](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

<span data-ttu-id="6a45e-112">hello exempelprogram som skapats av den här guiden kan en iOS-program tooquery hello Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6a45e-112">hello sample application created by this guide enables an iOS application tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="6a45e-113">I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="6a45e-113">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="6a45e-114">Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="6a45e-114">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="6a45e-115">Hantera token förvärv för att komma åt skyddade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="6a45e-115">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="6a45e-116">När hello användare autentiseras får hello exempelprogrammet en token som kan vara används tooquery hello Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="6a45e-116">After hello user authenticates, hello sample application receives a token that can be used tooquery hello Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="6a45e-117">API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6a45e-117">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="6a45e-118">Programmet kan begära en åtkomst-token som använder MSAL genom att ange API-scope.</span><span class="sxs-lookup"><span data-stu-id="6a45e-118">Your application can request an access token using MSAL by specifying API scopes.</span></span> <span data-ttu-id="6a45e-119">Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="6a45e-119">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span>

<span data-ttu-id="6a45e-120">MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.</span><span class="sxs-lookup"><span data-stu-id="6a45e-120">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="6a45e-121">NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="6a45e-121">NuGet packages</span></span>

<span data-ttu-id="6a45e-122">Den här guiden använder hello följande NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="6a45e-122">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="6a45e-123">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="6a45e-123">Library</span></span>|<span data-ttu-id="6a45e-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6a45e-124">Description</span></span>|
|---|---|
|[<span data-ttu-id="6a45e-125">MSAL.framework</span><span class="sxs-lookup"><span data-stu-id="6a45e-125">MSAL.framework</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|<span data-ttu-id="6a45e-126">Microsoft Authentication Library Preview för iOS</span><span class="sxs-lookup"><span data-stu-id="6a45e-126">Microsoft Authentication Library Preview for iOS</span></span>|

