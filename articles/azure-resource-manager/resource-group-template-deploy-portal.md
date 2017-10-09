---
title: aaaUse Azure portal toodeploy Azure-resurser | Microsoft Docs
description: "Använd Azure-portalen och Azure Resource Manager toodeploy dina resurser."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Distribuera resurser med Resource Manager-mallar och Azure Portal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

Det här avsnittet visar hur toouse hello [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) toodeploy Azure-resurser. toolearn om hur du hanterar dina resurser, se [hantera Azure-resurser via portalen](resource-group-portal.md).

Inte alla tjänsten stöder för närvarande hello-portalen eller Resource Manager. För dessa tjänster måste toouse hello [klassiska portalen](https://manage.windowsazure.com). Hello statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Skapa resursgrupp
1. Välj toocreate en tom resursgrupp **ny** > **Management** > **resursgruppen**.
   
    ![Skapa tom resursgrupp](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Ge det ett namn och en plats och, om det behövs väljer du en prenumeration. Du behöver tooprovide en plats för resursgruppen hello eftersom hello resursgruppen lagras metadata om hello resurser. Av kompatibilitetsskäl vill du kanske toospecify där den metadata lagras. I allmänhet rekommenderar vi att du anger en plats där de flesta av dina resurser ska placeras. Med hjälp av hello samma plats kan förenkla din mall.
   
    ![gruppvärden](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Distribuera resurser från Marketplace
När du har skapat en resursgrupp kan du distribuera resurser tooit från hello Marketplace. hello Marketplace innehåller fördefinierade lösningar för vanliga scenarier.

1. Välj toostart en distribution **ny** och hello typ av resurs som toodeploy. Leta sedan för hello viss version av hello resurs som toodeploy.
   
    ![distribuera resurs](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Om du inte ser hello som toodeploy lösning kan du söka hello Marketplace för den.
   
    ![Sök på marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. Beroende på hello typ av valda resurs har du en samling med relevanta egenskaper tooset före distributionen. Dessa alternativ visas inte här, eftersom de kan variera, beroende på resurstypen. Du måste välja en resursgrupp för målet för alla typer. hello följande bild visar hur toocreate en webbapp och distribuera den toohello resursgrupp som du skapade.
   
    ![Skapa resursgrupp](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Alternativt kan du bestämma toocreate en resursgrupp när du distribuerar dina resurser. Välj **Skapa nytt** och namnge hello resursgruppen.
   
    ![Skapa ny resursgrupp](./media/resource-group-template-deploy-portal/select-new-group.png)
4. Börjar din distribution. hello distributionen kan ta några minuter. När hello distributionen är klar visas ett meddelande.
   
    ![Visa meddelande](./media/resource-group-template-deploy-portal/view-notification.png)
5. När du har distribuerat dina resurser du kan lägga till flera resurser toohello resursgrupp med hjälp av hello **Lägg till** på hello blad för resursgrupp.
   
    ![lägga till en resurs](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Distribuera resurser från anpassad mall
Om du vill tooexecute en distribution, men inte använda någon av hello mallar i hello Marketplace, kan du skapa en anpassad mall som definierar hello infrastrukturen för lösningen. toolearn om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

1. toodeploy en anpassad mall hello-portalen, Välj **ny**, och starta söker efter **malldistribution** tills du kan välja bland hello alternativ.
   
    ![Sök malldistribution](./media/resource-group-template-deploy-portal/search-template.png)
2. Välj **malldistribution** från hello tillgängliga resurser.
   
    ![Välj för malldistribution](./media/resource-group-template-deploy-portal/select-template.png)
3. Öppna hello tom mall som är tillgänglig för att anpassa när du startar hello malldistribution.
   
    ![Skapa mall](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    Lägg till hello JSON-syntax som definierar hello-resurser som du vill att toodeploy i hello-redigeraren. Välj **spara** när du är klar. Anvisningar om hur du skriver hello JSON syntax finns [genomgång av Resource Manager-mall](resource-manager-template-walkthrough.md).
   
    ![redigera mall](./media/resource-group-template-deploy-portal/edit-template.png)
4. Du kan också välja en befintlig mall från hello [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/). Dessa mallar tillhandahålls av hello community. De täcker många vanliga scenarier och någon kan ha lagt till en mall som är liknande toowhat som du försöker toodeploy. Du kan söka hello mallar toofind något som matchar ditt scenario.
   
    ![Välj mall för Snabbstart](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Du kan visa hello valda mallen i hello-redigeraren.
5. När du har angett alla hello andra värden, Välj **skapa** toodeploy hello mallen. 
   
    ![distribuera mallen](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a>Distribuera resurser från en mall som sparats tooyour konto
hello portal gör toosave en mall tooyour Azure-konto och distribuera den senare. Mer information om hur du arbetar med dessa sparade mallar, [Kom igång med privata mallar på hello Azure-portalen](../marketplace-consumer/mytemplates-getstarted.md).

1. toofind dina sparade mallar, Välj **Bläddra** > **mallar**.
   
    ![Bläddra efter mallar](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Hello lista över mallar som har sparats tooyour konto, Välj hello som du vill toowork på.
   
    ![sparade mallar](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Välj **distribuera** tooredeploy detta spara mallen.
   
    ![distribuera sparade mallen](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Nästa steg
* tooview granskningsloggarna finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).
* tootroubleshoot distributionsfel finns [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* tooretrieve en mall från en distribution eller resursgrupp, se [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

