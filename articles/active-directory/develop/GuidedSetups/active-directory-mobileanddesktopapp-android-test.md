---
title: "aaaAzure AD v2 Android komma igång - Test | Microsoft Docs"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Testa din kod

1. Distribuera din kod tooyour/emulatorn.
2. När du är klar tootest kan använda Microsoft Azure Active Directory (organisationskonto) eller en Account (live.com, outlook.com) konto toosign i. 

![Exempel skärmbild](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Logga in](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Medgivande
hello första gången en användare loggar in tooyour program, de kommer att visas ett medgivande skärm liknande toohello nedan, där de måste acceptera tooexplicitly: 

![Medgivande](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Förväntat resultat
Du bör se hello resultatet av ett anrop tooMicrosoft Graph API ”jag” endpoint används tootooobtain hello användarprofil - https://graph.microsoft.com/v1.0/me. En lista över vanliga Microsoft Graph slutpunkter finns [artikel](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter

hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil. Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal. Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope. Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar. I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilentAsync` anropa. hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.

<!--end-collapse-->
