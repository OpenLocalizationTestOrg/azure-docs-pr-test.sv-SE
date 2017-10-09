---
title: "aaaGet igång med privata mallar | Microsoft Docs"
description: "Lägga till, hantera och dela dina privata mallar med hjälp av hello Azure-portalen, hello Azure CLI eller PowerShell."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.openlocfilehash: 1fe2c6422f62a98f7ae9ba5c61b9639d993f0bca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-private-templates-on-hello-azure-portal"></a><span data-ttu-id="8d4da-103">Kom igång med privata mallar på hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8d4da-103">Get started with private Templates on hello Azure Portal</span></span>
<span data-ttu-id="8d4da-104">En [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) mallen är en deklarativ mall används toodefine din distribution.</span><span class="sxs-lookup"><span data-stu-id="8d4da-104">An [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) template is a declarative template used toodefine your deployment.</span></span> <span data-ttu-id="8d4da-105">Du kan definiera hello resurser toodeploy för en lösning och ange parametrar och variabler som gör att du tooinput värden för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="8d4da-105">You can define hello resources toodeploy for a solution, and specify parameters and variables that enable you tooinput values for different environments.</span></span> <span data-ttu-id="8d4da-106">hello mallen består av JSON och uttryck som du kan använda tooconstruct värden för din distribution.</span><span class="sxs-lookup"><span data-stu-id="8d4da-106">hello template consists of JSON and expressions which you can use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="8d4da-107">Du kan använda hello nya **mallar** funktionen hello [Azure Portal](https://portal.azure.com) tillsammans med hello **Microsoft.Gallery** resursprovidern som ett tillägg för hello [ Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable användare toocreate, hantera och distribuera privata mallar från personliga bibliotek.</span><span class="sxs-lookup"><span data-stu-id="8d4da-107">You can use hello new **Templates** capability in hello [Azure Portal](https://portal.azure.com) along with hello **Microsoft.Gallery** resource provider as an extension of hello [Azure Marketplace](https://azure.microsoft.com/marketplace/) tooenable users toocreate, manage and deploy private templates from a personal library.</span></span>

<span data-ttu-id="8d4da-108">Det här dokumentet vägleder dig genom att lägga till, hantera och dela en privat **mallen** med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-108">This document walks you through adding, managing and sharing a private **Template** using hello Azure Portal.</span></span>

## <a name="guidance"></a><span data-ttu-id="8d4da-109">Riktlinjer</span><span class="sxs-lookup"><span data-stu-id="8d4da-109">Guidance</span></span>
<span data-ttu-id="8d4da-110">hello följande rekommendationer hjälper dig att dra full nytta av **mallar** när du arbetar med dina lösningar:</span><span class="sxs-lookup"><span data-stu-id="8d4da-110">hello following suggestions will help you take full advantage of **Templates** when working with your solutions:</span></span>

* <span data-ttu-id="8d4da-111">En **Mall** är en inkapslande resurs som innehåller en Resource Manager-mall och ytterligare metadata.</span><span class="sxs-lookup"><span data-stu-id="8d4da-111">A **Template** is an encapsulating resource that contains an Resource Manager template and additional metadata.</span></span> <span data-ttu-id="8d4da-112">Den fungerar väldigt likt tooan objekt i hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8d4da-112">It behaves very similarly tooan item in hello Marketplace.</span></span> <span data-ttu-id="8d4da-113">hello viktigaste skillnaden är att den är privat som skillnad från toohello offentliga Marketplace-objekt.</span><span class="sxs-lookup"><span data-stu-id="8d4da-113">hello key difference is that it is a private item as opposed toohello public Marketplace items.</span></span>
* <span data-ttu-id="8d4da-114">Hej **mallar** biblioteket fungerar bra för användare som behöver toocustomize distributioner.</span><span class="sxs-lookup"><span data-stu-id="8d4da-114">hello **Templates** library works well for users who need toocustomize their deployments.</span></span>
* <span data-ttu-id="8d4da-115">**Mallar** fungerar bra för användare som behöver en enkel lagringsplats i Azure.</span><span class="sxs-lookup"><span data-stu-id="8d4da-115">**Templates** work well for users who need a simple repository within Azure.</span></span>
* <span data-ttu-id="8d4da-116">Börja med en befintlig Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="8d4da-116">Start with an existing Resource Manager template.</span></span> <span data-ttu-id="8d4da-117">Hitta mallar i [github](https://github.com/Azure/azure-quickstart-templates) eller [Exportera mallen](../azure-resource-manager/resource-manager-export-template.md) från en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8d4da-117">Find templates in [github](https://github.com/Azure/azure-quickstart-templates) or [Export template](../azure-resource-manager/resource-manager-export-template.md) from an existing resource group.</span></span>
* <span data-ttu-id="8d4da-118">**Mallar** är bundet toohello användare som publicerar dem..</span><span class="sxs-lookup"><span data-stu-id="8d4da-118">**Templates** are tied toohello user who publishes them.</span></span> <span data-ttu-id="8d4da-119">hello utgivarens namn är synligt tooeveryone som har läsbehörighet tooit.</span><span class="sxs-lookup"><span data-stu-id="8d4da-119">hello publisher name is visible tooeveryone who has read access tooit.</span></span>
* <span data-ttu-id="8d4da-120">**Mallar** är Resource Manager-resurser och det går inte att byta namn på dem efter att de publicerats.</span><span class="sxs-lookup"><span data-stu-id="8d4da-120">**Templates** are Resource Manager resources and cannot be renamed once published.</span></span>

## <a name="add-a-template-resource"></a><span data-ttu-id="8d4da-121">Lägg till en mallresurs</span><span class="sxs-lookup"><span data-stu-id="8d4da-121">Add a Template resource</span></span>
<span data-ttu-id="8d4da-122">Det finns två sätt toocreate en **mallen** resurs i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-122">There are two ways toocreate a **Template** resource in hello Azure portal.</span></span>

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a><span data-ttu-id="8d4da-123">Metod 1: Skapa en ny mallresurs från en resursgrupp som körs</span><span class="sxs-lookup"><span data-stu-id="8d4da-123">Method 1 : Create a new Template resource from a running resource group</span></span>
1. <span data-ttu-id="8d4da-124">Navigera tooan befintlig resursgrupp hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-124">Navigate tooan existing resource group on hello Azure Portal.</span></span> <span data-ttu-id="8d4da-125">Välj **Exportera mall** i **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-125">Select **Export template** in **Settings**.</span></span>
2. <span data-ttu-id="8d4da-126">När hello Resource Manager-mall har exporterats kan du använda hello **Spara mall** knappen toosave den toohello **mallar** databasen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-126">Once hello Resource Manager template is exported, use hello **Save Template** button toosave it toohello **Templates** repository.</span></span> <span data-ttu-id="8d4da-127">Hitta komplett information för att Exportera mall [här](../azure-resource-manager/resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="8d4da-127">Find complete details for Export template [here](../azure-resource-manager/resource-manager-export-template.md).</span></span>
   <br /><br /><span data-ttu-id="8d4da-128">
   ![Exportera resursgrupp](media/rg-export-portal1.PNG)</span><span class="sxs-lookup"><span data-stu-id="8d4da-128">
![Resource group export](media/rg-export-portal1.PNG)</span></span>  <br />
3. <span data-ttu-id="8d4da-129">Välj hello **spara tooTemplate** kommandoknapp.</span><span class="sxs-lookup"><span data-stu-id="8d4da-129">Select hello **Save tooTemplate** command button.</span></span>
   <br /><br />
4. <span data-ttu-id="8d4da-130">Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="8d4da-130">Enter hello following information:</span></span>
   
   * <span data-ttu-id="8d4da-131">Namn – namnet på hello mallobjektet (Obs: Detta är ett Azure Resource Manager-baserat namn.</span><span class="sxs-lookup"><span data-stu-id="8d4da-131">Name – Name of hello template object (NOTE: This is an Azure Resource Manager based name.</span></span> <span data-ttu-id="8d4da-132">Alla namngivningsbegränsningar gäller och namn kan inte ändras efter att de skapats).</span><span class="sxs-lookup"><span data-stu-id="8d4da-132">All naming restrictions apply and it cannot be changed once created).</span></span>
   * <span data-ttu-id="8d4da-133">Beskrivning – snabb sammanfattning om hello mallen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-133">Description – Quick summary about hello template.</span></span>
     
     ![Spara mall](media/save-template-portal1.PNG)  <br />
5. <span data-ttu-id="8d4da-135">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-135">Click **Save**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8d4da-136">hello visar Export mall bladet meddelanden när hello exporterade Resource Manager-mallen innehåller fel, men du kommer fortfarande att kunna toosave mallar för toohello denna Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="8d4da-136">hello Export template blade shows notifications when hello exported Resource Manager template has errors, but you will still be able toosave this Resource Manager template toohello Templates.</span></span> <span data-ttu-id="8d4da-137">Se till att du kontrollerar och korrigerar eventuella Resource Manager mallen problem innan omdistribuera hello exporterade Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="8d4da-137">Ensure that you check and fix any Resource Manager template issues before redeploying hello exported Resource Manager template.</span></span>
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a><span data-ttu-id="8d4da-138">Metod 2: Lägg till en ny Mall-resurs från bläddra</span><span class="sxs-lookup"><span data-stu-id="8d4da-138">Method 2 : Add a new Template resource from browse</span></span>
<span data-ttu-id="8d4da-139">Du kan också lägga till en ny **mallen** med hello kommandoknappen + Lägg till i **Bläddra > mallar**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-139">You can also add a new **Template** from scratch using hello +Add command button in **Browse > Templates**.</span></span> <span data-ttu-id="8d4da-140">Du behöver tooprovide ett namn, beskrivning och hello Resource Manager-mallens JSON.</span><span class="sxs-lookup"><span data-stu-id="8d4da-140">You will need tooprovide a Name, Description and hello Resource Manager template JSON.</span></span>

![Lägg till mall](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> <span data-ttu-id="8d4da-142">Microsoft.Gallery är en klientbaserad Azure-resursprovider.</span><span class="sxs-lookup"><span data-stu-id="8d4da-142">Microsoft.Gallery is a Tenant based Azure resource provider.</span></span> <span data-ttu-id="8d4da-143">hello är mallresursen bundet toohello användare som skapade den.</span><span class="sxs-lookup"><span data-stu-id="8d4da-143">hello Template resource is tied toohello user who created it.</span></span> <span data-ttu-id="8d4da-144">Det är inte bunden tooany viss prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8d4da-144">It is not tied tooany specific subscription.</span></span> <span data-ttu-id="8d4da-145">En prenumeration måste toobe valt endast när du distribuerar en mall.</span><span class="sxs-lookup"><span data-stu-id="8d4da-145">A subscription needs toobe chosen only when deploying a Template.</span></span>
> 
> 

## <a name="view-template-resources"></a><span data-ttu-id="8d4da-146">Visa mallresurser</span><span class="sxs-lookup"><span data-stu-id="8d4da-146">View Template resources</span></span>
<span data-ttu-id="8d4da-147">Alla **mallar** tillgängliga tooyou kan ses i **Bläddra > mallar**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-147">All **Templates** available tooyou can be seen at **Browse > Templates**.</span></span> <span data-ttu-id="8d4da-148">Detta omfattar de **mallar** du har skapat samt de som har delats med dig med olika behörighetsnivåer.</span><span class="sxs-lookup"><span data-stu-id="8d4da-148">This includes **Templates** you have created as well as ones that have been shared with you with varying levels of permissions.</span></span> <span data-ttu-id="8d4da-149">Mer information finns i hello [åtkomstkontroll](#access-control-for-a-tenant-resource-provider) nedan.</span><span class="sxs-lookup"><span data-stu-id="8d4da-149">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section below.</span></span>

![Visa mall](media/view-template-portal1.PNG)  <br />

<span data-ttu-id="8d4da-151">Du kan visa hello information om en **mallen** genom att klicka på ett objekt i listan hello.</span><span class="sxs-lookup"><span data-stu-id="8d4da-151">You can view hello details of a **Template** by clicking into an item in hello list.</span></span>

![Visa mall](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a><span data-ttu-id="8d4da-153">Redigera en mallresurs</span><span class="sxs-lookup"><span data-stu-id="8d4da-153">Edit a Template resource</span></span>
<span data-ttu-id="8d4da-154">Du kan initiera hello redigera flödet för en **mallen** genom att högerklicka på hello artikeln på hello bläddringslistan eller genom att välja kommandoknappen Redigera för hello.</span><span class="sxs-lookup"><span data-stu-id="8d4da-154">You can initiate hello edit flow for a **Template** by right clicking hello item on hello Browse list or by choosing hello Edit command button.</span></span>

![Redigera mall](media/edit-template-portal1a.PNG)  <br />

<span data-ttu-id="8d4da-156">Du kan redigera hello beskrivningen eller Resource Manager-mallens text.</span><span class="sxs-lookup"><span data-stu-id="8d4da-156">You can edit hello description or Resource Manager template text.</span></span> <span data-ttu-id="8d4da-157">Du kan inte redigera hello namn eftersom det är ett resursnamn för resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8d4da-157">You cannot edit hello name since it is an Resource Manager resource name.</span></span> <span data-ttu-id="8d4da-158">När du redigerar hello Resource Manager-mallens JSON kommer vi verifiera tooensure att den är giltig JSON.</span><span class="sxs-lookup"><span data-stu-id="8d4da-158">When you edit hello Resource Manager template JSON we will validate tooensure that it is valid JSON.</span></span> <span data-ttu-id="8d4da-159">Välj **OK** och sedan **spara** toosave den uppdaterade mallen.</span><span class="sxs-lookup"><span data-stu-id="8d4da-159">Choose **OK** and then **Save** toosave your updated template.</span></span>

![Redigera mall](media/edit-template-portal2a.PNG)  <br />

<span data-ttu-id="8d4da-161">En gång hello **mallen** har sparats visas ett meddelande om bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="8d4da-161">Once hello **Template** is saved you will see a confirmation notification.</span></span>

![Redigera mall](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a><span data-ttu-id="8d4da-163">Distribuera en mallresurs</span><span class="sxs-lookup"><span data-stu-id="8d4da-163">Deploy a Template resource</span></span>
<span data-ttu-id="8d4da-164">Du kan distribuera alla **mallar** som du har **läs**behörigheter på.</span><span class="sxs-lookup"><span data-stu-id="8d4da-164">You can deploy any **Template** that you have **Read** permissions on.</span></span> <span data-ttu-id="8d4da-165">Hej distributionsflödet startar hello standard Azure-mallens distributionsblad.</span><span class="sxs-lookup"><span data-stu-id="8d4da-165">hello deployment flow launches hello standard Azure Template deployment blade.</span></span> <span data-ttu-id="8d4da-166">Fyll i hello värden för Resource Manager hello mallen parametrar tooproceed med hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="8d4da-166">Fill out hello values for hello Resource Manager template parameters tooproceed with hello deployment.</span></span>

![Distribuera mallen](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a><span data-ttu-id="8d4da-168">Dela en mallresurs</span><span class="sxs-lookup"><span data-stu-id="8d4da-168">Share a Template resource</span></span>
<span data-ttu-id="8d4da-169">En **mall**resurs kan delas med kollegor.</span><span class="sxs-lookup"><span data-stu-id="8d4da-169">A **Template** resource can be shared with your peers.</span></span> <span data-ttu-id="8d4da-170">Delning fungerar på samma sätt för[rolltilldelning för en resurs på Azure](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="8d4da-170">Sharing behaves similarly too[role assignment for any resource on Azure](../active-directory/role-based-access-control-configure.md).</span></span> <span data-ttu-id="8d4da-171">Hej **mallen** ägare ger tooother användare som kan interagera med en mallresurs.</span><span class="sxs-lookup"><span data-stu-id="8d4da-171">hello **Template** owner provides permissions tooother users who can interact with a Template resource.</span></span> <span data-ttu-id="8d4da-172">hello person eller grupp med personer som du delar hello **mallen** med blir kan toosee hello Resource Manager-mall och dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="8d4da-172">hello person or group of people you share hello **Template** with will be able toosee hello Resource Manager template and its gallery properties.</span></span>

### <a name="access-control-for-hello-microsoftgallery-resources"></a><span data-ttu-id="8d4da-173">Åtkomstkontroll för hello Microsoft.Gallery-resurser</span><span class="sxs-lookup"><span data-stu-id="8d4da-173">Access control for hello Microsoft.Gallery resources</span></span>
| <span data-ttu-id="8d4da-174">Roll</span><span class="sxs-lookup"><span data-stu-id="8d4da-174">Role</span></span> | <span data-ttu-id="8d4da-175">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="8d4da-175">Permissions</span></span> |
| --- | --- |
| <span data-ttu-id="8d4da-176">Ägare</span><span class="sxs-lookup"><span data-stu-id="8d4da-176">Owner</span></span> |<span data-ttu-id="8d4da-177">Tillåter fullständig behörighet för hello mallresursen inklusive delning</span><span class="sxs-lookup"><span data-stu-id="8d4da-177">Allows full control on hello Template resource including Share</span></span> |
| <span data-ttu-id="8d4da-178">Läsare</span><span class="sxs-lookup"><span data-stu-id="8d4da-178">Reader</span></span> |<span data-ttu-id="8d4da-179">Tillåter Läs- och mallresursen på hello mall-resurs</span><span class="sxs-lookup"><span data-stu-id="8d4da-179">Allows Read and Execute(Deploy) on hello Template resource</span></span> |
| <span data-ttu-id="8d4da-180">Deltagare</span><span class="sxs-lookup"><span data-stu-id="8d4da-180">Contributor</span></span> |<span data-ttu-id="8d4da-181">Tillåter redigera och ta bort på hello mall-resurs.</span><span class="sxs-lookup"><span data-stu-id="8d4da-181">Allows Edit and Delete permission on hello Template resource.</span></span> <span data-ttu-id="8d4da-182">Användare kan inte dela hello mallen med andra</span><span class="sxs-lookup"><span data-stu-id="8d4da-182">User cannot Share hello Template with others</span></span> |

<span data-ttu-id="8d4da-183">Välj **resursen** på hello objektet genom att högerklicka eller på hello vybladet för ett specifikt objekt.</span><span class="sxs-lookup"><span data-stu-id="8d4da-183">Select **Share** on hello browse item by right clicking or on hello view blade of a specific item.</span></span> <span data-ttu-id="8d4da-184">Detta startar en delningsupplevelse.</span><span class="sxs-lookup"><span data-stu-id="8d4da-184">This launches a Share experience.</span></span>

![Dela mall](media/share-template-portal1a.png)  <br />

 <span data-ttu-id="8d4da-186">Nu kan du välja en roll och en användare eller grupp tooprovide åtkomst tooa viss **mallen**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-186">You can now choose a role and a user or group tooprovide access tooa particular **Template**.</span></span> <span data-ttu-id="8d4da-187">hello tillgängliga roller är ägare, läsare och deltagare.</span><span class="sxs-lookup"><span data-stu-id="8d4da-187">hello available roles are Owner, Reader and Contributor.</span></span> <span data-ttu-id="8d4da-188">Mer information finns i hello [åtkomstkontroll](#access-control-for-a-tenant-resource-provider) ovan.</span><span class="sxs-lookup"><span data-stu-id="8d4da-188">More details in hello [access control](#access-control-for-a-tenant-resource-provider) section above.</span></span>

![Dela mall](media/share-template-portal2b.png)  <br />

![Dela mall](media/share-template-portal3b.png)  <br />

<span data-ttu-id="8d4da-191">Klicka på **Välj** och **Ok**.</span><span class="sxs-lookup"><span data-stu-id="8d4da-191">Click **Select** and **Ok**.</span></span> <span data-ttu-id="8d4da-192">Du kan nu se hello användare eller grupper du har lagt till toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="8d4da-192">You can now see hello users or groups you added toohello resource.</span></span>

![Dela mall](media/share-template-portal4b.png)  <br />

> [!NOTE]
> <span data-ttu-id="8d4da-194">En mall kan endast delas med användare och grupper i hello samma Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="8d4da-194">A Template can only be shared with users and groups in hello same Azure Active Directory tenant.</span></span> <span data-ttu-id="8d4da-195">Om du delar en mall med en e-postadress som inte är i din klient skickas en inbjudan ber användaren hello toojoin hello klientorganisationen som en gäst.</span><span class="sxs-lookup"><span data-stu-id="8d4da-195">If you share a Template with an email address that is not in your tenant, an invitation will be sent asking hello user toojoin hello tenant as a guest.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8d4da-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d4da-196">Next steps</span></span>
* <span data-ttu-id="8d4da-197">toolearn om hur du skapar Resource Manager-mallar finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md)</span><span class="sxs-lookup"><span data-stu-id="8d4da-197">toolearn about creating Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md)</span></span>
* <span data-ttu-id="8d4da-198">toounderstand hello funktioner som kan användas i en Resource Manager-mallen finns [Mallfunktioner](../azure-resource-manager/resource-group-template-functions.md)</span><span class="sxs-lookup"><span data-stu-id="8d4da-198">toounderstand hello functions you can use in an Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md)</span></span>
* <span data-ttu-id="8d4da-199">Anvisningar om hur du skapar mallar finns [Metodtips för att utforma Azure Resource Manager-mallar](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span><span class="sxs-lookup"><span data-stu-id="8d4da-199">For guidance on designing your templates, see [Best practices for designing Azure Resource Manager templates](../azure-resource-manager/best-practices-resource-manager-design-templates.md)</span></span>

