---
title: "Azure AD Connect: Komma igång med standardinställningarna | Microsoft Docs"
description: "Lär dig hur toodownload, installera och köra hello installationsguiden för Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: curtand
ms.assetid: b6ce45fd-554d-4f4d-95d1-47996d561c9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 79f796fa7738b85e9236e856bddb529379f60390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Komma igång med Azure AD Connect med standardinställningar
**Standardinställningar** för Azure AD Connect används om du har en topologi med en enda skog och om du använder [lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) för autentisering. **Standardinställningar** hello standardalternativet och används för hello distribueras oftast scenario. Du är bara några få klick bort tooextend din lokala katalog toohello moln.

Kontrollera innan du börjar installera Azure AD Connect, för[hämta Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) och krav för fullständig hello stegen i [Azure AD Connect: maskin- och](active-directory-aadconnect-prerequisites.md).

Om standardinställningarna inte passar din topologi läser du den [relaterade dokumentationen](#related-documentation) för andra scenarier.

## <a name="express-installation-of-azure-ad-connect"></a>Snabbinstallation av Azure AD Connect
Du kan se dessa steg i praktiken i hello [videor](#videos) avsnitt.

1. Logga in som lokal administratör toohello server du vill tooinstall Azure AD Connect på. Du bör göra detta på hello-server du vill toobe hello synkroniseringsserver.
2. Navigera tooand dubbelklicka **AzureADConnect.msi**.
3. Markerar hello rutan godkänna toohello licensvillkoren och klickar på välkomstskärmen hello **Fortsätt**.  
4. Hello Express inställningar, klickar du på i **Använd standardinställningar**.  
   ![Välkommen tooAzure AD Connect](./media/active-directory-aadconnect-get-started-express/express.png)
5. Ange hello användarnamn och lösenord för en global administratör för din Azure AD hello Anslut tooAzure AD skärmen. Klicka på **Nästa**.  
   ![Ansluta tooAzure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) om du får ett felmeddelande och har problem med anslutningen, se [Felsöka anslutningsproblem](active-directory-aadconnect-troubleshoot-connectivity.md).
6. Ange hello användarnamn och lösenord för ett företagsadministratörskonto hello Anslut tooAD DS skärmen. Du kan ange hello domändelen i NetBios- eller FQDN-format, d.v.s. FABRIKAM\administrator eller fabrikam.com\administrator. Klicka på **Nästa**.  
   ![Ansluta tooAD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Hej [ **inloggningskonfiguration för Azure AD** ](active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) sidan visar bara om du inte slutförde [domänverifieringen](../active-directory-add-domain.md) i hello [krav](active-directory-aadconnect-prerequisites.md).
   ![Overifierade domäner](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
   Om den här sidan visas granskar du alla domäner som har markerats med **Inte tillagd** och **Inte verifierad**. Kontrollera att de domäner som du använder har verifierats i Azure AD. Klicka på symbolen för hello uppdatera när du har verifierat dina domäner.
8. Hello redo tooconfigure skärmen klickar du på **installera**.
   * Hello redo tooconfigure på sidan kan du om du vill avmarkera hello **starta hello synkroniseringsprocessen så snart som konfigurationen är klar** kryssrutan. Du måste avmarkera kryssrutan om du vill toodo ytterligare konfiguration, som [filtrering](active-directory-aadconnectsync-configure-filtering.md). Om du avmarkerar det här alternativet hello konfigurerar guiden synkronisering men lämnar hello Schemaläggaren inaktiverad. Det kan inte köras förrän du aktiverar den manuellt genom [köra installationsguiden för hello](active-directory-aadconnectsync-installation-wizard.md).
   * Om du har Exchange i din lokala Active Directory, så att du har också ett alternativ tooenable [ **Exchange-hybridinstallation**](https://technet.microsoft.com/library/jj200581.aspx). Aktivera det här alternativet om du planerar toohave Exchange-postlådor både i hello molnet och lokalt på hello samma tid.
     ![Redo tooconfigure Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. När hello installationen är klar klickar du på **avsluta**.
10. När hello-installationen har slutförts, logga ut och logga in igen innan du använder Synchronization Service Manager eller Synchronization Rule Editor.

## <a name="videos"></a>Videoklipp
En video om hur du använder hello Snabbinstallation finns:

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Express-Settings/player]
> 
> 

## <a name="next-steps"></a>Nästa steg
Nu när du har Azure AD Connect installeras kan du [verifiera hello installationen och tilldela licenser](active-directory-aadconnect-whats-next.md).

Mer information om dessa funktioner, som aktiverades med installationen hello: [automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md), [förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md), och [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Mer information om dessa vanliga avsnitt: [Schemaläggaren och hur tootrigger synkronisera](active-directory-aadconnectsync-feature-scheduler.md).

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).

## <a name="related-documentation"></a>Relaterad dokumentation
| Avsnitt |
| --- | --- |
| Översikt över Azure AD Connect |
| Installera med anpassade inställningar |
| Uppgradera från DirSync |
| Konton som används för installation |

