---
title: "aaaAzure AD nivåer lösenordssäkerhet | Microsoft Docs"
description: "Förklarar hur Azure AD tillämpar starka lösenord och skyddar användarnas lösenord från cyber kriminella"
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: joflore
ms.openlocfilehash: 10d8b600d9f4c02355b2cd8c5dccf8505aaf210d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="a-multi-tiered-approach-tooazure-ad-password-security"></a>En säkerhet för flera nivåer metoden tooAzure AD lösenord

Den här artikeln beskrivs några metoder du kan följa som en användare eller som en administratör tooprotect din Azure Active Directory (AD Azure) eller Account.

 > [!NOTE]
 > Azure AD-administratörer kan återställa användarlösenord med hello vägledning i hello artikel [hello Återställ lösenord för en användare i Azure Active Directory](active-directory-users-reset-password-azure-portal.md).
 >
 > Användare kan återställa sina egna lösenord med hjälp av hello vägledning i hello artikeln [hjälp som jag har glömt mitt Azure AD-lösenord](active-directory-passwords-update-your-own-password.md).
 >

## <a name="password-requirements"></a>Krav på lösenord

Azure AD omfattar hello följande vanliga metoder toosecuring lösenord:

* Krav på lösenordslängd
* Kraven på lösenordskomplexitet
* Regelbundet och periodiskt upphörande av lösenord

Information om lösenordsåterställning i Azure Active Directory finns i avsnittet hello [Azure AD lösenordsåterställning via Självbetjäning för hello IT-proffs](active-directory-passwords.md).

## <a name="azure-ad-password-protections"></a>Skydd med Azure AD-lösenord

Azure AD och hello Microsofts kontosystem använda branschen beprövade närmar sig tooensure säker skydd av användar- och lösenord, inklusive:

* Lösenord som förbjuds dynamiskt
* Smart lösenordsutelåsning

Information om lösenordshantering baserat på aktuella research finns hello whitepaper [lösenord vägledning](http://aka.ms/passwordguidance).

### <a name="dynamically-banned-passwords"></a>Lösenord som förbjuds dynamiskt

Azure AD och Microsoft Accounts skydda lösenordsskydd genom dynamiskt förbjuda vanliga lösenord. hello Azure ID Identity Protection-teamet analyserar regelbundet förbjudna lösenordslistor, vilket förhindrar användare från att välja vanliga lösenord. Den här tjänsten är tillgänglig tooAzure AD och hello Microsoft-konto-kunder.

När du skapar lösenord, är det viktigt för administratörer tooencourage användare toochoose lösenord fraser som innehåller en unik kombination av bokstäver, siffror, tecknen eller orden. Den här metoden kan nästan omöjligt toobe komprometteras men enklare för användare tooremember toomake lösenord.

#### <a name="password-breaches"></a>Överträdelser av lösenord

Microsoft arbetar alltid toostay ett steg i cyber kriminella.

hello Azure AD Identity Protection-teamet analyserar kontinuerligt lösenord som används ofta. Cyber brottslingar också använder liknande strategier tooinform sina attacker, till exempel skapa en [rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) för sprickbildning lösenordshashvärden.

Microsoft analyserar kontinuerligt [dataintrång](https://www.privacyrights.org/data-breaches) toomaintain en lista med dynamiskt uppdaterade förbjudna lösenord, vilket garanterar att sårbara lösenord är förbjuden innan de blir en verklig hot tooAzure AD-kunder. Mer information om våra nuvarande säkerhet åtgärder finns hello [Microsoft Security Intelligence-rapporten](https://www.microsoft.com/security/sir/default.aspx).

### <a name="smart-password-lockout"></a>Smart lösenordsutelåsning

När Azure AD upptäcker ett potentiellt cyber straffrättsliga försök toohack till en användarlösenord, låsa vi hello användarkonto med Smart lösenord kontoutelåsning. Azure AD är utformad toodetermine hello risker som är kopplade till specifika inloggningsessioner. Sedan använder hello senaste säkerhetsdata det använda kontoutelåsning semantik toostop cyber hot.

Om en användare har låsts ute från Azure AD, ser skärmen liknande toohello som följer:

  ![Utelåst från Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)

För andra Microsoft-konton ut skärmen liknande toohello som följer:

  ![Utelåst från ett Microsoft-konto](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Information om lösenordsåterställning i Azure Active Directory finns i avsnittet hello [Azure AD lösenordsåterställning via Självbetjäning för hello IT-proffs](active-directory-passwords.md).

  >[!NOTE]
  >Om du är en Azure AD-administratör vill du kanske toouse [Windows Hello](https://www.microsoft.com/windows/windows-hello) tooavoid med dina användare skapa traditionella lösenord helt och hållet.
  >

## <a name="next-steps"></a>Nästa steg

* [Hur tooupdate ditt eget lösenord](active-directory-passwords-update-your-own-password.md)
* [hello grunderna i Azure Identitetshantering](fundamentals-identity.md)
* [Återställningsaktivitet för rapporten på lösenord](active-directory-passwords-reporting.md)


