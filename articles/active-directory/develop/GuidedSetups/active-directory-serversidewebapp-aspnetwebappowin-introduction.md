---
title: "aaaAzure AD v2 ASP.NET Web Server komma igång - introduktion | Microsoft Docs"
description: "Implementera Microsoft logga In på en ASP.NET-lösning med ett traditionellt webbläsarbaserade program med OpenID Connect standard"
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
ms.openlocfilehash: d6449926af2bdad24cbc8e91f74885a08f909103
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a><span data-ttu-id="a2326-103">Lägga till inloggning med Microsoft tooan ASP.NET-webbprogram</span><span class="sxs-lookup"><span data-stu-id="a2326-103">Add sign-in with Microsoft tooan ASP.NET web app</span></span>

<span data-ttu-id="a2326-104">Den här guiden visar hur tooimplement logga in med Microsoft med hjälp av en ASP.NET MVC-lösning med en traditionell webbläsarbaserad webbapp med OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="a2326-104">This guide demonstrates how tooimplement sign-in with Microsoft using an ASP.NET MVC solution with a traditional web browser-based application using OpenID Connect.</span></span> 

<span data-ttu-id="a2326-105">Hello slutet av den här guiden, ska ditt program kunna tooaccept logga moduler av personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a2326-105">At hello end of this guide, your application will be able tooaccept sign ins of personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory.</span></span> 

> <span data-ttu-id="a2326-106">Den här guiden kräver Visual Studio 2015 Update 3 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a2326-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="a2326-107">Har det?</span><span class="sxs-lookup"><span data-stu-id="a2326-107">Don’t have it?</span></span>  [<span data-ttu-id="a2326-108">Hämta Visual Studio 2017 gratis</span><span class="sxs-lookup"><span data-stu-id="a2326-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a><span data-ttu-id="a2326-109">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="a2326-109">How this guide works</span></span>

![Hur den här guiden fungerar](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

<span data-ttu-id="a2326-111">Den här guiden bygger på hello scenario där en webbläsare har åtkomst till en ASP.NET-webbplats, begär en användare tooauthenticate via en knapp för inloggning.</span><span class="sxs-lookup"><span data-stu-id="a2326-111">This guide is based on hello scenario where a browser accesses an ASP.NET web site, requesting a user tooauthenticate via a sign-in button.</span></span> <span data-ttu-id="a2326-112">I det här scenariot uppstår de flesta av hello arbete toorender hello webbsida på hello på serversidan.</span><span class="sxs-lookup"><span data-stu-id="a2326-112">In this scenario, most of hello work toorender hello web page occurs on hello server side.</span></span>

## <a name="libraries"></a><span data-ttu-id="a2326-113">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="a2326-113">Libraries</span></span>

<span data-ttu-id="a2326-114">Den här guiden använder hello följande bibliotek:</span><span class="sxs-lookup"><span data-stu-id="a2326-114">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="a2326-115">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="a2326-115">Library</span></span>|<span data-ttu-id="a2326-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a2326-116">Description</span></span>|
|---|---|
|[<span data-ttu-id="a2326-117">Microsoft.Owin.Security.OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="a2326-117">Microsoft.Owin.Security.OpenIdConnect</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|<span data-ttu-id="a2326-118">Mellanprogram som gör att ett program toouse OpenIdConnect för autentisering</span><span class="sxs-lookup"><span data-stu-id="a2326-118">Middleware that enables an application toouse OpenIdConnect for authentication</span></span>|
|[<span data-ttu-id="a2326-119">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="a2326-119">Microsoft.Owin.Security.Cookies</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|<span data-ttu-id="a2326-120">Mellanprogram som gör att ett program toomaintain användarsession med hjälp av cookies</span><span class="sxs-lookup"><span data-stu-id="a2326-120">Middleware that enables an application toomaintain user session using cookies</span></span>|
|[<span data-ttu-id="a2326-121">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="a2326-121">Microsoft.Owin.Host.SystemWeb</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|<span data-ttu-id="a2326-122">Aktiverar toorun OWIN-baserade program på IIS med hello ASP.NET förfrågnings-pipelinen</span><span class="sxs-lookup"><span data-stu-id="a2326-122">Enables OWIN-based applications toorun on IIS using hello ASP.NET request pipeline</span></span>|

