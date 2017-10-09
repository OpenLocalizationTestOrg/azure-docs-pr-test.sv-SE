---
title: aaaUse Azure portal toomanage Azure-resurser | Microsoft Docs
description: "Använd Azure-portalen och Azure Resource Manager toomanage dina resurser. Visar hur toowork med instrumentpaneler toomonitor resurser."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a>Hantera Azure-resurser via portalen
> [!div class="op_single_selector"]
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Portal](resource-group-portal.md) 
> * [REST-API](resource-manager-rest-api.md)
> 
> 

Det här avsnittet visar hur toouse hello [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) toomanage Azure-resurser. toolearn om hur du distribuerar resurser via hello portal finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).

Inte alla tjänsten stöder för närvarande hello-portalen eller Resource Manager. För dessa tjänster måste toouse hello [klassiska portalen](https://manage.windowsazure.com). Hello statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="manage-resource-groups"></a>Hantera resursgrupper

En resursgrupp är en behållare som innehåller relaterade resurser för en Azure-lösning. hello resursgrupp kan innehålla alla hello resurser för hello lösning eller bara de resurser som du vill toomanage som en grupp. Du bestämma hur du vill att tooallocate resurser tooresource grupper baserat på vad som är hello bäst för din organisation. I allmänhet kan lägga till resurser som delar hello samma livscykel toohello samma resursgrupp, så att du enkelt kan distribuera, uppdatera och ta bort dem som en grupp. 

hello resursgruppen lagras metadata om hello resurser. När du anger en plats för hello resursgrupp kan anger du därför där den metadata lagras. Av kompatibilitetsskäl, kanske du måste tooensure som dina data lagras i ett visst område.

1. toosee alla hello resursgrupper i din prenumeration, Välj **resursgrupper**.
   
    ![Bläddra resursgrupper](./media/resource-group-portal/browse-groups.png)
2. Välj toocreate en tom resursgrupp **Lägg till**.
   
    ![Lägg till resursgrupp](./media/resource-group-portal/add-resource-group.png)
3. Ange ett namn och plats för hello ny resursgrupp. Välj **Skapa**.
   
    ![Skapa resursgrupp](./media/resource-group-portal/create-empty-group.png)
4. Du kan behöva tooselect **uppdatera** toosee hello nyligen skapade resursgruppen.
   
    ![Uppdatera resursgruppen.](./media/resource-group-portal/refresh-resource-groups.png)
5. Välj toocustomize hello information som visas för dina resursgrupper **kolumner**.
   
    ![Anpassa kolumner](./media/resource-group-portal/select-columns.png)
6. Välj hello kolumner tooadd och välj sedan **uppdatering**.
   
    ![lägga till kolumner](./media/resource-group-portal/add-columns.png)
7. toolearn om hur du distribuerar resurser tooyour ny resursgrupp finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).
8. Du kan fästa hello bladet tooyour instrumentpanelen för Snabbåtkomst tooa resursgruppen.
   
    ![resursgruppen för PIN-kod](./media/resource-group-portal/pin-group.png)
9. hello instrumentpanelen visar hello resursgrupp och dess resurser. Du kan välja hello resursgrupper eller någon av dess resurser toonavigate toohello objekt.
   
    ![resursgruppen för PIN-kod](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Taggen resurser
Du kan använda taggar tooresource grupper och resurser toologically ordna dina tillgångar. Information om hur du arbetar med taggar finns [med hjälp av taggar tooorganize resurserna i Azure](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Övervaka resurser
När du väljer en resurs, visar hello resursbladet standard diagram och tabeller för att övervaka den resurstypen.

1. Välj en resurs och Observera hello **övervakning** avsnitt. Den omfattar diagram som är relevanta toohello resurstypen. hello visar följande bild hello standard övervakningsdata för ett lagringskonto.
   
    ![Visa övervakning](./media/resource-group-portal/show-monitoring.png)
2. Du kan fästa en del av hello bladet tooyour instrumentpanelen genom att välja hello ellips (...) ovanför hello-avsnittet. Du kan också anpassa hello storlek hello avsnitt i hello bladet eller ta bort den helt. hello följande bild visar hur toopin, anpassa eller ta bort hello CPU och minne avsnitt.
   
    ![avsnittet för PIN-kod](./media/resource-group-portal/pin-cpu-section.png)
3. Efter att fästa hello avsnittet toohello instrumentpanelen visas hello sammanfattning på hello instrumentpanel. Och markera den omedelbart tar toomore information om hello data.
   
    ![visa instrumentpanelen](./media/resource-group-portal/view-startboard.png)
4. toocompletely anpassa hello data du övervaka hello-portalen, navigera tooyour standardinstrumentpanelen och välj **ny instrumentpanel**.
   
    ![instrumentpanel](./media/resource-group-portal/dashboard.png)
5. Namnge din nya instrumentpanel och dra paneler till hello instrumentpanelen. hello paneler filtreras efter de olika alternativ.
   
    ![instrumentpanel](./media/resource-group-portal/create-dashboard.png)
   
     toolearn om hur du arbetar med instrumentpaneler, se [skapa och dela instrumentpaneler i hello Azure-portalen](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Hantera resurser
Hello alternativ för att hantera hello resurs visas hello bladet för en resurs som. hello portal anger alternativ för den specifika resurstypen. Du ser hello management kommandon överst hello hello resursbladet och hello vänster.

![Hantera resurser](./media/resource-group-portal/manage-resources.png)

Du kan utföra åtgärder som till exempel starta och stoppa en virtuell dator och konfigurera om hello egenskaper för hello virtuell dator från dessa alternativ.

## <a name="move-resources"></a>Flytta resurser
Om du behöver toomove resurser tooanother resursgruppens namn eller en annan prenumeration, se [flytta resurser toonew resursgrupp eller prenumeration](resource-group-move-resources.md).

## <a name="lock-resources"></a>Lås resurser
Du kan låsa en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser. Mer information finns i [Låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Visa din prenumeration och kostnader
Du kan visa information om din prenumeration och hello upplyfta kostnader för alla resurser. Välj **prenumerationer** och hello-prenumeration som du vill toosee. Du kan bara ha en prenumeration tooselect.

![prenumeration](./media/resource-group-portal/select-subscription.png)

I hello prenumerationsbladet se en bränna hastighet.

![bränna hastighet](./media/resource-group-portal/burn-rate.png)

Och en uppdelning av kostnader efter resurstyp.

![resurskostnader](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Exportera mall
När du har installerat din resursgrupp, vill du kanske tooview hello Resource Manager-mall för hello resursgrupp. Exporterar hello mallen har två fördelar:

1. Enkelt kan du automatisera framtida distributioner av hello lösning eftersom hello mallen innehåller alla hello hela infrastrukturen.
2. Du kan bekanta dig med mallens syntax genom att titta på hello JavaScript Object Notation (JSON) som representerar din lösning.

Detaljerade anvisningar finns [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Ta bort resursgruppen eller resurser
Tar bort en resursgrupp alla hello-resurser som ingår i den. Du kan också ta bort enskilda resurser inom en resursgrupp. Du vill tooexercise försiktig när du tar bort en resursgrupp eftersom det kan finnas resurser i andra resursgrupper som är länkade tooit. Resource Manager tas inte bort länkade resurser, men de kan inte fungera utan hello förväntades resurser.

![Ta bort grupp](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Nästa steg
* tooview aktivitetsloggar finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).
* tooview information om en distribution finns [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* toodeploy resurser via hello portal finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).
* toomanage åtkomst tooresources finns [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

