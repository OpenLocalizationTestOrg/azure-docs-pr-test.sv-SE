---
title: "Hantera grupper som gruppen tillhör i Azure Active Directory | Microsoft Docs"
description: "Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur du hanterar dessa medlemskap."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: e785c2d0-7724-47d4-a56e-c58280c08a14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 08e04a6590176c4084ca47b4bd6cbb22500eca2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Hantera vilka grupper som en grupp tillhör i Azure Active Directory-klient
Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur du hanterar dessa medlemskap.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Hur hittar jag min grupp är medlem i grupperna?
1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. På den **användare och grupper** bladet väljer **alla grupper**.

   ![Öppna bladet grupper](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. På den **användare och grupper – alla grupper** bladet Välj en grupp.
5. På den **grupp - *groupname***  bladet väljer **gruppmedlemskap**.

   ![Öppna bladet för medlemskap](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. Att lägga till gruppen som en medlem i en annan grupp på den **Group - gruppmedlemskap** bladet väljer den **Lägg till** kommando.
7. Väljer du en grupp i **Välj grupp** bladet och välj sedan den **Välj** längst ned på bladet. Du kan lägga till din grupp bara en grupp i taget. Den **användaren** rutan filtrerar baserat på matchning inmatningen till alla delar av namnet på en användare eller enhet. Inga jokertecken godkänns i rutan.

   ![Lägg till en gruppmedlemskap](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Ta bort gruppen som en medlem i en annan grupp på den **Group - gruppmedlemskap** bladet Välj en grupp.
9. På den ***groupname*** bladet väljer den **ta bort** kommando och bekräfta valet i Kommandotolken.

   ![ta bort medlemskap kommandot](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. När du har ändrat medlemskap för gruppen, Välj **spara**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Skapa en ny grupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](active-directory-groups-members-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
