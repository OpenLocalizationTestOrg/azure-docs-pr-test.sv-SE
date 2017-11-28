---
title: "Azure AD v2 ASP.NET Web Server komma igång - introduktion | Microsoft Docs"
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
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a><span data-ttu-id="58856-103">Lägga till inloggning med Microsoft till ett ASP.NET-webbprogram</span><span class="sxs-lookup"><span data-stu-id="58856-103">Add sign-in with Microsoft to an ASP.NET web app</span></span>

<span data-ttu-id="58856-104">Den här guiden visar hur du implementerar inloggning med Microsoft med hjälp av en ASP.NET MVC-lösning med en traditionell webbläsarbaserad webbapp med OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="58856-104">This guide demonstrates how to implement sign-in with Microsoft using an ASP.NET MVC solution with a traditional web browser-based application using OpenID Connect.</span></span> 

<span data-ttu-id="58856-105">I slutet av den här guiden kommer programmet att kunna acceptera logga moduler av personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har integrerat med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="58856-105">At the end of this guide, your application will be able to accept sign ins of personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory.</span></span> 

> <span data-ttu-id="58856-106">Den här guiden kräver Visual Studio 2015 Update 3 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="58856-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="58856-107">Har det?</span><span class="sxs-lookup"><span data-stu-id="58856-107">Don’t have it?</span></span>  [<span data-ttu-id="58856-108">Hämta Visual Studio 2017 gratis</span><span class="sxs-lookup"><span data-stu-id="58856-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a><span data-ttu-id="58856-109">Hur den här guiden fungerar</span><span class="sxs-lookup"><span data-stu-id="58856-109">How this guide works</span></span>

![Hur den här guiden fungerar](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

<span data-ttu-id="58856-111">Den här guiden bygger på ett scenario där en webbläsare har åtkomst till en ASP.NET-webbplats, begär en användare autentiseras via en knapp för inloggning.</span><span class="sxs-lookup"><span data-stu-id="58856-111">This guide is based on the scenario where a browser accesses an ASP.NET web site, requesting a user to authenticate via a sign-in button.</span></span> <span data-ttu-id="58856-112">I det här scenariot inträffar mesta av arbetet ska renderas webbsidan på serversidan.</span><span class="sxs-lookup"><span data-stu-id="58856-112">In this scenario, most of the work to render the web page occurs on the server side.</span></span>

## <a name="libraries"></a><span data-ttu-id="58856-113">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="58856-113">Libraries</span></span>

<span data-ttu-id="58856-114">Den här guiden använder följande bibliotek:</span><span class="sxs-lookup"><span data-stu-id="58856-114">This guide uses the following libraries:</span></span>

|<span data-ttu-id="58856-115">Bibliotek</span><span class="sxs-lookup"><span data-stu-id="58856-115">Library</span></span>|<span data-ttu-id="58856-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="58856-116">Description</span></span>|
|---|---|
|[<span data-ttu-id="58856-117">Microsoft.Owin.Security.OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="58856-117">Microsoft.Owin.Security.OpenIdConnect</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|<span data-ttu-id="58856-118">Mellanprogram som aktiverar ett program att använda OpenIdConnect för autentisering</span><span class="sxs-lookup"><span data-stu-id="58856-118">Middleware that enables an application to use OpenIdConnect for authentication</span></span>|
|[<span data-ttu-id="58856-119">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="58856-119">Microsoft.Owin.Security.Cookies</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|<span data-ttu-id="58856-120">Mellanprogram som aktiverar ett program att underhålla användarsession med hjälp av cookies</span><span class="sxs-lookup"><span data-stu-id="58856-120">Middleware that enables an application to maintain user session using cookies</span></span>|
|[<span data-ttu-id="58856-121">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="58856-121">Microsoft.Owin.Host.SystemWeb</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|<span data-ttu-id="58856-122">Gör att OWIN-baserade program körs på IIS med hjälp av ASP.NET förfrågnings-pipelinen</span><span class="sxs-lookup"><span data-stu-id="58856-122">Enables OWIN-based applications to run on IIS using the ASP.NET request pipeline</span></span>|

