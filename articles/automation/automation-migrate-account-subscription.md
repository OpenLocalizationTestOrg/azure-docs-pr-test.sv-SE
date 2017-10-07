---
title: aaaMigrate Automation-konto och resurser | Microsoft Docs
description: "Den här artikeln beskriver hur toomove ett Automation-konto i Azure Automation och associerade resurser från en prenumeration tooanother."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
ms.openlocfilehash: 201c9091cd2d78d7ea407c1e5fb27f366bb4fa8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-automation-account-and-resources"></a>Migrera Automation-konto och resurser
För Automation-konton och dess associerade resurser (d.v.s. tillgångar, runbooks, moduler, etc.) som du har skapat i hello Azure-portalen och toomigrate från en resurs grupp tooanother eller från en prenumeration tooanother kan du göra det enkelt med Hej [flyttar resurser](../azure-resource-manager/resource-group-move-resources.md) funktion i hello Azure-portalen. Men innan du fortsätter med den här åtgärden, bör du först granska hello följande [checklistan innan du flyttar resurser](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) och dessutom hello listan nedan specifika tooAutomation.   

1. hello mål prenumeration/resursgrupp måste vara i samma region som hello källa.  D.v.s. kan Automation-konton inte flyttas över regioner.
2. När du flyttar resurser (t.ex. runbooks, jobb, etc.), är såväl hello källgrupp hello målgruppen låsta för hello drifttiden hello. Skriva och ta bort blockeras på hello grupper tills hello flytta har slutförts.  
3. Alla runbooks eller variabler som refererar till en resurs eller prenumerations-ID från hello befintliga prenumeration behöver toobe uppdateras när migreringen är klar.   

> [!NOTE]
> Den här funktionen stöder inte flytta klassisk automation resurser.
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a>toomove hello Automation-konto med hjälp av hello portal
1. Från ditt Automation-konto klickar du på **flytta** hello överst i hello-bladet.<br> ![Flytta alternativet](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. På hello **flyttar resurser** bladet, Observera att det innehåller resurser relaterade tooboth ditt Automation-konto och resurs-grupper.  Välj hello **prenumeration** och **resursgruppen** från hello listrutorna eller välj hello alternativet **skapa en ny resursgrupp** och ange ett namn på ny resursgrupp i hello fält har angetts.  
3. Granska och välj hello kryssrutan tooacknowledge du *förstå verktyg och skript kommer måste toobe uppdateras toouse ny resurs-ID när resurser har flyttats* och klicka sedan på **OK**.<br> ![Flytta resurser bladet](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Den här åtgärden tar flera minuter toocomplete.  I **meddelanden**, visas med status för alla åtgärder som sker - verifieringen, migrering, och sedan slutligen när den har slutförts.     

## <a name="toomove-hello-automation-account-using-powershell"></a>toomove hello Automation-konto med hjälp av PowerShell
toomove befintlig Automation resurser tooanother resursgrupp eller prenumeration, använda hello **Get-AzureRmResource** cmdlet-tooget hello specifika Automation-konto och sedan **flytta AzureRmResource** cmdlet tooperform hello flytta.

hello första exemplet visar hur toomove ett Automation-kontot tooa ny resursgrupp.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

När du kör hello ovan kodexempel, kommer du att ange tooverify som du vill tooperform den här åtgärden.  När du klickar på **Ja** och tillåta Hej skriptet tooproceed, du får inga meddelanden när den utför hello migrering.  

toomove tooa ny prenumeration, innehåller ett värde för hello *DestinationSubscriptionId* parameter.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Du kommer att tillfrågas tooconfirm hello flytta som i föregående exempel hello.  

## <a name="next-steps"></a>Nästa steg
* Mer information om att flytta resurser toonew resursgrupp eller prenumeration finns [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md)
* Mer information om rollbaserad åtkomstkontroll i Azure Automation finns för[rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).
* toolearn om PowerShell-cmdletar för att hantera din prenumeration finns [med hjälp av Azure PowerShell med Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* toolearn om portalen funktioner för att hantera din prenumeration, se [använder hello Azure Portal toomanage resurser](../azure-resource-manager/resource-group-portal.md).
