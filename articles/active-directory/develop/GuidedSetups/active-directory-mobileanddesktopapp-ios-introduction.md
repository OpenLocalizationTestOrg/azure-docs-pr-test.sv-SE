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
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a>Anropa hello Microsoft Graph API från en iOS-app

Den här guiden visar hur interna iOS-program (Swift) kan hämta en åtkomst-token och anropa hello Microsoft Graph API eller andra API: er som kräver åtkomst-token från Azure Active Directory v2-slutpunkten.

Hello slutet av den här guiden, ditt program kommer att kunna toocall en skyddad API: et med personliga konton (inklusive outlook.com och live.com) som fungerar och skolkonton från alla företag eller organisation som har Azure Active Directory.

> ### <a name="pre-requisites"></a>Förutsättningar
> - XCode 8.x krävs för den här guiden. Du kan hämta XCode [härifrån](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode hämta URL").
> - [Carthage](https://github.com/Carthage/Carthage) för hantering av paketet

### <a name="how-this-guide-works"></a>Hur den här guiden fungerar

![Hur den här guiden fungerar](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

hello exempelprogram som skapats av den här guiden kan en iOS-program tooquery hello Microsoft Graph API eller ett webb-API som accepterar token från Azure Active Directory v2-slutpunkten. I det här scenariot läggs en token tooHTTP begäranden via hello Authorization-huvud. Token förvärv och förnyelse hanteras av hello Microsoft Authentication Library (MSAL).


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Hantera token förvärv för att komma åt skyddade webb-API: er

När hello användare autentiseras får hello exempelprogrammet en token som kan vara används tooquery hello Microsoft Graph API eller ett webb-API som skyddas av Microsoft Azure Active Directory v2.

API: er som Microsoft Graph kräver en åtkomst-token tooallow åtkomst till specifika resurser – till exempel tooread en användarprofil åtkomst användares kalender eller skicka ett e-postmeddelande. Programmet kan begära en åtkomst-token som använder MSAL genom att ange API-scope. Åtkomsttoken är tillagda toohello HTTP Authorization-huvud för varje anrop som görs mot hello skyddad resurs.

MSAL hanterar cachelagring och uppdatera åtkomsttoken, så att programmet inte behöver.


### <a name="nuget-packages"></a>NuGet-paket

Den här guiden använder hello följande NuGet-paket:

|Bibliotek|Beskrivning|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Microsoft Authentication Library Preview för iOS|

