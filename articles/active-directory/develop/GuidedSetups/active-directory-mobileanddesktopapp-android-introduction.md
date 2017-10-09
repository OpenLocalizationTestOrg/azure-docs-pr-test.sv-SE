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
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a>Anropa hello Microsoft Graph API från en Android-app

Den här guiden visar hur en ursprunglig Android-program kan få en åtkomst-token och anropa Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2 slutpunkten.

Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.  

### <a name="how-this-sample-works"></a>Hur det här exemplet fungerar
![Hur det här exemplet fungerar](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

hello-exempel som skapats av den här guiden bygger på ett scenario där en Android-App är används tooquery ett webb-API som accepterar token från Azure Active Directory v2 slutpunkten – i det här fallet Microsoft Graph API. I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud. Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).

### <a name="pre-requisites"></a>Förutsättningar
* Den interaktiva installationen fokuserar på Android Studio, men några andra utvecklingsmiljö för Android-program är också tillåtet. 
* Android SDK 21 eller senare krävs (SDK 25 rekommenderas).
* Google Chrome eller en webbläsare med hjälp av anpassade flikar krävs för den här versionen av Microsoft Authentication Library (MSAL) för Android.

> Obs: Google Chrome ingår inte i Visual Studio-emulatorn för Android. Vi rekommenderar att du tootest koden på en Emulator med API 25 eller en bild med API 21 eller senare som har med Google Chrome installerat.


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a>Hur token toohandle förvärv tooaccess skyddade webb-API

När hello användare autentiseras får hello exempelprogrammet en token som kan använda tooquery Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.

API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande. Programmet kan begära en åtkomst-token med MSAL tooaccess dessa resurser genom att ange API-scope. Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs. 

MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.

### <a name="libraries"></a>Bibliotek

Den här guiden använder hello följande bibliotek:

|Bibliotek|Beskrivning|
|---|---|
|[com.microsoft.Identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|
