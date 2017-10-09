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
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a>Anropa hello Microsoft Graph API från en app för Windows-skrivbordet

Den här guiden visar hur en intern Windows Desktop .NET (XAML)-programmet kan få en åtkomsttoken och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2-slutpunkten.

Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.  

> Den här guiden kräver Visual Studio 2015 Update 3 eller Visual Studio 2017.  Har det? [Hämta Visual Studio 2017 gratis](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Hur den här guiden fungerar

![Hur den här guiden fungerar](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

hello exempelprogram som skapats av den här guiden kan ett program för Windows-skrivbordet tooquery Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten. I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud. Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Hantera token förvärv för att komma åt skyddade webb-API: er

När hello användare autentiseras får hello exempelprogrammet en token som kan använda tooquery Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.

API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande. Programmet kan begära en åtkomst-token med MSAL tooaccess dessa resurser genom att ange API-scope. Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs. 

MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.


### <a name="nuget-packages"></a>NuGet-paket

Den här guiden använder hello följande NuGet-paket:

|Bibliotek|Beskrivning|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft Authentication Library (MSAL)|

