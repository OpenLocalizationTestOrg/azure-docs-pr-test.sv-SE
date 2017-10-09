---
title: "aaaHow tooadd eller ta bort en användarroll | Microsoft Docs"
description: "Lär dig hur hello tooadd roller tooprivileged identiteter med Azure Active Directory Privileged Identity Management-program."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 6a47ced8-cf34-4ce8-bea2-e4fc548cfe22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim;oldportal;it-pro;
ms.openlocfilehash: f84639757dd76061ea12ed6ea7ec9e62ad942109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-privileged-identity-management-how-tooadd-or-remove-a-user-role"></a>Azure AD Privileged Identity Management: Hur tooadd eller ta bort en användarroll
Med Azure Active Directory (AD), en global administratör (eller företagets administratör) kan uppdatera som användare kan **permanent** tilldelade tooroles i Azure AD. Detta görs med PowerShell-cmdlets som `Add-MsolRoleMember` och `Remove-MsolRoleMember`. De kan också använda hello klassiska Azure-portalen enligt beskrivningen i [Tilldela administratörsroller i Azure Active Directory](active-directory-assign-admin-roles.md).

hello programmet Azure AD Privileged Identity Management administratörer Privilegierade roller toomake permanent rolltilldelningar samt. Dessutom Privilegierade rollen administratörer kan utföra **berättigade** för administratörsroller. En berättigad administratör kan aktivera hello roll när det behövs och sedan deras behörigheter ut när de är klar.

## <a name="manage-roles-with-pim-in-hello-azure-portal"></a>Hantera roller med PIM i hello Azure-portalen
I din organisation, kan du tilldela användare toodifferent administrativa roller i Azure AD och Office 365 och andra Microsoft-tjänster och program.  Mer information om hello tillgängliga roller kan hittas på [roller i Azure AD PIM](active-directory-privileged-identity-management-roles.md).

tooadd eller ta bort en användare i en roll med hjälp av Privileged Identity Management registreringspunkter hello PIM-instrumentpanelen. Sedan antingen på hello **användare i administratörsroller** knappen eller väljer en viss roll (till exempel Global administratör) hello roller tabell.

> [!NOTE]
> Om du inte har aktiverat PIM i hello Azure-portalen ännu går för[Kom igång med Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) mer information.

Om du vill toogive till en annan användare åtkomst tooPIM hello roller som PIM kräver hello användaren toohave beskrivs ytterligare i själva [hur toogive åt tooPIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="add-a-user-tooa-role"></a>Lägg till en användarroll för tooa
1. I hello [Azure-portalen](https://portal.azure.com/)väljer hello **Azure AD Privileged Identity Management** rutan på hello instrumentpanel.
2. Välj **hantera Privilegierade roller**.
3. I hello **Rollsammanfattning** tabell, Välj hello roll som du vill använda toomanage.
4. Hello rollen bladet välj **Lägg till**.
5. Klicka på **Välj användare** och Sök efter hello användare på hello **Välj användare** bladet.  
6. Välj hello användaren hello sökresultatet och klicka på **klar**.
7. Klicka på **OK** toosave valet. hello användare som du har valt visas i hello lista som är berättigade till hello roll.

> [!NOTE]
> Nya användare i en roll är bara tillgängliga för hello roll som standard. Om du vill toomake hello rollen permanenta, klickar du på hello användare i hello listan. hello användarens information visas i ett nytt blad. Välj **Se behörighet** hello användaren information-menyn.  
> Om en användare kan registrera dig för Azure Multi-Factor Authentication (MFA) eller använder ett Microsoft-konto (vanligtvis @outlook.com), behöver du toomake dem permanent i deras roller. Berättigad administratörer uppmanas tooregister MFA vid aktivering.

Nu när hello användaren är berättigad till en roll, meddela dem om att de kan aktivera den enligt toohello instruktionerna i [hur tooactivate eller inaktivera en roll](active-directory-privileged-identity-management-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Ta bort en användare från en roll
Du kan ta bort användare från berättigade rolltilldelningar, men kontrollera att det finns alltid minst en användare som är en permanent global administratör.

Följ dessa steg tooremove en viss användare från en roll:

1. Navigera toohello roll i hello rollen listan genom att välja en roll i hello Azure AD PIM instrumentpanelen eller genom att klicka på hello **användare i administratörsroller** knappen.
2. Klicka på hello användare i hello användarlistan.
3. Klicka på **ta bort**. Ett meddelande som ber dig tooconfirm.
4. Klicka på **Ja** tooremove hello roll från hello användare.

Om du inte vet vilka användarna måste fortfarande sina rolltilldelningar sedan kan du [startar en åtkomst-granskning för hello rollen](active-directory-privileged-identity-management-how-to-start-security-review.md).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

