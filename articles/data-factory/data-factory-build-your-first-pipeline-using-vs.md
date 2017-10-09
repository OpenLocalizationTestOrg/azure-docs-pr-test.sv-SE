---
title: "aaaBuild din första data factory (Visual Studio) | Microsoft Docs"
description: "I den här självstudien skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a><span data-ttu-id="80a5a-103">Självstudiekurs: Skapa en datafabrik med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80a5a-103">Tutorial: Create a data factory by using Visual Studio</span></span>
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [<span data-ttu-id="80a5a-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="80a5a-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="80a5a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="80a5a-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="80a5a-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80a5a-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="80a5a-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80a5a-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="80a5a-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="80a5a-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="80a5a-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="80a5a-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)

<span data-ttu-id="80a5a-110">De här självstudierna visar hur toocreate ett Azure data factory med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80a5a-110">This tutorial shows you how toocreate an Azure data factory by using Visual Studio.</span></span> <span data-ttu-id="80a5a-111">Du skapar ett Visual Studio-projekt med hello Data Factory projektmall, definiera Data Factory-enheter (länkade tjänster, datauppsättningar och pipeline) i JSON-format och sedan publicera/distribuera dessa enheter toohello moln.</span><span class="sxs-lookup"><span data-stu-id="80a5a-111">You create a Visual Studio project using hello Data Factory project template, define Data Factory entities (linked services, datasets, and pipeline) in JSON format, and then publish/deploy these entities toohello cloud.</span></span> 

<span data-ttu-id="80a5a-112">hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="80a5a-113">Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="80a5a-114">hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="80a5a-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="80a5a-115">Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-115">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="80a5a-116">En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="80a5a-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="80a5a-117">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="80a5a-118">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="80a5a-119">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="80a5a-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="walkthrough-create-and-publish-data-factory-entities"></a><span data-ttu-id="80a5a-120">Genomgång: Skapa och publicera datafabriksentiteter</span><span class="sxs-lookup"><span data-stu-id="80a5a-120">Walkthrough: Create and publish Data Factory entities</span></span>
<span data-ttu-id="80a5a-121">Här är hello steg du utför som en del av den här genomgången:</span><span class="sxs-lookup"><span data-stu-id="80a5a-121">Here are hello steps you perform as part of this walkthrough:</span></span>

1. <span data-ttu-id="80a5a-122">Skapa två länkade tjänster: **AzureStorageLinkedService1** och **HDInsightOnDemandLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-122">Create two linked services: **AzureStorageLinkedService1** and **HDInsightOnDemandLinkedService1**.</span></span> 
   
    <span data-ttu-id="80a5a-123">I den här självstudiekursen hello både indata och utdata för hello hive aktiviteten är i du samma Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-123">In this tutorial, both input and output data for hello hive activity are in hello same Azure Blob Storage.</span></span> <span data-ttu-id="80a5a-124">Du använder en på-begäran HDInsight-kluster tooprocess befintlig indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-124">You use an on-demand HDInsight cluster tooprocess existing input data tooproduce output data.</span></span> <span data-ttu-id="80a5a-125">hello på begäran HDInsight-kluster skapas automatiskt för dig av Azure Data Factory vid körning när hello indata är klar toobe bearbetas.</span><span class="sxs-lookup"><span data-stu-id="80a5a-125">hello on-demand HDInsight cluster is automatically created for you by Azure Data Factory at run time when hello input data is ready toobe processed.</span></span> <span data-ttu-id="80a5a-126">Du måste toolink dina data lagras eller beräknar tooyour data factory så att hello Data Factory-tjänsten kan ansluta toothem vid körning.</span><span class="sxs-lookup"><span data-stu-id="80a5a-126">You need toolink your data stores or computes tooyour data factory so that hello Data Factory service can connect toothem at runtime.</span></span> <span data-ttu-id="80a5a-127">Därför du länka din Azure Storage-konto toohello data factory med hello AzureStorageLinkedService1 och länka ett HDInsight-kluster på begäran med hjälp av hello HDInsightOnDemandLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="80a5a-127">Therefore, you link your Azure Storage Account toohello data factory by using hello AzureStorageLinkedService1, and link an on-demand HDInsight cluster by using hello HDInsightOnDemandLinkedService1.</span></span> <span data-ttu-id="80a5a-128">När du publicerar kan ange du hello namn för hello data factory toobe skapas eller en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="80a5a-128">When publishing, you specify hello name for hello data factory toobe created or an existing data factory.</span></span>  
2. <span data-ttu-id="80a5a-129">Skapa två datamängder: **InputDataset** och **OutputDataset**, som representerar hello i/o-data som lagras i hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-129">Create two datasets: **InputDataset** and **OutputDataset**, which represent hello input/output data that is stored in hello Azure blob storage.</span></span> 
   
    <span data-ttu-id="80a5a-130">Dessa dataset-definitioner finns toohello länkad Azure Storage-tjänst som du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="80a5a-130">These dataset definitions refer toohello Azure Storage linked service you created in hello previous step.</span></span> <span data-ttu-id="80a5a-131">För hello InputDataset, ange hello blob-behållaren (adfgetstarted) och hello mapp (inptutdata) som innehåller en blob med hello indata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-131">For hello InputDataset, you specify hello blob container (adfgetstarted) and hello folder (inptutdata) that contains a blob with hello input data.</span></span> <span data-ttu-id="80a5a-132">För hello OutputDataset, anger du hello blob-behållaren (adfgetstarted) och hello mapp (partitioneddata) som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-132">For hello OutputDataset, you specify hello blob container (adfgetstarted) and hello folder (partitioneddata) that holds hello output data.</span></span> <span data-ttu-id="80a5a-133">Du kan också ange andra egenskaper som struktur, tillgänglighet och princip.</span><span class="sxs-lookup"><span data-stu-id="80a5a-133">You also specify other properties such as structure, availability, and policy.</span></span>
3. <span data-ttu-id="80a5a-134">Skapa en pipeline med namnet **MyFirstPipeline**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-134">Create a pipeline named **MyFirstPipeline**.</span></span> 
  
    <span data-ttu-id="80a5a-135">I den här genomgången hello pipeline har endast en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-135">In this walkthrough, hello pipeline has only one activity: **HDInsight Hive Activity**.</span></span> <span data-ttu-id="80a5a-136">Den här aktiviteten transformeringen indata tooproduce utdata genom att köra en hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="80a5a-136">This activity transform input data tooproduce output data by running a hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="80a5a-137">toolearn mer om hive-aktivitet, se [Hive-aktivitet](data-factory-hive-activity.md)</span><span class="sxs-lookup"><span data-stu-id="80a5a-137">toolearn more about hive activity, see [Hive Activity](data-factory-hive-activity.md)</span></span> 
4. <span data-ttu-id="80a5a-138">Skapa en datafabrik med namnet **DataFactoryUsingVS**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-138">Create a data factory named **DataFactoryUsingVS**.</span></span> <span data-ttu-id="80a5a-139">Distribuera hello data factory och alla Data Factory-enheter (länkade tjänster, tabeller och hello pipeline).</span><span class="sxs-lookup"><span data-stu-id="80a5a-139">Deploy hello data factory and all Data Factory entities (linked services, tables, and hello pipeline).</span></span>
5. <span data-ttu-id="80a5a-140">När du publicerar använder du Azure portal blad och övervakning & Management-appen toomonitor hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-140">After you publish, you use Azure portal blades and Monitoring & Management App toomonitor hello pipeline.</span></span> 
  
### <a name="prerequisites"></a><span data-ttu-id="80a5a-141">Krav</span><span class="sxs-lookup"><span data-stu-id="80a5a-141">Prerequisites</span></span>
1. <span data-ttu-id="80a5a-142">Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="80a5a-142">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span> <span data-ttu-id="80a5a-143">Du kan också välja hello **översikt och förutsättningar** alternativ i listrutan för hello på hello översta tooswitch toohello artikel.</span><span class="sxs-lookup"><span data-stu-id="80a5a-143">You can also select hello **Overview and prerequisites** option in hello drop-down list at hello top tooswitch toohello article.</span></span> <span data-ttu-id="80a5a-144">När du har slutfört hello krav växla tillbaka toothis artikel genom att välja **Visual Studio** alternativ i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="80a5a-144">After you complete hello prerequisites, switch back toothis article by selecting **Visual Studio** option in hello drop-down list.</span></span>
2. <span data-ttu-id="80a5a-145">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="80a5a-145">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>  
3. <span data-ttu-id="80a5a-146">Du måste ha hello följande installerat på datorn:</span><span class="sxs-lookup"><span data-stu-id="80a5a-146">You must have hello following installed on your computer:</span></span>
   * <span data-ttu-id="80a5a-147">Visual Studio 2013 eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="80a5a-147">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="80a5a-148">Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="80a5a-148">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="80a5a-149">Navigera för[Azure-hämtningssida](https://azure.microsoft.com/downloads/) och på **VS 2013** eller **VS 2015** i hello **.NET** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="80a5a-149">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="80a5a-150">Hämta hello senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="80a5a-150">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="80a5a-151">Du kan också uppdatera hello plugin-programmet genom att göra följande hello: på menyn hello **verktyg** -> **tillägg och uppdateringar** -> **Online**  ->  **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-151">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

<span data-ttu-id="80a5a-152">Nu ska vi använda Visual Studio toocreate ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-152">Now, let's use Visual Studio toocreate an Azure data factory.</span></span>

### <a name="create-visual-studio-project"></a><span data-ttu-id="80a5a-153">Skapa Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="80a5a-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="80a5a-154">Starta **Visual Studio 2013** eller **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-154">Launch **Visual Studio 2013** or **Visual Studio 2015**.</span></span> <span data-ttu-id="80a5a-155">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="80a5a-156">Du bör se hello **nytt projekt** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80a5a-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="80a5a-157">I hello **nytt projekt** dialogrutan, Välj hello **DataFactory** mall och klicka på **tomt Data Factory projekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>   

    ![Dialogrutan Nytt projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. <span data-ttu-id="80a5a-159">Ange en **namn** för hello projekt **plats**, och ett namn för hello **lösning**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-159">Enter a **name** for hello project, **location**, and a name for hello **solution**, and click **OK**.</span></span>

    ![Solution Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a><span data-ttu-id="80a5a-161">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="80a5a-161">Create linked services</span></span>
<span data-ttu-id="80a5a-162">I det här steget kan du skapa två länkade tjänster: **Azure Storage** och **HDInsight på begäran**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-162">In this step, you create two linked services: **Azure Storage** and **HDInsight on-demand**.</span></span> 

<span data-ttu-id="80a5a-163">hello Azure Storage länkade tjänsten länkar din toohello data factory för Azure Storage-konto genom att tillhandahålla hello anslutningsinformationen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-163">hello Azure Storage linked service links your Azure Storage account toohello data factory by providing hello connection information.</span></span> <span data-ttu-id="80a5a-164">Data Factory-tjänsten använder hello anslutningssträng från hello länkade tjänstinställning tooconnect toohello Azure storage vid körning.</span><span class="sxs-lookup"><span data-stu-id="80a5a-164">Data Factory service uses hello connection string from hello linked service setting tooconnect toohello Azure storage at runtime.</span></span> <span data-ttu-id="80a5a-165">Lagringen innehåller indata och utdata för hello pipeline och hello hive skriptfilen som används av hello hive-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-165">This storage holds input and output data for hello pipeline, and hello hive script file used by hello hive activity.</span></span> 

<span data-ttu-id="80a5a-166">Med på begäran HDInsight länkad tjänst skapas hello HDInsight-kluster automatiskt vid körning när hello indata är klar tooprocessed.</span><span class="sxs-lookup"><span data-stu-id="80a5a-166">With on-demand HDInsight linked service, hello HDInsight cluster is automatically created at runtime when hello input data is ready tooprocessed.</span></span> <span data-ttu-id="80a5a-167">hello klustret tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="80a5a-167">hello cluster is deleted after it is done processing and idle for hello specified amount of time.</span></span> 

> [!NOTE]
> <span data-ttu-id="80a5a-168">Du kan skapa en datafabrik genom att ange dess namn och inställningar för närvarande hello publicera din Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="80a5a-168">You create a data factory by specifying its name and settings at hello time of publishing your Data Factory solution.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="80a5a-169">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="80a5a-169">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="80a5a-170">Högerklicka på **länkade tjänster** i hello solution explorer peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-170">Right-click **Linked Services** in hello solution explorer, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="80a5a-171">I hello **Lägg till nytt objekt** dialogrutan **länkade Azure-lagringstjänsten** hello listan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-171">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span>
    <span data-ttu-id="80a5a-172">![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="80a5a-172">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span></span>
3. <span data-ttu-id="80a5a-173">Ersätt `<accountname>` och `<accountkey>` med hello namnet på ditt Azure storage-konto och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="80a5a-173">Replace `<accountname>` and `<accountkey>` with hello name of your Azure storage account and its key.</span></span> <span data-ttu-id="80a5a-174">toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="80a5a-174">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
    <span data-ttu-id="80a5a-175">![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="80a5a-175">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span></span>
4. <span data-ttu-id="80a5a-176">Spara hello **AzureStorageLinkedService1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="80a5a-176">Save hello **AzureStorageLinkedService1.json** file.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="80a5a-177">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="80a5a-177">Create Azure HDInsight linked service</span></span>
1. <span data-ttu-id="80a5a-178">I hello **Solution Explorer**, högerklicka på **länkade tjänster**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-178">In hello **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="80a5a-179">Välj **Länkad HDInsight-tjänst på begäran** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-179">Select **HDInsight On Demand Linked Service**, and click **Add**.</span></span>
3. <span data-ttu-id="80a5a-180">Ersätt hello **JSON** med hello följande JSON:</span><span class="sxs-lookup"><span data-stu-id="80a5a-180">Replace hello **JSON** with hello following JSON:</span></span>

     ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
        "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    <span data-ttu-id="80a5a-181">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="80a5a-181">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="80a5a-182">Egenskap</span><span class="sxs-lookup"><span data-stu-id="80a5a-182">Property</span></span> | <span data-ttu-id="80a5a-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="80a5a-183">Description</span></span>
    -------- | ----------- 
    <span data-ttu-id="80a5a-184">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="80a5a-184">ClusterSize</span></span> | <span data-ttu-id="80a5a-185">Anger hello storleken på hello HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="80a5a-185">Specifies hello size of hello HDInsight Hadoop cluster.</span></span>
    <span data-ttu-id="80a5a-186">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="80a5a-186">TimeToLive</span></span> | <span data-ttu-id="80a5a-187">Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="80a5a-187">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span>
    <span data-ttu-id="80a5a-188">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="80a5a-188">linkedServiceName</span></span> | <span data-ttu-id="80a5a-189">Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="80a5a-189">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight Hadoop cluster.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="80a5a-190">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="80a5a-190">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="80a5a-191">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="80a5a-191">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="80a5a-192">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="80a5a-192">This behavior is by design.</span></span> <span data-ttu-id="80a5a-193">Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="80a5a-193">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="80a5a-194">hello klustret tas bort automatiskt när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="80a5a-194">hello cluster is automatically deleted when hello processing is done.</span></span>
    > 
    > <span data-ttu-id="80a5a-195">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-195">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="80a5a-196">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="80a5a-196">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="80a5a-197">hello namnen på de här behållarna följer ett mönster: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-197">hello names of these containers follow a pattern: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span></span> <span data-ttu-id="80a5a-198">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-198">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

    <span data-ttu-id="80a5a-199">Mer information om JSON-egenskaper finns i artikeln [Länkade tjänster för Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="80a5a-199">For more information about JSON properties, see [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article.</span></span> 
4. <span data-ttu-id="80a5a-200">Spara hello **HDInsightOnDemandLinkedService1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="80a5a-200">Save hello **HDInsightOnDemandLinkedService1.json** file.</span></span>

### <a name="create-datasets"></a><span data-ttu-id="80a5a-201">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="80a5a-201">Create datasets</span></span>
<span data-ttu-id="80a5a-202">I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="80a5a-202">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="80a5a-203">De här datauppsättningarna finns toohello **AzureStorageLinkedService1** du har skapat tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-203">These datasets refer toohello **AzureStorageLinkedService1** you have created earlier in this tutorial.</span></span> <span data-ttu-id="80a5a-204">Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-204">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

#### <a name="create-input-dataset"></a><span data-ttu-id="80a5a-205">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="80a5a-205">Create input dataset</span></span>
1. <span data-ttu-id="80a5a-206">I hello **Solution Explorer**, högerklicka på **tabeller**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-206">In hello **Solution Explorer**, right-click **Tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="80a5a-207">Välj **Azure Blob** hello listan Ändra hello namnet på hello-filen för**InputDataSet.json**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-207">Select **Azure Blob** from hello list, change hello name of hello file too**InputDataSet.json**, and click **Add**.</span></span>
3. <span data-ttu-id="80a5a-208">Ersätt hello **JSON** i hello-redigeraren med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="80a5a-208">Replace hello **JSON** in hello editor with hello following JSON snippet:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    <span data-ttu-id="80a5a-209">Den här JSON-fragment definierar en datamängd som kallas **AzureBlobInput** som representerar indata för hello hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-209">This JSON snippet defines a dataset called **AzureBlobInput** that represents input data for hello hive activity in hello pipeline.</span></span> <span data-ttu-id="80a5a-210">Du anger att hello indata finns i hello blob-behållaren som kallas `adfgetstarted` och hello mapp som heter `inputdata`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-210">You specify that hello input data is located in hello blob container called `adfgetstarted` and hello folder called `inputdata`.</span></span>

    <span data-ttu-id="80a5a-211">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="80a5a-211">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="80a5a-212">Egenskap</span><span class="sxs-lookup"><span data-stu-id="80a5a-212">Property</span></span> | <span data-ttu-id="80a5a-213">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="80a5a-213">Description</span></span> |
    -------- | ----------- |
    <span data-ttu-id="80a5a-214">typ</span><span class="sxs-lookup"><span data-stu-id="80a5a-214">type</span></span> |<span data-ttu-id="80a5a-215">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-215">hello type property is set too**AzureBlob** because data resides in Azure Blob Storage.</span></span>
    <span data-ttu-id="80a5a-216">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="80a5a-216">linkedServiceName</span></span> | <span data-ttu-id="80a5a-217">Refererar toohello AzureStorageLinkedService1 som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="80a5a-217">Refers toohello AzureStorageLinkedService1 you created earlier.</span></span>
    <span data-ttu-id="80a5a-218">fileName</span><span class="sxs-lookup"><span data-stu-id="80a5a-218">fileName</span></span> |<span data-ttu-id="80a5a-219">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="80a5a-219">This property is optional.</span></span> <span data-ttu-id="80a5a-220">Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="80a5a-220">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="80a5a-221">I det här fallet bearbetas endast hello input.log.</span><span class="sxs-lookup"><span data-stu-id="80a5a-221">In this case, only hello input.log is processed.</span></span>
    <span data-ttu-id="80a5a-222">typ</span><span class="sxs-lookup"><span data-stu-id="80a5a-222">type</span></span> | <span data-ttu-id="80a5a-223">hello-loggfiler finns i textformat, så vi använder TextFormat.</span><span class="sxs-lookup"><span data-stu-id="80a5a-223">hello log files are in text format, so we use TextFormat.</span></span> |
    <span data-ttu-id="80a5a-224">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="80a5a-224">columnDelimiter</span></span> | <span data-ttu-id="80a5a-225">kolumner i hello loggfiler är avgränsade med kommatecken hello (`,`)</span><span class="sxs-lookup"><span data-stu-id="80a5a-225">columns in hello log files are delimited by hello comma character (`,`)</span></span>
    <span data-ttu-id="80a5a-226">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="80a5a-226">frequency/interval</span></span> | <span data-ttu-id="80a5a-227">frekvens ange tooMonth intervall är 1, vilket innebär att hello inkommande segment är tillgängliga varje månad.</span><span class="sxs-lookup"><span data-stu-id="80a5a-227">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span>
    <span data-ttu-id="80a5a-228">extern</span><span class="sxs-lookup"><span data-stu-id="80a5a-228">external</span></span> | <span data-ttu-id="80a5a-229">Den här egenskapen anges tootrue om hello indata för aktivitet hello inte genereras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-229">This property is set tootrue if hello input data for hello activity is not generated by hello pipeline.</span></span> <span data-ttu-id="80a5a-230">Den här egenskapen anges endast för indatauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="80a5a-230">This property is only specified on input datasets.</span></span> <span data-ttu-id="80a5a-231">För hello inkommande dataset hello första aktivitet alltid ange tootrue.</span><span class="sxs-lookup"><span data-stu-id="80a5a-231">For hello input dataset of hello first activity, always set it tootrue.</span></span>
4. <span data-ttu-id="80a5a-232">Spara hello **InputDataset.json** fil.</span><span class="sxs-lookup"><span data-stu-id="80a5a-232">Save hello **InputDataset.json** file.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="80a5a-233">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="80a5a-233">Create output dataset</span></span>
<span data-ttu-id="80a5a-234">Nu kan skapa du hello utdata dataset toorepresent utdata data som lagras i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-234">Now, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="80a5a-235">I hello **Solution Explorer**, högerklicka på **tabeller**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-235">In hello **Solution Explorer**, right-click **tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="80a5a-236">Välj **Azure Blob** hello listan Ändra hello namnet på hello-filen för**OutputDataset.json**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-236">Select **Azure Blob** from hello list, change hello name of hello file too**OutputDataset.json**, and click **Add**.</span></span>
3. <span data-ttu-id="80a5a-237">Ersätt hello **JSON** i hello-redigeraren med hello följande JSON:</span><span class="sxs-lookup"><span data-stu-id="80a5a-237">Replace hello **JSON** in hello editor with hello following JSON:</span></span>
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }
    ```
    <span data-ttu-id="80a5a-238">hello JSON fragment definierar en datamängd som kallas **AzureBlobOutput** som representerar utdata produceras av hello hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-238">hello JSON snippet defines a dataset called **AzureBlobOutput** that represents output data produced by hello hive activity in hello pipeline.</span></span> <span data-ttu-id="80a5a-239">Du anger att hello utgående data skapas av hello hive aktiviteten placeras i hello blob-behållaren som kallas `adfgetstarted` och hello mapp som heter `partitioneddata`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-239">You specify that hello output data is produced by hello hive activity is placed in hello blob container called `adfgetstarted` and hello folder called `partitioneddata`.</span></span> 
    
    <span data-ttu-id="80a5a-240">Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.</span><span class="sxs-lookup"><span data-stu-id="80a5a-240">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span> <span data-ttu-id="80a5a-241">hello utdata dataset enheter hello schema för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-241">hello output dataset drives hello schedule of hello pipeline.</span></span> <span data-ttu-id="80a5a-242">hello pipeline körs varje månad mellan dess start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="80a5a-242">hello pipeline runs monthly between its start and end times.</span></span> 

    <span data-ttu-id="80a5a-243">Se **skapa hello inkommande dataset** avsnitt beskrivs dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="80a5a-243">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="80a5a-244">Du anger inte externa hello-egenskapen på en datamängd för utdata som hello dataset produceras av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-244">You do not set hello external property on an output dataset as hello dataset is produced by hello pipeline.</span></span>
4. <span data-ttu-id="80a5a-245">Spara hello **OutputDataset.json** fil.</span><span class="sxs-lookup"><span data-stu-id="80a5a-245">Save hello **OutputDataset.json** file.</span></span>

### <a name="create-pipeline"></a><span data-ttu-id="80a5a-246">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="80a5a-246">Create pipeline</span></span>
<span data-ttu-id="80a5a-247">Du har skapat hello länkad Azure Storage-tjänst och inkommande och utgående datauppsättningar hittills.</span><span class="sxs-lookup"><span data-stu-id="80a5a-247">You have created hello Azure Storage linked service, and input and output datasets so far.</span></span> <span data-ttu-id="80a5a-248">Nu ska du skapa en pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-248">Now, you create a pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="80a5a-249">Hej **inkommande** för hello hive aktiviteten är inställd för**AzureBlobInput** och **utdata** har angetts för**AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-249">hello **input** for hello hive activity is set too**AzureBlobInput** and **output** is set too**AzureBlobOutput**.</span></span> <span data-ttu-id="80a5a-250">En sektor i ett inkommande dataset finns varje månad (frekvens: månad, intervall: 1), och hello utdata segment skapas varje månad för.</span><span class="sxs-lookup"><span data-stu-id="80a5a-250">A slice of an input dataset is available monthly (frequency: Month, interval: 1), and hello output slice is produced monthly too.</span></span> 

1. <span data-ttu-id="80a5a-251">I hello **Solution Explorer**, högerklicka på **Pipelines**, peka för**Lägg till**, och klicka på **nytt objekt.**</span><span class="sxs-lookup"><span data-stu-id="80a5a-251">In hello **Solution Explorer**, right-click **Pipelines**, point too**Add**, and click **New Item.**</span></span>
2. <span data-ttu-id="80a5a-252">Välj **Hive omvandling Pipeline** hello listan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-252">Select **Hive Transformation Pipeline** from hello list, and click **Add**.</span></span>
3. <span data-ttu-id="80a5a-253">Ersätt hello **JSON** med följande kodavsnitt hello:</span><span class="sxs-lookup"><span data-stu-id="80a5a-253">Replace hello **JSON** with hello following snippet:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="80a5a-254">Ersätt `<storageaccountname>` med hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="80a5a-254">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService1",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="80a5a-255">Ersätt `<storageaccountname>` med hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="80a5a-255">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

    <span data-ttu-id="80a5a-256">hello JSON fragment definierar en pipeline som består av en enskild aktivitet (Hive aktivitet).</span><span class="sxs-lookup"><span data-stu-id="80a5a-256">hello JSON snippet defines a pipeline that consists of a single activity (Hive Activity).</span></span> <span data-ttu-id="80a5a-257">Den här aktiviteten körs en Hive-skript tooprocess inkommande data på en på-begäran HDInsight-kluster tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-257">This activity runs a Hive script tooprocess input data on an on-demand HDInsight cluster tooproduce output data.</span></span> <span data-ttu-id="80a5a-258">I hello aktiviteter i pipeline-hello JSON kan du se endast en aktivitet i hello matris med typen som angetts för**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-258">In hello activities section of hello pipeline JSON, you see only one activity in hello array with type set too**HDInsightHive**.</span></span> 

    <span data-ttu-id="80a5a-259">I hello egenskaper som är specifika tooHDInsight Hive aktivitet, anger du vilken länkad Azure Storage-tjänst har hello hive-skriptfil, hello sökvägen toohello skriptfilen och parametrar toohello skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-259">In hello type properties that are specific tooHDInsight Hive activity, you specify what Azure Storage linked service has hello hive script file, hello path toohello script file, and parameters toohello script file.</span></span> 

    <span data-ttu-id="80a5a-260">hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService) och i hello `script` mapp i hello behållaren `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-260">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService), and in hello `script` folder in hello container `adfgetstarted`.</span></span>

    <span data-ttu-id="80a5a-261">Hej `defines` avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-261">hello `defines` section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span></span>

    <span data-ttu-id="80a5a-262">Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-262">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span> <span data-ttu-id="80a5a-263">Du har konfigurerat hello dataset toobe producerade därför månadsvis, bara en gång sektorn produceras av hello pipeline (eftersom hello månad är samma i start- och slutdatum).</span><span class="sxs-lookup"><span data-stu-id="80a5a-263">You configured hello dataset toobe produced monthly, therefore, only once slice is produced by hello pipeline (because hello month is same in start and end dates).</span></span>

    <span data-ttu-id="80a5a-264">I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-264">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>
4. <span data-ttu-id="80a5a-265">Spara hello **HiveActivity1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="80a5a-265">Save hello **HiveActivity1.json** file.</span></span>

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a><span data-ttu-id="80a5a-266">Lägg till partitionweblogs.hql och input.log som ett beroende</span><span class="sxs-lookup"><span data-stu-id="80a5a-266">Add partitionweblogs.hql and input.log as a dependency</span></span>
1. <span data-ttu-id="80a5a-267">Högerklicka på **beroenden** i hello **Solution Explorer** fönstret peka för**Lägg till**, och klicka på **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-267">Right-click **Dependencies** in hello **Solution Explorer** window, point too**Add**, and click **Existing Item**.</span></span>  
2. <span data-ttu-id="80a5a-268">Navigera toohello **C:\ADFGettingStarted** och välj **partitionweblogs.hql**, **input.log** filer och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-268">Navigate toohello **C:\ADFGettingStarted** and select **partitionweblogs.hql**, **input.log** files, and click **Add**.</span></span> <span data-ttu-id="80a5a-269">Du har skapat dessa två filer som en del av krav från hello [kursen översikt](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="80a5a-269">You created these two files as part of prerequisites from hello [Tutorial Overview](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="80a5a-270">När du publicerar hello lösning i nästa steg i hello hello **partitionweblogs.hql** filen är överförda toohello **skriptet** mapp i hello `adfgetstarted` blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="80a5a-270">When you publish hello solution in hello next step, hello **partitionweblogs.hql** file is uploaded toohello **script** folder in hello `adfgetstarted` blob container.</span></span>   

### <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="80a5a-271">Publicera/distribuera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="80a5a-271">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="80a5a-272">I det här steget kan publicera du hello Data Factory entiteter (länkade tjänster, datauppsättningar och pipeline) i ditt projekt toohello Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="80a5a-272">In this step, you publish hello Data Factory entities (linked services, datasets, and pipeline) in your project toohello Azure Data Factory service.</span></span> <span data-ttu-id="80a5a-273">Pågående hello för publicering, kan du ange hello namn för din data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-273">In hello process of publishing, you specify hello name for your data factory.</span></span> 

1. <span data-ttu-id="80a5a-274">Högerklicka på projektet i hello Solution Explorer och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-274">Right-click project in hello Solution Explorer, and click **Publish**.</span></span>
2. <span data-ttu-id="80a5a-275">Om du ser **logga in tooyour Microsoft-konto** dialogrutan, ange dina autentiseringsuppgifter för hello-konto med Azure-prenumeration och på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-275">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="80a5a-276">Du bör se följande dialogrutan hello:</span><span class="sxs-lookup"><span data-stu-id="80a5a-276">You should see hello following dialog box:</span></span>

   ![Dialogrutan Publicera](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. <span data-ttu-id="80a5a-278">I hello **konfigurera datafabriken** sidan, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80a5a-278">In hello **Configure data factory** page, do hello following steps:</span></span>

    ![Publicera – nya datafabriksinställningar](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. <span data-ttu-id="80a5a-280">välj alternativet **Skapa ny Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-280">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="80a5a-281">Ange ett unikt **namn** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-281">Enter a unique **name** for hello data factory.</span></span> <span data-ttu-id="80a5a-282">Exempel: **DataFactoryUsingVS09152016**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-282">For example: **DataFactoryUsingVS09152016**.</span></span> <span data-ttu-id="80a5a-283">hello namn måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="80a5a-283">hello name must be globally unique.</span></span>
   3. <span data-ttu-id="80a5a-284">Välj rätt hello-prenumeration för hello **prenumeration** fältet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-284">Select hello right subscription for hello **Subscription** field.</span></span> 
        > [!IMPORTANT]
        > <span data-ttu-id="80a5a-285">Om du inte ser någon prenumeration, kontrollera att du har loggat in med ett konto som är en administratör eller medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="80a5a-285">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>
   4. <span data-ttu-id="80a5a-286">Välj hello **resursgruppen** för hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="80a5a-286">Select hello **resource group** for hello data factory toobe created.</span></span>
   5. <span data-ttu-id="80a5a-287">Välj hello **region** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-287">Select hello **region** for hello data factory.</span></span>
   6. <span data-ttu-id="80a5a-288">Klicka på **nästa** tooswitch toohello **publicera objekt** sidan.</span><span class="sxs-lookup"><span data-stu-id="80a5a-288">Click **Next** tooswitch toohello **Publish Items** page.</span></span> <span data-ttu-id="80a5a-289">(Tryck på **FLIKEN** toomove utanför hello namnet fältet tooif hello **nästa** är inaktiverat.)</span><span class="sxs-lookup"><span data-stu-id="80a5a-289">(Press **TAB** toomove out of hello Name field tooif hello **Next** button is disabled.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="80a5a-290">Om felmeddelandet hello **datafabriksnamnet ”DataFactoryUsingVS” är inte tillgänglig** vid publicering, ändra hello namn (till exempel yournameDataFactoryUsingVS).</span><span class="sxs-lookup"><span data-stu-id="80a5a-290">If you receive hello error **Data factory name “DataFactoryUsingVS” is not available** when publishing, change hello name (for example, yournameDataFactoryUsingVS).</span></span> <span data-ttu-id="80a5a-291">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-291">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
1. <span data-ttu-id="80a5a-292">I hello **publicera objekt** , se till att alla hello Datafabriker entiteter är markerade och klickar på **nästa** tooswitch toohello **sammanfattning** sidan.</span><span class="sxs-lookup"><span data-stu-id="80a5a-292">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>

    ![Sidan Publish items (Publicera objekt)](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. <span data-ttu-id="80a5a-294">Granska hello sammanfattning och klicka på **nästa** toostart hello distribution processen och visa hello **Distributionsstatus**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-294">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>

    ![Sammanfattningssida](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. <span data-ttu-id="80a5a-296">I hello **Distributionsstatus** sida, bör du se hello status hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-296">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="80a5a-297">Klicka på Slutför när hello distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="80a5a-297">Click Finish after hello deployment is done.</span></span>

<span data-ttu-id="80a5a-298">Viktiga punkter toonote:</span><span class="sxs-lookup"><span data-stu-id="80a5a-298">Important points toonote:</span></span>

- <span data-ttu-id="80a5a-299">Om felmeddelandet hello: **den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory**, gör du något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="80a5a-299">If you receive hello error: **This subscription is not registered toouse namespace Microsoft.DataFactory**, do one of hello following and try publishing again:</span></span>
    - <span data-ttu-id="80a5a-300">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="80a5a-300">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span>
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        <span data-ttu-id="80a5a-301">Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="80a5a-301">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span>

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - <span data-ttu-id="80a5a-302">Logga in med hjälp av hello Azure-prenumeration i toohello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-302">Login using hello Azure subscription in toohello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="80a5a-303">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="80a5a-303">This action automatically registers hello provider for you.</span></span>
- <span data-ttu-id="80a5a-304">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="80a5a-304">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
- <span data-ttu-id="80a5a-305">toocreate Data Factory instanser måste toobe administratör eller medadministratör av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="80a5a-305">toocreate Data Factory instances, you need toobe an admin or co-admin of hello Azure subscription</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="80a5a-306">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="80a5a-306">Monitor pipeline</span></span>
<span data-ttu-id="80a5a-307">I det här steget kan övervaka du hello pipeline med diagramvyn i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-307">In this step, you monitor hello pipeline using Diagram View of hello data factory.</span></span> 

#### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="80a5a-308">Övervaka pipeline med diagramvyn</span><span class="sxs-lookup"><span data-stu-id="80a5a-308">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="80a5a-309">Logga in toohello [Azure-portalen](https://portal.azure.com/), hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80a5a-309">Log in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>
   1. <span data-ttu-id="80a5a-310">Klicka på **Fler tjänster** och på **Datafabriker**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-310">Click **More services** and click **Data factories**.</span></span>
       
        ![Bläddra igenom datafabrikerna](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. <span data-ttu-id="80a5a-312">Välj hello namnet på din data factory (till exempel: **DataFactoryUsingVS09152016**) hello listan över datafabriker.</span><span class="sxs-lookup"><span data-stu-id="80a5a-312">Select hello name of your data factory (for example: **DataFactoryUsingVS09152016**) from hello list of data factories.</span></span>
   
       ![Välj din datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. <span data-ttu-id="80a5a-314">I hello startsidan för din data factory, klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-314">In hello home page for your data factory, click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. <span data-ttu-id="80a5a-316">I hello diagramvyn visas en översikt över hello pipelines och datauppsättningar som används i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-316">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. <span data-ttu-id="80a5a-318">tooview alla aktiviteter i hello pipeline, högerklicka på pipeline i hello diagram och klicka på Öppna Pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-318">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. <span data-ttu-id="80a5a-320">Bekräfta att du ser hello HDInsightHive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-320">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    <span data-ttu-id="80a5a-322">toonavigate bakifrån toohello tidigare, klicka på **datafabriken** i hello dynamiska menyn hello överst.</span><span class="sxs-lookup"><span data-stu-id="80a5a-322">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
6. <span data-ttu-id="80a5a-323">I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-323">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="80a5a-324">Kontrollera att hello-segment i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="80a5a-324">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="80a5a-325">Det kan ta några minuter för hello sektorn tooshow i tillståndet Ready.</span><span class="sxs-lookup"><span data-stu-id="80a5a-325">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="80a5a-326">Om det inte sker när du vänta ett tag, se om du har hello indatafilen (input.log) placeras i rätt hello-behållaren (`adfgetstarted`) och mappar (`inputdata`).</span><span class="sxs-lookup"><span data-stu-id="80a5a-326">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (`adfgetstarted`) and folder (`inputdata`).</span></span> <span data-ttu-id="80a5a-327">Och se till att hello **externa** för hello inkommande datauppsättningen egenskapen för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-327">And, make sure that hello **external** property on hello input dataset is set too**true**.</span></span> 

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. <span data-ttu-id="80a5a-329">Klicka på **X** tooclose **AzureBlobInput** bladet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-329">Click **X** tooclose **AzureBlobInput** blade.</span></span>
8. <span data-ttu-id="80a5a-330">I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-330">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="80a5a-331">Du ser att hello-segment som håller på att behandlas.</span><span class="sxs-lookup"><span data-stu-id="80a5a-331">You see that hello slice that is currently being processed.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. <span data-ttu-id="80a5a-333">När bearbetningen är klar visas hello sektor i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="80a5a-333">When processing is done, you see hello slice in **Ready** state.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="80a5a-334">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="80a5a-334">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="80a5a-335">Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="80a5a-335">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>  
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. <span data-ttu-id="80a5a-337">När hello sektorn är i **klar** tillstånd, kontrollera hello `partitioneddata` mapp i hello `adfgetstarted` behållare i blobblagring för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-337">When hello slice is in **Ready** state, check hello `partitioneddata` folder in hello `adfgetstarted` container in your blob storage for hello output data.</span></span>  

    ![utdata](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. <span data-ttu-id="80a5a-339">Klicka på hello sektorn toosee information om den i en **datasektorn** bladet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-339">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

    ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. <span data-ttu-id="80a5a-341">Klicka på en aktivitet som körs i hello **aktiviteten körs listan** toosee information om en aktivitet kör (Hive aktivitet i vårt scenario) i en **aktivitet köras information** fönster.</span><span class="sxs-lookup"><span data-stu-id="80a5a-341">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span> 
  
    ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    <span data-ttu-id="80a5a-343">Du kan se hello Hive-fråga som har utförts och statusinformation från hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="80a5a-343">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="80a5a-344">Dessa loggar är användbara vid felsökning av eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="80a5a-344">These logs are useful for troubleshooting any issues.</span></span>  

<span data-ttu-id="80a5a-345">Se [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) anvisningar för hur toouse hello Azure portal toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-345">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

#### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="80a5a-346">Övervaka pipeline med övervaknings- och hanteringsappen</span><span class="sxs-lookup"><span data-stu-id="80a5a-346">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="80a5a-347">Du kan också använda Övervakare och hantera program toomonitor din pipelines.</span><span class="sxs-lookup"><span data-stu-id="80a5a-347">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="80a5a-348">Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-348">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="80a5a-349">Klicka på ikonen Övervaka och hantera.</span><span class="sxs-lookup"><span data-stu-id="80a5a-349">Click Monitor & Manage tile.</span></span>

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. <span data-ttu-id="80a5a-351">Du bör se programmet Övervaka och hantera.</span><span class="sxs-lookup"><span data-stu-id="80a5a-351">You should see Monitor & Manage application.</span></span> <span data-ttu-id="80a5a-352">Ändra hello **starttid** och **sluttiden** toomatch start (2016-04-01 12:00 AM)- och sluttider (2016-02-04 12:00 AM) i din pipeline och klickar på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-352">Change hello **Start time** and **End time** toomatch start (04-01-2016 12:00 AM) and end times (04-02-2016 12:00 AM) of your pipeline, and click **Apply**.</span></span>

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. <span data-ttu-id="80a5a-354">toosee information om en aktivitetsfönstret markerar du den i hello **aktivitet Windows lista** toosee information om den.</span><span class="sxs-lookup"><span data-stu-id="80a5a-354">toosee details about an activity window, select it in hello **Activity Windows list** toosee details about it.</span></span>
    <span data-ttu-id="80a5a-355">![Aktivitetsfönsterinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="80a5a-355">![Activity window details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80a5a-356">hello indatafilen hämtar bort när hello segment har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="80a5a-356">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="80a5a-357">Därför, om du vill toorerun hello segment eller hello kursen igen överför hello indatafilen (input.log) toohello `inputdata` för hello `adfgetstarted` behållare.</span><span class="sxs-lookup"><span data-stu-id="80a5a-357">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello `inputdata` folder of hello `adfgetstarted` container.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="80a5a-358">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="80a5a-358">Additional notes</span></span>
- <span data-ttu-id="80a5a-359">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="80a5a-359">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="80a5a-360">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-360">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="80a5a-361">Till exempel indata en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun en Hive-skript tootransform.</span><span class="sxs-lookup"><span data-stu-id="80a5a-361">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="80a5a-362">Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="80a5a-362">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="80a5a-363">Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-363">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span>
- <span data-ttu-id="80a5a-364">Länkade tjänster länka datalager eller compute services tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-364">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="80a5a-365">Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="80a5a-365">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="80a5a-366">Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory och [omvandling aktiviteter](data-factory-data-transformation-activities.md) som körs på dem..</span><span class="sxs-lookup"><span data-stu-id="80a5a-366">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory and [transformation activities](data-factory-data-transformation-activities.md) that can run on them.</span></span>
- <span data-ttu-id="80a5a-367">Se [flytta data från / tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om JSON-egenskaper som används i hello länkad Azure Storage service definition.</span><span class="sxs-lookup"><span data-stu-id="80a5a-367">See [Move data from/tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used in hello Azure Storage linked service definition.</span></span>
- <span data-ttu-id="80a5a-368">Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="80a5a-368">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="80a5a-369">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="80a5a-369">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>
-  <span data-ttu-id="80a5a-370">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello föregående JSON.</span><span class="sxs-lookup"><span data-stu-id="80a5a-370">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello preceding JSON.</span></span> <span data-ttu-id="80a5a-371">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="80a5a-371">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
- <span data-ttu-id="80a5a-372">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="80a5a-372">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="80a5a-373">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="80a5a-373">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="80a5a-374">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="80a5a-374">This behavior is by design.</span></span> <span data-ttu-id="80a5a-375">Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="80a5a-375">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="80a5a-376">hello klustret tas bort automatiskt när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="80a5a-376">hello cluster is automatically deleted when hello processing is done.</span></span>
    
    <span data-ttu-id="80a5a-377">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-377">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="80a5a-378">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="80a5a-378">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="80a5a-379">hello namnen på de här behållarna följer ett mönster: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="80a5a-379">hello names of these containers follow a pattern: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span></span> <span data-ttu-id="80a5a-380">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="80a5a-380">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>
- <span data-ttu-id="80a5a-381">Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="80a5a-381">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="80a5a-382">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="80a5a-382">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 
- <span data-ttu-id="80a5a-383">Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-383">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="80a5a-384">En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="80a5a-384">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>


## <a name="use-server-explorer-tooview-data-factories"></a><span data-ttu-id="80a5a-385">Använd Server Explorer tooview datafabriker</span><span class="sxs-lookup"><span data-stu-id="80a5a-385">Use Server Explorer tooview data factories</span></span>
1. <span data-ttu-id="80a5a-386">I **Visual Studio**, klickar du på **visa** på hello menyn och klickar på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-386">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="80a5a-387">I hello Server Explorer, expandera **Azure** och expandera **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-387">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="80a5a-388">Om du ser **logga in tooVisual Studio**, ange hello **konto** som är associerade med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-388">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="80a5a-389">Ange **lösenordet** och klicka på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-389">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="80a5a-390">Visual Studio försöker tooget information om alla Azure datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="80a5a-390">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="80a5a-391">Du ser hello status för den här åtgärden i hello **Data Factory uppgiftslista** fönster.</span><span class="sxs-lookup"><span data-stu-id="80a5a-391">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. <span data-ttu-id="80a5a-393">Du kan högerklicka på en datafabrik och välj **exportera Data Factory tooNew projekt** toocreate Visual Studio-projekt utifrån en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="80a5a-393">You can right-click a data factory, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Exportera datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="80a5a-395">Uppdatera Data Factory-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80a5a-395">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="80a5a-396">tooupdate Azure Data Factory tools för Visual Studio hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80a5a-396">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="80a5a-397">Klicka på **verktyg** på hello-menyn och välj **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-397">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="80a5a-398">Välj **uppdateringar** i hello till vänster och välj sedan **Visual Studio-galleriet**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-398">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="80a5a-399">Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-399">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="80a5a-400">Om du inte ser den här posten har du redan hello senaste versionen av hello verktyg.</span><span class="sxs-lookup"><span data-stu-id="80a5a-400">If you do not see this entry, you already have hello latest version of hello tools.</span></span>

## <a name="use-configuration-files"></a><span data-ttu-id="80a5a-401">Använda konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="80a5a-401">Use configuration files</span></span>
<span data-ttu-id="80a5a-402">Du kan använda konfigurationsfiler i Visual Studio tooconfigure egenskaper för länkade tjänster/tabeller/pipelines på olika sätt för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="80a5a-402">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="80a5a-403">Överväg att hello JSON-definitionen för en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="80a5a-403">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="80a5a-404">toospecify **connectionString** med olika värden för accountname eller accountkey baserat på hello miljö (Dev/Test/produktion) toowhich du distribuerar Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-404">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="80a5a-405">Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="80a5a-405">You can achieve this behavior by using separate configuration file for each environment.</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a><span data-ttu-id="80a5a-406">Lägga till en konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="80a5a-406">Add a configuration file</span></span>
<span data-ttu-id="80a5a-407">Lägg till en konfigurationsfil för varje miljö genom att utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80a5a-407">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="80a5a-408">Högerklicka på hello Data Factory-projekt i Visual Studio-lösning, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-408">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="80a5a-409">Välj **Config** hello listan över installerade mallar hello vänster och välj **konfigurationsfilen**, ange en **namn** för konfiguration av hello fil och klicka på **Lägga till**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-409">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="80a5a-411">Lägg till konfigurationsparametrar och deras värden i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="80a5a-411">Add configuration parameters and their values in hello following format:</span></span>

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    <span data-ttu-id="80a5a-412">Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="80a5a-412">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="80a5a-413">Observera att hello syntax för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="80a5a-413">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="80a5a-414">Om JSON har en egenskap som innehåller en matris med värden som visas i följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="80a5a-414">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    <span data-ttu-id="80a5a-415">Konfigurera egenskaperna som visas i hello följande konfigurationsfilen (Använd Nollbaserad indexering):</span><span class="sxs-lookup"><span data-stu-id="80a5a-415">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a><span data-ttu-id="80a5a-416">Egenskapsnamn med blanksteg</span><span class="sxs-lookup"><span data-stu-id="80a5a-416">Property names with spaces</span></span>
<span data-ttu-id="80a5a-417">Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser som visas i följande exempel (Databasservernamnet) hello:</span><span class="sxs-lookup"><span data-stu-id="80a5a-417">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="80a5a-418">Distribuera lösningen med en konfiguration</span><span class="sxs-lookup"><span data-stu-id="80a5a-418">Deploy solution using a configuration</span></span>
<span data-ttu-id="80a5a-419">När du publicerar Azure Data Factory-entiteter i VS anger du hello-konfiguration som du vill toouse för publishing operationen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-419">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="80a5a-420">toopublish entiteter i ett Azure Data Factory-projekt med hjälp av konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="80a5a-420">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="80a5a-421">Högerklicka på Data Factory-projektet och klicka på **publicera** toosee hello **publicera objekt** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80a5a-421">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="80a5a-422">Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på hello **konfigurera datafabriken** och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-422">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="80a5a-423">På hello **publicera objekt** sida: du kan se en lista med tillgängliga konfigurationer för hello **Välj distributionskonfiguration** fältet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-423">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="80a5a-425">Välj hello **konfigurationsfilen** som du skulle t.ex. toouse och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-425">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="80a5a-426">Bekräfta att du ser hello namnet på JSON-fil i hello **sammanfattning** och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-426">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="80a5a-427">Klicka på **Slutför** när hello Distributionsåtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="80a5a-427">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="80a5a-428">När du distribuerar är hello värden från konfigurationsfilen hello används tooset värden för egenskaper i hello JSON-filer innan hello entiteter som är distribuerade tooAzure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="80a5a-428">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="80a5a-429">Använda Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="80a5a-429">Use Azure Key Vault</span></span>
<span data-ttu-id="80a5a-430">Det är inte tillrådligt och ofta mot säkerhet princip toocommit känsliga data, till exempel anslutning strängar toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-430">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="80a5a-431">Se [ADF Secure publicera](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub toolearn om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-431">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="80a5a-432">hello Secure publicera tillägget för Visual Studio kan hello hemligheter toobe lagras i Nyckelvalvet och endast referenser toothem har angetts i länkade tjänster / distributionskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="80a5a-432">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="80a5a-433">När du publicerar Data Factory entiteter tooAzure matchas dessa referenser.</span><span class="sxs-lookup"><span data-stu-id="80a5a-433">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="80a5a-434">De här filerna kan sedan vara allokerat toosource databasen utan att exponera alla hemligheter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-434">These files can then be committed toosource repository without exposing any secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="80a5a-435">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="80a5a-435">Summary</span></span>
<span data-ttu-id="80a5a-436">I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="80a5a-436">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="80a5a-437">Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80a5a-437">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="80a5a-438">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="80a5a-438">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="80a5a-439">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="80a5a-439">Created two **linked services**:</span></span>
   1. <span data-ttu-id="80a5a-440">**Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-440">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="80a5a-441">**Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-441">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="80a5a-442">Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.</span><span class="sxs-lookup"><span data-stu-id="80a5a-442">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="80a5a-443">Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="80a5a-443">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="80a5a-444">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="80a5a-444">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="80a5a-445">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="80a5a-445">Next Steps</span></span>
<span data-ttu-id="80a5a-446">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="80a5a-446">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="80a5a-447">hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="80a5a-447">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="80a5a-448">Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="80a5a-448">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="80a5a-449">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="80a5a-449">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 


## <a name="see-also"></a><span data-ttu-id="80a5a-450">Se även</span><span class="sxs-lookup"><span data-stu-id="80a5a-450">See Also</span></span>
| <span data-ttu-id="80a5a-451">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="80a5a-451">Topic</span></span> | <span data-ttu-id="80a5a-452">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="80a5a-452">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="80a5a-453">Pipelines</span><span class="sxs-lookup"><span data-stu-id="80a5a-453">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="80a5a-454">Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct datadrivna arbetsflöden för din scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="80a5a-454">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="80a5a-455">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="80a5a-455">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="80a5a-456">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-456">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="80a5a-457">Datatransformeringsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="80a5a-457">Data Transformation Activities</span></span>](data-factory-data-transformation-activities.md) |<span data-ttu-id="80a5a-458">Den här artikeln innehåller en lista med de datatransformeringsaktiviteter (till exempel HDInsight Hive-transformeringen som du använde i självstudien) som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="80a5a-458">This article provides a list of data transformation activities (such as HDInsight Hive transformation you used in this tutorial) supported by Azure Data Factory.</span></span> |
| [<span data-ttu-id="80a5a-459">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="80a5a-459">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="80a5a-460">Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="80a5a-460">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="80a5a-461">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="80a5a-461">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="80a5a-462">Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="80a5a-462">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
