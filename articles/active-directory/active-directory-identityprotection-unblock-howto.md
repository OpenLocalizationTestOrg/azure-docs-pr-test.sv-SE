---
title: "aaaAzure identitetsskydd för Active Directory - hur toounblock användare | Microsoft Docs"
description: "Lär dig hur avblockera användare som har blockerats av en princip för Azure Active Directory Identity Protection."
services: active-directory
keywords: "Azure active directory identitetsskydd avblockera användare"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: cdda2808822888f76aa75cf46478738c94df51a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection---how-toounblock-users"></a>Azure Active Directory Identity Protection - hur toounblock användare
Med Azure Active Directory identitetsskydd, kan du konfigurera principer tooblock användare om hello konfigurerad villkor är uppfyllda. Normalt en blockerad användare kontakter help desk toobecome avblockerad. Detta avsnitt beskrivs hello steg som du kan utföra toounblock en blockerad användare.

## <a name="determine-hello-reason-for-blocking"></a>Ta reda på orsaken hello för blockering
Som ett första steg toounblock en användare måste toodetermine hello typen av princip som har blockerats hello användaren eftersom nästa steg är beroende av den.
Med Azure Active Directory identitetsskydd, kan en användare antingen blockeras av inloggning riskprincipen eller en användarprofil för risk.

Du kan få hello typen av princip som har blockerat en användare från hello rubriken i hello dialogruta som angavs toohello användaren under ett försök att logga in:

| Princip | Användardialogrutan |
| --- | --- |
| Logga in risk |![Blockerade inloggning](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Användaren risk |![Blockerade konto](./media/active-directory-identityprotection-unblock-howto/104.png) |

En användare som har blockerats av:

* Logga in riskprincipen kallas också misstänkta inloggning
* En risk användarprincip kallas även för ett konto i fara

## <a name="unblocking-suspicious-sign-ins"></a>Avblockera misstänkta inloggningar
toounblock en misstänkt inloggning har hello följande alternativ:

1. **Logga in från en bekant plats eller en enhet** – en vanlig orsak till blockerade misstänkta inloggningar är inloggningsförsök från okända platser eller enheter. Användarna kan snabbt se om detta är hello blockerar orsak genom toosign i från en bekant plats eller en enhet.
2. **Undantas från principen** - om du tror att hello aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du utesluta hello användare från den. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.
3. **Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera hello princip. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.

## <a name="unblocking-accounts-at-risk"></a>Avblockera konton i fara
toounblock ett konto i fara har hello följande alternativ:

1. **Återställ lösenord** -du kan återställa hello användarens lösenord. Se [manuell säker lösenordsåterställning](active-directory-identityprotection.md#manual-secure-password-reset) för mer information.
2. **Ignorera alla riskhändelser** -hello användarprincip risk blockerar en användare om hello konfigurerade användaren risknivå för blockerar åtkomsten har uppnåtts. Du kan minska en användare har rapporterat risknivå manuellt stänger riskhändelser. Mer information finns i [stänger riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Undantas från principen** - om du tror att hello aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du utesluta hello användare från den. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.
4. **Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera hello princip. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.

## <a name="next-steps"></a>Nästa steg
 Vill du tooknow mer om Azure AD Identity Protection? Checka ut [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
