---
title: aaaExport Azure Resource Manager-mall | Microsoft Docs
description: "Använd Azure Resource Manager tooexport en mall från en befintlig resursgrupp."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/06/2017
ms.author: tomfitz
ms.openlocfilehash: 94daa4812da2fec705044ca31c8e74e6d59bd53f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a><span data-ttu-id="69891-103">Exportera en Azure Resource Manager-mall från befintliga resurser</span><span class="sxs-lookup"><span data-stu-id="69891-103">Export an Azure Resource Manager template from existing resources</span></span>
<span data-ttu-id="69891-104">I den här artikeln får du lära dig hur tooexport Resource Manager-mall från befintliga resurser i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69891-104">In this article, you learn how tooexport a Resource Manager template from existing resources in your subscription.</span></span> <span data-ttu-id="69891-105">Du kan använda den genererade mallen toogain en bättre förståelse av mallens syntax.</span><span class="sxs-lookup"><span data-stu-id="69891-105">You can use that generated template toogain a better understanding of template syntax.</span></span>

<span data-ttu-id="69891-106">Det finns två sätt tooexport en mall:</span><span class="sxs-lookup"><span data-stu-id="69891-106">There are two ways tooexport a template:</span></span>

* <span data-ttu-id="69891-107">Du kan exportera hello **faktiska mall som används för distribution av**.</span><span class="sxs-lookup"><span data-stu-id="69891-107">You can export hello **actual template used for deployment**.</span></span> <span data-ttu-id="69891-108">hello exporterade mallen innehåller alla hello parametrar och variabler exakt som de visas i hello ursprungliga mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-108">hello exported template includes all hello parameters and variables exactly as they appeared in hello original template.</span></span> <span data-ttu-id="69891-109">Den här metoden är användbar när du distribuerade resurser via hello portal och vill toosee hello mallen toocreate resurserna.</span><span class="sxs-lookup"><span data-stu-id="69891-109">This approach is helpful when you deployed resources through hello portal, and want toosee hello template toocreate those resources.</span></span> <span data-ttu-id="69891-110">Mallen är enkel att använda.</span><span class="sxs-lookup"><span data-stu-id="69891-110">This template is readily usable.</span></span> 
* <span data-ttu-id="69891-111">Du kan exportera en **genereras mall som representerar hello aktuell status för hello resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="69891-111">You can export a **generated template that represents hello current state of hello resource group**.</span></span> <span data-ttu-id="69891-112">exporterad mall för hello baseras inte på en mall som du använde för distribution.</span><span class="sxs-lookup"><span data-stu-id="69891-112">hello exported template is not based on any template that you used for deployment.</span></span> <span data-ttu-id="69891-113">I stället skapar en mall som är en ögonblicksbild av hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="69891-113">Instead, it creates a template that is a snapshot of hello resource group.</span></span> <span data-ttu-id="69891-114">hello exporterad mall har många hårdkodade värden och förmodligen inte så många parametrar som du vanligtvis definierar.</span><span class="sxs-lookup"><span data-stu-id="69891-114">hello exported template has many hard-coded values and probably not as many parameters as you would typically define.</span></span> <span data-ttu-id="69891-115">Den här metoden är användbar när du har ändrat hello resursgruppen efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-115">This approach is useful when you have modified hello resource group after deployment.</span></span> <span data-ttu-id="69891-116">Du måste vanligtvis göra ändringar i mallen innan du kan använda den.</span><span class="sxs-lookup"><span data-stu-id="69891-116">This template usually requires modifications before it is usable.</span></span>

<span data-ttu-id="69891-117">Det här avsnittet visar båda metoderna hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-117">This topic shows both approaches through hello portal.</span></span>

## <a name="deploy-resources"></a><span data-ttu-id="69891-118">Distribuera resurser</span><span class="sxs-lookup"><span data-stu-id="69891-118">Deploy resources</span></span>
<span data-ttu-id="69891-119">Låt oss börja med att distribuera resurser tooAzure som du kan använda för att exportera som en mall.</span><span class="sxs-lookup"><span data-stu-id="69891-119">Let's start by deploying resources tooAzure that you can use for exporting as a template.</span></span> <span data-ttu-id="69891-120">Om du redan har en resursgrupp i din prenumeration som du vill tooexport tooa mall kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="69891-120">If you already have a resource group in your subscription that you want tooexport tooa template, you can skip this section.</span></span> <span data-ttu-id="69891-121">hello resten av den här artikeln förutsätter att du har distribuerat hello webbapp och SQL database-lösning som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="69891-121">hello remainder of this article assumes you have deployed hello web app and SQL database solution shown in this section.</span></span> <span data-ttu-id="69891-122">Om du använder en annan lösning din upplevelse kan vara lite annorlunda, men hello steg tooexport en mall är hello samma.</span><span class="sxs-lookup"><span data-stu-id="69891-122">If you use a different solution, your experience might be a little different, but hello steps tooexport a template are hello same.</span></span> 

1. <span data-ttu-id="69891-123">I hello [Azure-portalen](https://portal.azure.com)väljer **ny**.</span><span class="sxs-lookup"><span data-stu-id="69891-123">In hello [Azure portal](https://portal.azure.com), select **New**.</span></span>
   
      ![Välj ny](./media/resource-manager-export-template/new.png)
2. <span data-ttu-id="69891-125">Sök efter **webbapp + SQL** och välj den hello tillgängliga alternativ.</span><span class="sxs-lookup"><span data-stu-id="69891-125">Search for **web app + SQL** and select it from hello available options.</span></span>
   
      ![Sök efter webbapp och SQL](./media/resource-manager-export-template/webapp-sql.png)

3. <span data-ttu-id="69891-127">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="69891-127">Select **Create**.</span></span>

      ![Välj Skapa](./media/resource-manager-export-template/create.png)

4. <span data-ttu-id="69891-129">Ange hello krävs värden för hello webbapp och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="69891-129">Provide hello required values for hello web app and SQL database.</span></span> <span data-ttu-id="69891-130">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="69891-130">Select **Create**.</span></span>

      ![Ange webb- och SQL-värde](./media/resource-manager-export-template/provide-web-values.png)

<span data-ttu-id="69891-132">hello distributionen kan ta någon minut.</span><span class="sxs-lookup"><span data-stu-id="69891-132">hello deployment may take a minute.</span></span> <span data-ttu-id="69891-133">När hello distributionen är klar innehåller din prenumeration hello lösning.</span><span class="sxs-lookup"><span data-stu-id="69891-133">After hello deployment finishes, your subscription contains hello solution.</span></span>

## <a name="view-template-from-deployment-history"></a><span data-ttu-id="69891-134">Visa en mall från distributionshistoriken</span><span class="sxs-lookup"><span data-stu-id="69891-134">View template from deployment history</span></span>
1. <span data-ttu-id="69891-135">Gå toohello resursgruppsbladet för din nya resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="69891-135">Go toohello resource group blade for your new resource group.</span></span> <span data-ttu-id="69891-136">Observera att hello-bladet visar hello resultatet av hello senaste distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-136">Notice that hello blade shows hello result of hello last deployment.</span></span> <span data-ttu-id="69891-137">Välj den här länken.</span><span class="sxs-lookup"><span data-stu-id="69891-137">Select this link.</span></span>
   
      ![blad för resursgrupp](./media/resource-manager-export-template/select-deployment.png)
2. <span data-ttu-id="69891-139">Du ser distributionshistoriken för hello grupp.</span><span class="sxs-lookup"><span data-stu-id="69891-139">You see a history of deployments for hello group.</span></span> <span data-ttu-id="69891-140">I ditt fall visar hello bladet förmodligen bara en distribution.</span><span class="sxs-lookup"><span data-stu-id="69891-140">In your case, hello blade probably lists only one deployment.</span></span> <span data-ttu-id="69891-141">Välj den här distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-141">Select this deployment.</span></span>
   
     ![den senaste distributionen](./media/resource-manager-export-template/select-history.png)
3. <span data-ttu-id="69891-143">hello bladet visar en sammanfattning av hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="69891-143">hello blade displays a summary of hello deployment.</span></span> <span data-ttu-id="69891-144">Översikt över hello innehåller hello status för hello-distributionen och dess åtgärder och hello-värden som du angav för parametrarna.</span><span class="sxs-lookup"><span data-stu-id="69891-144">hello summary includes hello status of hello deployment and its operations and hello values that you provided for parameters.</span></span> <span data-ttu-id="69891-145">toosee hello mallen som du använde för hello-distribution, väljer **visa mall**.</span><span class="sxs-lookup"><span data-stu-id="69891-145">toosee hello template that you used for hello deployment, select **View template**.</span></span>
   
     ![visa distributionssammanfattning](./media/resource-manager-export-template/view-template.png)
4. <span data-ttu-id="69891-147">Resource Manager hämtar hello följande sju filer åt dig:</span><span class="sxs-lookup"><span data-stu-id="69891-147">Resource Manager retrieves hello following seven files for you:</span></span>
   
   1. <span data-ttu-id="69891-148">**Mallen** -hello-mallen som definierar hello infrastrukturen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="69891-148">**Template** - hello template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="69891-149">När du skapade hello storage-konto via hello portal Resource Manager används en mall toodeploy det och sparade mallen för framtida bruk.</span><span class="sxs-lookup"><span data-stu-id="69891-149">When you created hello storage account through hello portal, Resource Manager used a template toodeploy it and saved that template for future reference.</span></span>
   2. <span data-ttu-id="69891-150">**Parametrarna** -en parameterfil som du kan använda toopass i värden under distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-150">**Parameters** - A parameter file that you can use toopass in values during deployment.</span></span> <span data-ttu-id="69891-151">Den innehåller hello-värden som du angav under hello första distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-151">It contains hello values that you provided during hello first deployment.</span></span> <span data-ttu-id="69891-152">Du kan ändra dessa värden när du distribuerar hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-152">You can change any of these values when you redeploy hello template.</span></span>
   3. <span data-ttu-id="69891-153">**CLI** -Azure Command-Line-interface (CLI) skriptfilen som du kan använda toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-153">**CLI** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   3. <span data-ttu-id="69891-154">**CLI 2.0** -Azure Command-Line-interface (CLI) skriptfilen som du kan använda toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-154">**CLI 2.0** - An Azure command-line-interface (CLI) script file that you can use toodeploy hello template.</span></span>
   4. <span data-ttu-id="69891-155">**PowerShell** -en Azure PowerShell-skriptfil som du kan använda toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-155">**PowerShell** - An Azure PowerShell script file that you can use toodeploy hello template.</span></span>
   5. <span data-ttu-id="69891-156">**.NET** -A .NET-klass som du kan använda toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-156">**.NET** - A .NET class that you can use toodeploy hello template.</span></span>
   6. <span data-ttu-id="69891-157">**Ruby** -A Ruby-klass som du kan använda toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-157">**Ruby** - A Ruby class that you can use toodeploy hello template.</span></span>
      
      <span data-ttu-id="69891-158">hello filer är tillgängliga via länkar mellan hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="69891-158">hello files are available through links across hello blade.</span></span> <span data-ttu-id="69891-159">Som standard visar hello bladet hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-159">By default, hello blade displays hello template.</span></span>
      
       ![Visa mall](./media/resource-manager-export-template/see-template.png)
      
<span data-ttu-id="69891-161">Den här mallen är hello faktiska mall används toocreate din webbapp och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="69891-161">This template is hello actual template used toocreate your web app and SQL database.</span></span> <span data-ttu-id="69891-162">Observera att den innehåller parametrar som gör att du tooprovide olika värden under distributionen.</span><span class="sxs-lookup"><span data-stu-id="69891-162">Notice it contains parameters that enable you tooprovide different values during deployment.</span></span> <span data-ttu-id="69891-163">toolearn mer om hello strukturen i en mall finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="69891-163">toolearn more about hello structure of a template, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

## <a name="export-hello-template-from-resource-group"></a><span data-ttu-id="69891-164">Exportera hello-mall från resursgruppen</span><span class="sxs-lookup"><span data-stu-id="69891-164">Export hello template from resource group</span></span>
<span data-ttu-id="69891-165">Om du har manuellt dina resurser har ändrats eller lagts till resurser i flera distributioner, avspeglar hämta en mall från hello distributionshistoriken inte hello hello resursgruppen aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69891-165">If you have manually changed your resources or added resources in multiple deployments, retrieving a template from hello deployment history does not reflect hello current state of hello resource group.</span></span> <span data-ttu-id="69891-166">Det här avsnittet visar hur tooexport en mall som visar hello hello resursgruppen aktuella tillstånd.</span><span class="sxs-lookup"><span data-stu-id="69891-166">This section shows you how tooexport a template that reflects hello current state of hello resource group.</span></span> 

> [!NOTE]
> <span data-ttu-id="69891-167">Du kan inte exportera en mall för en resursgrupp som har fler än 200 resurser.</span><span class="sxs-lookup"><span data-stu-id="69891-167">You cannot export a template for a resource group that has more than 200 resources.</span></span>
> 
> 

1. <span data-ttu-id="69891-168">tooview hello mall för en resursgrupp väljer **automatiseringsskriptet**.</span><span class="sxs-lookup"><span data-stu-id="69891-168">tooview hello template for a resource group, select **Automation script**.</span></span>
   
      ![Exportera resursgrupp](./media/resource-manager-export-template/select-automation.png)
   
     <span data-ttu-id="69891-170">Resource Manager utvärderar hello resurser i hello resursgrupp och genererar en mall för dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="69891-170">Resource Manager evaluates hello resources in hello resource group, and generates a template for those resources.</span></span> <span data-ttu-id="69891-171">Inte alla resurstyper stöder hello mallfunktionen för export.</span><span class="sxs-lookup"><span data-stu-id="69891-171">Not all resource types support hello export template function.</span></span> <span data-ttu-id="69891-172">Du kan se ett felmeddelande om att det finns ett problem med hello export.</span><span class="sxs-lookup"><span data-stu-id="69891-172">You may see an error stating that there is a problem with hello export.</span></span> <span data-ttu-id="69891-173">Du lär dig hur toohandle de problem i hello [korrigering export problem](#fix-export-issues) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="69891-173">You learn how toohandle those issues in hello [Fix export issues](#fix-export-issues) section.</span></span>
2. <span data-ttu-id="69891-174">Du ser igen hello sex filer som du kan använda tooredeploy hello lösning.</span><span class="sxs-lookup"><span data-stu-id="69891-174">You again see hello six files that you can use tooredeploy hello solution.</span></span> <span data-ttu-id="69891-175">Den här gången hello mallen är dock något annorlunda.</span><span class="sxs-lookup"><span data-stu-id="69891-175">However, this time hello template is a little different.</span></span> <span data-ttu-id="69891-176">Observera att hello genererade mallen innehåller färre parametrar än hello mallen i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="69891-176">Notice that hello generated template contains fewer parameters than hello template in previous section.</span></span> <span data-ttu-id="69891-177">Många av hello värden (t.ex. platsen och SKU-värden) är också hårdkodat i den här mallen i stället för att acceptera ett parametervärde.</span><span class="sxs-lookup"><span data-stu-id="69891-177">Also, many of hello values (like location and SKU values) are hard-coded in this template rather than accepting a parameter value.</span></span> <span data-ttu-id="69891-178">Innan den här mallen, kanske du vill tooedit hello mallen toomake bättre användning av parametrar.</span><span class="sxs-lookup"><span data-stu-id="69891-178">Before reusing this template, you might want tooedit hello template toomake better use of parameters.</span></span> 
   
3. <span data-ttu-id="69891-179">Du har två olika alternativ för fortsatt toowork med den här mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-179">You have a couple of options for continuing toowork with this template.</span></span> <span data-ttu-id="69891-180">Du kan hämta hello mall och arbeta med den lokalt med en JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="69891-180">You can either download hello template and work on it locally with a JSON editor.</span></span> <span data-ttu-id="69891-181">Eller, du kan spara hello mallbibliotek tooyour och arbeta med den hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-181">Or, you can save hello template tooyour library and work on it through hello portal.</span></span>
   
     <span data-ttu-id="69891-182">Om du föredrar att använda en JSON-redigerare som [VS kod](https://code.visualstudio.com/) eller [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), kanske du föredrar hämta hello mall lokalt och använda den editor.</span><span class="sxs-lookup"><span data-stu-id="69891-182">If you are comfortable using a JSON editor like [VS Code](https://code.visualstudio.com/) or [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), you might prefer downloading hello template locally and using that editor.</span></span> <span data-ttu-id="69891-183">toowork lokalt, Välj **hämta**.</span><span class="sxs-lookup"><span data-stu-id="69891-183">toowork locally, select **Download**.</span></span>
   
      ![Ladda ned mall](./media/resource-manager-export-template/download-template.png)
   
     <span data-ttu-id="69891-185">Om du inte har ställt in med en JSON-redigerare kanske du föredrar att redigera mallen hello hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-185">If you are not set up with a JSON editor, you might prefer editing hello template through hello portal.</span></span> <span data-ttu-id="69891-186">hello resten av det här avsnittet förutsätter att du har sparat hello tooyour mallbibliotek i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-186">hello remainder of this topic assumes you have saved hello template tooyour library in hello portal.</span></span> <span data-ttu-id="69891-187">Dock se hello samma syntax ändrar toohello mallen om du arbetar lokalt med en JSON-redigerare eller hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-187">However, you make hello same syntax changes toohello template whether working locally with a JSON editor or through hello portal.</span></span> <span data-ttu-id="69891-188">toowork hello-portalen, Välj **lägga till toolibrary**.</span><span class="sxs-lookup"><span data-stu-id="69891-188">toowork through hello portal, select **Add toolibrary**.</span></span>
   
      ![Lägg till toolibrary](./media/resource-manager-export-template/add-to-library.png)
   
     <span data-ttu-id="69891-190">När du lägger till ett bibliotek av mallar toohello ge hello mallen ett namn och beskrivning.</span><span class="sxs-lookup"><span data-stu-id="69891-190">When adding a template toohello library, give hello template a name and description.</span></span> <span data-ttu-id="69891-191">Välj sedan **Spara**.</span><span class="sxs-lookup"><span data-stu-id="69891-191">Then, select **Save**.</span></span>
   
     ![ange mallvärden](./media/resource-manager-export-template/save-library-template.png)
4. <span data-ttu-id="69891-193">tooview en mall som sparats i biblioteket, Välj **fler tjänster**, typen **mallar** toofilter resultat, väljer **mallar**.</span><span class="sxs-lookup"><span data-stu-id="69891-193">tooview a template saved in your library, select **More services**, type **Templates** toofilter results, select **Templates**.</span></span>
   
      ![hitta mallar](./media/resource-manager-export-template/find-templates.png)
5. <span data-ttu-id="69891-195">Välj hello mall med hello-namnet som du har sparat.</span><span class="sxs-lookup"><span data-stu-id="69891-195">Select hello template with hello name you saved.</span></span>
   
      ![välj mall](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-hello-template"></a><span data-ttu-id="69891-197">Anpassa hello-mall</span><span class="sxs-lookup"><span data-stu-id="69891-197">Customize hello template</span></span>
<span data-ttu-id="69891-198">hello exporterade mallen fungerar bra om du vill toocreate hello samma web app och SQL-databasen för varje distribution.</span><span class="sxs-lookup"><span data-stu-id="69891-198">hello exported template works fine if you want toocreate hello same web app and SQL database for every deployment.</span></span> <span data-ttu-id="69891-199">Resource Manager innehåller dock alternativ som gör att du kan distribuera mallar med mycket bättre flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="69891-199">However, Resource Manager provides options so that you can deploy templates with a lot more flexibility.</span></span> <span data-ttu-id="69891-200">Den här artikeln visar hur tooadd parametrar för hello databasen administratörens namn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="69891-200">This article shows you how tooadd parameters for hello database administrator name and password.</span></span> <span data-ttu-id="69891-201">Du kan använda den här samma metod tooadd mer flexibilitet för andra värden i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-201">You can use this same approach tooadd more flexibility for other values in hello template.</span></span>

1. <span data-ttu-id="69891-202">toocustomize hello mall och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="69891-202">toocustomize hello template, select **Edit**.</span></span>
   
     ![visa mall](./media/resource-manager-export-template/select-edit.png)
2. <span data-ttu-id="69891-204">Välj hello mall.</span><span class="sxs-lookup"><span data-stu-id="69891-204">Select hello template.</span></span>
   
     ![redigera mall](./media/resource-manager-export-template/select-added-template.png)
3. <span data-ttu-id="69891-206">toobe kan toopass hello värden som du kanske vill toospecify under distributionen som lägger till följande två parametrar toohello hello **parametrar** avsnitt i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="69891-206">toobe able toopass hello values that you might want toospecify during deployment, add hello following two parameters toohello **parameters** section in hello template:</span></span>

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. <span data-ttu-id="69891-207">toouse hello nya parametrar, Ersätt hello SQL server-definition i hello **resurser** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="69891-207">toouse hello new parameters, replace hello SQL server definition in hello **resources** section.</span></span> <span data-ttu-id="69891-208">Observera att parametervärden nu används för **administratorLogin** och **administratorLoginPassword**.</span><span class="sxs-lookup"><span data-stu-id="69891-208">Notice that **administratorLogin** and **administratorLoginPassword** now use parameter values.</span></span>

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. <span data-ttu-id="69891-209">Välj **OK** när du är klar redigera hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="69891-209">Select **OK** when you are done editing hello template.</span></span>
7. <span data-ttu-id="69891-210">Välj **spara** toosave hello ändringar toohello mall.</span><span class="sxs-lookup"><span data-stu-id="69891-210">Select **Save** toosave hello changes toohello template.</span></span>
   
     ![spara mall](./media/resource-manager-export-template/save-template.png)
8. <span data-ttu-id="69891-212">tooredeploy hello uppdateras mall och välj **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="69891-212">tooredeploy hello updated template, select **Deploy**.</span></span>
   
     ![distribuera mallen](./media/resource-manager-export-template/redeploy-template.png)
9. <span data-ttu-id="69891-214">Ange parametervärden och välj en resurs toodeploy hello gruppresurser till.</span><span class="sxs-lookup"><span data-stu-id="69891-214">Provide parameter values, and select a resource group toodeploy hello resources to.</span></span>


## <a name="fix-export-issues"></a><span data-ttu-id="69891-215">Åtgärda exportproblem</span><span class="sxs-lookup"><span data-stu-id="69891-215">Fix export issues</span></span>
<span data-ttu-id="69891-216">Inte alla resurstyper stöder hello mallfunktionen för export.</span><span class="sxs-lookup"><span data-stu-id="69891-216">Not all resource types support hello export template function.</span></span> <span data-ttu-id="69891-217">tooresolve problemet kan manuellt lägga till hello saknas resurser i mallen igen.</span><span class="sxs-lookup"><span data-stu-id="69891-217">tooresolve this issue, manually add hello missing resources back into your template.</span></span> <span data-ttu-id="69891-218">hello felmeddelande innehåller hello resurstyper som inte kan exporteras.</span><span class="sxs-lookup"><span data-stu-id="69891-218">hello error message includes hello resource types that cannot be exported.</span></span> <span data-ttu-id="69891-219">Leta reda på resurstypen i [mallreferensen](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="69891-219">Find that resource type in [Template reference](/azure/templates/).</span></span> <span data-ttu-id="69891-220">Till exempel toomanually lägga till en virtuell nätverksgateway, se [Microsoft.Network/virtualNetworkGateways mallreferensen](/azure/templates/microsoft.network/virtualnetworkgateways).</span><span class="sxs-lookup"><span data-stu-id="69891-220">For example, toomanually add a virtual network gateway, see [Microsoft.Network/virtualNetworkGateways template reference](/azure/templates/microsoft.network/virtualnetworkgateways).</span></span>

> [!NOTE]
> <span data-ttu-id="69891-221">Exportproblem uppstår bara när du exporterar från en resursgrupp i stället för från distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="69891-221">You only encounter export issues when exporting from a resource group rather than from your deployment history.</span></span> <span data-ttu-id="69891-222">Om distributionen senaste motsvarar hello aktuell status för hello resursgrupp, ska du exportera hello mallen från hello distributionshistoriken i stället för från hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="69891-222">If your last deployment accurately represents hello current state of hello resource group, you should export hello template from hello deployment history rather than from hello resource group.</span></span> <span data-ttu-id="69891-223">Exportera endast från en resursgrupp när du har gjort ändringar toohello resursgrupp som inte har definierats i en mall.</span><span class="sxs-lookup"><span data-stu-id="69891-223">Only export from a resource group when you have made changes toohello resource group that are not defined in a single template.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="69891-224">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69891-224">Next steps</span></span>
<span data-ttu-id="69891-225">Du har lärt dig hur tooexport en mall från resurser som du skapade i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="69891-225">You have learned how tooexport a template from resources that you created in hello portal.</span></span>

* <span data-ttu-id="69891-226">Du kan distribuera en mall genom [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md) eller [REST API](resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="69891-226">You can deploy a template through [PowerShell](resource-group-template-deploy.md), [Azure CLI](resource-group-template-deploy-cli.md), or [REST API](resource-group-template-deploy-rest.md).</span></span>
* <span data-ttu-id="69891-227">hur tooexport en mall med PowerShell, se toosee [med hjälp av Azure PowerShell med Azure Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="69891-227">toosee how tooexport a template through PowerShell, see [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="69891-228">hur tooexport en mall med Azure CLI, se toosee [Använd hello Azure CLI för Mac, Linux och Windows med Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="69891-228">toosee how tooexport a template through Azure CLI, see [Use hello Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>

