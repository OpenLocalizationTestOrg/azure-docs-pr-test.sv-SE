---
title: "aaaView Azure-resurs åtkomst tilldelningar | Microsoft Docs"
description: "Visa och hantera alla hello rollbaserad åtkomstkontroll tilldelningar för alla användare eller grupp i hello Azure-portalen"
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: jeffsta
ms.assetid: e6f9e657-8ee3-4eec-a21c-78fe1b52a005
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: andredm
ms.openlocfilehash: ec96c9d4b9e2cb4925949b8bac78767bd564c8ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-access-assignments-for-users-and-groups-in-hello-azure-portal"></a>Visa åtkomst-tilldelningar för användare och grupper i hello Azure-portalen
> [!div class="op_single_selector"]
> * [Hantera åtkomst enligt användare eller grupp](role-based-access-control-manage-assignments.md)
> * [Hantera åtkomst enligt resurs](role-based-access-control-configure.md)

Med rollbaserad åtkomstkontroll (RBAC) i hello Azure Active Directory (Azure AD), kan du hantera åtkomst tooyour Azure resurser. 

Åtkomst tilldelas med RBAC är detaljerad eftersom du kan begränsa hello behörigheter på två sätt:

* **Omfattning:** RBAC rolltilldelningar är begränsade tooa specifika prenumerationen, resursgruppen eller resursen. En användare får åtkomst tooa enskild resurs kan inte komma åt andra resurser i hello samma prenumeration.
* **Roll:** inom hello omfattning hello tilldelning åtkomst minskar även ytterligare genom att tilldela en roll. Roller kan vara hög nivå som ägare eller specifika som virtuella läsare.

Roller kan bara tilldelas från inom hello prenumeration, resursgrupp eller resurs som hello omfånget för hello tilldelning. Men du kan visa alla hello åtkomst tilldelningar för en viss användare eller grupp i en enda plats. Du kan ha upp too2000 rolltilldelningar i varje prenumeration. 

Hämta mer information om hur för[använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](role-based-access-control-configure.md).

## <a name="view-access-assignments"></a>Visa åtkomst tilldelningar
toolook in hello åtkomst tilldelningar för en enskild användare eller grupp, startar i Azure Active Directory i hello [Azure-portalen](http://portal.azure.com).

1. Välj **Azure Active Directory**. Om det här alternativet inte visas i listan navigering väljer **fler tjänster** och bläddrar sedan nedåt toofind **Azure Active Directory**.
2. Välj **användare och grupper**, och sedan antingen **alla användare** eller **alla grupper**. I det här exemplet fokuserar vi på enskilda användare.
    ![Hantera användare och grupper i Azure Active Directory – skärmbild](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Sök efter hello användare efter namn eller användarnamn.
4. Välj **Azure-resurser** hello användaren bladet. Alla hello åtkomst tilldelningar för användaren visas.

### <a name="read-permissions-tooview-assignments"></a>Läsbehörighet tooview tilldelningar
Den här sidan visar endast hello åtkomst tilldelningar att du har behörighet tooread. Till exempel har läsbehörighet toosubscription A och gå toohello Azure-resurser bladet toocheck tilldelningar för en användare. Du kan se sin åtkomst tilldelningar för prenumerationen A, men inte kan se att hon också har åtkomst tilldelningar för prenumerationen B.

## <a name="delete-access-assignments"></a>Ta bort åtkomst tilldelningar
Från det här bladet kan du ta bort åtkomst tilldelningar som har tilldelats direkt tooa användare eller grupp. Om hello åtkomst tilldelning ärvdes från en överordnad grupp, måste toogo toohello resurs eller en prenumeration och hantera det hello tilldelning.

1. Hello lista över alla hello åtkomst tilldelningar för en användare eller grupp, Välj hello en du vill toodelete.
2. Välj **ta bort** och sedan **Ja** tooconfirm.
    ![Ta bort åtkomst tilldelning – skärmbild](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="next-steps"></a>Nästa steg

* Kom igång med rollbaserad åtkomstkontroll för[använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](role-based-access-control-configure.md)
* Se hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md)

