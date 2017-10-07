---
title: "aaaRole-baserad åtkomstkontroll i hello Azure-portalen | Microsoft Docs"
description: "Kom igång med åtkomsthantering med rollbaserad åtkomstkontroll i hello Azure-portalen. Använda rollen tilldelningar tooassign behörigheter tooyour resurser."
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: b87e00089b0fc93fb212b318330a6f22bfbf59e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-role-based-access-control-toomanage-access-tooyour-azure-subscription-resources"></a>Använda rollbaserad åtkomstkontroll toomanage åtkomst tooyour Azure-prenumerationsresurser
> [!div class="op_single_selector"]
> * [Hantera åtkomst enligt användare eller grupp](role-based-access-control-manage-assignments.md)
> * [Hantera åtkomst enligt resurs](role-based-access-control-configure.md)

Rollbaserad åtkomstkontroll (RBAC) i Azure ger tillgång till ingående åtkomsthantering för Azure. Med RBAC kan bevilja du endast hello mängd åtkomst att användarna måste tooperform sitt arbete. Den här artikeln hjälper dig att komma igång med RBAC i hello Azure-portalen. Mer information om hur RBAC kan hjälpa dig att hantera åtkomsten finns i [Vad är rollbaserad åtkomstkontroll?](role-based-access-control-what-is.md)

Inom varje prenumeration kan du bevilja too2000 rolltilldelningar. 

## <a name="view-access"></a>Visa åtkomst
Du kan se vem som har åtkomst tooa resurs, resursgrupp eller prenumeration från dess huvudblad i hello [Azure-portalen](https://portal.azure.com). Vi vill till exempel toosee som har åtkomst till tooone av våra resursgrupper:

1. Välj **resursgrupper** i hello hello vänstra navigeringsfältet.  
    ![Resursgrupper - ikon](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Välj hello namnet på hello resursgruppen från hello **resursgrupper** bladet.
3. Välj **åtkomstkontroll (IAM)** hello vänstra menyn.  
4. hello Access control bladet visar en lista över alla användare, grupper och program som har beviljats åtkomst toohello resursgruppen.  
   
    ![Skärmbild av bladet Användare – ärvd eller tilldelad åtkomst](./media/role-based-access-control-configure/view-access.png)

Observera att vissa roller är begränsade för**resursen** medan andra är **ärvda** den från ett annat omfång. Åtkomst tilldelas specifikt toohello resursgruppen eller ärvs från en tilldelning toohello överordnade prenumerationen.

> [!NOTE]
> Klassiska prenumerationsadministratörer och medadministratörer betraktas som ägare av hello-prenumeration i hello nya RBAC-modellen.

## <a name="add-access"></a>Lägga till åtkomst
Du beviljar åtkomst inifrån hello resursen, resursgruppen eller prenumerationen som är hello omfattning hello rolltilldelning.

1. Välj **Lägg till** på hello Access control-bladet.  
2. Välj hello rollen tooassign från hello gärna **Välj en roll** bladet.
3. Välj hello användare, grupp eller program i katalogen som du vill toogrant åtkomst till. Du kan söka hello katalogen med visningsnamn, e-postadresser och objektidentifierare.  
   
    ![Skärmbild av bladet Lägg till användare – sök](./media/role-based-access-control-configure/grant-access2.png)
4. Välj **OK** toocreate hello tilldelning. Hej **lägger till användare** popup spårar hello pågår.  
    ![Lägga till användarförloppsindikatorn - skärmbild](./media/role-based-access-control-configure/addinguser_popup.png)

När du har lagt till en rolltilldelning, visas den på hello **användare** bladet.

## <a name="remove-access"></a>Ta bort åtkomst
1. Håll markören över hello namnet på hello tilldelning som du vill tooremove. En kryssruta visas nästa toohello namn.
2. Använd hello kryssrutorna tooselect en eller flera rolltilldelningar.
2. Välj **Ta bort**.  
3. Välj **Ja** tooconfirm hello borttagning.

Ärvda tilldelningar kan inte tas bort. Om du behöver tooremove en ärvd tilldelning måste toodo den på hello omfång där hello rolltilldelning skapades. I hello **omfång** kolumn, nästa för**ärvda** finns en länk som tar dig toohello resurser där den här rollen har tilldelats. Gå toohello resurs i listan tooremove hello rolltilldelning.

![Skärmbild av bladet Användare – ärvd åtkomst inaktiverar knappen Ta bort](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-toomanage-access"></a>Andra verktyg toomanage åtkomst
Du kan tilldela roller och hantera åtkomst med Azure RBAC-kommandon i andra verktyg än hello Azure-portalen.  Följ hello länkar toolearn mer om hello förutsättningar och kom igång med hello Azure RBAC-kommandon.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Azure kommandoradsgränssnittet](role-based-access-control-manage-access-azure-cli.md)
* [REST-API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Nästa steg
* [Skapa en rapport över åtkomständringshistorik](role-based-access-control-access-change-history-report.md)
* Se hello [inbyggda RBAC-roller](role-based-access-built-in-roles.md)
* Definiera egna [anpassade roller i Azure RBAC](role-based-access-control-custom-roles.md)

