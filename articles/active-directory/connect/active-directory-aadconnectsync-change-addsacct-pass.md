---
title: "Azure AD Connect-synkronisering: ändra hello AD DS-kontolösenord | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooupdate Azure AD Connect efter hello lösenord för hello AD DS-konto har ändrats."
services: active-directory
keywords: "AD DS-konto, Active Directory-konto, lösenord"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a>Ändra lösenordet för hello AD DS-konto
hello AD DS-konto refererar toohello användarkontot som används av Azure AD Connect toocommunicate med lokala Active Directory. Om du ändrar hello lösenordet för hello AD DS-konto, måste du uppdatera Azure AD Connect-synkroniseringstjänsten med hello nytt lösenord. Annars hello synkronisering kan inte längre synkronisera korrekt med hello lokala Active Directory och du kommer märka hello följande fel:

* I hello Synchronization Service Manager, alla importera eller exportera igen med lokala AD misslyckas med **inga start autentiseringsuppgifter** fel.

* Under Windows Loggboken hello programmets händelselogg innehåller ett fel med **händelse-ID 6000** och meddelandet **'hello hanteringsagenten ”contoso.com” misslyckade toorun eftersom hello autentiseringsuppgifter var ogiltiga'** .


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a>Hur tooupdate hello synkroniseringstjänsten med nytt lösenord för AD DS-konto
tooupdate hello synkroniseringstjänsten med hello nytt lösenord:

1. Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).
</br>![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Gå toohello **kopplingar** fliken.

3. Välj hello **AD-koppling** som motsvarar toohello AD DS-konto som dess lösenord har ändrats.

4. Under **åtgärder**väljer **egenskaper**.

5. I popup-dialogrutan hello väljer **ansluta tooActive Directory-skog**:

6. Ange hello nya lösenord för hello AD DS-konto i hello **lösenord** textruta.

7. Klicka på **OK** toosave hello nytt lösenord och Stäng hello popup-fönstret.

8. Starta om hello Azure AD Connect-synkroniseringstjänsten under Windows Service Control Manager. Detta är tooensure som alla referens toohello gamla lösenord tas bort från cache-minnet hello.

## <a name="next-steps"></a>Nästa steg
**Översiktsavsnitt**

* [Azure AD Connect-synkronisering: Förstå och anpassa synkronisering](active-directory-aadconnectsync-whatis.md)

* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
