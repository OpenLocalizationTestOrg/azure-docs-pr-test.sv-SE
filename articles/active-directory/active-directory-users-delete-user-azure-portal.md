---
title: "Tar bort en användare från en katalog i Azure Active Directory | Microsoft Docs"
description: "Beskriver hur du tar bort en användare och alla dess information från Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bd1c9ccc-2d80-42bf-82cc-db37bb1a1d67
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: curtand;jeffsta
ms.reviewer: jeffsta
ms.openlocfilehash: 19a4d2c37c785b3b56a2e0e1b6797ec56dad9835
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="delete-a-user-from-a-directory-in-azure-active-directory"></a>Tar bort en användare från en katalog i Azure Active Directory
Den här artikeln beskriver hur du tar bort en användare från en katalog i Azure Active Directory (AD Azure). Information om att lägga till nya användare i din organisation finns [lägga till nya användare i Azure Active Directory](active-directory-users-create-azure-portal.md).

## <a name="to-delete-a-user"></a>Ta bort en användare
1. Logga in på [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-users-delete-user-azure-portal/create-users-user-management.png)
3. På den **användare och grupper** bladet väljer **användare**.

   ![Öppna bladet användare](./media/active-directory-users-delete-user-azure-portal/create-users-open-users-blade.png)
4. På bladet **Användare och grupper – användare** väljer du en användare i listan.
5. På bladet för den valda användaren väljer **översikt**, och välj sedan i kommandofältet **ta bort**.

    ![Att välja kommandot Delete](./media/active-directory-users-delete-user-azure-portal/create-users-delete-command.png)

## <a name="next-steps"></a>Nästa steg
* [Lägga till nya användare i Azure Active Directory](active-directory-users-create-azure-portal.md)
* [Återställa lösenordet för en användare i Azure Active Directory](active-directory-users-reset-password-azure-portal.md)
* [Tilldela administratörsroller i Azure Active Directory en användare](active-directory-users-assign-role-azure-portal.md)
* [Lägga till eller ändra profilinformation för en användare i Azure Active Directory](active-directory-users-work-info-azure-portal.md)
* [Tar bort en användare från en katalog i Azure Active Directory](active-directory-users-profile-azure-portal.md)
