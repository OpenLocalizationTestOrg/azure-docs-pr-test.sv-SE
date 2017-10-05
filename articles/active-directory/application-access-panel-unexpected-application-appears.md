---
title: "Hur program visas på åtkomstpanelen | Microsoft Docs"
description: "Felsöka anledningen till att ett program inte visas i panelen åtkomst"
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
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a>Hur program visas på åtkomstpanelen

Åtkomstpanelen är en webbaserad portal som gör att en användare med ett arbets- eller skolkonto konto i Azure Active Directory (Azure AD) för att visa och starta molnbaserade program att Azure AD-administratör har beviljats dem åtkomst till. Dessa program är konfigurerade för användarens räkning i Azure AD-portalen. Administratören kan etablera programmet till användaren direkt eller till en grupp en användare tillhör ledde till ett program som visas på användarens åtkomstpanelen.

## <a name="general-issues-to-check-first"></a>Allmänna problem med att kontrollera först

-   Om ett program bara togs bort från en användare eller grupp som användaren är medlem i, försök att logga in och ut igen i användarens åtkomstpanelen efter några minuter att se om programmet tas bort.

-   Om en licens precis togs bort från en användare eller grupp som användaren är medlem i det här kan ta lång tid, beroende på storleken och komplexiteten i gruppen för ändringar görs. Tillåt extra tid innan du loggar in på åtkomstpanelen.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problem med att tilldela program till användare

En användare kan ser ett program på deras åtkomstpanelen eftersom de tidigare har tilldelats till den. Nedan visas några metoder för att kontrollera:

-   [Kontrollera om en användare är kopplad till programmet](#check-if-a-user-is-assigned-to-the-application)

-   [Kontrollera om en användare är under en licens som hör till programmet](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Kontrollera om en användare är kopplad till programmet

Följ stegen nedan om du vill kontrollera om en användare har tilldelats till programmet:

1.  Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** att visa en lista över alla program.

6.  **Sök** för namnet på programmet i fråga.

7.  Klicka på **användare och grupper**.

8.  Kontrollera om användaren har tilldelats programmet.

  * Om du vill ta bort användaren från programmet, **klickar du på raden** för användaren och välj **ta bort**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Kontrollera om en användare är under en licens som hör till programmet

Följ stegen nedan om du vill kontrollera en användares tilldelade licenser:

1.  Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** på navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.

7.  Klicka på **licenser** finns som licenser användaren för närvarande har tilldelats.

   * Om användaren har tilldelats en licens denna aktivera första part Office-program ska visas på användarens åtkomstpanelen.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problem med att tilldela program till grupper

En användare visas kanske ett program på deras åtkomstpanelen eftersom de är en del av en grupp som har tilldelats programmet. Nedan visas några metoder för att kontrollera:

-   [Kontrollera en användares gruppmedlemskap](#check-a-users-group-memberships)

-   [Kontrollera om en användare är medlem i en grupp som har tilldelats en licens](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Kontrollera en användares gruppmedlemskap

Följ stegen nedan om du vill kontrollera en grupps medlemskap:

1.  Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** på navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.

7.  Klicka på **grupper.**

8.  Kontrollera om användaren är en del av en grupp som tilldelats programmet.

   * Om du vill ta bort användaren från gruppen **klickar du på raden** av grupprinciper och väljer Ta bort.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Kontrollera om en användare är medlem i en grupp som har tilldelats en licens

1.  Öppna den [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna den **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst ned i den huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för filter och välj den **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** på navigeringsmenyn.

5.  Klicka på **alla användare**.

6.  **Sök** för den användare som du är intresserad av och **klickar du på raden** att välja.

7.  Klicka på **grupper.**

8.  Klicka på raden för en viss grupp.

9.  Klicka på **licenser** att se vilka licenser gruppen har tilldelats.

  * Om gruppen har tilldelats en licens för Office detta kan aktivera vissa första part Office-program ska visas på användarens åtkomstpanelen.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Om felsökningen gör inte åtgärda problemet

Öppna ett supportärende med följande information om de är tillgängliga:

-   Fel-ID för korrelation

-   UPN (användarens e-postadress)

-   Klient-ID:t

-   Typ av webbläsare

-   Tidszon och tid/tidsperioden under fel inträffar

-   Fiddler spårningar

## <a name="next-steps"></a>Nästa steg
[Hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md)
