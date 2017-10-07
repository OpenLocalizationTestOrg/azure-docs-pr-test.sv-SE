---
title: "felkoder för aaaSign i aktiviteten rapporten i hello Azure Active Directory-portalen | Microsoft Docs"
description: "Referens för felkoder för inloggningsaktivitet."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: a0ca5b706bfeb0c7ce669712468a083a394712b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-report-error-codes-in-hello-azure-active-directory-portal"></a>Inloggningsaktivitet rapporten felkoder i hello Azure Active Directory-portalen

Hello information som tillhandahålls av hello användaren inloggningar rapport, kan du hitta svar tooquestions som:

- Vem har loggat in med Azure Active Directory?
- Vilka appar loggade de in på?
- Vilka inloggningar misslyckades och varför?

Det här avsnittet visar hello felet koder och hello relaterade beskrivningar. 

## <a name="how-can-i-display-failed-sign-ins"></a>Hur visar jag misslyckade inloggningar? 

Dina första posten punkt tooall inloggning aktiviteter data är  **[inloggningar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)**  i hello **aktiviteten** avsnitt i **Azure Active**.


![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins-errors/61.png "Inloggningsaktivitet")


I rapporten inloggningar, kan du visa alla misslyckade inloggningar genom att välja **Misslyckad** som **Inloggningsstatus**.


![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins-errors/06.png "Inloggningsaktivitet")

Klicka på ett objekt i listan visas hello öppnar hello **aktivitetsinformation: inloggningar** bladet. Den här vyn innehåller alla hello information som Azure Active Directory spårar om inloggningar, inklusive hello **inloggning felkoden** och en **felorsak**.

![Inloggningsaktivitet](./media/active-directory-reporting-activity-sign-ins-errors/05.png "Inloggningsaktivitet")


Som ett alternativ toousing hello Azure portal tooaccess hello inloggningar data, kan du också använda hello [reporting API](active-directory-reporting-api-getting-started-azure-portal.md).


hello följande avsnitt ger en fullständig översikt över alla eventuella fel och hello relaterade beskrivningar. 

## <a name="error-codes"></a>Felkoder

| Fel| Beskrivning |
| --- | --- |
| 50001| hello tjänstens huvudnamn med namnet X hittades inte i hello-klient med namnet Y. Detta kan inträffa om hello program inte har installerats av Hej administratör för hello-klienten. Eller resurs principal kunde inte hittas i hello directory eller är ogiltig|
| 50008| SAML-kontroll är saknas eller är felkonfigurerad i hello-token.|
| 50011| hello svarsadressen saknas, felkonfigurerad eller matchar inte svara adresser som har konfigurerats för hello program.|
| 50053| Kontot är låst eftersom användaren försökte toosign i för många gånger med ett felaktigt användar-ID eller lösenord.|
| 50054| Det gamla lösenordet krävs för autentiseringen.|
| 50055| Ogiltigt lösenord, ange lösenordet som har gått ut.|
| 50057| Användarkontot är inaktiverat.|
| 50058| Ingen information om användarens identitet finns bland angivna autentiseringsuppgifter eller användaren hittades inte i klient eller en tyst inloggning-begäran har skickats men ingen användare är inloggad eller -tjänsten kunde tooauthenticate hello användare.|
| 50074| Stark autentisering (tvåfaktorsautentisering) krävs|
| 50079| Användare behöver tooenroll för tvåfaktorautentisering|
| 50126| Ogiltigt användarnamn eller lösenord eller ogiltigt lokalt användarnamn eller lösenord.|
| 50131| Används i olika fel för villkorlig åtkomst. T.ex felaktiga Windows-enhet tillstånd begäran som blockerats på grund av toosuspicious aktivitet, åtkomstprincip och säkerhetsprincip beslut.|
| 50133| Sessionen är ogiltig på grund av tooexpiration eller lösenordsändring.|
| 50144| Användarens Active Directory-lösenord har upphört att gälla.|
| 65001| Programmet X har inte behörighet tooaccess programmet Y eller hello behörighet har återkallats. Eller hello användaren eller administratören har inte godkänt toouse hello program med ID X. Send en interaktiv auktoriseringsbegäran för den här användaren och resurs. Eller hello användaren eller administratören har inte godkänt toouse hello program med ID X. Send en auktorisering begäran tooyour klient admin tooact uppdrag hello App: Y för resursen: Z.|
| 65005| programmet hello krävs resurs åtkomstlista inte innehåller program som kan upptäckas av hello resurs eller hello klientprogrammet har begärt åtkomst tooresource som inte har angetts i dess lista för åtkomst av resurser som krävs eller diagram tjänsten returnerade felaktiga begäran eller resursen hittades inte.|
| 70001| hello programmet X hittades inte i hello-klient med namnet Y. Detta kan hända om hello program inte har installerats av Hej administratör hello klient eller tilllåten tooby alla användare i hello-klient. Du kanske har skickar din autentisering begäran toohello fel-klient.|
| 80001| Ingen autentiseringsagent är tillgänglig.|
| 80002| Den tillåtna tiden för autentiseringsagentens lösenordsvalidering överskreds.|
| 80003| Ogiltigt svar har tagits emot av autentiseringsagenten.|
| 80004| Felaktig UPN (User Principal Name) används i begäran om inloggning.|
| 80005| Autentiseringsagent: Fel uppstod.|
| 80007| Autentisering Agent tooconnect tooActive Directory.|
| 80010| Autentisering Agent toodecrypt lösenord.|
| 81001| Användarens Kerberos-biljett är för stor.|
| 81002| Det går inte toovalidate användarens Kerberos-biljett.|
| 81003| Det går inte toovalidate användarens Kerberos-biljett.|
| 81004| Kerberos-autentiseringsförsök misslyckades.|
| 81008| Det går inte toovalidate användarens Kerberos-biljett.|
| 81009| Det går inte toovalidate användarens Kerberos-biljett.|
| 81010| Sömlös SSO misslyckades eftersom hello användarens Kerberos-biljetten har upphört att gälla eller är ogiltig.|
| 81011| Det går inte toofind användarobjektet baserat på informationen i hello användarens Kerberos-biljett.|
| 81012| hello användaren försöker toosign i tooAzure AD skiljer sig från hello användaren loggat in hello enhet.|
| 81013| Det går inte toofind användarobjektet baserat på informationen i hello användarens Kerberos-biljett.|
| 90014| Används i olika fall när en förväntade fältet inte finns i hello autentiseringsuppgifter.|
| 90093| Diagram som returneras med otillåtna felkoden för hello-begäran.|



## <a name="next-steps"></a>Nästa steg

Mer information finns i hello [inloggning aktivitetsrapporter hello Azure Active Directory-portalen](active-directory-reporting-activity-sign-ins.md).

