---
title: "aaaAssign en användare eller grupp tooan enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur tooselect tooassign en enterprise-appen en användare eller grupp tooit i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5817ad48-d916-492b-a8d0-2ade8c50a224
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 86c11f19892b9c947a5331677c17759178ed2806
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-a-user-or-group-tooan-enterprise-app-in-azure-active-directory"></a>Tilldela en användare eller grupp tooan enterprise-app i Azure Active Directory
Det är enkelt tooassign en användare eller en grupp tooyour företagsprogram i Azure Active Directory (AD Azure). Du måste ha en hello behörighet toomanage hello och du måste vara global administratör för hello-katalogen.

## <a name="how-do-i-assign-user-access-tooan-enterprise-app"></a>Hur tilldelar användaren åtkomst tooan enterprise app?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange Azure Active Directory i hello och väljer sedan **RETUR**.
3. På hello **Azure Active Directory - *directoryname***  bladet (det vill säga hello Azure AD bladet för hello-katalog som du hanterar) Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)
4. På hello **företagsprogram** bladet väljer **alla program**. Visas en lista över hello-appar som du kan hantera.
5. På hello **företagsprogram - alla program** bladet väljer du en app.
6. På hello ***appname*** bladet (det vill säga hello bladet med hello namn för valda hello-appen i hello rubrik) Välj **användare och grupper**.

    ![Att välja hello kommandot för alla program](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)
7. På hello ***appname*** **-användaren & grupptilldelning** bladet, Välj hello **Lägg till** kommando.
8. På hello **Lägg uppdrag** bladet väljer **användare och grupper**.

    ![Tilldela en användare eller grupp toohello app](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)
9. På hello **användare och grupper** bladet, Välj en eller flera användare eller grupper från hello och sedan väljer hello **Välj** knappen längst ned hello hello-bladet.
10. På hello **Lägg uppdrag** bladet väljer **rollen**. Klicka sedan på hello **Välj roll** bladet, Välj en roll tooapply toohello markerade användare eller grupper och välj sedan hello **OK** knappen längst ned hello hello-bladet.
11. På hello **Lägg uppdrag** bladet, Välj hello **tilldela** knappen längst ned hello hello-bladet. hello tilldelad användare eller grupper har hello behörigheter som definieras av hello valda rollen för den här enterprise-appen.

## <a name="next-steps"></a>Nästa steg
* [Visa alla mina grupper](active-directory-groups-view-azure-portal.md)
* [Ta bort en användare eller grupp från en enterprise-app](active-directory-coreapps-remove-assignment-azure-portal.md)
* [Inaktivera användarinloggningar för en enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Ändra hello namn eller logotyp av en enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
