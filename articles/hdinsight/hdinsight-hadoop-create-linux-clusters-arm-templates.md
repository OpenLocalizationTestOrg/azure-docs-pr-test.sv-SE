---
title: "aaaCreate Hadoop-kluster med hjälp av mallar - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate kluster för HDInsight med hjälp av Resource Manager-mallar"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a><span data-ttu-id="1cc7f-103">Skapa Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1cc7f-103">Create Hadoop clusters in HDInsight by using Resource Manager templates</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="1cc7f-104">I den här artikeln får du lära dig på flera olika sätt toocreate Azure HDInsight-kluster med Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-104">In this article, you learn several ways toocreate Azure HDInsight clusters with Azure Resource Manager templates.</span></span> <span data-ttu-id="1cc7f-105">Mer information finns i [distribuera ett program med Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-105">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="1cc7f-106">toolearn om andra verktyg för att skapa klustret och funktioner, klickar du på flikväljaren för hello hello längst upp i den här sidan eller se [kluster metoder för att skapa](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-106">toolearn about other cluster creation tools and features, click hello tab selector on hello top of this page or see [Cluster creation methods](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cc7f-107">Krav</span><span class="sxs-lookup"><span data-stu-id="1cc7f-107">Prerequisites</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="1cc7f-108">toofollow hello anvisningarna i den här artikeln, behöver du:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-108">toofollow hello instructions in this article, you'll need:</span></span>

* <span data-ttu-id="1cc7f-109">En [Azure-prenumeration](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-109">An [Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="1cc7f-110">Azure PowerShell och/eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-110">Azure PowerShell and/or Azure CLI.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a><span data-ttu-id="1cc7f-111">Mallar för Resurshanteraren</span><span class="sxs-lookup"><span data-stu-id="1cc7f-111">Resource Manager templates</span></span>
<span data-ttu-id="1cc7f-112">En mall för Resource Manager gör det enkelt toocreate hello följande för ditt program i en enda, samordnad åtgärd:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-112">A Resource Manager template makes it easy toocreate hello following for your application in a single, coordinated operation:</span></span>
* <span data-ttu-id="1cc7f-113">HDInsight-kluster och deras beroende resurser (till exempel hello standardkontot för lagring)</span><span class="sxs-lookup"><span data-stu-id="1cc7f-113">HDInsight clusters and their dependent resources (such as hello default storage account)</span></span>
* <span data-ttu-id="1cc7f-114">Andra resurser (till exempel Azure SQL Database toouse Apache Sqoop)</span><span class="sxs-lookup"><span data-stu-id="1cc7f-114">Other resources (such as Azure SQL Database toouse Apache Sqoop)</span></span>

<span data-ttu-id="1cc7f-115">I hello mallen definierar du hello-resurser som behövs för hello program.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-115">In hello template, you define hello resources that are needed for hello application.</span></span> <span data-ttu-id="1cc7f-116">Du kan också ange distribution parametrar tooinput värden för olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-116">You also specify deployment parameters tooinput values for different environments.</span></span> <span data-ttu-id="1cc7f-117">hello mallen består av JSON och uttryck som du använder tooconstruct värden för din distribution.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-117">hello template consists of JSON and expressions that you use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="1cc7f-118">Du kan hitta HDInsight mall prov på [Azure Quickstart mallar](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-118">You can find HDInsight template samples at [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span></span> <span data-ttu-id="1cc7f-119">Använda plattformsoberoende [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) med hello [Resource Manager tillägget](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) eller en text editor toosave hello-mall till en fil på din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-119">Use cross-platform [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) with hello [Resource Manager extension](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) or a text editor toosave hello template into a file on your workstation.</span></span> <span data-ttu-id="1cc7f-120">Du lär dig hur toocall hello mallen med hjälp av olika metoder.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-120">You learn how toocall hello template by using different methods.</span></span>

<span data-ttu-id="1cc7f-121">Mer information om Resource Manager-mallar finns i hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-121">For more information about Resource Manager templates, see hello following articles:</span></span>

* [<span data-ttu-id="1cc7f-122">Redigera Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1cc7f-122">Author Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="1cc7f-123">Distribuera ett program med Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="1cc7f-123">Deploy an application with Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a><span data-ttu-id="1cc7f-124">Skapa mallar</span><span class="sxs-lookup"><span data-stu-id="1cc7f-124">Generate templates</span></span>

<span data-ttu-id="1cc7f-125">Med hjälp av hello Azure-portalen kan du konfigurera alla hello egenskaper i ett kluster och sedan spara hello mallen innan du distribuerar den.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-125">By using hello Azure portal, you can configure all hello properties of a cluster and then save hello template before deploying it.</span></span> <span data-ttu-id="1cc7f-126">Du kan sedan återanvända hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-126">You can then reuse hello template.</span></span>

<span data-ttu-id="1cc7f-127">**toogenerate en mall med hjälp av hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="1cc7f-127">**toogenerate a template by using hello Azure portal**</span></span>

1. <span data-ttu-id="1cc7f-128">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="1cc7f-129">Klicka på **ny** på hello vänstra menyn **Intelligence + analys**, och klicka sedan på **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-129">Click **New** on hello left menu, click **Intelligence + analytics**, and then click **HDInsight**.</span></span>
3. <span data-ttu-id="1cc7f-130">Följ hello instruktioner tooenter egenskaper.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-130">Follow hello instructions tooenter properties.</span></span> <span data-ttu-id="1cc7f-131">Du kan använda antingen hello **Snabbregistrering** eller hello **anpassad** alternativet.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-131">You can use either hello **Quick create** or hello **Custom** option.</span></span>
4. <span data-ttu-id="1cc7f-132">På hello **sammanfattning** klickar du på **ladda ned mall och parametrar**:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-132">On hello **Summary** tab, click **Download template and parameters**:</span></span>

    ![HDInsight Hadoop skapa klustret Resource Manager hämtar en mall](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    <span data-ttu-id="1cc7f-134">Du kan se en lista över hello mallfil, parameterfilen och koden exempel som används för toodeploy hello mall:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-134">You see a list of hello template file, parameters file, and code samples used toodeploy hello template:</span></span>

    ![HDInsight Hadoop Skapa kluster Hämtningsalternativ för Resource Manager-mall](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    <span data-ttu-id="1cc7f-136">Från den här du hämta hello mall, spara den tooyour mallbibliotek eller distribuera hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-136">From here, you can download hello template, save it tooyour template library, or deploy hello template.</span></span>

    <span data-ttu-id="1cc7f-137">tooaccess en mall i biblioteket, klicka på **fler tjänster** hello vänstra menyn och klicka sedan på **mallar** (under hello **andra** kategori).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-137">tooaccess a template in your library, click **More services** from hello left menu, and then click **Templates** (under hello **Other** category).</span></span>

    > [!Note]
    > <span data-ttu-id="1cc7f-138">hello mall och parametrar filen måste användas tillsammans.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-138">hello template and parameters file must be used together.</span></span> <span data-ttu-id="1cc7f-139">Annars kan du få oväntade resultat.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-139">Otherwise, you might get unexpected results.</span></span> <span data-ttu-id="1cc7f-140">Till exempel hello standard **clusterKind** egenskapsvärdet är alltid **hadoop**, trots vad du ange innan du hämtar hello mall.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-140">For example, hello default **clusterKind** property value is always **hadoop**, despite what you specify before you download hello template.</span></span>



## <a name="deploy-with-powershell"></a><span data-ttu-id="1cc7f-141">Distribuera med PowerShell</span><span class="sxs-lookup"><span data-stu-id="1cc7f-141">Deploy with PowerShell</span></span>

<span data-ttu-id="1cc7f-142">Den här proceduren skapar ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-142">This procedure creates a Hadoop cluster in HDInsight.</span></span>

1. <span data-ttu-id="1cc7f-143">Spara hello JSON-fil i hello [bilaga](#appx-a-arm-template) tooyour arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-143">Save hello JSON file in hello [Appendix](#appx-a-arm-template) tooyour workstation.</span></span> <span data-ttu-id="1cc7f-144">I hello PowerShell-skript, hello filnamnet är `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-144">In hello PowerShell script, hello file name is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span></span>
2. <span data-ttu-id="1cc7f-145">Ange hello parametrar och variabler vid behov.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-145">Set hello parameters and variables if needed.</span></span>
3. <span data-ttu-id="1cc7f-146">Kör hello mallen med hjälp av hello följande PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-146">Run hello template by using hello following PowerShell script:</span></span>

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    <span data-ttu-id="1cc7f-147">hello PowerShell-skript som konfigurerar bara hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-147">hello PowerShell script configures only hello cluster name.</span></span> <span data-ttu-id="1cc7f-148">Hej lagringskontonamnet är hårdkodat i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-148">hello storage account name is hard-coded in hello template.</span></span> <span data-ttu-id="1cc7f-149">Du kan ange tooenter hello klustret användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-149">You are prompted tooenter hello cluster user password.</span></span> <span data-ttu-id="1cc7f-150">(hello Standardanvändarnamnet är **admin**.) Du kan också ange tooenter hello SSH användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-150">(hello default username is **admin**.) You are also prompted tooenter hello SSH user password.</span></span> <span data-ttu-id="1cc7f-151">(hello SSH Standardanvändarnamnet är **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="1cc7f-151">(hello default SSH username is **sshuser**.)</span></span>  

<span data-ttu-id="1cc7f-152">Mer information finns i [distribuera med PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-152">For more information, see  [Deploy with PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span>

## <a name="deploy-with-cli"></a><span data-ttu-id="1cc7f-153">Distribuera med CLI</span><span class="sxs-lookup"><span data-stu-id="1cc7f-153">Deploy with CLI</span></span>
<span data-ttu-id="1cc7f-154">följande exempel hello använder Azure-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-154">hello following sample uses Azure command-line interface (CLI).</span></span> <span data-ttu-id="1cc7f-155">Den skapar ett kluster och dess beroende lagringskontot och behållare genom att anropa en Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-155">It creates a cluster and its dependent storage account and container by calling a Resource Manager template:</span></span>

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

<span data-ttu-id="1cc7f-156">Du kommer att tillfrågas tooenter:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-156">You are prompted tooenter:</span></span>
* <span data-ttu-id="1cc7f-157">hello klusternamnet.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-157">hello cluster name.</span></span>
* <span data-ttu-id="1cc7f-158">hello klustret användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-158">hello cluster user password.</span></span> <span data-ttu-id="1cc7f-159">(hello Standardanvändarnamnet är **admin**.)</span><span class="sxs-lookup"><span data-stu-id="1cc7f-159">(hello default username is **admin**.)</span></span>
* <span data-ttu-id="1cc7f-160">hello SSH användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-160">hello SSH user password.</span></span> <span data-ttu-id="1cc7f-161">(hello SSH Standardanvändarnamnet är **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="1cc7f-161">(hello default SSH username is **sshuser**.)</span></span>

<span data-ttu-id="1cc7f-162">hello följande kod innehåller infogade parametrar:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-162">hello following code provides inline parameters:</span></span>

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="1cc7f-163">Distribuera med hello REST API</span><span class="sxs-lookup"><span data-stu-id="1cc7f-163">Deploy with hello REST API</span></span>
<span data-ttu-id="1cc7f-164">Se [distribuera med hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-164">See [Deploy with hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span></span>

## <a name="deploy-with-visual-studio"></a><span data-ttu-id="1cc7f-165">Distribuera med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1cc7f-165">Deploy with Visual Studio</span></span>
 <span data-ttu-id="1cc7f-166">Använda Visual Studio toocreate en resursgruppsprojektet och distribuera det tooAzure via hello användargränssnitt.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-166">Use Visual Studio toocreate a resource group project and deploy it tooAzure through hello user interface.</span></span> <span data-ttu-id="1cc7f-167">Du kan välja hello typ av resurser tooinclude i projektet.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-167">You select hello type of resources tooinclude in your project.</span></span> <span data-ttu-id="1cc7f-168">Resurserna läggs automatiskt toohello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-168">Those resources are automatically added toohello Resource Manager template.</span></span> <span data-ttu-id="1cc7f-169">hello projektet innehåller också en PowerShell-skriptet toodeploy hello mall.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-169">hello project also provides a PowerShell script toodeploy hello template.</span></span>

<span data-ttu-id="1cc7f-170">En introduktion toousing Visual Studio med resursgrupper, se [skapa och distribuera Azure-resursgrupper via Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-170">For an introduction toousing Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="1cc7f-171">Felsöka</span><span class="sxs-lookup"><span data-stu-id="1cc7f-171">Troubleshoot</span></span>

<span data-ttu-id="1cc7f-172">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-172">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1cc7f-173">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1cc7f-173">Next steps</span></span>
<span data-ttu-id="1cc7f-174">I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-174">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="1cc7f-175">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="1cc7f-175">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="1cc7f-176">Ett exempel för att distribuera resurser via hello .NET-klientbibliotek finns [distribuera resurser med hjälp av .NET-bibliotek och en mall](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-176">For an example of deploying resources through hello .NET client library, see [Deploy resources by using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="1cc7f-177">En detaljerad exempel för att distribuera ett program finns i [etablera och distribuera mikrotjänster förutsägbart i Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-177">For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span></span>
* <span data-ttu-id="1cc7f-178">Anvisningar om hur du distribuerar din lösning toodifferent miljöer finns [utvecklings- och testmiljöer i Microsoft Azure](../solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-178">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](../solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="1cc7f-179">toolearn om hello avsnitt i hello Azure Resource Manager-mallen finns [Webbsidemallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-179">toolearn about hello sections of hello Azure Resource Manager template, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1cc7f-180">En lista över hello-funktioner som kan användas i en Azure Resource Manager-mallen finns [Mallfunktioner](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-180">For a list of hello functions you can use in an Azure Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md).</span></span>

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a><span data-ttu-id="1cc7f-181">Bilaga: Resource Manager mallen toocreate ett Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1cc7f-181">Appendix: Resource Manager template toocreate a Hadoop cluster</span></span>
<span data-ttu-id="1cc7f-182">hello skapar följande mall för Azure Resource Manager ett Linux-baserade Hadoop-kluster med hello beroende Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-182">hello following Azure Resource Manager template creates a Linux-based Hadoop cluster with hello dependent Azure storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="1cc7f-183">Det här exemplet innehåller konfigurationsinformation för Hive metastore och Oozie metastore.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-183">This sample includes configuration information for Hive metastore and Oozie metastore.</span></span> <span data-ttu-id="1cc7f-184">Ta bort hello avsnittet eller konfigurera hello avsnitt innan du använder hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-184">Remove hello section or configure hello section before using hello template.</span></span>
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used tooremotely access hello cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "hello location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a><span data-ttu-id="1cc7f-185">Bilaga: Resource Manager mallen toocreate ett Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="1cc7f-185">Appendix: Resource Manager template toocreate a Spark cluster</span></span>

<span data-ttu-id="1cc7f-186">Det här avsnittet innehåller en Resource Manager-mall som du kan använda toocreate ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-186">This section provides a Resource Manager template that you can use toocreate an HDInsight Spark cluster.</span></span> <span data-ttu-id="1cc7f-187">Den här mallen innehåller konfigurationer för `spark-defaults` och `spark-thrift-sparkconf` (för 1.6 Spark-kluster) och `spark2-defaults` och `spark2-thrift-sparkconf` (för 2 Spark-kluster).</span><span class="sxs-lookup"><span data-stu-id="1cc7f-187">This template includes configurations for `spark-defaults` and `spark-thrift-sparkconf` (for Spark 1.6 clusters) and `spark2-defaults` and `spark2-thrift-sparkconf` (for Spark 2 clusters).</span></span> <span data-ttu-id="1cc7f-188">Dessutom toothis, HDInsight beräknas och anger konfigurationer som `spark.executor.instances`, `spark.executor.memory`, och `spark.executor.cores` utifrån hello klusterstorleken.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-188">In addition toothis, HDInsight calculates and sets configurations such as `spark.executor.instances`, `spark.executor.memory`, and `spark.executor.cores` based on hello cluster size.</span></span> 

<span data-ttu-id="1cc7f-189">Om du ställer in alla en parameter i ett avsnitt som en del av själva hello mallen HDInsight inte beräkna och ange hello andra parametrar av hello samma avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-189">If you set any one parameter in a section as part of hello template itself, HDInsight does not calculate and set hello other parameters of hello same section.</span></span> <span data-ttu-id="1cc7f-190">Till exempel parametern `spark.executor.instances` i hello `spark-defaults` konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-190">For example, parameter `spark.executor.instances` is in hello  `spark-defaults` configuration.</span></span> <span data-ttu-id="1cc7f-191">Om du anger en annan parametern (till exempel `spark.yarn.exector.memoryOverhead`) i hello `spark-defaults` konfiguration, HDInsight inte beräkna och ange hello `spark.executor.instances` samt parametern.</span><span class="sxs-lookup"><span data-stu-id="1cc7f-191">If you set another parameter (for example, `spark.yarn.exector.memoryOverhead`) in hello `spark-defaults` configuration, HDInsight does not calculate and set hello `spark.executor.instances` parameter as well.</span></span>

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
