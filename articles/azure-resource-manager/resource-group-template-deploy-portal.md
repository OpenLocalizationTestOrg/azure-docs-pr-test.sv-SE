---
title: "Använda Azure-portalen för att distribuera Azure-resurser | Microsoft Docs"
description: "Använd Azure-portalen och Azure Resource Manager för att distribuera dina resurser."
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
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Distribuera resurser med Resource Manager-mallar och Azure Portal
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

Det här avsnittet visar hur du använder den [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) att distribuera Azure-resurser. Läs om hur du hanterar dina resurser i [hantera Azure-resurser via portalen](resource-group-portal.md).

Inte alla tjänsten stöder för närvarande portalen eller Resource Manager. För dessa tjänster måste du använda den [klassiska portalen](https://manage.windowsazure.com). Statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Skapa resursgrupp
1. Om du vill skapa en tom resursgrupp **ny** > **Management** > **resursgruppen**.
   
    ![Skapa tom resursgrupp](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. Ge det ett namn och en plats och, om det behövs väljer du en prenumeration. Du måste ange en plats för resursgruppen eftersom resursgruppen lagras metadata om resurserna. Av kompatibilitetsskäl, kanske du vill ange var den metadata lagras. I allmänhet rekommenderar vi att du anger en plats där de flesta av dina resurser ska placeras. Med hjälp av samma plats kan förenkla din mall.
   
    ![gruppvärden](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Distribuera resurser från Marketplace
När du har skapat en resursgrupp kan du distribuera resurser till den från Marketplace. Marketplace innehåller fördefinierade lösningar för vanliga scenarier.

1. Om du vill starta en distribution väljer **ny** och typ av resurs som du vill distribuera. Leta efter en viss version av resursen som du vill distribuera.
   
    ![distribuera resurs](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. Om du inte ser den lösning som du vill distribuera, kan du söka Marketplace för den.
   
    ![Sök på marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. Beroende på vilken typ av markerade resursen har en samling relevanta egenskaper för att ange före distributionen. Dessa alternativ visas inte här, eftersom de kan variera, beroende på resurstypen. Du måste välja en resursgrupp för målet för alla typer. Följande bild visar hur du skapar en webbapp och distribuera den till den resursgrupp som du skapade.
   
    ![Skapa resursgrupp](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    Du kan också välja att skapa en resursgrupp när du distribuerar dina resurser. Välj **Skapa nytt** och namnge resursgruppen.
   
    ![Skapa ny resursgrupp](./media/resource-group-template-deploy-portal/select-new-group.png)
4. Börjar din distribution. Distributionen kan ta några minuter. När distributionen är klar visas ett meddelande.
   
    ![Visa meddelande](./media/resource-group-template-deploy-portal/view-notification.png)
5. När du har distribuerat dina resurser, du kan lägga till fler resurser i resursgruppen med hjälp av den **Lägg till** på bladet för resursgruppen.
   
    ![lägga till en resurs](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Distribuera resurser från anpassad mall
Om du vill köra en distribution utan att använda någon av mallar i Marketplace, kan du skapa en anpassad mall som definierar infrastrukturen för lösningen. Läs om hur du skapar mallar i [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).

1. Om du vill distribuera en anpassad mall via portalen, Välj **ny**, och starta söker efter **malldistribution** tills du kan välja bland alternativ.
   
    ![Sök malldistribution](./media/resource-group-template-deploy-portal/search-template.png)
2. Välj **malldistribution** från de tillgängliga resurserna.
   
    ![Välj för malldistribution](./media/resource-group-template-deploy-portal/select-template.png)
3. Öppna den tomma mallen som är tillgänglig för att anpassa när du startar malldistributionen av.
   
    ![Skapa mall](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    I redigeraren, lägger du till JSON-syntax som definierar de resurser som du vill distribuera. Välj **spara** när du är klar. Anvisningar om hur du skriver JSON-syntax finns [genomgång av Resource Manager-mall](resource-manager-template-walkthrough.md).
   
    ![redigera mall](./media/resource-group-template-deploy-portal/edit-template.png)
4. Du kan också välja en befintlig mall från den [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/). Dessa mallar tillhandahålls av gruppen. De täcker många vanliga scenarier och någon kan ha lagt till en mall som liknar vad du vill distribuera. Du kan söka mallar för att hitta något som passar ditt scenario.
   
    ![Välj mall för Snabbstart](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    Du kan visa den valda mallen i redigeraren.
5. När du har angett alla andra värden, Välj **skapa** att distribuera mallen. 
   
    ![distribuera mallen](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Distribuera resurser från en mall som sparats i ditt konto
Portalen kan du spara en mall i Azure-konto och distribuera den senare. Mer information om hur du arbetar med dessa sparade mallar, [Kom igång med privata mallar på Azure portal](../marketplace-consumer/mytemplates-getstarted.md).

1. Om du vill hitta dina sparade mallar, Välj **Bläddra** > **mallar**.
   
    ![Bläddra efter mallar](./media/resource-group-template-deploy-portal/browse-templates.png)
2. Välj det du vill arbeta med i listan över mallar som sparas i ditt konto.
   
    ![sparade mallar](./media/resource-group-template-deploy-portal/saved-templates.png)
3. Välj **distribuera** distribuera sparade mallen.
   
    ![distribuera sparade mallen](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Nästa steg
* Om du vill visa granskningsloggarna finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).
* Om du vill felsöka distribution, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* Om du vill hämta en mall från en distribution eller resursgruppen finns [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).
* Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).

