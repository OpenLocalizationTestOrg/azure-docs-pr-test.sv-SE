---
title: aaaShare Azure portal instrumentpaneler med RBAC | Microsoft Docs
description: "Den här artikeln förklarar hur tooshare en instrumentpanel i hello Azure-portalen med hjälp av rollbaserad åtkomstkontroll."
services: azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 8908a6ce-ae0c-4f60-a0c9-b3acfe823365
ms.service: multiple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2016
ms.author: tomfitz
ms.openlocfilehash: b12f9f8582596ee14aa8bfdfb4772cc139e3bf45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="share-azure-dashboards-by-using-role-based-access-control"></a>Dela Azure instrumentpaneler med hjälp av rollbaserad åtkomstkontroll
När du har konfigurerat en instrumentpanel kan du publicera den och dela den med andra användare i din organisation. Du tillåta andra tooview din instrumentpanel med hjälp av Azure [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md). Du tilldelar en användare eller grupp av användare tooa roll och rollen definierar om dessa användare kan visa eller ändra hello publicerade instrumentpanelen. 

Alla publicerade instrumentpaneler implementeras som Azure-resurser, vilket innebär att de finns som hanterbara objekt inom din prenumeration och ingår i en resursgrupp.  Ur ett access control skiljer instrumentpaneler sig vissa resurser, till exempel en virtuell dator eller ett lagringskonto.

> [!TIP]
> Enskilda paneler på hello instrumentpanelen tillämpa sina egna krav på åtkomstkontroll baserat på hello resurser som de visas.  Därför kan du utforma en instrumentpanel som delas brett samtidigt skyddas hello data på enskilda paneler.
> 
> 

## <a name="understanding-access-control-for-dashboards"></a>Förstå behörighet för instrumentpaneler
Med rollbaserad åtkomstkontroll (RBAC), kan du tilldela användare tooroles på tre olika nivåer av omfång:

* prenumeration
* Resursgrupp
* Resursen

hello behörigheter som du tilldelar ärvs från prenumerationen ned toohello resurs. hello publicerade instrumentpanelen är en resurs. Därför kanske redan användare som är tilldelade tooroles för hello prenumeration som fungerar även för hello publicerade instrumentpanelen. 

Här är ett exempel.  Anta att du har en Azure-prenumeration och olika medlemmarna i gruppen har tilldelats hello roller i **ägare**, **deltagare**, eller **reader** för hello prenumeration. Användare som är ägare eller deltagare är kan toolist, visa, skapa, ändra eller ta bort instrumentpaneler inom hello prenumeration.  Användare som är läsare kan inte är kan toolist och visa instrumentpaneler, men ändra eller ta bort dem.  Användare med åtkomst läsaren är kan toomake lokala redigeringar tooa publicerade instrumentpanelen (t.ex, när du felsöker ett problem), men är inte toopublish dessa ändringar tillbaka toohello server.  De har också hello alternativet toomake en privat kopia av hello instrumentpanelen för sig själva

Du kan också tilldela behörigheter toohello resursgruppen som innehåller flera instrumentpaneler eller tooan enskild instrumentpanel. Du kan till exempel bestämma att en grupp användare bör ha begränsad behörighet över hello prenumeration, men större tooa viss instrumentpanelen. Du kan tilldela dessa användare tooa roll för instrumentpanelen. 

## <a name="publish-dashboard"></a>publicera instrumentpanelen
Anta att du har konfigurerat en instrumentpanel som du vill tooshare med en grupp användare i din prenumeration. hello stegen nedan beskriver en anpassad grupp som heter Storage Manager, men du kan också namnge din grupp vad du vill. Information om att skapa en Active Directory-gruppen och lägger till användare toothat gruppen finns [hantera grupper i Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. I hello instrumentpanelen, väljer **resursen**.
   
     ![Välj resurs](./media/azure-portal-dashboard-share-access/select-share.png)
2. Innan du tilldelar åtkomst, måste du publicera hello instrumentpanelen. Som standard hello instrumentpanelen ska vara publicerade tooa resursgrupp med namnet **instrumentpaneler**. Välj **publicera**.
   
     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Instrumentpanelen publiceras nu. Om hello behörigheter ärvs från hello prenumeration är lämpligt kan behöver du inte toodo något mer. Andra användare i din organisation kommer att kunna tooaccess och ändra hello instrumentpanelen med syftet för prenumerationen. Men för den här kursen ska vi tilldela en grupp av användare tooa roll för instrumentpanelen.

## <a name="assign-access-tooa-dashboard"></a>Tilldela tooa instrumentpanelen
1. När du har publicerat hello instrumentpanelen, Välj **hantera användare**.
   
     ![hantera användare](./media/azure-portal-dashboard-share-access/manage-users.png)
2. Du kommer se en lista över befintliga användare som redan har tilldelats en roll för den här instrumentpanelen. I listan över befintliga användare kommer att olika än hello bilden nedan. Troligen ärvs hello tilldelningar från hello prenumeration. tooadd en ny användare eller grupp, Välj **Lägg till**.
   
     ![Lägg till användare](./media/azure-portal-dashboard-share-access/existing-users.png)
3. Välj hello roll som representerar hello behörigheter som du vill att toogrant. Det här exemplet väljer du **deltagare**.
   
     ![Välj rollen](./media/azure-portal-dashboard-share-access/select-role.png)
4. Välj hello användaren eller gruppen som du vill tooassign toohello roll. Använd hello sökrutan Om du inte ser hello användare eller grupp som du söker efter i hello-listan. I listan över tillgängliga grupper beror på hello grupper som du har skapat i Active Directory.
   
     ![Välj användare](./media/azure-portal-dashboard-share-access/select-user.png) 
5. När du har lagt till användare eller grupper, Välj **OK**. 
6. hello ny tilldelning läggs toohello lista över användare. Observera att dess **åtkomst** anges som **tilldelad** snarare än **ärvda**.
   
     ![tilldelade roller](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## <a name="next-steps"></a>Nästa steg
* En lista över roller finns [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md).
* toolearn om hur du hanterar resurser, se [hantera Azure-resurser via portalen](resource-group-portal.md).

