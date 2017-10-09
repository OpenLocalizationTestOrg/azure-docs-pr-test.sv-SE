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
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a>Lägga till inloggning med Microsoft tooan ASP.NET-webbprogram

Den här guiden visar hur tooimplement logga in med Microsoft med hjälp av en ASP.NET MVC-lösning med en traditionell webbläsarbaserad webbapp med OpenID Connect. 

Hello slutet av den här guiden, ska ditt program kunna tooaccept logga moduler av personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har integrerat med Azure Active Directory. 

> Den här guiden kräver Visual Studio 2015 Update 3 eller Visual Studio 2017.  Har det?  [Hämta Visual Studio 2017 gratis](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Hur den här guiden fungerar

![Hur den här guiden fungerar](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Den här guiden bygger på hello scenario där en webbläsare har åtkomst till en ASP.NET-webbplats, begär en användare tooauthenticate via en knapp för inloggning. I det här scenariot uppstår de flesta av hello arbete toorender hello webbsida på hello på serversidan.

## <a name="libraries"></a>Bibliotek

Den här guiden använder hello följande bibliotek:

|Bibliotek|Beskrivning|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Mellanprogram som gör att ett program toouse OpenIdConnect för autentisering|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Mellanprogram som gör att ett program toomaintain användarsession med hjälp av cookies|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Aktiverar toorun OWIN-baserade program på IIS med hello ASP.NET förfrågnings-pipelinen|

