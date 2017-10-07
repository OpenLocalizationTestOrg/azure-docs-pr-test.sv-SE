---
title: "aaaAdd nya användare tooAzure Active Directory | Microsoft Docs"
description: "Förklarar hur tooadd nya användare i Azure Active Directory."
services: active-directory
documentationcenter: 
author: jeffgilb
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 6ca413c84a7a5238a30fd26fc751d687d827b24a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-new-users-tooazure-active-directory"></a>Snabbstart: Lägga till nya användare tooAzure Active Directory
Den här artikeln förklarar hur tooadd nya användare i din organisation i hello Azure Active Directory (AD Azure) en i taget med hello Azure-portalen eller genom att synkronisera lokala Windows Server AD-kontot data. 

## <a name="add-cloud-based-users"></a>Lägga till molnbaserade användare
1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **Azure Active Directory** och sedan **användare och grupper**.
3. På hello **användare och grupper** bladet väljer **alla användare**, och välj sedan **ny användare**.
   ![Att välja hello tilläggskommando](./media/add-users-azure-active-directory/add-user.png)
4. Ange information om hello användaren som **namn** och **användarnamn**. Hej domännamndelen för hello användarnamn måste antingen vara hello inledande standard domain name ”[namn].onmicrosoft.com” eller en verifierad ofedererad [domännamn](add-custom-domain.md) , till exempel ”contoso.com”.
5. Kopiera eller på annat sätt Obs hello genereras användarlösenord så att du kan ange den toohello användaren när processen har slutförts.
6. Du kan också öppna och fylla i hello information i hello **profil** bladet, hello **grupper** bladet eller hello **Directory rollen** bladet för hello användare. Mer information om användar- och administratörsroller finns i [Tilldela administratörsroller i Azure AD](active-directory-assign-admin-roles.md).
7. På hello **användare** bladet väljer **skapa**.
8. Distribuera hello genererade lösenordet toohello ny användare på ett säkert sätt så att hello användare kan logga in.

> [!TIP]
> Du kan också synkronisera konto användardata från lokala Windows Server AD. Microsofts identitetslösningar omfattar lokala och molnbaserade funktioner, och skapa en enda användaridentitet för autentisering och auktorisering tooall resurser, oavsett plats. Vi kallar detta Hybrididentitet. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) kan vara används toointegrate dina lokala kataloger med Azure Active Directory hybrid identity scenarier. Detta gör att du tooprovide en gemensam identitet för dina användare för Office 365 och Azure SaaS-program som är integrerade med Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Ta bort användare från Azure AD
1. Logga in toohello [Azure Active Directory Administrationscenter](https://aad.portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **användare och grupper**.
3. På hello **användare och grupper** bladet, Välj hello användaren toodelete hello-listan. 
4. På hello bladet för valda hello-användaren väljer **översikt**, och välj sedan i kommandofältet hello **ta bort**.
   ![Att välja hello tilläggskommando](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Läs mer 
* [Lägg till en extern användare](active-directory-users-create-external-azure-portal.md)

* [Tilldela en användarroll tooa i din Azure AD](active-directory-users-assign-role-azure-portal.md)

## <a name="next-steps"></a>Nästa steg
I den här snabbstarten du har lärt dig hur tooadd nya användare tooAzure AD Premium. 

Du kan använda hello följande länk toocreate en ny användare i Azure AD från hello Azure-portalen.

> [!div class="nextstepaction"]
> [Lägg till användare tooAzure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
