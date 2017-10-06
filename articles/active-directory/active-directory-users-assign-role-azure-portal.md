---
title: "aaaAssign en användare tooadministrator roller i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur användaren toochange administrativ information i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: 8bb6711c2570843962fe075b6026542a4bca525f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-tooadministrator-roles-in-azure-active-directory"></a>Tilldela en användare tooadministrator roller i Azure Active Directory
Den här artikeln förklarar hur tooassign en administrativ roll tooa användare i Azure Active Directory (AD Azure). Information om att lägga till nya användare i din organisation finns i [lägga till nya användare tooAzure Active Directory](active-directory-users-create-azure-portal.md). Tillagda användare har inte administratörsbehörighet som standard, men du kan tilldela roller toothem när som helst.

## <a name="assign-a-role-tooa-user"></a>Tilldela en roll tooa användare
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. På hello **användare och grupper** bladet väljer **alla användare**.

   ![Öppna hello bladet för alla användare](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. På hello **användare och grupper – alla användare** bladet Välj en användare hello listan.
5. På hello bladet för valda hello-användaren väljer **Directory rollen**, och tilldela sedan hello användarrollen tooa hello **Directory rollen** lista. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).

      ![Tilldela en användarroll för tooa](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Välj **Spara**.

## <a name="next-steps"></a>Nästa steg
* [Lägga till en användare](active-directory-users-create-azure-portal.md)
* [Återställa en användares lösenord i hello nya Azure-portalen](active-directory-users-reset-password-azure-portal.md)
* [Ändra en användares arbetsinformation](active-directory-users-work-info-azure-portal.md)
* [Hantera användarprofiler](active-directory-users-profile-azure-portal.md)
* [Tar bort en användare i din Azure AD](active-directory-users-delete-user-azure-portal.md)
