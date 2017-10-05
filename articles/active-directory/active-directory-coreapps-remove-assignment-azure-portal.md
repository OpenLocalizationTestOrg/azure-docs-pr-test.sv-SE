---
title: "Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory | Microsoft Docs"
description: "Hur du tar bort tilldelningen åtkomst av en användare eller grupp från en enterprise-app i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7b2d365b-ae92-477f-9702-353cc6acc5ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 02f122acfb53c2107e2b0af66c6195aa127a2c77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory"></a>Ta bort en användare eller grupp från en enterprise-app i Azure Active Directory
Det är enkelt att ta bort en användare eller grupp tilldelas åtkomst till en enterprise-program i Azure Active Directory (AD Azure). Du måste ha behörighet att hantera enterprise-appen och du måste vara global administratör för katalogen.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Hur tar jag bort en användare eller grupptilldelning?
1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **Azure Active Directory** i textrutan och välj sedan **RETUR**.
3. På den **Azure Active Directory - *directoryname***  bladet (det vill säga Azure AD bladet för den katalog som du hanterar), Välj **företagsprogram**.

    ![Öppna företagsappar](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)
4. På den **företagsprogram** bladet väljer **alla program**. Visas en lista över appar som du kan hantera.
5. På den **företagsprogram - alla program** bladet väljer du en app.
6. På den ***appname*** bladet (det vill säga bladet med namnet på den valda appen i namnet), Välj **användare och grupper**.

    ![Att välja användare eller grupper](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)
7. På den ***appname*** **-användaren & grupptilldelning** bladet Välj en av flera användare eller grupper och välj sedan den **ta bort** kommando. Bekräfta beslutet i Kommandotolken.

    ![Att välja kommandot Ta bort](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Nästa steg
* [Visa alla mina grupper](active-directory-groups-view-azure-portal.md)
* [Tilldela en användare eller grupp till en enterprise-app](active-directory-coreapps-assign-user-azure-portal.md)
* [Inaktivera användarinloggningar för en enterprise-app](active-directory-coreapps-disable-app-azure-portal.md)
* [Ändra namnet eller logotypen av en enterprise-app](active-directory-coreapps-change-app-logo-user-azure-portal.md)
