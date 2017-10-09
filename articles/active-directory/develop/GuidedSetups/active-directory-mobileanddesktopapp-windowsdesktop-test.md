---
title: "aaaAzure AD v2 Windows Desktop komma igång - Test | Microsoft Docs"
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
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Testa din kod

Ordna tootest programmet, tryck på `F5` toorun ditt projekt i Visual Studio. Main-fönster visas:

![Exempel skärmbild](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

När du är klar tootest du *anropa Microsoft Graph API* och använda Microsoft Azure Active Directory (organisationskonto) eller en Account (live.com, outlook.com) konto toosign i. Den hello är första gången, visas ett fönster som ber användaren toosign i:

![Logga in](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Medgivande
hello första gången du loggar in tooyour program visas med en medgivande skärm liknande toohello nedan, där du måste acceptera tooexplicitly:

![Medgivande skärmen](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Förväntat resultat
Användarens profilinformation som returneras av hello Microsoft Graph API-anrop på API-anrop resultat hello-skärmen bör visas.

Du bör också se grundläggande information om hello-token har köpt `AcquireTokenAsync` eller `AcquireTokenSilentAsync` i rutan för Token information om hello:

|Egenskap  |Format  |Beskrivning |
|---------|---------|---------|
|Namn | {Fullständig användarnamn} |hello användarens förnamn och efternamn|
|Användarnamn |<span>user@domain.com</span> |hello användarnamn används tooidentify hello användare|
|Token upphör att gälla |{DateTime}         |hello tid på vilka hello token upphör att gälla. MSAL kommer förlänga giltighetstiden för hello du genom att förnya hello token vid behov|
|Åtkomst-token |{Sträng}         |Hej tokensträng skickas som kommer att skickas tooHTTP förfrågningar som kräver ett authorization-huvud|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter
Graph API kräver hello `user.read` omfång tooread användarprofil. Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal. Vissa andra Graph API: er samt anpassade API: er för backend-servern kräver ytterligare scope. Till exempel för diagram, `Calendars.Read` är nödvändiga toolist användarkalendrar. I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd `Calendars.Read` delegerade programmet registreringsinformation och Lägg sedan till `Calendars.Read` toohello `AcquireTokenAsync` anropa. Användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.

<!--end-collapse-->



