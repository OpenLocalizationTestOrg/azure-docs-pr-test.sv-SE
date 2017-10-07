---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare eller ändrar användarinformation i Azure Active Directory."
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
ms.openlocfilehash: c4a156ea31b81202bb0d0ac224afbfc3f1785532
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-new-users-tooazure-active-directory"></a>Lägga till nya användare tooAzure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-users-create-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-create-users.md)
>
>

Den här artikeln förklarar hur tooadd nya användare i din organisation i hello Azure Active Directory (AD Azure). 

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användare och grupper](./media/active-directory-users-create-azure-portal/create-users-user-management.png)
3. På hello **användare och grupper** bladet väljer **alla användare**, och välj sedan **Lägg till**.

   ![Att välja hello tilläggskommando](./media/active-directory-users-create-azure-portal/create-users-add-command.png)
4. Ange information om hello användaren som **namn** och **användarnamn**. Hej domännamndelen för hello användarnamn måste antingen vara hello inledande domain name ”foo.onmicrosoft.com” standarddomännamnet eller verifierad, ofedererad domännamn, till exempel ”contoso.com”.
5. Kopiera eller på annat sätt Obs hello genereras användarlösenord så att du kan ange den toohello användaren när processen har slutförts.
6. Du kan också öppna och fylla i hello information i hello **profil** bladet, hello **grupper** bladet eller hello **Directory rollen** bladet för hello användare. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).
7. På hello **användare** bladet väljer **skapa**.
8. Distribuera hello genererade lösenordet toohello ny användare på ett säkert sätt så att hello användare kan logga in.

### <a name="next-steps"></a>Nästa steg
* [Lägg till en extern användare](active-directory-users-create-external-azure-portal.md)
* [Återställa en användares lösenord i hello nya Azure-portalen](active-directory-users-reset-password-azure-portal.md)
* [Ändra en användares arbetsinformation](active-directory-users-work-info-azure-portal.md)
* [Hantera användarprofiler](active-directory-users-profile-azure-portal.md)
* [Tar bort en användare i din Azure AD](active-directory-users-delete-user-azure-portal.md)
* [Tilldela en användarroll tooa i din Azure AD](active-directory-users-assign-role-azure-portal.md)
