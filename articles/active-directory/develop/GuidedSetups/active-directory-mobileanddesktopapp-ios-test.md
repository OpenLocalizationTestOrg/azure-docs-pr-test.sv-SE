---
title: "aaaAzure AD v2 iOS komma igång - Test | Microsoft Docs"
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
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a>Testa fråga hello Microsoft Graph API från iOS-program

Tryck på `Command`  +  `R` toorun hello koden i hello-simulatorn.

![Exempel skärmbild](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

När du är klar tootest trycker du på *'anropa Microsoft Graph API-* och du kommer att tillfrågas tootype ditt användarnamn och lösenord.

### <a name="consent"></a>Medgivande
hello första gången du loggar in tooyour program visas med en medgivande skärm liknande toohello nedan, där du måste acceptera tooexplicitly:

![Medgivande skärmen](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Förväntat resultat
Du bör se användarens profilinformation som returneras av hello Microsoft Graph API-anrop i hello *loggning* avsnitt.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter

hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil. Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal. Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope. Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar. I ordning tooaccess hello användarens kalender i en kontext med ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilent` anropa. hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.

<!--end-collapse-->



