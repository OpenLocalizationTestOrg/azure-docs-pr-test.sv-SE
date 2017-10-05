---
title: "Lägga till nya användare i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur du lägger till nya användare eller ändrar användarinformation i Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: bfe0c556d94d50207a23d2e3984371fb602e9406
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-new-users-to-azure-active-directory"></a>Lägga till nya användare i Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-create-users.md)
>
>

Den här artikeln förklarar hur du lägger till nya användare i din organisation i Azure Active Directory (AD Azure). 

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användare och grupper](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. På den **användare och grupper** bladet väljer **alla användare**, och välj sedan **Lägg till**.

   ![Att välja kommandot Lägg till](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Ange information för användaren, till exempel **namn** och **användarnamn**. Domännamndelen för användarnamn måste antingen vara inledande domain name ”foo.onmicrosoft.com” standarddomännamnet eller verifierad, ofedererad domännamn, till exempel ”contoso.com”.
5. Kopiera eller annars Observera genererade användarens lösenord så att du kan ange den till användaren när processen har slutförts.
6. Du kan också öppna och fylla i informationen i den **profil** bladet den **grupper** bladet eller **Directory rollen** bladet för användaren. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).
7. På den **användare** bladet väljer **skapa**.
8. På ett säkert sätt distribuera genererade lösenordet till den nya användaren så att användaren kan logga in.

### <a name="next-steps"></a>Nästa steg
* [Lägg till en extern användare](active-directory-users-create-external-azure-portal.md)
* [Återställa en användares lösenord i den nya Azure-portalen](active-directory-users-reset-password-azure-portal.md)
* [Ändra en användares arbetsinformation](active-directory-users-work-info-azure-portal.md)
* [Hantera användarprofiler](active-directory-users-profile-azure-portal.md)
* [Tar bort en användare i din Azure AD](active-directory-users-delete-user-azure-portal.md)
* [Tilldela en användare till en roll i din Azure AD](active-directory-users-assign-role-azure-portal.md)
