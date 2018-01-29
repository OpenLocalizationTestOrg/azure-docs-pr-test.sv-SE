---
title: "Hantera medlemmar för en grupp i Azure Active Directory | Microsoft Docs"
description: "Lägga till eller ta bort användare och enheter från en grupp i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: 49c975aa34f28c6c00591435726e538bb79a78cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Hantera gruppmedlemskap för användare i din Azure Active Directory-klient
Den här artikeln förklarar hur du hanterar medlemmar för en grupp i Azure Active Directory (AD Azure).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Hur gör hitta medlemmarna och hantera dem?
1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. På den **användare och grupper** bladet väljer **alla grupper**.

   ![Öppna bladet grupper](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. På den **användare och grupper – alla grupper** bladet Välj en grupp.
5. På den **grupp - *groupname***  bladet väljer **medlemmar**.

   ![Öppna bladet medlemmar](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Lägga till medlemmar i gruppen på den **grupp - medlemmar** bladet väljer **lägga till medlemmar**.

   ![Lägga till medlemmar kommando](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. På den **medlemmar** bladet, Välj en eller flera användare eller enheter att lägga till i gruppen och välj den **Välj** längst ned på bladet för att lägga till dem i gruppen. Den **användaren** rutan filtrerar baserat på matchning inmatningen till alla delar av namnet på en användare eller enhet. Inga jokertecken godkänns i rutan.
8. Ta bort medlemmar från gruppen på den **grupp - medlemmar** bladet Välj en medlem.
9. På den ***membername*** bladet väljer den **ta bort** kommando och bekräfta valet i Kommandotolken.

   ![ta bort medlemmar kommandot](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. När du har ändrat medlemmar för gruppen, Välj **spara**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Skapa en ny grupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
