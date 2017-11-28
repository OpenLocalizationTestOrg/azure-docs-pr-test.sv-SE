---
title: "Azure AD v2 Android komma igång - introduktion | Microsoft Docs"
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
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a><span data-ttu-id="bec18-103">Anropa Microsoft Graph API från en Android-app</span><span class="sxs-lookup"><span data-stu-id="bec18-103">Call the Microsoft Graph API from an Android app</span></span>

<span data-ttu-id="bec18-104">Den här guiden visar hur en ursprunglig Android-program kan få en åtkomst-token och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2 slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="bec18-104">This guide demonstrates how a native Android application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="bec18-105">I slutet av den här guiden kommer programmet att kunna anropa en skyddad API med hjälp av personliga konton (inklusive outlook.com och live.com) samt arbets- och skolkonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="bec18-105">At the end of this guide, your application will be able to call a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

### <a name="how-this-sample-works"></a><span data-ttu-id="bec18-106">Hur det här exemplet fungerar</span><span class="sxs-lookup"><span data-stu-id="bec18-106">How this sample works</span></span>
![Hur det här exemplet fungerar](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

<span data-ttu-id="bec18-108">Exempel som skapats av den här guiden bygger på ett scenario där en Android-program används för att fråga en webb-API som accepterar token från Azure Active Directory v2 slutpunkten – i det här fallet Microsoft Graph API.</span><span class="sxs-lookup"><span data-stu-id="bec18-108">The sample created by this guide is based on a scenario where an Android application is used to query a Web API that accepts tokens from Azure Active Directory v2 endpoint – in this case, Microsoft Graph API.</span></span> <span data-ttu-id="bec18-109">I det här scenariot läggs en token på HTTP-förfrågningar via Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="bec18-109">For this scenario, a token is added to HTTP requests via the Authorization header.</span></span> <span data-ttu-id="bec18-110">Token förvärv och förnyelse hanteras av Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="bec18-110">Token acquisition and renewal is handled by the Microsoft Authentication Library (MSAL).</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="bec18-111">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="bec18-111">Pre-requisites</span></span>
* <span data-ttu-id="bec18-112">Den interaktiva installationen fokuserar på Android Studio, men några andra utvecklingsmiljö för Android-program är också tillåtet.</span><span class="sxs-lookup"><span data-stu-id="bec18-112">This guided setup is focused on Android Studio, but any other Android application development environment is also acceptable.</span></span> 
* <span data-ttu-id="bec18-113">Android SDK 21 eller senare krävs (SDK 25 rekommenderas).</span><span class="sxs-lookup"><span data-stu-id="bec18-113">Android SDK 21 or newer is required (SDK 25 is recommended).</span></span>
* <span data-ttu-id="bec18-114">Google Chrome eller en webbläsare med hjälp av anpassade flikar krävs för den här versionen av Microsoft Authentication Library (MSAL) för Android.</span><span class="sxs-lookup"><span data-stu-id="bec18-114">Google Chrome or a web browser using Custom Tabs is required for this release of Microsoft Authentication Library (MSAL) for Android.</span></span>

> <span data-ttu-id="bec18-115">Obs: Google Chrome ingår inte i Visual Studio-emulatorn för Android.</span><span class="sxs-lookup"><span data-stu-id="bec18-115">Note: Google Chrome is not included on Visual Studio Emulator for Android.</span></span> <span data-ttu-id="bec18-116">Vi rekommenderar att du kan testa den här koden på en Emulator med API 25 eller en bild med API 21 eller senare som har med Google Chrome installerat.</span><span class="sxs-lookup"><span data-stu-id="bec18-116">We recommend you to test this code on an Emulator with API 25 or an image with API 21 or newer that has with Google Chrome installed.</span></span>


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a><span data-ttu-id="bec18-117">Hantera token förvärv för att komma åt skyddade webb-API</span><span class="sxs-lookup"><span data-stu-id="bec18-117">How to handle token acquisition to access a protected Web API</span></span>

<span data-ttu-id="bec18-118">När användaren autentiseras får exempelprogrammet en token som kan användas för att fråga Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="bec18-118">After the user authenticates, the sample application receives a token that can be used to query Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="bec18-119">API: er som Microsoft Graph kräver en åtkomst-token för att tillåta åtkomst till specifika resurser – till exempel att läsa en användares profil, åtkomst till användarens kalender eller skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="bec18-119">APIs such as Microsoft Graph require an access token to allow accessing specific resources – for example, to read a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="bec18-120">Programmet kan begära en åtkomst-token med MSAL åtkomst till dessa resurser genom att ange API-scope.</span><span class="sxs-lookup"><span data-stu-id="bec18-120">Your application can request an access token using MSAL to access these resources by specifying API scopes.</span></span> <span data-ttu-id="bec18-121">Åtkomsttoken läggs sedan till http-Authorization-huvud för varje anrop som görs mot den skyddade resursen.</span><span class="sxs-lookup"><span data-stu-id="bec18-121">This access token is then added to the HTTP Authorization header for every call made against the protected resource.</span></span> 

<span data-ttu-id="bec18-122">MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.</span><span class="sxs-lookup"><span data-stu-id="bec18-122">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>

### <a name="libraries"></a><span data-ttu-id="bec18-123">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="bec18-123">Libraries</span></span>

<span data-ttu-id="bec18-124">Den här guiden använder följande bibliotek:</span><span class="sxs-lookup"><span data-stu-id="bec18-124">This guide uses the following libraries:</span></span>

|<span data-ttu-id="bec18-125">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="bec18-125">Library</span></span>|<span data-ttu-id="bec18-126">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bec18-126">Description</span></span>|
|---|---|
|[<span data-ttu-id="bec18-127">com.microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="bec18-127">com.microsoft.identity.client</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal)|<span data-ttu-id="bec18-128">Microsoft Authentication Library (MSAL)</span><span class="sxs-lookup"><span data-stu-id="bec18-128">Microsoft Authentication Library (MSAL)</span></span>|
