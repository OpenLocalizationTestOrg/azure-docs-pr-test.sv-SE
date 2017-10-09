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
# <a name="manage-hello-resources-associated-with-your-azure-accounts-in-visual-studio-cloud-explorer"></a><span data-ttu-id="e5a7a-103">Hantera hello resurser som är associerade med din Azure-konton i Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="e5a7a-103">Manage hello resources associated with your Azure accounts in Visual Studio Cloud Explorer</span></span>
<span data-ttu-id="e5a7a-104">Cloud Explorer kan du tooview Azure-resurser och resursgrupper, granska deras egenskaper och utföra åtgärder för viktiga developer diagnostik från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-104">Cloud Explorer enables you tooview your Azure resources and resource groups, inspect their properties, and perform key developer diagnostics actions from within Visual Studio.</span></span> 

<span data-ttu-id="e5a7a-105">Som hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer bygger på hello Azure Resource Manager-stacken.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-105">Like hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), Cloud Explorer is built on hello Azure Resource Manager stack.</span></span> <span data-ttu-id="e5a7a-106">Därför Cloud Explorer förstår resurser, t.ex Azure-resursgrupper och Azure-tjänster, till exempel Logic apps och API apps och stöder [rollbaserad åtkomstkontroll](active-directory/role-based-access-control-configure.md) (RBAC).</span><span class="sxs-lookup"><span data-stu-id="e5a7a-106">Therefore, Cloud Explorer understands resources such as Azure resource groups and Azure services such as Logic apps and API apps, and it supports [role-based access control](active-directory/role-based-access-control-configure.md) (RBAC).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e5a7a-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e5a7a-107">Prerequisites</span></span>
- <span data-ttu-id="e5a7a-108">[Visual Studio-2017](https://www.visualstudio.com/downloads/) med hello **arbetsbelastning i Azure** valt, eller en tidigare version av Visual Studio med hello [Microsoft Azure SDK för .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span><span class="sxs-lookup"><span data-stu-id="e5a7a-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello **Azure workload** selected, or an earlier version of Visual Studio with hello [Microsoft Azure SDK for .NET 2.9](https://www.microsoft.com/en-us/download/details.aspx?id=51657).</span></span>
- <span data-ttu-id="e5a7a-109">Microsoft Azure-konto - om du inte har ett konto kan du [registrera dig för en kostnadsfri utvärderingsversion](http://go.microsoft.com/fwlink/?LinkId=623901) eller [aktivera Visual Studio-prenumerantförmåner](http://go.microsoft.com/fwlink/?LinkId=623901).</span><span class="sxs-lookup"><span data-stu-id="e5a7a-109">Microsoft Azure account - If you don't have an account, you can [sign up for a free trial](http://go.microsoft.com/fwlink/?LinkId=623901) or [activate your Visual Studio subscriber benefits](http://go.microsoft.com/fwlink/?LinkId=623901).</span></span>

> [!NOTE]
> <span data-ttu-id="e5a7a-110">Välj tooview Cloud Explorer **visa** > **Cloud Explorer** hello menyraden.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-110">tooview Cloud Explorer, select **View** > **Cloud Explorer** on hello menu bar.</span></span>   
> 
> 

## <a name="add-an-azure-account-toocloud-explorer"></a><span data-ttu-id="e5a7a-111">Lägg till en Azure-konto tooCloud Explorer</span><span class="sxs-lookup"><span data-stu-id="e5a7a-111">Add an Azure account tooCloud Explorer</span></span>
<span data-ttu-id="e5a7a-112">tooview hello resurser som är associerade med ett Azure-konto, måste du först lägga till hello konto tooCloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-112">tooview hello resources associated with an Azure account, you must first add hello account tooCloud Explorer.</span></span> 

1. <span data-ttu-id="e5a7a-113">I **Cloud Explorer**väljer **Azure kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-113">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="e5a7a-115">Välj **Lägg till nytt konto**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-115">Select **Add new account**.</span></span> 

    ![Cloud Explorer Lägg till konto länk](media/vs-azure-tools-resources-managing-with-cloud-explorer/add-account-link.png)

1. <span data-ttu-id="e5a7a-117">Logga in toohello Azure-konto vars resurser som du vill toobrowse.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-117">Log in toohello Azure account whose resources you want toobrowse.</span></span> 

1. <span data-ttu-id="e5a7a-118">När loggade in tooan Azure-konto, visa hello prenumerationer som är associerade med det kontot.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-118">Once logged in tooan Azure account, hello subscriptions associated with that account display.</span></span> <span data-ttu-id="e5a7a-119">Välj hello kryssrutorna för hello prenumerantkonton toobrowse och sedan markera **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-119">Select hello check boxes for hello account subscriptions you want toobrowse and then select **Apply**.</span></span> 
 
    ![Cloud Explorer: Välj Azure-prenumerationer toodisplay](media/vs-azure-tools-resources-managing-with-cloud-explorer/select-subscriptions.png)

1. <span data-ttu-id="e5a7a-121">När du har valt hello prenumerationer vars resurser som du vill visa toobrowse, dessa prenumerationer och resurser i hello Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-121">After selecting hello subscriptions whose resources you want toobrowse, those subscriptions and resources display in hello Cloud Explorer.</span></span>

    ![Cloud Explorer resurs lista för ett Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-listed.png)

## <a name="remove-an-azure-account-from-cloud-explorer"></a><span data-ttu-id="e5a7a-123">Ta bort ett Azure-konto från Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="e5a7a-123">Remove an Azure account from Cloud Explorer</span></span> 

1. <span data-ttu-id="e5a7a-124">I **Cloud Explorer**väljer **Azure kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-124">In **Cloud Explorer**, select **Azure account settings**.</span></span>

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/azure-account-settings.png)

1. <span data-ttu-id="e5a7a-126">Nästa toohello konto som du vill tooremove, Välj **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-126">Next toohello account you want tooremove, select **Remove**.</span></span>

    ![Ikonen för inställningar av cloud Explorer Azure-konto](media/vs-azure-tools-resources-managing-with-cloud-explorer/remove-account.png)

## <a name="view-resource-types-or-resource-groups"></a><span data-ttu-id="e5a7a-128">Visa resurstyper eller resursgrupper</span><span class="sxs-lookup"><span data-stu-id="e5a7a-128">View resource types or resource groups</span></span>
<span data-ttu-id="e5a7a-129">tooview resurserna i Azure kan du välja antingen **resurstyper** eller **resursgrupper** vyn.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-129">tooview your Azure resources, you can choose either **Resource Types** or **Resource Groups** view.</span></span>

1. <span data-ttu-id="e5a7a-130">I **Cloud Explorer**, Välj hello resurs visa listrutan.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-130">In **Cloud Explorer**, select hello resource view dropdown.</span></span>

    ![Cloud Explorer nedrullningsbara listan tooselect hello önskad vy för resurser](media/vs-azure-tools-resources-managing-with-cloud-explorer/resources-view-dropdown.png)

1. <span data-ttu-id="e5a7a-132">Hello snabbmenyn hello Välj önskad vy:</span><span class="sxs-lookup"><span data-stu-id="e5a7a-132">From hello context menu, select hello desired view:</span></span> 

    - <span data-ttu-id="e5a7a-133">**Resurstyper** visa - hello vanliga vy som används på hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040), visar resurserna i Azure kategoriserade efter typ, till exempel webbappar, lagringskonton och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-133">**Resource Types** view - hello common view used on hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), shows your Azure resources categorized by their type, such as web apps, storage accounts, and virtual machines.</span></span> 
    - <span data-ttu-id="e5a7a-134">**Resursgrupper** view - kategoriserar Azure-resurser genom hello Azure-resursgrupp som de är kopplade.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-134">**Resource Groups** view - Categorizes Azure resources by hello Azure resource group with which they're associated.</span></span> <span data-ttu-id="e5a7a-135">En resursgrupp är en samling Azure-resurser, används vanligtvis av ett visst program.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-135">A resource group is a bundle of Azure resources, typically used by a specific application.</span></span> <span data-ttu-id="e5a7a-136">toolearn mer om Azure-resursgrupper, se [översikt över Azure Resource Manager](./azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5a7a-136">toolearn more about Azure resource groups, see [Azure Resource Manager Overview](./azure-resource-manager/resource-group-overview.md).</span></span>

    <span data-ttu-id="e5a7a-137">hello följande bild visar en jämförelse av hello två resursvyer:</span><span class="sxs-lookup"><span data-stu-id="e5a7a-137">hello following image shows a comparison of hello two resource views:</span></span>

    ![Cloud Explorer resurs vyer jämförelse](media/vs-azure-tools-resources-managing-with-cloud-explorer/resource-views-comparison.png)

## <a name="view-and-navigate-resources-in-cloud-explorer"></a><span data-ttu-id="e5a7a-139">Visa och navigera resurser i Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="e5a7a-139">View and navigate resources in Cloud Explorer</span></span>
<span data-ttu-id="e5a7a-140">toonavigate tooan Azure-resurs och visa informationen i Cloud Explorer, expandera hello objektets typ eller associerade resursgrupp och välj sedan hello resurs.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-140">toonavigate tooan Azure resource and view its information in Cloud Explorer, expand hello item's type or associated resource group and then select hello resource.</span></span> <span data-ttu-id="e5a7a-141">När du väljer en resurs, visas information i hello två flikar - **åtgärder** och **egenskaper** - längst ned hello i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-141">When you select a resource, information appears in hello two tabs - **Actions** and **Properties** - at hello bottom of Cloud Explorer.</span></span> 

- <span data-ttu-id="e5a7a-142">**Åtgärder** fliken - listor hello åtgärder du kan vidta i Cloud Explorer för hello markerad resurs.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-142">**Actions** tab - Lists hello actions you can take in Cloud Explorer for hello selected resource.</span></span> <span data-ttu-id="e5a7a-143">Du kan också visa dessa alternativ genom att högerklicka på hello resurs tooview dess snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-143">You can also view these options by right-clicking hello resource tooview its context menu.</span></span>

- <span data-ttu-id="e5a7a-144">**Egenskaper för** fliken - visar hello egenskaper för hello resurs, till exempel dess typ, språk och resurs-grupp som den är associerad med.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-144">**Properties** tab - Shows hello properties of hello resource, such as its type, locale, and resource group with which it is associated.</span></span>

<span data-ttu-id="e5a7a-145">hello visar följande bild ett exempel jämförelse av vad som visas på varje flik för en Apptjänst:</span><span class="sxs-lookup"><span data-stu-id="e5a7a-145">hello following image shows an example comparison of what you see on each tab for an App Service:</span></span>

![](./media/vs-azure-tools-resources-managing-with-cloud-explorer/actions-and-properties.png)

<span data-ttu-id="e5a7a-146">Alla resurser har hello åtgärd **öppna i portalen**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-146">Every resource has hello action **Open in portal**.</span></span> <span data-ttu-id="e5a7a-147">När du väljer den här åtgärden Cloud Explorer visar hello markerad resurs i hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="e5a7a-147">When you choose this action, Cloud Explorer displays hello selected resource in hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span> <span data-ttu-id="e5a7a-148">Hej **öppna i portalen** funktionen är praktisk för navigering toodeeply kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-148">hello **Open in portal** feature is handy for navigating toodeeply nested resources.</span></span>

<span data-ttu-id="e5a7a-149">Ytterligare åtgärder och egenskapsvärden kan också visas baserat på hello Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-149">Additional actions and property values may also appear based on hello Azure resource.</span></span> <span data-ttu-id="e5a7a-150">Till exempel web apps och logikappar också ha hello åtgärder **öppna i webbläsare** och **koppla felsökare** dessutom för**öppna i portalen**.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-150">For example, web apps and logic apps also have hello actions **Open in browser** and **Attach debugger** in addition too**Open in portal**.</span></span> <span data-ttu-id="e5a7a-151">Åtgärder tooopen redigerare visas när du väljer ett konto lagringsblob, kö eller tabell.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-151">Actions tooopen editors appear when you choose a storage account blob, queue, or table.</span></span> <span data-ttu-id="e5a7a-152">Azure appar har **URL** och **Status** egenskaper, medan lagringsresurser har nyckeln och anslutningen egenskaperna för anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-152">Azure apps have **URL** and **Status** properties, while storage resources have key and connection string properties.</span></span>

## <a name="find-resources-in-cloud-explorer"></a><span data-ttu-id="e5a7a-153">Söka efter resurser i Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="e5a7a-153">Find resources in Cloud Explorer</span></span>
<span data-ttu-id="e5a7a-154">toolocate resurser med ett visst namn i din prenumeration på Azure-konto, ange hello namn i hello **Sök** rutan i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-154">toolocate resources with a specific name in your Azure account subscriptions, enter hello name in hello **Search** box in Cloud Explorer.</span></span>

![Söka efter resurser i Cloud Explorer](./media/vs-azure-tools-resources-managing-with-cloud-explorer/search-for-resources.png)

<span data-ttu-id="e5a7a-156">När du anger tecken i hello **Sök** , resurser som matchar de tecken som ska visas i trädet för hello-resurser.</span><span class="sxs-lookup"><span data-stu-id="e5a7a-156">As you enter characters in hello **Search** box, only resources that match those characters appear in hello resource tree.</span></span>
