---
title: aaaAzure AD v2 JS SPA interaktiv installation - Test | Microsoft Docs
description: "Hur JavaScript SPA program anropar en API som kräver åtkomst-token i Azure Active Directory v2 slutpunkten"
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
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Testa din kod

> ### <a name="testing-with-visual-studio"></a>Testa med Visual Studio
> Om du använder Visual Studio trycker du på `F5` toorun projektet: hello webbläsaren öppnas och dirigerar dig för*http://localhost: {port}* där du ser hello *anropa Microsoft Graph API* knappen.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testa med Python eller en annan webbserver
> Om du inte använder Visual Studio, kontrollera din webbserver är igång och den är konfigurerad toolisten tooa TCP-port utifrån hello mappen som innehåller din *index.html* fil. För Python, kan du börja lyssna toohello port genom att köra hello i hello kommando fråga / terminal, från hello app mappen:
> 
> ```bash
> python -m http.server 8080
> ```
>  Öppna hello webbläsare och skriv sedan *http://localhost: 8080* eller *http://localhost: {port}* - när hello *port* motsvarar toohello-port som webbservern lyssnar på. Du bör se hello innehållet i index.html-sida med hello *anropa Microsoft Graph API* knappen.

## <a name="test-your-application"></a>Testa programmet

Efter hello webbläsaren läser du in din *index.html*, klicka på hello *anropa Microsoft Graph API* knappen. Om det är hello första gången hello webbläsaren omdirigeras du toohello Microsoft Azure Active Directory v2 slutpunkt, där du har uppmanas toosign i.
 
![Exempel skärmbild](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Medgivande
hello visas första gången du loggar in tooyour program, en medgivande skärm liknande toohello följande, där du behöver tooaccept:

 ![Medgivande skärmen](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Förväntat resultat
Du bör se användarens profilinformation som returneras av hello svar för Microsoft Graph API-anrop.
 
 ![Resultat](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Du också se grundläggande information om hello-token har införskaffade i hello *åtkomst-Token* och *ID Token anspråk* rutor.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Mer information om scope och delegerade behörigheter

hello Microsoft Graph API kräver hello `user.read` omfång tooread hello användarens profil. Detta scope läggs automatiskt som standard i alla program som registreras på vår registreringsportal. Vissa andra API: er för Microsoft Graph samt anpassade API: er för backend-servern kan kräva ytterligare scope. Till exempel hello omfånget för Microsoft Graph `Calendars.Read` är nödvändiga toolist hello användares kalendrar. I ordning tooaccess hello användares kalender hello kontexten för ett program måste du tooadd hello `Calendars.Read` delegerad behörighet toohello programmet registreringens information och Lägg sedan till hello `Calendars.Read` omfång toohello `acquireTokenSilent` anropa. hello användare kan uppmanas att ytterligare medgivanden när du ökar hello antal scope.

Om en serverdel API inte kräver ett omfång (rekommenderas inte), kan du använda hello `clientId` som hello omfattning i hello `acquireTokenSilent` och/eller `acquireTokenRedirect` anrop.

<!--end-collapse-->
