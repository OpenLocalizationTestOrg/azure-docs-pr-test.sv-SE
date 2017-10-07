---
title: "aaaManage hello grupper som gruppen tillhör tooin Azure Active Directory | Microsoft Docs"
description: "Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur toomanage dessa medlemskap."
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
ms.openlocfilehash: d0a0a1967084de0968e1e802559f9cdfd7ca6ae4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-toowhich-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Hantera toowhich grupper som en grupp tillhör i Azure Active Directory-klient
Grupper kan innehålla andra grupper i Azure Active Directory. Här är hur toomanage dessa medlemskap.

## <a name="how-do-i-find-hello-groups-my-group-is-a-member-of"></a>Hur hittar hello grupper min grupp är medlem i?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
3. På hello **användare och grupper** bladet väljer **alla grupper**.

   ![Öppna hello grupper bladet](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
4. På hello **användare och grupper – alla grupper** bladet Välj en grupp.
5. På hello **grupp - *groupname***  bladet väljer **gruppmedlemskap**.

   ![Öppna hello blad för medlemskap](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
6. tooadd din grupp som en medlem i en annan grupp på hello **Group - gruppmedlemskap** bladet, Välj hello **Lägg till** kommando.
7. Välj en grupp i hello **Välj grupp** bladet och välj sedan hello **Välj** knappen längst ned hello hello-bladet. Du kan lägga till din grupp tooonly en grupp i taget. Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten. Inga jokertecken godkänns i rutan.

   ![Lägg till en gruppmedlemskap](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. tooremove din grupp som en medlem i en annan grupp på hello **Group - gruppmedlemskap** bladet Välj en grupp.
9. På hello ***groupname*** bladet, Välj hello **ta bort** kommando och bekräfta valet hello i Kommandotolken.

   ![ta bort medlemskap kommandot](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. När du har ändrat medlemskap för gruppen, Välj **spara**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Skapa en ny grupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](active-directory-groups-members-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
