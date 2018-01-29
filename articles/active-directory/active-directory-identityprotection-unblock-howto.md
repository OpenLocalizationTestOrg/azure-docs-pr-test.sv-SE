---
title: "Azure Active Directory Identity Protection – hur du avblockerar användare | Microsoft Docs"
description: "Lär dig hur avblockera användare som har blockerats av en princip för Azure Active Directory Identity Protection."
services: active-directory
keywords: "Azure active directory identitetsskydd avblockera användare"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: ab11f27ceed716ba90e9027eee1775c190d2d93f
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory Identity Protection - så att avblockera användare
Med Azure Active Directory identitetsskydd, kan du konfigurera principer för att blockera användare om de konfigurerade villkor är uppfyllda. Normalt en blockerad användare kontakter supportavdelningen att bli blockerad. Detta avsnitt beskrivs de steg som du kan utföra för att låsa upp en blockerad användare.

## <a name="determine-the-reason-for-blocking"></a>Ta reda på varför för blockering
Som ett första steg att avblockera en användare måste du bestämma vilken typ av princip som har blockerats användaren eftersom nästa steg är beroende av den.
Med Azure Active Directory identitetsskydd, kan en användare antingen blockeras av inloggning riskprincipen eller en användarprofil för risk.

Du kan hämta typ av princip som har blockerat en användare från rubriken i den dialogruta som angavs för användaren under ett försök att logga in:

| Princip | Användardialogrutan |
| --- | --- |
| Inloggningsrisk |![Blockerade inloggning](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Användarrisk |![Blockerade konto](./media/active-directory-identityprotection-unblock-howto/104.png) |

En användare som har blockerats av:

* Logga in riskprincipen kallas också misstänkta inloggning
* En risk användarprincip kallas även för ett konto i fara

## <a name="unblocking-suspicious-sign-ins"></a>Avblockera misstänkta inloggningar
För att låsa upp en misstänkt inloggning, har du följande alternativ:

1. **Logga in från en bekant plats eller en enhet** – en vanlig orsak till blockerade misstänkta inloggningar är inloggningsförsök från okända platser eller enheter. Användarna kan snabbt se om detta är orsaken till blockerande genom att logga in från en bekant plats eller en enhet.
2. **Undantas från principen** - om du tror att den aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du undanta användare från den. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.
3. **Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera principen. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.

## <a name="unblocking-accounts-at-risk"></a>Avblockera konton i fara
För att låsa upp ett konto i fara, har du följande alternativ:

1. **Återställ lösenord** -du kan återställa användarens lösenord. Se [manuell säker lösenordsåterställning](active-directory-identityprotection.md#manual-secure-password-reset) för mer information.
2. **Ignorera alla riskhändelser** – användaren risk principen blockerar en användare om den konfigurerade användaren risknivå för blockerar åtkomsten har uppnåtts. Du kan minska en användare har rapporterat risknivå manuellt stänger riskhändelser. Mer information finns i [stänger riskhändelser manuellt](active-directory-identityprotection.md#closing-risk-events-manually).
3. **Undantas från principen** - om du tror att den aktuella konfigurationen av din inloggningsprincip orsakar problem för specifika användare, kan du undanta användare från den. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.
4. **Inaktivera principen** - om du tror att din principkonfiguration som orsakar problem för alla användare kan du inaktivera principen. Se [Azure Active Directory Identity Protection](active-directory-identityprotection.md) för mer information.

## <a name="next-steps"></a>Nästa steg
 Vill du veta mer om Azure AD Identity Protection? Checka ut [Azure Active Directory Identity Protection](active-directory-identityprotection.md).
