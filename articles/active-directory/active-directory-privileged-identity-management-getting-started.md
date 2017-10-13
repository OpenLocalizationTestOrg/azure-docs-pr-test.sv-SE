---
title: "Komma igång med Azure AD Privileged Identity Management | Microsoft Docs"
description: "Lär dig hur du hanterar privilegierade identiteter med programmet Azure Active Directory Privileged Identity Management på Azure-portalen."
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.assetid: 2299db7d-bee7-40d0-b3c6-8d628ac61071
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: 0f9f09ca8fb30d494433ed8d26b808d1b5d4d0b1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="start-using-azure-ad-privileged-identity-management"></a>Börja använda Azure AD Privileged Identity Management

Med Azure Active Directory (AD) Privileged Identity Management kan du hantera, kontrollera och övervaka åtkomst inom din organisation. Det här omfånget inkluderar åtkomst till resurser i Azure-resurser, Azure AD och andra Microsoft onlinetjänster som Office 365 eller Microsoft Intune.

Den här artikeln visar hur du lägger till appen Azure AD PIM på instrumentpanelen på Azure-portalen.

## <a name="add-the-privileged-identity-management-application"></a>Lägga till programmet Privileged Identity Management

Innan du använder Azure AD Privileged Identity Management måste du lägga till programmet på instrumentpanelen på Azure-portalen.

1. Logga in på [Azure-portalen](https://portal.azure.com/) som global administratör för din katalog.
2. Om din organisation har mer än en katalog väljer du ditt användarnamn längst upp till höger på Azure-portalen. Välj den katalog där du vill använda PIM.
3. Välj **Fler tjänster** och använd textrutan Filter för att söka efter **Azure AD Privileged Identity Management**.
4. Markera **Fäst på instrumentpanelen** och klicka sedan på **Skapa**. Privileged Identity Management-programmet öppnas.

Om du är den första personen som använder Azure AD Privileged Identity Management i din katalog så tilldelas du automatiskt rollerna **Säkerhetsadministratör** och **Privilegierad rolladministratör** i katalogen. Endast privilegierade rolladministratörer kan hantera rolltilldelningar för användare i Azure AD-katalogen. Dessutom kan du välja att köra [Säkerhetsguiden.](active-directory-privileged-identity-management-security-wizard.md) som från grunden lär dig hur du identifierar och tilldelar.

## <a name="navigate-to-your-tasks"></a>Gå till dina uppgifter

När Azure AD Privileged Identity Management har konfigurerats ser du alltid navigeringsbladet när du öppnar programmet. Använd det här bladet för att utföra dina identitetshanteringsaktiviteter.

![Toppnivåaktiviteter för PIM - skärmbild](./media/active-directory-privileged-identity-management-getting-started/PIM_Tasks_New.png)

- **My Roles** (Mina roller) visar en lista över tillgängliga och aktiva roller som är tilldelade till dig. Här kan du aktivera tilldelade berättigade roller.
- **Godkänna förfrågningar (förhandsversion)** visar en lista över förfrågningar om att aktivera berättigade Azure AD-katalogroller av användare i din katalog som du ska godkänna. [Läs mer.](./privileged-identity-management/azure-ad-pim-approval-workflow.md)
- **Väntande förfrågningar (förhandsversion)** visar väntande förfrågningar om att aktivera kvalificerade rolltilldelningar.
- **Granska åtkomst** visar alla aktiva åtkomstgranskningar som du måste slutföra, oavsett om du granskar åtkomst åt dig själv eller någon annan.
- **Azure AD-katalogroller** finns under avsnittet ”Hantera” på den vänstra navigeringsmenyn och visar instrumentpanelen för privilegierade rolladministratörer. Där kan du hantera rolltilldelningar, ändra rollaktiveringsinställningar, starta åtkomstgranskningar och mycket mer. Instrumentpanelen är inaktiverad för alla som inte är en privilegierad rolladministratör. De här användarna har åtkomst till en särskild instrumentpanel som heter My view (Min vy). Instrumentpanelen My view (Min vy) visar endast information om användaren som har åtkomst till instrumentpanelen, inte hela klientorganisationen.
- **Azure Resource roles (Preview)** (Azure-resursroller (förhandsversion)) under hanteringsavsnittet på vänster navigeringsmeny visar en lista över prenumerationsresurser som du har rolltilldelningar för 

## <a name="next-steps"></a>Nästa steg
[Azure AD Privileged Identity Management-översikt](active-directory-privileged-identity-management-configure.md) innehåller mer information om hur du kan hantera administrativ åtkomst i din organisation.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
