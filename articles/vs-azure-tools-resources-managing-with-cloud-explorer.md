---
title: aaaManaging Azure resurser med Cloud Explorer | Microsoft Docs
description: "Lär dig hur toouse Cloud Explorer toobrowse och hantera Azure-resurser i Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 6347dc53-f497-49d5-b29b-e8b9f0e939d7
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/25/2017
ms.author: kraigb
ms.openlocfilehash: 8a81660074d5d04be063df9e25076b7a97586514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a>Hantera hello resurser som är associerade med din Azure-konton i Visual Studio Cloud Explorer
Cloud Explorer kan du tooview Azure-resurser och resursgrupper, granska deras egenskaper och utföra åtgärder för viktiga developer diagnostik från Visual Studio. 

Som hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer bygger på hello Azure Resource Manager-stacken. Därför Cloud Explorer förstår resurser, t.ex Azure-resursgrupper och Azure-tjänster, till exempel Logic apps och API apps och stöder [rollbaserad åtkomstkontroll](active-directory/role-based-access-control-configure.md) (RBAC). 

## <a name="prerequisites"></a>Krav
- [Visual Studio-2017](https://www.visualstudio.com/downloads/) med hello **arbetsbelastning i Azure** valt, eller en tidigare version av Visual Studio med hello [Microsoft Azure SDK för .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).
- Microsoft Azure-konto - om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](http://go.microsoft.com/fwlink/?LinkId=623901) eller [aktivera Visual Studio-prenumerantförmåner](http://go.microsoft.com/fwlink/?LinkId=623901).

> [!NOTE]
> Välj tooview Cloud Explorer **visa** > **Cloud Explorer** hello menyraden.   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a>Lägg till en Azure-konto tooCloud Explorer
tooview hello resurser som är associerade med ett Azure-konto, måste du först lägga till hello konto tooCloud Explorer. 

1. I **Cloud Explorer**väljer **Azure kontoinställningar**.

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Välj **Lägg till nytt konto**. 

    ![Cloud Explorer Lägg till konto länk](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. Logga in toohello Azure-konto vars resurser som du vill toobrowse. 

1. När loggade in tooan Azure-konto, visa hello prenumerationer som är associerade med det kontot. Välj hello kryssrutorna för hello prenumerantkonton toobrowse och sedan markera **tillämpa**. 
 
    ![Cloud Explorer: Välj Azure-prenumerationer toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. När du har valt hello prenumerationer vars resurser som du vill visa toobrowse, dessa prenumerationer och resurser i hello Cloud Explorer.

    ![Cloud Explorer resurs lista för ett Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a>Ta bort ett Azure-konto från Cloud Explorer 

1. I **Cloud Explorer**väljer **Azure kontoinställningar**.

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. Nästa toohello konto som du vill tooremove, Välj **ta bort**.

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a>Visa resurstyper eller resursgrupper
tooview resurserna i Azure kan du välja antingen **resurstyper** eller **resursgrupper** vyn.

1. I **Cloud Explorer**, Välj hello resurs visa listrutan.

    ![Cloud Explorer nedrullningsbara listan tooselect hello önskad vy för resurser](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. Hello snabbmenyn hello Välj önskad vy: 

    - **Resurstyper** visa - hello vanliga vy som används på hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040), visar resurserna i Azure kategoriserade efter typ, till exempel webbappar, lagringskonton och virtuella datorer. 
    - **Resursgrupper** view - kategoriserar Azure-resurser genom hello Azure-resursgrupp som de är kopplade. En resursgrupp är en samling Azure-resurser, används vanligtvis av ett visst program. toolearn mer om Azure-resursgrupper, se [översikt över Azure Resource Manager](./azure-resource-manager/resource-group-overview.md).

    hello följande bild visar en jämförelse av hello två resursvyer:

    ![Cloud Explorer resurs vyer jämförelse](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a>Visa och navigera resurser i Cloud Explorer
toonavigate tooan Azure-resurs och visa informationen i Cloud Explorer, expandera hello objektets typ eller associerade resursgrupp och välj sedan hello resurs. När du väljer en resurs, visas information i hello två flikar - **åtgärder** och **egenskaper** - längst ned hello i Cloud Explorer. 

- **Åtgärder** fliken - listor hello åtgärder du kan vidta i Cloud Explorer för hello markerad resurs. Du kan också visa dessa alternativ genom att högerklicka på hello resurs tooview dess snabbmenyn.

- **Egenskaper för** fliken - visar hello egenskaper för hello resurs, till exempel dess typ, språk och resurs-grupp som den är associerad med.

hello visar följande bild ett exempel jämförelse av vad som visas på varje flik för en Apptjänst:

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

Alla resurser har hello åtgärd **öppna i portalen**. När du väljer den här åtgärden Cloud Explorer visar hello markerad resurs i hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040). Hej **öppna i portalen** funktionen är praktisk för navigering toodeeply kapslade resurser.

Ytterligare åtgärder och egenskapsvärden kan också visas baserat på hello Azure-resurs. Till exempel web apps och logikappar också ha hello åtgärder **öppna i webbläsare** och **koppla felsökare** dessutom för**öppna i portalen**. Åtgärder tooopen redigerare visas när du väljer ett konto lagringsblob, kö eller tabell. Azure appar har **URL** och **Status** egenskaper, medan lagringsresurser har nyckeln och anslutningen egenskaperna för anslutningssträngen.

## <a name="find-resources-in-cloud-explorer"></a>Söka efter resurser i Cloud Explorer
toolocate resurser med ett visst namn i din prenumeration på Azure-konto, ange hello namn i hello **Sök** rutan i Cloud Explorer.

![Söka efter resurser i Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

När du anger tecken i hello **Sök** , resurser som matchar de tecken som ska visas i trädet för hello-resurser.
