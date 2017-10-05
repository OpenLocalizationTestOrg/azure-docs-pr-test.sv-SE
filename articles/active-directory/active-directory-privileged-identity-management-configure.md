---
title: Konfigurera Azure AD Privileged Identity Management | Microsoft Docs
description: "Ett avsnitt som förklarar vad Azure AD Privileged Identity Management är och hur du förbättrar säkerheten för molnet med hjälp av PIM."
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
ms.openlocfilehash: eb7059368cb80be7dd625f9dc6ad2aab1bad709a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-privileged-identity-management"></a>Vad är Azure AD Privileged Identity Management?
Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation. Detta inkluderar åtkomst till resurser i Azure AD och andra Microsoft online-tjänster som Office 365 eller Microsoft Intune.  

> [!NOTE]
> Privileged Identity Management är tillgänglig för hela organisationen när du licensierar dina administratörer med Premium P2-versionen av Azure Active Directory. Mer information finns i [Azure Active Directory-versioner](active-directory-editions.md).

Organisationer som vill minimera antalet personer som har åtkomst till säker information eller resurser, eftersom som minskar risken för en obehörig användare komma att åtkomsten. Användare behöver dock fortfarande att utföra Privilegierade åtgärder i Azure, Office 365 eller SaaS-appar. Organisationer som ger användare privilegierad åtkomst i Azure AD utan övervakning vad användarna gör med deras administratörsrättigheter. Azure AD Privileged Identity Management kan lösa denna risk.  

Azure AD Privileged Identity Management kan du:  

* Se vilka användare som är administratörer för Azure AD
* Aktivera på begäran, just-in-time ”administrativ åtkomst till Microsofts onlinetjänster som Office 365 och Intune
* Hämta rapporter om administratören åtkomsthistorik och ändringar i administratörstilldelningar
* Få meddelanden om åtkomst till en privilegierad roll
* Kräv godkännande för att aktivera (förhandsgranskning)

Azure AD Privileged Identity Management kan hantera inbyggt Azure AD organisationens roller, inklusive (men inte begränsat till):  

* Global administratör
* Faktureringsadministratör
* Tjänstadministratör  
* Användare med rollen
* Lösenordsadministratör

## <a name="just-in-time-administrator-access"></a>Precis i tid administratörsåtkomst
Tidigare kan du tilldela en användare till en administratörsroll via den klassiska Azure-portalen eller Windows PowerShell. Därför blir en **permanent admin**alltid är aktiva i den tilldelade rollen. Azure AD Privileged Identity Management introducerar konceptet för en **berättigade admin**. Berättigad administratörer ska användare som behöver privilegierad åtkomst då och då, men inte varje dag. Rollen är inaktiv tills användaren behöver åtkomst, och sedan de slutföra aktiveringsprocessen och bli administratör active för en förinställd tidsperiod.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Aktivera Privileged Identity Management för din katalog
Du kan börja använda Azure AD Privileged Identity Management i den [Azure-portalen](https://portal.azure.com/).

> [!NOTE]
> Du måste vara en global administratör med ett organisationskonto (till exempel @yourdomain.com), inte ett Microsoft-konto (till exempel @outlook.com), för att aktivera Azure AD Privileged Identity Management för en katalog.

1. Logga in på [Azure-portalen](https://portal.azure.com/) som global administratör för din katalog.
2. Om din organisation har mer än en katalog väljer du ditt användarnamn längst upp till höger på Azure-portalen. Välj den katalog där du ska använda Azure AD Privileged Identity Management.
3. Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.
4. Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**. Privileged Identity Management-programmet öppnas.

Om du är den första som använder Azure AD Privileged Identity Management i din katalog kommer [säkerhetsguiden](active-directory-privileged-identity-management-security-wizard.md) att vägleda dig genom den första tilldelningen. Därefter blir du automatiskt först **säkerhetsadministratör** och **administratör av Privilegierade roller** av katalogen.

Endast en administratör av Privilegierade roller kan hantera åtkomst till andra administratörer. Du kan [ge andra användare möjlighet att hantera i PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-admin-dashboard"></a>Privileged Identity Management admin-instrumentpanelen
Azure AD Privileged Identity Manager innehåller en admin-instrumentpanel som innehåller viktig information såsom:

* Aviseringar som påpekas möjligheter att öka säkerheten
* Antalet användare som har tilldelats varje privilegierad roll  
* Antalet berättigade och permanenta administratörer
* Ett diagram över aktiveringar av Privilegierade roller i din katalog

![PIM instrumentpanelen – skärmbild][2]

## <a name="privileged-role-management"></a>Hantering av Privilegierade roller
Med Azure AD Privileged Identity Management kan du hantera administratörer genom att lägga till eller ta bort permanent eller berättigade administratörer för varje roll.

![PIM Lägg till/ta bort systemadministratörer – skärmbild][3]

## <a name="configure-the-role-activation-settings"></a>Konfigurera inställningar för roll-aktivering
Med hjälp av den [rollinställningar](active-directory-privileged-identity-management-how-to-change-default-settings.md) du kan konfigurera egenskaper för aktivering berättigade roll, inklusive:

* Varaktighet för aktiveringsperioden roll
* Rollaktiveringsmeddelande
* Informationen om en användare måste ange under aktiveringen roll
* Biljett eller incident automatiskt
* [Krav för godkännande arbetsflöde - förhandsgranskning](./privileged-identity-management/azure-ad-pim-approval-workflow.md)

![PIM inställningar - aktivering av administratör – skärmbild][4]

Observera att i bild knapparna för **Multifaktorautentisering** har inaktiverats. För vissa, mycket Privilegierade roller, vi kräver MFA för förhöjd skydd.

## <a name="role-activation"></a>Rollaktivering
Att [aktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md), administratör berättigade begär en Tidsbundna ”aktivering” för rollen. Aktiveringen kan begäras med hjälp av den **aktivera min roll** alternativ i Azure AD Privileged Identity Management.

En administratör som vill aktivera en roll måste initiera Azure AD Privileged Identity Management i Azure-portalen.

Rollaktivering kan anpassas. I PIM-inställningar kan du ange längden på aktivering och vad information som administratören behöver för att aktivera rollen.

![PIM administratör begäran rollaktivering – skärmbild][5]

## <a name="review-role-activity"></a>Granska rollen aktiviteten
Det finns två sätt att spåra hur dina anställda och administratörer använder Privilegierade roller. Med hjälp av alternativet först [Directory roller granskningshistorik](active-directory-privileged-identity-management-how-to-use-audit-log.md). Granskningshistoriken loggar spåra ändringar i Privilegierade rolltilldelningar och historik för aktivering av rollen.

![Historik för aktivering av PIM - skärmbild][6]

Det andra alternativet är att konfigurera vanliga [åtkomst till granskningar](active-directory-privileged-identity-management-how-to-start-security-review.md). Rapporterna åtkomst kan utföras av och tilldelade granskare (till exempel ett team manager) eller anställda kan granska sig själva. Detta är det bästa sättet att övervaka som kräver fortfarande åtkomst och som inte längre finns.

## <a name="azure-ad-pim-at-subscription-expiration"></a>Azure AD PIM på prenumerationen upphör att gälla
Innan du når allmän tillgänglighet Azure AD PIM var i förhandsversionen och det fanns ingen licens kontroller för en klient att förhandsgranska Azure AD PIM.  Nu när Azure AD PIM har nått allmän tillgänglighet, måste utvärderingsversion eller betald licenser tilldelas till administratörer för klienten att fortsätta använda PIM.  Om din organisation inte köpa Azure AD Premium P2 eller din utvärderingsversion upphör att gälla, vara främst alla Azure AD PIM-funktioner inte längre tillgängliga i din klient.  Du kan läsa mer i den [krav för Azure AD PIM-prenumeration](./privileged-identity-management/subscription-requirements.md)

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Admin_Overview.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_Settings_w_Approval_Disabled.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
