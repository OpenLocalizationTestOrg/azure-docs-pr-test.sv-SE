---
title: "Ge åtkomst till Privileged Identity Management - Azure | Microsoft Docs"
description: "Lär dig mer om att lägga till roller för användare med Azure Active Directory Privileged Identity Management-tillägget så att de kan hantera PIM."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d4c53b53-2b37-41e6-813c-96ec08a1c897
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 39caeef2648730194827e04e020d8eaea5414f4f
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="giving-access-to-manage-azure-ad-privileged-identity-management"></a>Ger tillgång till att hantera Azure AD Privileged Identity Management
Den globala administratören som aktiverar Azure AD Privileged Identity Management (PIM) för en organisation automatiskt få rolltilldelningar och åtkomst till PIM. Ingen annan får skrivåtkomst som standard, men även andra globala administratörer. Andra globala administratörer, säkerhetsadministratörer och säkerhet läsare har skrivskyddad åtkomst till Azure AD PIM. Om du vill ge åtkomst till PIM, kan den första användaren tilldela andra användare till den **administratör av Privilegierade roller** roll. Den här tilldelningen måste göras i PIM sig själv och kan inte ändras via PowerShell eller andra portaler.

> [!NOTE]
> Hantera Azure AD PIM kräver Azure MFA. Eftersom Microsoft-konton inte kan registrera dig för Azure MFA, när en användare loggar in med ett Microsoft-konto kan inte komma åt Azure AD PIM.
> 
> 

Kontrollera att det finns alltid minst två användare i en privilegierad roll administratörsroll om en användare är utelåst eller deras kontot har tagits bort.

## <a name="give-another-user-access-to-manage-pim"></a>Ge en annan användare tillgång till att hantera PIM
1. Logga in på den [Azure-portalen](https://portal.azure.com/) och välj den **Azure AD Privileged Identity Management** app på instrumentpanelen.
2. Välj **hantera Privilegierade roller** > **administratör av Privilegierade roller** > **Lägg till**.
   
    ![Lägg till Privilegierade rollen administratörer – skärmbild][1]
3. Steg 1 har redan slutförts på bladet Lägg till hanterade användare. Välj steg 2, **Välj användare** och Sök efter den användare som du vill lägga till.
   
    ![Välj användare – skärmbild][2]
4. Välj användaren i sökresultaten och på **klar**.
5. Klicka på **OK** att spara ditt val. Användare som du har valt visas i listan över Privilegierade rollen administratörer.
   
   * När du tilldelar en ny roll till någon konfigureras de automatiskt som kvalificerade att aktivera rollen. Klicka på användare i listan om du vill göra dem permanent i rollen. Välj **Se behörighet** i menyn användaren information.
6. Skicka till användarna en länk till [komma igång med Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Ta bort en annan användare behörighet som krävs för att hantera PIM
Innan du tar bort någon från en administratör för privilegierade roller du alltid kontrollera kommer fortfarande att två användare som är tilldelade till den.

1. I PIM-instrumentpanelen, klickar du på rollen **administratör av Privilegierade roller**.  Listan över användare för närvarande i rollen visas.
2. Klicka på användare i användarlistan.
3. Klicka på **ta bort**.  Visas ett bekräftelsemeddelande.
4. Klicka på **Ja** att ta bort användaren från rollen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nästa steg
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
