---
title: "aaaAzure AD v2 Windows Desktop komma igång - introduktion | Microsoft Docs"
description: "Hur Windows Desktop .NET (XAML) program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.openlocfilehash: 89d98fc46190ba9e47b7c3095f91e32eca455fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a><span data-ttu-id="d73ce-103">Anropa hello Microsoft Graph API från en app för Windows-skrivbordet</span><span class="sxs-lookup"><span data-stu-id="d73ce-103">Call hello Microsoft Graph API from a Windows Desktop app</span></span>

<span data-ttu-id="d73ce-104">Den här guiden visar hur en intern Windows Desktop .NET (XAML)-programmet kan få en åtkomsttoken och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d73ce-104">This guide demonstrates how a native Windows Desktop .NET (XAML) application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="d73ce-105">Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d73ce-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

> <span data-ttu-id="d73ce-106">Den här guiden kräver Visual Studio 2015 Update 3 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d73ce-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="d73ce-107">Har det?</span><span class="sxs-lookup"><span data-stu-id="d73ce-107">Don’t have it?</span></span> [<span data-ttu-id="d73ce-108">Hämta Visual Studio 2017 gratis</span><span class="sxs-lookup"><span data-stu-id="d73ce-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a><span data-ttu-id="d73ce-109">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="d73ce-109">How this guide works</span></span>

![Hur den här guiden fungerar](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

<span data-ttu-id="d73ce-111">hello exempelprogram som skapats av den här guiden kan ett program för Windows-skrivbordet tooquery Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d73ce-111">hello sample application created by this guide enables a Windows Desktop Application tooquery Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="d73ce-112">I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud.</span><span class="sxs-lookup"><span data-stu-id="d73ce-112">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="d73ce-113">Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).</span><span class="sxs-lookup"><span data-stu-id="d73ce-113">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="d73ce-114">Hantera token förvärv för att komma åt skyddade webb-API: er</span><span class="sxs-lookup"><span data-stu-id="d73ce-114">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="d73ce-115">När hello användare autentiseras får hello exempelprogrammet en token som kan använda tooquery Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.</span><span class="sxs-lookup"><span data-stu-id="d73ce-115">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="d73ce-116">API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="d73ce-116">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="d73ce-117">Programmet kan begära en åtkomst-token med MSAL tooaccess dessa resurser genom att ange API-scope.</span><span class="sxs-lookup"><span data-stu-id="d73ce-117">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="d73ce-118">Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs.</span><span class="sxs-lookup"><span data-stu-id="d73ce-118">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="d73ce-119">MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.</span><span class="sxs-lookup"><span data-stu-id="d73ce-119">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="d73ce-120">NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="d73ce-120">NuGet Packages</span></span>

<span data-ttu-id="d73ce-121">Den här guiden använder hello följande NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="d73ce-121">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="d73ce-122">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="d73ce-122">Library</span></span>|<span data-ttu-id="d73ce-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d73ce-123">Description</span></span>|
|---|---|
|[<span data-ttu-id="d73ce-124">Microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="d73ce-124">Microsoft.Identity.Client</span></span>](https://www.nuget.org/packages/Microsoft.Identity.Client)|<span data-ttu-id="d73ce-125">Microsoft Authentication Library (MSAL)</span><span class="sxs-lookup"><span data-stu-id="d73ce-125">Microsoft Authentication Library (MSAL)</span></span>|

