---
title: "aaaAzure AD v2 Android komma igång - introduktion | Microsoft Docs"
description: "Hur en Android-app kan få en åtkomst-token och anropa API: erna som kräver åtkomst-token från Azure Active Directory v2 slutpunkten eller Microsoft Graph API"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 7c76ab8bbf1a6c934ab672cccf85ae82f03f601a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a><span data-ttu-id="b8503-103">Anropa hello Microsoft Graph API från en Android-app</span><span class="sxs-lookup"><span data-stu-id="b8503-103">Call hello Microsoft Graph API from an Android app</span></span>

<span data-ttu-id="b8503-104">Den här guiden visar hur en ursprunglig Android-program kan få en åtkomst-token och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2 slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="b8503-104">This guide demonstrates how a native Android application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="b8503-105">Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b8503-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

### <a name="how-this-sample-works"></a><span data-ttu-id="b8503-106">Hur det här exemplet fungerar</span><span class="sxs-lookup"><span data-stu-id="b8503-106">How this sample works</span></span>
![Hur det här exemplet fungerar](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

<span data-ttu-id="b8503-108">hello-exempel som skapats av den här guiden bygger på ett scenario där en Android-App är används tooquery ett webb-API som accepterar token från Azure Active Directory v2 slutpunkten – i det här fallet Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="b8503-108">hello sample created by this guide is based on a scenario where an Android application is used tooquery a Web API that accepts tokens from Azure Active Directory v2 endpoint – in this case, Microsoft Graph API.</span></span> <span data-ttu-id="b8503-109">I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="b8503-109">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="b8503-110">Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="b8503-110">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="b8503-111">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="b8503-111">Pre-requisites</span></span>
* <span data-ttu-id="b8503-112">Den interaktiva installationen fokuserar på Android Studio, men några andra utvecklingsmiljö för Android-program är också tillåtet.</span><span class="sxs-lookup"><span data-stu-id="b8503-112">This guided setup is focused on Android Studio, but any other Android application development environment is also acceptable.</span></span> 
* <span data-ttu-id="b8503-113">Android SDK 21 eller senare krävs (SDK 25 rekommenderas).</span><span class="sxs-lookup"><span data-stu-id="b8503-113">Android SDK 21 or newer is required (SDK 25 is recommended).</span></span>
* <span data-ttu-id="b8503-114">Google Chrome eller en webbläsare med hjälp av anpassade flikar krävs för den här versionen av Microsoft Authentication Library (MSAL) för Android.</span><span class="sxs-lookup"><span data-stu-id="b8503-114">Google Chrome or a web browser using Custom Tabs is required for this release of Microsoft Authentication Library (MSAL) for Android.</span></span>

> <span data-ttu-id="b8503-115">Obs: Google Chrome ingår inte i Visual Studio-emulatorn för Android.</span><span class="sxs-lookup"><span data-stu-id="b8503-115">Note: Google Chrome is not included on Visual Studio Emulator for Android.</span></span> <span data-ttu-id="b8503-116">Vi rekommenderar att du tootest koden på en Emulator med API 25 eller en bild med API 21 eller senare som har med Google Chrome installerat.</span><span class="sxs-lookup"><span data-stu-id="b8503-116">We recommend you tootest this code on an Emulator with API 25 or an image with API 21 or newer that has with Google Chrome installed.</span></span>


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a><span data-ttu-id="b8503-117">Hur token toohandle förvärv tooaccess skyddade webb-API</span><span class="sxs-lookup"><span data-stu-id="b8503-117">How toohandle token acquisition tooaccess a protected Web API</span></span>

<span data-ttu-id="b8503-118">När hello användare autentiseras får hello exempelprogrammet en token som kan använda tooquery Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="b8503-118">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="b8503-119">API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="b8503-119">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="b8503-120">Programmet kan begära en åtkomst-token med MSAL tooaccess dessa resurser genom att ange API-scope.</span><span class="sxs-lookup"><span data-stu-id="b8503-120">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="b8503-121">Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="b8503-121">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="b8503-122">MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.</span><span class="sxs-lookup"><span data-stu-id="b8503-122">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>

### <a name="libraries"></a><span data-ttu-id="b8503-123">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="b8503-123">Libraries</span></span>

<span data-ttu-id="b8503-124">Den här guiden använder hello följande bibliotek:</span><span class="sxs-lookup"><span data-stu-id="b8503-124">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="b8503-125">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="b8503-125">Library</span></span>|<span data-ttu-id="b8503-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="b8503-126">Description</span></span>|
|---|---|
|[<span data-ttu-id="b8503-127">com.microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="b8503-127">com.microsoft.identity.client</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal)|<span data-ttu-id="b8503-128">Microsoft Authentication Library (MSAL)</span><span class="sxs-lookup"><span data-stu-id="b8503-128">Microsoft Authentication Library (MSAL)</span></span>|
