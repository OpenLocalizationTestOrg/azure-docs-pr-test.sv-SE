---
title: "aaaHow tooremove användarens åtkomst tooan programmet | Microsoft Docs"
description: "Förstå hur tooremove en användare åtkomst till tooan program"
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
ms.openlocfilehash: 17017bddb73aad5a0ef3a411ac91bf0423f0b600
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooremove-a-users-access-tooan-application"></a>Hur tooremove en användare åtkomst till tooan program

Den här artikeln hjälper dig toounderstand hur tooremove en användare åtkomst till tooan programmet.

## <a name="i-want-tooremove-a-specific-users-or-groups-assignment-tooan-application"></a>Jag vill tooremove en specifik användare eller grupp tilldelning tooan program

tooremove en användare eller grupp tilldelning tooan program, följ hello steg i hello [ta bort en användare eller grupp från en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

. ## jag vill toodisable alla åtkomst tooan program för varje användare

toodisable alla inloggningar tooan användarprogram, följ hello stegen i hello [inaktivera användarinloggningar för en enterprise-app i Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-toodelete-an-application-entirely"></a>Jag vill toodelete ett program helt

för**ta bort ett program**, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör** eller **Co-administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **företagsprogram** från hello Azure Active Directory vänstra navigeringsmenyn.

5.  Klicka på **alla program** tooview en lista över alla program.

   * Om du inte ser hello-program som du vill visa här använder du hello **Filter** kontroll hello överst i hello **listan med alla program** och ange hello **visa** alternativ för **Alla program.**

6.  Välj hello-program som du vill toodelete.

7.  När programmet hello läses in klickar du på **ta bort** ikon från hello översta program **översikt** bladet.

## <a name="i-want-toodisable-all-future-user-consent-operations-tooany-application"></a>Jag vill toodisable alla framtida medgivande operations tooany-användarprogram

Inaktiverar användargodkännande för hela katalogen förhindra användare från principer tooany program. Administratörer kan fortfarande medgivande på användarens behalves. toolearn mer om programmet samtycke och varför du kanske eller kanske inte vill toodo detta, Läs [förstå användare och admin medgivande](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

för**inaktivera alla framtida användarens medgivande åtgärder i hela katalogen**, följ hello instruktionerna nedan:

1.  Öppna hello [ **Azure Portal** ](https://portal.azure.com/) och logga in som en **Global administratör.**

2.  Öppna hello **Azure Active Directory-tillägget** genom att klicka på **fler tjänster** längst hello hello huvudsakliga vänstra navigeringsmenyn.

3.  Skriv i **”Azure Active Directory**” i sökrutan för hello filter och väljer hello **Azure Active Directory** objekt.

4.  Klicka på **användare och grupper** i hello navigeringsmenyn.

5.  Klicka på **användarinställningar**.

6.  Inaktivera alla framtida användarens medgivande åtgärder genom att ange hello **användare kan låta appar tooaccess sina data** växla för**nr** och klicka på hello **spara** knappen.


# <a name="next-steps"></a>Nästa steg
[Hantera åtkomst tooapps](active-directory-managing-access-to-apps.md)
