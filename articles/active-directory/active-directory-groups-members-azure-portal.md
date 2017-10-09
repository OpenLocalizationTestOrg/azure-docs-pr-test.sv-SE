---
title: "aaaManage hello medlemmar för en grupp i Azure Active Directory | Microsoft Docs"
description: "Hur tooadd eller ta bort användare och enheter från en grupp i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d399a97d-fd2a-4b2d-b73d-0975db83f41b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4cb16ee63828003da251423a04736f7174dd4896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Hantera gruppmedlemskap för användare i din Azure Active Directory-klient
Den här artikeln förklarar hur toomanage hello medlemmar för en grupp i Azure Active Directory (AD Azure).

## <a name="how-do-i-find-hello-members-and-manage-them"></a>Hur gör hitta hello medlemmar och hantera dem?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. På hello **användare och grupper** bladet väljer **alla grupper**.

   ![Öppna hello grupper bladet](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. På hello **användare och grupper – alla grupper** bladet Välj en grupp.
5. På hello **grupp - *groupname***  bladet väljer **medlemmar**.

   ![Öppna hello medlemmar bladet](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. tooadd medlemmar toohello grupp på hello **grupp - medlemmar** bladet väljer **lägga till medlemmar**.

   ![Lägga till medlemmar kommando](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. På hello **medlemmar** bladet, Välj en eller flera användare eller enheter tooadd toohello gruppen och välj hello **Välj** knappen längst ned hello hello bladet tooadd dem toohello grupp. Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten. Inga jokertecken godkänns i rutan.
8. tooremove medlemmar från hello gruppen på hello **grupp - medlemmar** bladet Välj en medlem.
9. På hello ***membername*** bladet, Välj hello **ta bort** kommando och bekräfta valet hello i Kommandotolken.

   ![ta bort medlemmar kommandot](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. När du har ändrat medlemmar för hello grupp, Välj **spara**.

## <a name="additional-information"></a>Ytterligare information
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Skapa en ny grupp och lägga till medlemmar](active-directory-groups-create-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
