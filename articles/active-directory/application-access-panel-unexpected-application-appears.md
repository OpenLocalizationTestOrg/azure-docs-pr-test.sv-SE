---
title: "aaaHow program visas på hello åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till att ett program inte visas i hello åtkomstpanelen"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewr: japere
ms.openlocfilehash: 14ee732c4ed5260cba878e949cf9d90877aee67e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-applications-appear-on-hello-access-panel"></a>Hur program visas på hello åtkomstpanelen

Hej åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets eller skolkonto i Azure Active Directory (AD Azure) tooview och starta molnbaserade program som hello Azure AD-administratör har ge dem tillgång till. Dessa program är konfigurerade för hello användare i hello Azure AD-portalen. Hej administratör kan etablera hello toohello programanvändare direkt eller tooa grupp som en användare är en del av ledde hello program visas på hello användarens åtkomstpanelen.

## <a name="general-issues-toocheck-first"></a>Allmänna problem toocheck först

-   Om ett program bara togs bort från en användare eller grupp hello användaren är medlem i, försök toosign in och ut i hello användarens åtkomstpanelen efter några minuter toosee om hello programmet tas bort.

-   Om en licens precis har tagits bort från en användare eller grupp hello användaren är medlem i det här kan ta lång tid, beroende på hello storleken och komplexiteten för hello grupp för ändringar toobe görs. Tillåt extra tid innan du loggar in på hello åtkomstpanelen.

## <a name="problems-related-tooassigning-applications-toousers"></a>Problem relaterade tooassigning program toousers

En användare visas kanske ett program på deras åtkomstpanelen eftersom de hade tidigare tilldelats tooit. Nedan visas några sätt toocheck:

-   [Kontrollera om en användare har tilldelats toohello program](#check-if-a-user-is-assigned-to-the-application)

-   [Kontrollera om en användare är under en licens relaterade toohello program](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-toohello-application"></a>Kontrollera om en användare har tilldelats toohello program

toocheck om en användare har tilldelats toohello programmet gör hello nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

6.  **Sök** för hello namnet på hello-programmet i fråga.

7.  Klicka på **användare och grupper**.

8.  Kontrollera toosee om dina användare har tilldelats toohello program.

  * Om du vill tooremove hello användare från hello program **klickar du på raden hello** för hello användaren och välj **ta bort**.

### <a name="check-if-a-user-is-under-a-license-related-toohello-application"></a>Kontrollera om en användare är under en licens relaterade toohello program

toocheck en användare tilldelade licenser, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **licenser** toosee som licenser hello användare för närvarande har tilldelats.

   * Hello användare som är tilldelade tooan Office-licens möjliggör detta första part Office program tooappear på hello användarens åtkomstpanelen.

## <a name="problems-related-tooassigning-applications-toogroups"></a>Problem relaterade tooassigning program toogroups

En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats hello program. Nedan visas några sätt toocheck:

-   [Kontrollera en användares gruppmedlemskap](#check-a-users-group-memberships)

-   [Kontrollera om en användare är medlem i en grupp som har tilldelats tooa licens](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Kontrollera en användares gruppmedlemskap

toocheck en grupps medlemskap, följ hello stegen nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **grupper.**

8.  Kontrollera toosee om användaren är en del av en grupp som tilldelats toohello program.

   * Om du vill tooremove hello användare från hello grupp **klickar du på raden hello** i hello gruppen och väljer Ta bort.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-tooa-license"></a>Kontrollera om en användare är medlem i en grupp som har tilldelats tooa licens

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för hello-användare som du är intresserad av och **klickar du på raden hello** tooselect.

7.  Klicka på **grupper.**

8.  Klicka på hello rad i en viss grupp.

9.  Klicka på **licenser** toosee vilken licenser hello-grupp har tilldelats tooit.

  * Om hello är tilldelade tooan Office-licens möjliggör detta tooappear vissa första part Office-program på hello användarens åtkomstpanelen.


## <a name="if-these-troubleshooting-steps-do-not-hello-resolve-hello-issue"></a>Om de här åtgärderna inte hello problemet hello

Öppna ett supportärende med hello efter information om tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID:t

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
