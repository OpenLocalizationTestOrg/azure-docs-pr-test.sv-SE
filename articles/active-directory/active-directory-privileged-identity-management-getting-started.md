---
title: "aaaGet igång med Azure AD Privileged Identity Management | Microsoft Docs"
description: "Lär dig hur toomanage Privilegierade identiteter med hello Azure Active Directory Privileged Identity Management-program i Azure-portalen."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: a89205023a8dbcc3649fa732735ca927e64736ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Börja använda Azure AD Privileged Identity Management
Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation. Det här området innehåller åtkomst tooresources i Azure AD och andra Microsoft online services som Office 365 eller Microsoft Intune.

Den här artikeln visar hur tooadd hello Azure AD PIM app tooyour Azure portalens instrumentpanel.

## <a name="add-hello-privileged-identity-management-application"></a>Lägg till hello Privileged Identity Management-program
Innan du använder Azure AD Privileged Identity Management måste tooadd hello programmet tooyour Azure portalens instrumentpanel.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/) som global administratör i din katalog.
2. Om din organisation har mer än en katalog, väljer du ditt användarnamn i hello övre högra hörnet av hello Azure-portalen. Välj hello katalog där du vill toouse PIM.
3. Välj **fler tjänster** och använda hello Filter textruta toosearch för **Azure AD Privileged Identity Management**.
4. Kontrollera **PIN-kod toodashboard** och klicka sedan på **skapa**. hello programmet Privileged Identity Management öppnas.

Om du använder hello första personen toouse Azure AD Privileged Identity Management i din katalog, tilldelas du automatiskt hello **säkerhetsadministratör** och **administratör av Privilegierade roller** roller i hello directory. Endast privilegierade rolladministratörer kan hantera rolltilldelningar för användare. Dessutom kan du välja toorun hello [säkerhetsguiden.](active-directory-privileged-identity-management-security-wizard.md) som vägleder dig genom hello inledande identifiering och tilldelning upplevelse.

## <a name="navigate-tooyour-tasks"></a>Navigera tooyour uppgifter
När Azure AD Privileged Identity Management har konfigurerats, finns hello navigering bladet varje gång du öppnar programmet hello. Använd det här bladet tooaccomplish hanteringsuppgifter din identitet.

![Toppnivåaktiviteter för PIM - skärmbild](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

* **Min roller** tar tooa lista över roller som är tilldelade tooyou. Du aktiverar alla roller som du är berättigad till i det här avsnittet.
* **Godkänna förfrågningar (förhandsgranskning)** visar en lista över väntande aktiveringsförfrågningar från användare i din katalog. [Läs mer.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
* **Väntande begäranden (förhandsgranskning)** visar alla aktuella begäranden toohave gjorts tooactivate.
* **Granska åtkomst** tar tooany väntande åtkomst granskar du behöver toocomplete, om du granskar åtkomst för dig själv eller någon annan.
* **Azure AD Directory roller** finns under hello ”hantera” avsnittet är hello instrumentpanel för privilegierade rollen administratörer toomanage rolltilldelningar, ändra inställningar för aktivering av rollen, start åtkomst omdömen och mycket mer. hello alternativ i den här instrumentpanelen är inaktiverad för alla som inte är en administratör av Privilegierade roller.

## <a name="next-steps"></a>Nästa steg
Hej [översikt över Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) innehåller mer information om hur du kan hantera administrativ åtkomst i din organisation.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
