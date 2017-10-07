---
title: "aaaCreate en grupp för användare i Azure Active Directory | Microsoft Docs"
description: "Hur toocreate en grupp i Azure Active Directory och lägga till medlemmar toohello grupp"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc583a7f02ce50e7f3b2c8f97a9c032a3e2dc33a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Skapa en grupp och lägga till medlemmar i Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

Den här artikeln förklarar hur toocreate och fylla i en ny grupp i Azure Active Directory. Använd en grupp tooperform hanteringsuppgifter, till exempel tilldela licenser eller behörigheter tooa antalet användare eller enheter på samma gång.

## <a name="how-do-i-create-a-group"></a>Hur skapar jag en grupp?
1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.
2. Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.

   ![Öppna användarhantering](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. På hello **användare och grupper** bladet väljer **alla grupper**.

   ![Öppna hello grupper bladet](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. På hello **användare och grupper – alla grupper** bladet, Välj hello **Lägg till** kommando.

   ![Att välja hello tilläggskommando](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. På hello **grupp** bladet Lägg till ett namn och beskrivning för hello grupp.
6. tooselect medlemmar tooadd toohello gruppen väljer **tilldelad** i hello **medlemskapstypen** och välj sedan **medlemmar**. Mer information om hur toomanage hello medlemskap i en grupp dynamiskt finns [genom att använda attribut toocreate avancerade regler för gruppmedlemskap](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Att välja medlemmar tooadd](./media/active-directory-groups-create-azure-portal/select-members.png)
7. På hello **medlemmar** bladet, Välj en eller flera användare eller enheter tooadd toohello gruppen och välj hello **Välj** knappen längst ned hello hello bladet tooadd dem toohello grupp. Hej **användaren** filtrerar hello visas baserat på matchning post tooany-tillhör ett namn för användaren eller enheten. Inga jokertecken godkänns i rutan.
8. När du har lagt till medlemmar toohello gruppen, Välj **skapa** på hello **grupp** bladet.    

   ![Skapa grupp bekräftelse](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Nästa steg
Dessa artiklar innehåller ytterligare information om Azure Active Directory.

* [Se befintliga grupper](active-directory-groups-view-azure-portal.md)
* [Hantera inställningar för en grupp](active-directory-groups-settings-azure-portal.md)
* [Hantera medlemmar i en grupp](active-directory-groups-members-azure-portal.md)
* [Hantera medlemskap i en grupp](active-directory-groups-membership-azure-portal.md)
* [Hantera dynamiska regler för användare i en grupp](active-directory-groups-dynamic-membership-azure-portal.md)
