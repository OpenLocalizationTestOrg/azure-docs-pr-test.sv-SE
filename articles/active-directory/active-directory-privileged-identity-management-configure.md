---
title: aaaConfigure Azure AD Privileged Identity Management | Microsoft Docs
description: "Ett avsnitt som förklarar vad Azure AD Privileged Identity Management är och hur toouse PIM tooimprove moln-säkerhet."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: c548ed2e-06e3-4eaf-a63d-0f02ee72da25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: dbe49fe4a0f6e5b46ed5a17fc7e8dcdacafe3846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Vad är Azure AD Privileged Identity Management?
Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation. Detta inkluderar åtkomst tooresources i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.  

> [!NOTE]
> Privileged Identity Management är tillgängliga tooyour hela organisationen när du licensierar dina administratörer med hello Premium P2-versionen av Azure Active Directory. Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).

Organisationer som vill toominimize hello antalet personer som har åtkomst till toosecure information och resurser för eftersom som minskar hello risken för en obehörig användare komma att åtkomsten. Användare behöver dock fortfarande toocarry Privilegierade åtgärder i Azure, Office 365 eller SaaS-appar. Organisationer som ger användare privilegierad åtkomst i Azure AD utan övervakning vad användarna gör med deras administratörsrättigheter. Azure AD Privileged Identity Management hjälper tooresolve denna risk.  

Azure AD Privileged Identity Management kan du:  

* Se vilka användare som är administratörer för Azure AD
* Aktivera på begäran, just-in-time ”administrativ åtkomst tooMicrosoft onlinetjänster som Office 365 och Intune
* Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar
* Få meddelanden om åtkomst tooa Privilegierade roller
* Kräv godkännande tooactivate (förhandsgranskning)

Azure AD Privileged Identity Management kan hantera hello inbyggda Azure AD organisationens roller, inklusive (men inte begränsat till):  

* Global administratör
* Faktureringsadministratör
* Tjänstadministratör  
* Användare med rollen
* Lösenordsadministratör

## <a name="just-in-time-administrator-access"></a>Precis i tid administratörsåtkomst
Tidigare kan du tilldela en tooan admin användarroll via hello klassiska Azure-portalen eller Windows PowerShell. Därför blir en **permanent admin**alltid är aktiva i hello som har tilldelats rollen. Azure AD Privileged Identity Management introducerar hello begreppet en **berättigade admin**. Berättigad administratörer ska användare som behöver privilegierad åtkomst då och då, men inte varje dag. hello-rollen är inaktiv tills hello användare behöver åtkomst, och sedan de slutföra aktiveringsprocessen och bli administratör active för en förinställd tidsperiod.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Aktivera Privileged Identity Management för din katalog
Du kan börja använda Azure AD Privileged Identity Management i hello [Azure-portalen](https://portal.azure.com/).

> [!NOTE]
> Du måste vara en global administratör med ett organisationskonto (till exempel @yourdomain.com), inte ett Microsoft-konto (till exempel @outlook.com), tooenable Azure AD Privileged Identity Management för en katalog.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) som global administratör i din katalog.
2. Om din organisation har mer än en katalog, väljer du ditt användarnamn i hello övre högra hörnet av hello Azure-portalen. Välj hello katalog där du ska använda Azure AD Privileged Identity Management.
3. Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.
4. Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**. hello programmet Privileged Identity Management öppnas.

Om du använder hello första personen toouse Azure AD Privileged Identity Management i din katalog, sedan hello [säkerhetsguiden](active-directory-privileged-identity-management-security-wizard.md) vägleder dig genom hello första tilldelningsupplevelse. Därefter blir du automatiskt hello först **säkerhetsadministratör** och **administratör av Privilegierade roller** för hello katalog.

Endast en administratör av Privilegierade roller kan hantera åtkomst till andra administratörer. Du kan [ge andra användare hello möjlighet toomanage i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management admin-instrumentpanelen
Azure AD Privileged Identity Manager innehåller en admin-instrumentpanel som innehåller viktig information såsom:

* Aviseringar som påpekas affärsmöjligheter tooimprove säkerhet
* hello antalet användare som är tilldelade tooeach Privilegierade roller  
* hello antalet berättigade och permanenta administratörer
* Ett diagram över aktiveringar av Privilegierade roller i din katalog

![PIM instrumentpanelen – skärmbild][2]

## <a name="privileged-role-management"></a>Hantering av Privilegierade roller
Med Azure AD Privileged Identity Management kan du hantera hello administratörer genom att lägga till eller ta bort permanent eller är tillämpliga tooeach administratörsrollen.

![PIM Lägg till/ta bort systemadministratörer – skärmbild][3]

## <a name="configure-hello-role-activation-settings"></a>Konfigurera hello produktaktivering rollinställningar
Med hjälp av hello [rollinställningar](active-directory-privileged-identity-management-how-to-change-default-settings.md) du kan konfigurera hello berättigade aktivering rollegenskaper inklusive:

* hello rollen aktiveringsperioden hello varaktighet
* Hej rollaktiveringsmeddelande
* hello information en användare måste tooprovide under aktiveringen för hello roll
* Biljett eller incident automatiskt
* [Krav för godkännande arbetsflöde - förhandsgranskning](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM inställningar - aktivering av administratör – skärmbild][4]

Observera att hello hello bild knappar för **Multifaktorautentisering** har inaktiverats. För vissa, mycket Privilegierade roller, vi kräver MFA för förhöjd skydd.

## <a name="role-activation"></a>Rollaktivering
för[aktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md), administratör berättigade begär en Tidsbundna ”aktivering” för hello roll. hello aktiveringen kan begäras med hello **aktivera min roll** alternativ i Azure AD Privileged Identity Management.

En administratör som vill ha tooactivate en roll måste tooinitialize Azure AD Privileged Identity Management i hello Azure-portalen.

Rollaktivering kan anpassas. I hello PIM-inställningar, kan du ange hello hello aktivering och vilken information Hej administratör måste tooprovide tooactivate hello roll.

![PIM administratör begäran rollaktivering – skärmbild][5]

## <a name="review-role-activity"></a>Granska rollen aktiviteten
Det finns två sätt tootrack hur dina anställda och administratörer använder Privilegierade roller. med hjälp av hello första alternativet [Directory roller granskningshistorik](active-directory-privileged-identity-management-how-to-use-audit-log.md). hello granskningshistorik loggar spåra ändringar i Privilegierade rolltilldelningar och historik för aktivering av rollen.

![Historik för aktivering av PIM - skärmbild][6]

hello andra alternativ är tooset upp vanliga [åtkomst till granskningar](active-directory-privileged-identity-management-how-to-start-security-review.md). Rapporterna åtkomst kan utföras av och tilldelade granskare (till exempel ett team manager) eller hello anställda kan granska sig själva. Detta är hello bästa sätt toomonitor som kräver fortfarande åtkomst och som inte längre.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM på prenumerationen upphör att gälla
Tidigare tooreaching allmän tillgänglighet Azure AD PIM var i förhandsversionen och det fanns ingen licens söker efter klient-toopreview Azure AD PIM.  Nu när Azure AD PIM har nått allmän tillgänglighet, tilldelas utvärderingsversion eller betald licenser toohello administratörer av hello klient toocontinue med PIM.  Om din organisation inte köpa Azure AD Premium P2 eller din utvärderingsversion upphör att gälla, vara främst alla hello Azure AD PIM-funktioner inte längre tillgängliga i din klient.  Du kan läsa mer i hello [krav för Azure AD PIM-prenumeration](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
