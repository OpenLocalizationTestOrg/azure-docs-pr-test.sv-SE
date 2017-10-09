---
title: "aaaDeploy Skalningsuppsättning virtuell dator med hjälp av Visual Studio | Microsoft Docs"
description: "Distribuera Skalningsuppsättningar i virtuella datorer med hjälp av Visual Studio och Resource Manager-mall"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a><span data-ttu-id="a4bd0-103">Hur toocreate en Virtual Machine Scale Set med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4bd0-103">How toocreate a Virtual Machine Scale Set with Visual Studio</span></span>
<span data-ttu-id="a4bd0-104">Den här artikeln visar hur toodeploy ett Azure Virtual Machine Scale Set med hjälp av en Visual Studio resurs distribution.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-104">This article shows you how toodeploy an Azure Virtual Machine Scale Set using a Visual Studio Resource Group Deployment.</span></span>

<span data-ttu-id="a4bd0-105">[Azure Virtual Machine-Skalningsuppsättningar](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) är en Azure Compute resurs toodeploy och hantera en samling liknande virtuella datorer med Autoskala och belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-105">[Azure Virtual Machine Scale Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) is an Azure Compute resource toodeploy and manage a collection of similar virtual machines with auto-scale and load balancing.</span></span> <span data-ttu-id="a4bd0-106">Du kan etablera och distribuera Skalningsuppsättningar i virtuella datorer med hjälp av [Azure Resource Manager-mallar](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-106">You can provision and deploy Virtual Machine Scale Sets using [Azure Resource Manager Templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="a4bd0-107">Azure Resource Manager-mallar som kan distribueras med hjälp av Azure CLI, PowerShell, REST och även direkt från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-107">Azure Resource Manager Templates can be deployed using Azure CLI, PowerShell, REST and also directly from Visual Studio.</span></span> <span data-ttu-id="a4bd0-108">Visual Studio tillhandahåller en uppsättning exempel mallar, som kan distribueras som en del av en grupp distributionsprojekt för Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-108">Visual Studio provides a set of example templates, which can be deployed as part of an Azure Resource Group Deployment project.</span></span>

<span data-ttu-id="a4bd0-109">Azure-resursgrupp distributioner är ett sätt toogroup och publicera en uppsättning relaterade Azure-resurser i en enskild distribution-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-109">Azure Resource Group deployments are a way toogroup and publish a set of related Azure resources in a single deployment operation.</span></span> <span data-ttu-id="a4bd0-110">Du kan lära dig mer om de här: [skapa och distribuera Azure-resursgrupper via Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-110">You can learn more about them here: [Creating and deploying Azure resource groups through Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a4bd0-111">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="a4bd0-111">Pre-requisites</span></span>
<span data-ttu-id="a4bd0-112">tooget igång med att distribuera Skalningsuppsättningar i virtuella datorer i Visual Studio, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a4bd0-112">tooget started deploying Virtual Machine Scale Sets in Visual Studio, you need hello following:</span></span>

* <span data-ttu-id="a4bd0-113">Visual Studio 2013 eller senare</span><span class="sxs-lookup"><span data-stu-id="a4bd0-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="a4bd0-114">Azure SDK 2.7, 2.8 eller 2.9</span><span class="sxs-lookup"><span data-stu-id="a4bd0-114">Azure SDK 2.7, 2.8 or 2.9</span></span>

>[!NOTE]
><span data-ttu-id="a4bd0-115">Dessa instruktioner förutsätter att du använder Visual Studio med [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-115">These instructions assume you are using Visual Studio with [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span></span>

## <a name="creating-a-project"></a><span data-ttu-id="a4bd0-116">Skapa ett projekt</span><span class="sxs-lookup"><span data-stu-id="a4bd0-116">Creating a Project</span></span>
1. <span data-ttu-id="a4bd0-117">Skapa ett nytt projekt i Visual Studio genom att välja **filen | Nya | Projektet**.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-117">Create a new project in Visual Studio by choosing **File | New | Project**.</span></span>
   
    ![Ny fil][file_new]

2. <span data-ttu-id="a4bd0-119">Under **Visual C# | Molnet**, Välj **Azure Resource Manager** toocreate ett projekt för distribution av en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-119">Under **Visual C# | Cloud**, choose **Azure Resource Manager** toocreate a project for deploying an Azure Resource Manager Template.</span></span>
   
    ![Skapa projekt][create_project]

3. <span data-ttu-id="a4bd0-121">Hello listan över mallar och väljer du hello Linux eller Windows Virtual Machine Scale ange mall.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-121">From hello list of Templates, select either hello Linux or Windows Virtual Machine Scale Set Template.</span></span>
   
   ![Välj mall][select_Template]

4. <span data-ttu-id="a4bd0-123">När projektet har skapats visas PowerShell-skript för distribution, en Azure Resource Manager-mall och en parameterfil för hello Skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-123">Once your project is created you see PowerShell deployment scripts, an Azure Resource Manager Template, and a parameter file for hello Virtual Machine Scale Set.</span></span>
   
    ![Solution Explorer][solution_explorer]

## <a name="customize-your-project"></a><span data-ttu-id="a4bd0-125">Anpassa ditt projekt</span><span class="sxs-lookup"><span data-stu-id="a4bd0-125">Customize your project</span></span>
<span data-ttu-id="a4bd0-126">Nu kan du redigera hello mallen toocustomize ladda den för programmets behov, till exempel lägga till egenskaperna för VM-tillägget eller redigera regler för belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-126">Now you can edit hello Template toocustomize it for your application's needs, such as adding VM extension properties or editing load balancing rules.</span></span> <span data-ttu-id="a4bd0-127">Ange hello Virtual Machine Scale-mallar är som standard konfigurerade toodeploy hello AzureDiagnostics tillägg, vilket gör det enkelt tooadd Autoskala regler.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-127">By default hello Virtual Machine Scale Set Templates are configured toodeploy hello AzureDiagnostics extension, which makes it easy tooadd autoscale rules.</span></span> <span data-ttu-id="a4bd0-128">Den distribuerar också en belastningsutjämnare med en offentlig IP-adress som konfigurerats med ingående NAT-regler.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-128">It also deploys a load balancer with a public IP address, configured with inbound NAT rules.</span></span> 

<span data-ttu-id="a4bd0-129">hello belastningsutjämnare kan du ansluta toohello VM-instanser med SSH (Linux) eller RDP (Windows).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-129">hello load balancer lets you connect toohello VM instances with SSH (Linux) or RDP (Windows).</span></span> <span data-ttu-id="a4bd0-130">hello frontend portintervall startar vid 50000.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-130">hello front-end port range starts at 50000.</span></span> <span data-ttu-id="a4bd0-131">För linux det innebär att om du SSH tooport 50000, är routade tooport 22 av hello första VM i hello skalan.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-131">For linux this means that if you SSH tooport 50000, you are routed tooport 22 of hello first VM in hello Scale Set.</span></span> <span data-ttu-id="a4bd0-132">Ansluta tooport 50001 är routade tooport 22 av hello andra VM och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-132">Connecting tooport 50001 is routed tooport 22 of hello second VM and so on.</span></span>

 <span data-ttu-id="a4bd0-133">Ett bra sätt tooedit mallarna med Visual Studio är toouse hello JSON-disposition tooorganize hello parametrar, variabler och resurser.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-133">A good way tooedit your Templates with Visual Studio is toouse hello JSON Outline tooorganize hello parameters, variables, and resources.</span></span> <span data-ttu-id="a4bd0-134">Förstå hello peka schemat Visual Studio ut fel i mallen innan du distribuerar den.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-134">With an understanding of hello schema Visual Studio can point out errors in your Template before you deploy it.</span></span>

![JSON-Explorer][json_explorer]

## <a name="deploy-hello-project"></a><span data-ttu-id="a4bd0-136">Distribuera hello-projekt</span><span class="sxs-lookup"><span data-stu-id="a4bd0-136">Deploy hello project</span></span>
1. <span data-ttu-id="a4bd0-137">Distribuera hello Azure Resource Manager-mall toocreate hello Skaluppsättning för virtuell dator resurs.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-137">Deploy hello Azure Resource Manager Template toocreate hello Virtual Machine Scale Set resource.</span></span> <span data-ttu-id="a4bd0-138">Högerklicka på projektnoden hello och välj **distribuera | Ny distribution**.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-138">Right-click on hello project node and choose **Deploy | New Deployment**.</span></span>
   
    ![Distribuera mallen][5deploy_Template]
    
2. <span data-ttu-id="a4bd0-140">Välj prenumerationen i dialogrutan för hello ”distribuera tooResource grupp”.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-140">Select your subscription in hello “Deploy tooResource Group” dialog.</span></span>
   
    ![Distribuera mallen][6deploy_Template]

3. <span data-ttu-id="a4bd0-142">Härifrån kan skapa du en Azure-resursgrupp toodeploy din mall.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-142">From here, you can create an Azure Resource Group toodeploy your Template to.</span></span>
   
    ![Ny resursgrupp][new_resource]

4. <span data-ttu-id="a4bd0-144">Klicka sedan på **redigera parametrar** tooenter parametrar som skickas tooyour mall.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-144">Next, click **Edit Parameters** tooenter parameters that are passed tooyour Template.</span></span> <span data-ttu-id="a4bd0-145">Ange hello användarnamn och lösenord för hello OS, som är nödvändiga toocreate hello distribution.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-145">Provide hello username and password for hello OS, which is required toocreate hello deployment.</span></span> <span data-ttu-id="a4bd0-146">Om du inte har PowerShell-verktyg för Visual Studio installerat, bör toocheck **spara lösenord** tooavoid en dold PowerShell kommandoradsverktyget fråga eller Använd [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-146">If you don't have PowerShell Tools for Visual Studio installed, it is recommended toocheck **Save passwords** tooavoid a hidden PowerShell command-line prompt, or use [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span></span>
   
    ![Redigera parametrar][edit_parameters]

5. <span data-ttu-id="a4bd0-148">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-148">Now click **Deploy**.</span></span> <span data-ttu-id="a4bd0-149">Hej **utdata** visar hello distribution pågår.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-149">hello **Output** window shows hello deployment progress.</span></span> <span data-ttu-id="a4bd0-150">Observera att hello-åtgärden körs hello **distribuera AzureResourceGroup.ps1** skript.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-150">Note that hello action is executing hello **Deploy-AzureResourceGroup.ps1** script.</span></span>
   
   ![Utdatafönstret][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a><span data-ttu-id="a4bd0-152">Utforska dina virtuella datorns Skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="a4bd0-152">Exploring your Virtual Machine Scale Set</span></span>
<span data-ttu-id="a4bd0-153">När hello distributionen är klar kan du visa hello nya Virtual Machine Scale Set i hello Visual Studio **Cloud Explorer** (uppdatera hello lista).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-153">Once hello deployment completes, you can view hello new Virtual Machine Scale Set in hello Visual Studio **Cloud Explorer** (refresh hello list).</span></span> <span data-ttu-id="a4bd0-154">Cloud Explorer kan du hantera Azure-resurser i Visual Studio när du utvecklar program.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-154">Cloud Explorer lets you manage Azure resources in Visual Studio while developing applications.</span></span> <span data-ttu-id="a4bd0-155">Du kan också visa Skalningsuppsättning i virtuell dator i hello [Azure-portalen](https://portal.azure.com) och [resursutforskaren Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-155">You can also view your Virtual Machine Scale Set in hello [Azure portal](https://portal.azure.com) and [Azure Resource Explorer](https://resources.azure.com/).</span></span>

![Cloud Explorer][cloud_explorer]

 <span data-ttu-id="a4bd0-157">hello portal innehåller hello bästa sättet toovisually hanterar din Azure-infrastrukturen med en webbläsare, medan resursutforskaren Azure tillhandahåller ett enkelt sätt tooexplore och felsöka Azure-resurser, ger ett fönster i hello ”instans visa” och också med PowerShell kommandon för hello resurser du tittar på.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-157">hello portal provides hello best way toovisually manage your Azure infrastructure with a web browser, while Azure Resource Explorer provides an easy way tooexplore and debug Azure resources, giving a window into hello "instance view" and also showing PowerShell commands for hello resources you are looking at.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4bd0-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4bd0-158">Next steps</span></span>
<span data-ttu-id="a4bd0-159">När du har distribuerats Skalningsuppsättningar i virtuella datorer via Visual Studio, kan du anpassa din project toosuit kraven för application.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-159">Once you’ve successfully deployed Virtual Machine Scale Sets through Visual Studio, you can further customize your project toosuit your application requirements.</span></span> <span data-ttu-id="a4bd0-160">Till exempel konfigurera Autoskala genom att lägga till en **Insights** resurs, lägga till infrastruktur tooyour mall (t.ex. fristående virtuella datorer) eller distribuera program med hjälp av hello tillägget för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="a4bd0-160">For example, configure auto-scale by adding an **Insights** resource, adding infrastructure tooyour Template (like standalone VMs), or deploying applications using hello custom script extension.</span></span> <span data-ttu-id="a4bd0-161">Bra exempel mallar finns i hello [Azure Quickstart mallar](https://github.com/Azure/azure-quickstart-templates) GitHub-lagringsplatsen (Sök efter ”vmss”).</span><span class="sxs-lookup"><span data-stu-id="a4bd0-161">Good example templates can be found in hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) GitHub repository (search for "vmss").</span></span>

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
