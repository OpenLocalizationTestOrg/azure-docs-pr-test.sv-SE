---
title: "Skapa din första datafabrik (Visual Studio) | Microsoft Docs"
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
ms.openlocfilehash: 77042219cbe698a33ab9447aa952586772897241
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a><span data-ttu-id="f98ef-103">Självstudiekurs: Skapa en datafabrik med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f98ef-103">Tutorial: Create a data factory by using Visual Studio</span></span>
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [<span data-ttu-id="f98ef-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="f98ef-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="f98ef-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f98ef-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="f98ef-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f98ef-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="f98ef-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f98ef-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="f98ef-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="f98ef-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="f98ef-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="f98ef-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)

<span data-ttu-id="f98ef-110">Den här självstudiekursen visar hur du skapar en Azure-datafabrik med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f98ef-110">This tutorial shows you how to create an Azure data factory by using Visual Studio.</span></span> <span data-ttu-id="f98ef-111">Du skapar ett Visual Studio-projekt med Data Factory-projektmallen, definierar Data Factory-enheter (länkade tjänster, datamängder och pipeline) i JSON-format och publicerar/distribuerar sedan dessa enheter till molnet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-111">You create a Visual Studio project using the Data Factory project template, define Data Factory entities (linked services, datasets, and pipeline) in JSON format, and then publish/deploy these entities to the cloud.</span></span> 

<span data-ttu-id="f98ef-112">Pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive-aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="f98ef-113">Aktiviteten kör ett Hive-skript i ett Azure HDInsight-kluster som omvandlar indata för till utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="f98ef-114">Denna pipeline är schemalagd att köras en gång i månaden mellan angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="f98ef-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="f98ef-115">Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f98ef-115">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="f98ef-116">En självstudiekurs om hur du kopierar data med Azure Data Factory finns i [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Självstudie: Kopiera data från Blob Storage till SQL Database).</span><span class="sxs-lookup"><span data-stu-id="f98ef-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="f98ef-117">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="f98ef-118">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-118">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="f98ef-119">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="f98ef-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="walkthrough-create-and-publish-data-factory-entities"></a><span data-ttu-id="f98ef-120">Genomgång: Skapa och publicera datafabriksentiteter</span><span class="sxs-lookup"><span data-stu-id="f98ef-120">Walkthrough: Create and publish Data Factory entities</span></span>
<span data-ttu-id="f98ef-121">Här är de steg du utför i självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="f98ef-121">Here are the steps you perform as part of this walkthrough:</span></span>

1. <span data-ttu-id="f98ef-122">Skapa två länkade tjänster: **AzureStorageLinkedService1** och **HDInsightOnDemandLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-122">Create two linked services: **AzureStorageLinkedService1** and **HDInsightOnDemandLinkedService1**.</span></span> 
   
    <span data-ttu-id="f98ef-123">I den här självstudiekursen är både indata och utdata för Hive-aktiviteten i samma Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-123">In this tutorial, both input and output data for the hive activity are in the same Azure Blob Storage.</span></span> <span data-ttu-id="f98ef-124">Du kan använda HDInsight-kluster på begäran för att bearbeta befintliga indata för att generera utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-124">You use an on-demand HDInsight cluster to process existing input data to produce output data.</span></span> <span data-ttu-id="f98ef-125">HDInsight-kluster på begäran skapas automatiskt åt dig av Azure Data Factory vid körning när indata är redo att bearbetas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-125">The on-demand HDInsight cluster is automatically created for you by Azure Data Factory at run time when the input data is ready to be processed.</span></span> <span data-ttu-id="f98ef-126">Du måste länka dina datalager eller beräkningar till din datafabrik så att Data Factory-tjänsten kan ansluta till dem vid körning.</span><span class="sxs-lookup"><span data-stu-id="f98ef-126">You need to link your data stores or computes to your data factory so that the Data Factory service can connect to them at runtime.</span></span> <span data-ttu-id="f98ef-127">Du ska alltså länka ditt Azure Storage-konto till datafabriken med hjälp av AzureStorageLinkedService1 och länka ett HDInsight-kluster på begäran med hjälp av HDInsightOnDemandLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="f98ef-127">Therefore, you link your Azure Storage Account to the data factory by using the AzureStorageLinkedService1, and link an on-demand HDInsight cluster by using the HDInsightOnDemandLinkedService1.</span></span> <span data-ttu-id="f98ef-128">När du publicerar kan du ange namnet på datafabriken som ska skapas eller en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f98ef-128">When publishing, you specify the name for the data factory to be created or an existing data factory.</span></span>  
2. <span data-ttu-id="f98ef-129">Skapa två datauppsättningar: **InputDataset** och **OutputDataset**, som visar de in- och utdata som lagras i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-129">Create two datasets: **InputDataset** and **OutputDataset**, which represent the input/output data that is stored in the Azure blob storage.</span></span> 
   
    <span data-ttu-id="f98ef-130">Dessa datauppsättningsdefinitioner avser den länkade Azure Storage-tjänsten som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="f98ef-130">These dataset definitions refer to the Azure Storage linked service you created in the previous step.</span></span> <span data-ttu-id="f98ef-131">För InputDataset anger du blobbehållaren (adfgetstarted) och den mapp (inptutdata) som innehåller en blob med indata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-131">For the InputDataset, you specify the blob container (adfgetstarted) and the folder (inptutdata) that contains a blob with the input data.</span></span> <span data-ttu-id="f98ef-132">För OutputDataset anger du blobbehållaren (adfgetstarted) och den mapp (partitioneddata) som innehåller en blob med utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-132">For the OutputDataset, you specify the blob container (adfgetstarted) and the folder (partitioneddata) that holds the output data.</span></span> <span data-ttu-id="f98ef-133">Du kan också ange andra egenskaper som struktur, tillgänglighet och princip.</span><span class="sxs-lookup"><span data-stu-id="f98ef-133">You also specify other properties such as structure, availability, and policy.</span></span>
3. <span data-ttu-id="f98ef-134">Skapa en pipeline med namnet **MyFirstPipeline**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-134">Create a pipeline named **MyFirstPipeline**.</span></span> 
  
    <span data-ttu-id="f98ef-135">Pipelinen i den här självstudiekursen har en aktivitet: **HDInsight Hive-aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-135">In this walkthrough, the pipeline has only one activity: **HDInsight Hive Activity**.</span></span> <span data-ttu-id="f98ef-136">Den här aktiviteten omvandlar indata till utdata genom att köra Hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="f98ef-136">This activity transform input data to produce output data by running a hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="f98ef-137">Mer information om Hive-aktiviteter finns i [Hive-aktivitet](data-factory-hive-activity.md)</span><span class="sxs-lookup"><span data-stu-id="f98ef-137">To learn more about hive activity, see [Hive Activity](data-factory-hive-activity.md)</span></span> 
4. <span data-ttu-id="f98ef-138">Skapa en datafabrik med namnet **DataFactoryUsingVS**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-138">Create a data factory named **DataFactoryUsingVS**.</span></span> <span data-ttu-id="f98ef-139">Distribuera en datafabrik och alla Data Factory-enheter (länkade tjänster, tabeller och pipelinen).</span><span class="sxs-lookup"><span data-stu-id="f98ef-139">Deploy the data factory and all Data Factory entities (linked services, tables, and the pipeline).</span></span>
5. <span data-ttu-id="f98ef-140">När du har publicerat kan du använda bladen på Azure Portal och övervaknings- och hanteringsappen för att övervaka pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-140">After you publish, you use Azure portal blades and Monitoring & Management App to monitor the pipeline.</span></span> 
  
### <a name="prerequisites"></a><span data-ttu-id="f98ef-141">Krav</span><span class="sxs-lookup"><span data-stu-id="f98ef-141">Prerequisites</span></span>
1. <span data-ttu-id="f98ef-142">Läs igenom artikeln [Självstudier – översikt](data-factory-build-your-first-pipeline.md) och slutför de **nödvändiga** stegen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-142">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span> <span data-ttu-id="f98ef-143">Du kan också välja alternativet **Översikt och förutsättningar** i listrutan längst upp för att gå till artikeln.</span><span class="sxs-lookup"><span data-stu-id="f98ef-143">You can also select the **Overview and prerequisites** option in the drop-down list at the top to switch to the article.</span></span> <span data-ttu-id="f98ef-144">När du har slutfört förutsättningarna kan du gå tillbaka till den här artikeln genom att välja alternativet **Visual Studio** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="f98ef-144">After you complete the prerequisites, switch back to this article by selecting **Visual Studio** option in the drop-down list.</span></span>
2. <span data-ttu-id="f98ef-145">Om du vill skapa Data Factory-instanser måste du vara medlem i [Data Factory-deltagarrollen](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) på gruppnivå/resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="f98ef-145">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>  
3. <span data-ttu-id="f98ef-146">Du måste ha följande installerat på datorn:</span><span class="sxs-lookup"><span data-stu-id="f98ef-146">You must have the following installed on your computer:</span></span>
   * <span data-ttu-id="f98ef-147">Visual Studio 2013 eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="f98ef-147">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="f98ef-148">Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="f98ef-148">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="f98ef-149">Gå till [Azures hämtningssida](https://azure.microsoft.com/downloads/) och klicka på **VS 2013** eller **VS 2015** i **.NET**-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-149">Navigate to [Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in the **.NET** section.</span></span>
   * <span data-ttu-id="f98ef-150">Hämta det senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="f98ef-150">Download the latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="f98ef-151">Du kan även uppdatera plugin-programmet genom att göra följande: På menyn klickar du på **Verktyg** -> **Tillägg och uppdateringar** -> **Online** -> **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-151">You can also update the plugin by doing the following steps: On the menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

<span data-ttu-id="f98ef-152">Nu ska vi använda Visual Studio för att skapa en Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f98ef-152">Now, let's use Visual Studio to create an Azure data factory.</span></span>

### <a name="create-visual-studio-project"></a><span data-ttu-id="f98ef-153">Skapa Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="f98ef-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="f98ef-154">Starta **Visual Studio 2013** eller **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-154">Launch **Visual Studio 2013** or **Visual Studio 2015**.</span></span> <span data-ttu-id="f98ef-155">Klicka på **Arkiv**, peka på **Nytt** och klicka på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-155">Click **File**, point to **New**, and click **Project**.</span></span> <span data-ttu-id="f98ef-156">Dialogrutan **Nytt projekt** visas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-156">You should see the **New Project** dialog box.</span></span>  
2. <span data-ttu-id="f98ef-157">I dialogrutan **Nytt projekt** väljer du mallen **DataFactory** och klickar på **Tomt Data Factory-projekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-157">In the **New Project** dialog, select the **DataFactory** template, and click **Empty Data Factory Project**.</span></span>   

    ![Dialogrutan Nytt projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. <span data-ttu-id="f98ef-159">Ange ett **namn** för projektet, en **plats** och ett namn för **lösningen**. Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-159">Enter a **name** for the project, **location**, and a name for the **solution**, and click **OK**.</span></span>

    ![Solution Explorer](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a><span data-ttu-id="f98ef-161">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="f98ef-161">Create linked services</span></span>
<span data-ttu-id="f98ef-162">I det här steget kan du skapa två länkade tjänster: **Azure Storage** och **HDInsight på begäran**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-162">In this step, you create two linked services: **Azure Storage** and **HDInsight on-demand**.</span></span> 

<span data-ttu-id="f98ef-163">Den länkade Azure Storage-tjänsten länkar ditt Azure Storage-konto till datafabriken genom att tillhandahålla anslutningsinformation.</span><span class="sxs-lookup"><span data-stu-id="f98ef-163">The Azure Storage linked service links your Azure Storage account to the data factory by providing the connection information.</span></span> <span data-ttu-id="f98ef-164">Data Factory-tjänsten använder anslutningssträngen från inställningen för den länkade tjänsten för att ansluta till Azure Storage vid körning.</span><span class="sxs-lookup"><span data-stu-id="f98ef-164">Data Factory service uses the connection string from the linked service setting to connect to the Azure storage at runtime.</span></span> <span data-ttu-id="f98ef-165">Det här lagringsutrymmet innehåller indata och utdata för pipelinen och Hive-skriptfilen som används av Hive-aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-165">This storage holds input and output data for the pipeline, and the hive script file used by the hive activity.</span></span> 

<span data-ttu-id="f98ef-166">Med den länkade tjänsten HDInsight på begäran skapas HDInsight-klustret automatiskt vid körning när indata är redo att bearbetas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-166">With on-demand HDInsight linked service, The HDInsight cluster is automatically created at runtime when the input data is ready to processed.</span></span> <span data-ttu-id="f98ef-167">Klustret tas bort när bearbetningen är klar. Det är inaktivt under en angiven tidsrymd.</span><span class="sxs-lookup"><span data-stu-id="f98ef-167">The cluster is deleted after it is done processing and idle for the specified amount of time.</span></span> 

> [!NOTE]
> <span data-ttu-id="f98ef-168">Du kan skapa en datafabrik genom att ange dess namn och inställningar när du publicerar din Data Factory-lösning.</span><span class="sxs-lookup"><span data-stu-id="f98ef-168">You create a data factory by specifying its name and settings at the time of publishing your Data Factory solution.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="f98ef-169">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="f98ef-169">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="f98ef-170">Högerklicka på **Länkade tjänster** i Solution Explorer, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-170">Right-click **Linked Services** in the solution explorer, point to **Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="f98ef-171">I dialogrutan **Lägg till nytt objekt** väljer du **Länkad Azure Storage-tjänst** i listan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-171">In the **Add New Item** dialog box, select **Azure Storage Linked Service** from the list, and click **Add**.</span></span>
    <span data-ttu-id="f98ef-172">![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="f98ef-172">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span></span>
3. <span data-ttu-id="f98ef-173">Byt ut `<accountname>` och `<accountkey>` mot namnet på ditt Azure-lagringskonto och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="f98ef-173">Replace `<accountname>` and `<accountkey>` with the name of your Azure storage account and its key.</span></span> <span data-ttu-id="f98ef-174">Information om hur du hämtar lagringsåtkomstnyckeln finns i avsnitten om hur du visar, kopierar och återskapar åtkomstnycklar i [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account) (Hantera ditt lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="f98ef-174">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
    <span data-ttu-id="f98ef-175">![Länkad Azure Storage-tjänst](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="f98ef-175">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span></span>
4. <span data-ttu-id="f98ef-176">Spara filen **AzureStorageLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-176">Save the **AzureStorageLinkedService1.json** file.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="f98ef-177">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="f98ef-177">Create Azure HDInsight linked service</span></span>
1. <span data-ttu-id="f98ef-178">I **Solution Explorer** högerklickar du på **Länkade tjänster**, pekar på **Lägg till** och klickar på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-178">In the **Solution Explorer**, right-click **Linked Services**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="f98ef-179">Välj **Länkad HDInsight-tjänst på begäran** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-179">Select **HDInsight On Demand Linked Service**, and click **Add**.</span></span>
3. <span data-ttu-id="f98ef-180">Ersätt **JSON** med följande JSON:</span><span class="sxs-lookup"><span data-stu-id="f98ef-180">Replace the **JSON** with the following JSON:</span></span>

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

    <span data-ttu-id="f98ef-181">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="f98ef-181">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    <span data-ttu-id="f98ef-182">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f98ef-182">Property</span></span> | <span data-ttu-id="f98ef-183">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f98ef-183">Description</span></span>
    -------- | ----------- 
    <span data-ttu-id="f98ef-184">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="f98ef-184">ClusterSize</span></span> | <span data-ttu-id="f98ef-185">Anger HDInsight Hadoop-klustrets storlek.</span><span class="sxs-lookup"><span data-stu-id="f98ef-185">Specifies the size of the HDInsight Hadoop cluster.</span></span>
    <span data-ttu-id="f98ef-186">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="f98ef-186">TimeToLive</span></span> | <span data-ttu-id="f98ef-187">Anger inaktivitetstiden för HDInsight-klustret innan det tas bort.</span><span class="sxs-lookup"><span data-stu-id="f98ef-187">Specifies that the idle time for the HDInsight cluster, before it is deleted.</span></span>
    <span data-ttu-id="f98ef-188">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="f98ef-188">linkedServiceName</span></span> | <span data-ttu-id="f98ef-189">Anger lagringskontot som används för att spara loggarna som genereras av HDInsight Hadoop-klustret.</span><span class="sxs-lookup"><span data-stu-id="f98ef-189">Specifies the storage account that is used to store the logs that are generated by HDInsight Hadoop cluster.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="f98ef-190">HDInsight-klustret skapar en **standardbehållare** i den bloblagring som du angav i JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="f98ef-190">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (linkedServiceName).</span></span> <span data-ttu-id="f98ef-191">HDInsight tar inte bort den här behållaren när klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="f98ef-191">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="f98ef-192">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="f98ef-192">This behavior is by design.</span></span> <span data-ttu-id="f98ef-193">Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="f98ef-193">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="f98ef-194">Klustret tas bort automatiskt när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-194">The cluster is automatically deleted when the processing is done.</span></span>
    > 
    > <span data-ttu-id="f98ef-195">Allteftersom fler sektorer bearbetas kan du se mång behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-195">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="f98ef-196">Om du inte behöver dem för att felsöka jobb, kan du ta bort dem för att minska lagringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="f98ef-196">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="f98ef-197">Namnen på de här behållarna följer ett mönster: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-197">The names of these containers follow a pattern: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span></span> <span data-ttu-id="f98ef-198">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) till att ta bort behållare i din Azure Blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="f98ef-198">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

    <span data-ttu-id="f98ef-199">Mer information om JSON-egenskaper finns i artikeln [Länkade tjänster för Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="f98ef-199">For more information about JSON properties, see [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article.</span></span> 
4. <span data-ttu-id="f98ef-200">Spara filen **HDInsightOnDemandLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-200">Save the **HDInsightOnDemandLinkedService1.json** file.</span></span>

### <a name="create-datasets"></a><span data-ttu-id="f98ef-201">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="f98ef-201">Create datasets</span></span>
<span data-ttu-id="f98ef-202">I det här steget skapar du datauppsättningar som ska representera in- och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="f98ef-202">In this step, you create datasets to represent the input and output data for Hive processing.</span></span> <span data-ttu-id="f98ef-203">Dessa datauppsättningar avser den **AzureStorageLinkedService1** som du skapade tidigare i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="f98ef-203">These datasets refer to the **AzureStorageLinkedService1** you have created earlier in this tutorial.</span></span> <span data-ttu-id="f98ef-204">Den länkade tjänsten pekar på ett Azure Storage-konto och datauppsättningarna anger behållare, mapp och filnamn i det lagringsutrymme som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-204">The linked service points to an Azure Storage account and datasets specify container, folder, file name in the storage that holds input and output data.</span></span>   

#### <a name="create-input-dataset"></a><span data-ttu-id="f98ef-205">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="f98ef-205">Create input dataset</span></span>
1. <span data-ttu-id="f98ef-206">I **Solution Explorer** högerklickar du på **Tabeller**, pekar på **Lägg till** och klickar på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-206">In the **Solution Explorer**, right-click **Tables**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="f98ef-207">Välj **Azure-blobb** i listan och ändra namnet på filen till **InputDataSet.json**. Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-207">Select **Azure Blob** from the list, change the name of the file to **InputDataSet.json**, and click **Add**.</span></span>
3. <span data-ttu-id="f98ef-208">Ersätt **JSON** i redigeraren med följande JSON-kodavsnitt:</span><span class="sxs-lookup"><span data-stu-id="f98ef-208">Replace the **JSON** in the editor with the following JSON snippet:</span></span>

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
    <span data-ttu-id="f98ef-209">Det här JSON-kodfragmentet definierar en datauppsättning med namnet **AzureBlobInput** som representerar indata för Hive-aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-209">This JSON snippet defines a dataset called **AzureBlobInput** that represents input data for the hive activity in the pipeline.</span></span> <span data-ttu-id="f98ef-210">Du anger att indata finns i blobbehållaren `adfgetstarted` och i mappen `inputdata`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-210">You specify that the input data is located in the blob container called `adfgetstarted` and the folder called `inputdata`.</span></span>

    <span data-ttu-id="f98ef-211">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="f98ef-211">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    <span data-ttu-id="f98ef-212">Egenskap</span><span class="sxs-lookup"><span data-stu-id="f98ef-212">Property</span></span> | <span data-ttu-id="f98ef-213">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f98ef-213">Description</span></span> |
    -------- | ----------- |
    <span data-ttu-id="f98ef-214">typ</span><span class="sxs-lookup"><span data-stu-id="f98ef-214">type</span></span> |<span data-ttu-id="f98ef-215">Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-215">The type property is set to **AzureBlob** because data resides in Azure Blob Storage.</span></span>
    <span data-ttu-id="f98ef-216">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="f98ef-216">linkedServiceName</span></span> | <span data-ttu-id="f98ef-217">Refererar till AzureStorageLinkedService1 som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f98ef-217">Refers to the AzureStorageLinkedService1 you created earlier.</span></span>
    <span data-ttu-id="f98ef-218">fileName</span><span class="sxs-lookup"><span data-stu-id="f98ef-218">fileName</span></span> |<span data-ttu-id="f98ef-219">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="f98ef-219">This property is optional.</span></span> <span data-ttu-id="f98ef-220">Om du tar bort egenskapen kommer alla filer från folderPath hämtas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-220">If you omit this property, all the files from the folderPath are picked.</span></span> <span data-ttu-id="f98ef-221">I det här fallet bearbetas bara input.log.</span><span class="sxs-lookup"><span data-stu-id="f98ef-221">In this case, only the input.log is processed.</span></span>
    <span data-ttu-id="f98ef-222">typ</span><span class="sxs-lookup"><span data-stu-id="f98ef-222">type</span></span> | <span data-ttu-id="f98ef-223">Loggfilerna är i textformat, så vi använder TextFormat.</span><span class="sxs-lookup"><span data-stu-id="f98ef-223">The log files are in text format, so we use TextFormat.</span></span> |
    <span data-ttu-id="f98ef-224">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="f98ef-224">columnDelimiter</span></span> | <span data-ttu-id="f98ef-225">kolumner i loggfilerna avgränsas med kommatecken (`,`)</span><span class="sxs-lookup"><span data-stu-id="f98ef-225">columns in the log files are delimited by the comma character (`,`)</span></span>
    <span data-ttu-id="f98ef-226">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="f98ef-226">frequency/interval</span></span> | <span data-ttu-id="f98ef-227">frekvensen är månad och intervallet är 1, vilket innebär att indatasektorerna är tillgängliga en gång i månaden.</span><span class="sxs-lookup"><span data-stu-id="f98ef-227">frequency set to Month and interval is 1, which means that the input slices are available monthly.</span></span>
    <span data-ttu-id="f98ef-228">extern</span><span class="sxs-lookup"><span data-stu-id="f98ef-228">external</span></span> | <span data-ttu-id="f98ef-229">Den här egenskapen anges som true om indata för aktiviteten inte skapades av pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-229">This property is set to true if the input data for the activity is not generated by the pipeline.</span></span> <span data-ttu-id="f98ef-230">Den här egenskapen anges endast för indatauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-230">This property is only specified on input datasets.</span></span> <span data-ttu-id="f98ef-231">Ange alltid true för indatauppsättningen för den första aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-231">For the input dataset of the first activity, always set it to true.</span></span>
4. <span data-ttu-id="f98ef-232">Spara filen **InputDataset.json**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-232">Save the **InputDataset.json** file.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="f98ef-233">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="f98ef-233">Create output dataset</span></span>
<span data-ttu-id="f98ef-234">Nu skapar du den utdatauppsättning som representerar de utdata som lagras i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-234">Now, you create the output dataset to represent output data stored in the Azure Blob storage.</span></span>

1. <span data-ttu-id="f98ef-235">I **Solution Explorer** högerklickar du på **Tabeller**, pekar på **Lägg till** och klickar på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-235">In the **Solution Explorer**, right-click **tables**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="f98ef-236">Välj **Azure-blobb** i listan och ändra namnet på filen till **OutputDataset.json**. Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-236">Select **Azure Blob** from the list, change the name of the file to **OutputDataset.json**, and click **Add**.</span></span>
3. <span data-ttu-id="f98ef-237">Ersätt **JSON** i redigeraren med följande JSON:</span><span class="sxs-lookup"><span data-stu-id="f98ef-237">Replace the **JSON** in the editor with the following JSON:</span></span>
    
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
    <span data-ttu-id="f98ef-238">Det här JSON-kodfragmentet definierar en datauppsättning med namnet **AzureBlobOutput** som representerar utdata för Hive-aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-238">The JSON snippet defines a dataset called **AzureBlobOutput** that represents output data produced by the hive activity in the pipeline.</span></span> <span data-ttu-id="f98ef-239">Du anger att utdata som genereras av Hive-aktiviteten finns i blobbehållaren `adfgetstarted` och i mappen `partitioneddata`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-239">You specify that the output data is produced by the hive activity is placed in the blob container called `adfgetstarted` and the folder called `partitioneddata`.</span></span> 
    
    <span data-ttu-id="f98ef-240">I avsnittet **tillgänglighet** anges att utdatauppsättningen skapas månadsvis.</span><span class="sxs-lookup"><span data-stu-id="f98ef-240">The **availability** section specifies that the output dataset is produced on a monthly basis.</span></span> <span data-ttu-id="f98ef-241">Utdatauppsättningen styr schemat för pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-241">The output dataset drives the schedule of the pipeline.</span></span> <span data-ttu-id="f98ef-242">Pipelinen körs varje månad mellan dess start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="f98ef-242">The pipeline runs monthly between its start and end times.</span></span> 

    <span data-ttu-id="f98ef-243">Se **Skapa datauppsättning för indata** för beskrivningar av dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="f98ef-243">See **Create the input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="f98ef-244">Du anger inte den externa egenskapen för en utdatauppsättning, eftersom datauppsättningen produceras av pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-244">You do not set the external property on an output dataset as the dataset is produced by the pipeline.</span></span>
4. <span data-ttu-id="f98ef-245">Spara filen **OutputDataset.json**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-245">Save the **OutputDataset.json** file.</span></span>

### <a name="create-pipeline"></a><span data-ttu-id="f98ef-246">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="f98ef-246">Create pipeline</span></span>
<span data-ttu-id="f98ef-247">Du har skapat den länkade Azure Storage-tjänsten och in- och utdatauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-247">You have created the Azure Storage linked service, and input and output datasets so far.</span></span> <span data-ttu-id="f98ef-248">Nu ska du skapa en pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-248">Now, you create a pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="f98ef-249">**Indata** för Hive-aktiviteten är inställd på **AzureBlobInput** och **utdata** är inställd på **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-249">The **input** for the hive activity is set to **AzureBlobInput** and **output** is set to **AzureBlobOutput**.</span></span> <span data-ttu-id="f98ef-250">En sektor av indatauppsättningen är tillgänglig varje månad (frekvens: Månad, intervall: 1), och utdatasektorn produceras också varje månad.</span><span class="sxs-lookup"><span data-stu-id="f98ef-250">A slice of an input dataset is available monthly (frequency: Month, interval: 1), and the output slice is produced monthly too.</span></span> 

1. <span data-ttu-id="f98ef-251">I **Solution Explorer** högerklickar du på **Pipelines**, pekar på **Lägg till** och klickar på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-251">In the **Solution Explorer**, right-click **Pipelines**, point to **Add**, and click **New Item.**</span></span>
2. <span data-ttu-id="f98ef-252">Välj **Pipeline för Hive-transformering** i listan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-252">Select **Hive Transformation Pipeline** from the list, and click **Add**.</span></span>
3. <span data-ttu-id="f98ef-253">Ersätt **JSON** med följande kodfragment:</span><span class="sxs-lookup"><span data-stu-id="f98ef-253">Replace the **JSON** with the following snippet:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f98ef-254">Ersätt `<storageaccountname>` med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f98ef-254">Replace `<storageaccountname>` with the name of your storage account.</span></span>

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
    > <span data-ttu-id="f98ef-255">Ersätt `<storageaccountname>` med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f98ef-255">Replace `<storageaccountname>` with the name of your storage account.</span></span>

    <span data-ttu-id="f98ef-256">JSON-kodfragmentet definierar en pipeline som består av en enskild aktivitet (Hive-aktivitet).</span><span class="sxs-lookup"><span data-stu-id="f98ef-256">The JSON snippet defines a pipeline that consists of a single activity (Hive Activity).</span></span> <span data-ttu-id="f98ef-257">Aktiviteten kör ett Hive-skript för att bearbeta indata på ett HDInsight-kluster på begäran för att producera utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-257">This activity runs a Hive script to process input data on an on-demand HDInsight cluster to produce output data.</span></span> <span data-ttu-id="f98ef-258">I aktivitetsavsnittet i JSON för pipelinen finns bara en aktivitet i matrisen med typen **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-258">In the activities section of the pipeline JSON, you see only one activity in the array with type set to **HDInsightHive**.</span></span> 

    <span data-ttu-id="f98ef-259">I typegenskaperna som är specifika för HDInsight Hive-aktiviteten anger du vilken länkad Azure Storage-tjänst som har Hive-skriptfilen, sökvägen till skriptfilen och parametrar till skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-259">In the type properties that are specific to HDInsight Hive activity, you specify what Azure Storage linked service has the hive script file, the path to the script file, and parameters to the script file.</span></span> 

    <span data-ttu-id="f98ef-260">Hive-skriptfilen **partitionweblogs.hql** lagras på Azure-lagringskontot (anges med scriptLinkedService), och i mappen `script` i behållaren `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-260">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService), and in the `script` folder in the container `adfgetstarted`.</span></span>

    <span data-ttu-id="f98ef-261">Avsnittet `defines` används för att ange körningsinställningar som skickas till Hive-skriptet som Hive-konfigurationsvärden (till exempel `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-261">The `defines` section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span></span>

    <span data-ttu-id="f98ef-262">Egenskaperna **start** och **slut** för pipelinen anger den aktiva perioden för pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-262">The **start** and **end** properties of the pipeline specifies the active period of the pipeline.</span></span> <span data-ttu-id="f98ef-263">Du har konfigurerat datauppsättningen som produceras varje månad. Därför produceras bara en sektor av pipelinen (eftersom månaden är densamma i start- och slutdatumen).</span><span class="sxs-lookup"><span data-stu-id="f98ef-263">You configured the dataset to be produced monthly, therefore, only once slice is produced by the pipeline (because the month is same in start and end dates).</span></span>

    <span data-ttu-id="f98ef-264">I JSON-aktiviteten anger du att Hive-skriptet körs i den beräkning som anges av **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-264">In the activity JSON, you specify that the Hive script runs on the compute specified by the **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>
4. <span data-ttu-id="f98ef-265">Spara filen **HiveActivity1.json**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-265">Save the **HiveActivity1.json** file.</span></span>

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a><span data-ttu-id="f98ef-266">Lägg till partitionweblogs.hql och input.log som ett beroende</span><span class="sxs-lookup"><span data-stu-id="f98ef-266">Add partitionweblogs.hql and input.log as a dependency</span></span>
1. <span data-ttu-id="f98ef-267">Högerklicka på **Beroenden** i fönstret **Solution Explorer**, peka på **Lägg till** och klicka på **Befintligt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-267">Right-click **Dependencies** in the **Solution Explorer** window, point to **Add**, and click **Existing Item**.</span></span>  
2. <span data-ttu-id="f98ef-268">Gå till **C:\ADFGettingStarted** och markera filerna **partitionweblogs.hql**, **input.log**. Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-268">Navigate to the **C:\ADFGettingStarted** and select **partitionweblogs.hql**, **input.log** files, and click **Add**.</span></span> <span data-ttu-id="f98ef-269">Du har skapat dessa två filer som en del av förutsättningarna från [Självstudier – översikt](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="f98ef-269">You created these two files as part of prerequisites from the [Tutorial Overview](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="f98ef-270">När du publicerar lösningen i nästa steg laddas filen **partitionweblogs.hql** upp till **skript**-mappen i blobbehållaren `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-270">When you publish the solution in the next step, the **partitionweblogs.hql** file is uploaded to the **script** folder in the `adfgetstarted` blob container.</span></span>   

### <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="f98ef-271">Publicera/distribuera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="f98ef-271">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="f98ef-272">I det här steget publicerar du datafabriksentiteter (länkade tjänster, datauppsättningar och pipeline) i projektet till Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-272">In this step, you publish the Data Factory entities (linked services, datasets, and pipeline) in your project to the Azure Data Factory service.</span></span> <span data-ttu-id="f98ef-273">Vid publiceringen anger du namnet på datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f98ef-273">In the process of publishing, you specify the name for your data factory.</span></span> 

1. <span data-ttu-id="f98ef-274">I Solution Explorer högerklickar du på projektet och klickar sedan på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-274">Right-click project in the Solution Explorer, and click **Publish**.</span></span>
2. <span data-ttu-id="f98ef-275">Om du ser dialogrutan **Logga in på ditt Microsoft-konto**, anger du dina autentiseringsuppgifter för det konto som har Azure-prenumerationen. Klicka sedan på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-275">If you see **Sign in to your Microsoft account** dialog box, enter your credentials for the account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="f98ef-276">Du bör se följande dialogruta:</span><span class="sxs-lookup"><span data-stu-id="f98ef-276">You should see the following dialog box:</span></span>

   ![Dialogrutan Publicera](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. <span data-ttu-id="f98ef-278">På sidan **Konfigurera datafabrik** går du igenom följande steg:</span><span class="sxs-lookup"><span data-stu-id="f98ef-278">In the **Configure data factory** page, do the following steps:</span></span>

    ![Publicera – nya datafabriksinställningar](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. <span data-ttu-id="f98ef-280">välj alternativet **Skapa ny Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-280">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="f98ef-281">Ange ett unikt **namn** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f98ef-281">Enter a unique **name** for the data factory.</span></span> <span data-ttu-id="f98ef-282">Exempel: **DataFactoryUsingVS09152016**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-282">For example: **DataFactoryUsingVS09152016**.</span></span> <span data-ttu-id="f98ef-283">Namnet måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="f98ef-283">The name must be globally unique.</span></span>
   3. <span data-ttu-id="f98ef-284">Välj rätt prenumeration i fältet **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-284">Select the right subscription for the **Subscription** field.</span></span> 
        > [!IMPORTANT]
        > <span data-ttu-id="f98ef-285">Om du inte ser någon prenumeration kontrollerar du att du har loggat in med ett konto som är en administratör eller en medadministratör för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-285">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of the subscription.</span></span>
   4. <span data-ttu-id="f98ef-286">Välj **resursgrupp** för datafabriken som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-286">Select the **resource group** for the data factory to be created.</span></span>
   5. <span data-ttu-id="f98ef-287">Välj **region** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f98ef-287">Select the **region** for the data factory.</span></span>
   6. <span data-ttu-id="f98ef-288">Klicka på **Nästa** för att växla till sidan **Publicera objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-288">Click **Next** to switch to the **Publish Items** page.</span></span> <span data-ttu-id="f98ef-289">(Tryck på **TAB** för att flytta ut från namnfältet om knappen **Nästa** är inaktiverad.)</span><span class="sxs-lookup"><span data-stu-id="f98ef-289">(Press **TAB** to move out of the Name field to if the **Next** button is disabled.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f98ef-290">Om du får felet **Datafabriksnamnet ”DataFactoryUsingVS” är inte tillgängligt** när du publicerar, ändrar du namnet (till exempel yournameDataFactoryUsingVS).</span><span class="sxs-lookup"><span data-stu-id="f98ef-290">If you receive the error **Data factory name “DataFactoryUsingVS” is not available** when publishing, change the name (for example, yournameDataFactoryUsingVS).</span></span> <span data-ttu-id="f98ef-291">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="f98ef-291">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
1. <span data-ttu-id="f98ef-292">På sidan **Publicera objekt** kontrollerar du att alla datafabriksentiteter har valts. Klicka på **Nästa** för att växla till sidan **Sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-292">In the **Publish Items** page, ensure that all the Data Factories entities are selected, and click **Next** to switch to the **Summary** page.</span></span>

    ![Sidan Publish items (Publicera objekt)](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. <span data-ttu-id="f98ef-294">Granska sammanfattningen och klicka på **Nästa** för att starta distributionsprocessen och visa **Distributionsstatus**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-294">Review the summary and click **Next** to start the deployment process and view the **Deployment Status**.</span></span>

    ![Sammanfattningssida](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. <span data-ttu-id="f98ef-296">På sidan **Distributionsstatus** bör du se statusen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-296">In the **Deployment Status** page, you should see the status of the deployment process.</span></span> <span data-ttu-id="f98ef-297">Klicka på Slutför när distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-297">Click Finish after the deployment is done.</span></span>

<span data-ttu-id="f98ef-298">Viktiga saker att observera:</span><span class="sxs-lookup"><span data-stu-id="f98ef-298">Important points to note:</span></span>

- <span data-ttu-id="f98ef-299">Om du får felet: **Den här prenumerationen har inte registrerats för användning av namnområdet Microsoft.DataFactory** gör du något av följande och försöker att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="f98ef-299">If you receive the error: **This subscription is not registered to use namespace Microsoft.DataFactory**, do one of the following and try publishing again:</span></span>
    - <span data-ttu-id="f98ef-300">I Azure PowerShell kör du följande kommando för att registrera Data Factory-providern.</span><span class="sxs-lookup"><span data-stu-id="f98ef-300">In Azure PowerShell, run the following command to register the Data Factory provider.</span></span>
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        <span data-ttu-id="f98ef-301">Du kan köra följande kommando om du vill kontrollera att Data Factory-providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="f98ef-301">You can run the following command to confirm that the Data Factory provider is registered.</span></span>

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - <span data-ttu-id="f98ef-302">Logga in med Azure-prenumerationen i [Azure Portal](https://portal.azure.com) och navigera till ett Data Factory-blad (eller) skapa en datafabrik i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="f98ef-302">Login using the Azure subscription in to the [Azure portal](https://portal.azure.com) and navigate to a Data Factory blade (or) create a data factory in the Azure portal.</span></span> <span data-ttu-id="f98ef-303">Med den här åtgärden registreras providern automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f98ef-303">This action automatically registers the provider for you.</span></span>
- <span data-ttu-id="f98ef-304">Namnet på datafabriken kan komma att registreras som ett DNS-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="f98ef-304">The name of the data factory may be registered as a DNS name in the future and hence become publically visible.</span></span>
- <span data-ttu-id="f98ef-305">Om du vill skapa Data Factory-instanser måste du vara administratör eller medadministratör för Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="f98ef-305">To create Data Factory instances, you need to be an admin or co-admin of the Azure subscription</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="f98ef-306">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="f98ef-306">Monitor pipeline</span></span>
<span data-ttu-id="f98ef-307">I det här steget övervakar du pipelinen med hjälp av datafabrikens diagramvy.</span><span class="sxs-lookup"><span data-stu-id="f98ef-307">In this step, you monitor the pipeline using Diagram View of the data factory.</span></span> 

#### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="f98ef-308">Övervaka pipeline med diagramvyn</span><span class="sxs-lookup"><span data-stu-id="f98ef-308">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="f98ef-309">Logga in på [Azure Portal](https://portal.azure.com/) genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="f98ef-309">Log in to the [Azure portal](https://portal.azure.com/), do the following steps:</span></span>
   1. <span data-ttu-id="f98ef-310">Klicka på **Fler tjänster** och på **Datafabriker**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-310">Click **More services** and click **Data factories**.</span></span>
       
        ![Bläddra igenom datafabrikerna](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. <span data-ttu-id="f98ef-312">Välj namnet på din datafabrik (till exempel: **DataFactoryUsingVS09152016**) från listan med datafabriker.</span><span class="sxs-lookup"><span data-stu-id="f98ef-312">Select the name of your data factory (for example: **DataFactoryUsingVS09152016**) from the list of data factories.</span></span>
   
       ![Välj din datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. <span data-ttu-id="f98ef-314">På startsidan för din datafabrik klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-314">In the home page for your data factory, click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. <span data-ttu-id="f98ef-316">I diagramvyn visas en översikt över pipelines och datauppsättningar som används i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="f98ef-316">In the Diagram View, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. <span data-ttu-id="f98ef-318">Högerklicka på pipelinen i diagrammet om du vill visa alla aktiviteter i pipelinen. Klicka sedan på Öppna pipeline.</span><span class="sxs-lookup"><span data-stu-id="f98ef-318">To view all activities in the pipeline, right-click pipeline in the diagram and click Open Pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. <span data-ttu-id="f98ef-320">Kontrollera att du ser HDInsightHive-aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-320">Confirm that you see the HDInsightHive activity in the pipeline.</span></span>

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    <span data-ttu-id="f98ef-322">Om du vill gå tillbaka till den föregående vyn klickar du på **Datafabrik** i adressfältmenyn längst upp.</span><span class="sxs-lookup"><span data-stu-id="f98ef-322">To navigate back to the previous view, click **Data factory** in the breadcrumb menu at the top.</span></span>
6. <span data-ttu-id="f98ef-323">Dubbelklicka på datauppsättningen **AzureBlobInput** i **diagramvyn**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-323">In the **Diagram View**, double-click the dataset **AzureBlobInput**.</span></span> <span data-ttu-id="f98ef-324">Kontrollera att sektorn har statusen **Klar**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-324">Confirm that the slice is in **Ready** state.</span></span> <span data-ttu-id="f98ef-325">Det kan ta några minuter innan sektorn visas med statusen Klar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-325">It may take a couple of minutes for the slice to show up in Ready state.</span></span> <span data-ttu-id="f98ef-326">Om det inte händer trots att du har väntat ett tag, kontrollerar du att du har indatafilen (input.log) placerad i rätt behållare (`adfgetstarted`) och mapp (`inputdata`).</span><span class="sxs-lookup"><span data-stu-id="f98ef-326">If it does not happen after you wait for sometime, see if you have the input file (input.log) placed in the right container (`adfgetstarted`) and folder (`inputdata`).</span></span> <span data-ttu-id="f98ef-327">Och se till att egenskapen **external** för indatauppsättningen är inställd på **true**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-327">And, make sure that the **external** property on the input dataset is set to **true**.</span></span> 

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. <span data-ttu-id="f98ef-329">Klicka på **X** för att stänga bladet **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-329">Click **X** to close **AzureBlobInput** blade.</span></span>
8. <span data-ttu-id="f98ef-330">Dubbelklicka på datauppsättningen **AzureBlobOutput** i **diagramvyn**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-330">In the **Diagram View**, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="f98ef-331">Den sektor som för närvarande bearbetas visas.</span><span class="sxs-lookup"><span data-stu-id="f98ef-331">You see that the slice that is currently being processed.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. <span data-ttu-id="f98ef-333">När bearbetningen är klar visas sektorn med statusen **Klar**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-333">When processing is done, you see the slice in **Ready** state.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="f98ef-334">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="f98ef-334">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="f98ef-335">Förvänta dig därför att det tar **cirka 30 minuter** för pipelinen att bearbeta sektorn.</span><span class="sxs-lookup"><span data-stu-id="f98ef-335">Therefore, expect the pipeline to take **approximately 30 minutes** to process the slice.</span></span>  
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. <span data-ttu-id="f98ef-337">När sektorn har statusen **Redo**, kontrollerar du mappen `partitioneddata` i behållaren `adfgetstarted` i ditt Blob Storage för utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-337">When the slice is in **Ready** state, check the `partitioneddata` folder in the `adfgetstarted` container in your blob storage for the output data.</span></span>  

    ![utdata](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. <span data-ttu-id="f98ef-339">Klicka på sektorn om du vill se information om den i ett **Datasektor**-blad.</span><span class="sxs-lookup"><span data-stu-id="f98ef-339">Click the slice to see details about it in a **Data slice** blade.</span></span>

    ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. <span data-ttu-id="f98ef-341">Klicka på en aktivitet som körs i **Lista med aktivitetskörningar** för att se information om en aktivitetskörning (Hive-aktivitet i vårt exempel) i ett fönster med **Aktivitetskörningsinformation**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-341">Click an activity run in the **Activity runs list** to see details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span> 
  
    ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    <span data-ttu-id="f98ef-343">Du kan se Hive-frågan som kördes och statusinformation i loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="f98ef-343">From the log files, you can see the Hive query that was executed and status information.</span></span> <span data-ttu-id="f98ef-344">Dessa loggar är användbara vid felsökning av eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="f98ef-344">These logs are useful for troubleshooting any issues.</span></span>  

<span data-ttu-id="f98ef-345">Se [Övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) för instruktioner om hur du använder Azure Portal till att övervaka pipeline och datauppsättningar som du har skapat i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="f98ef-345">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how to use the Azure portal to monitor the pipeline and datasets you have created in this tutorial.</span></span>

#### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="f98ef-346">Övervaka pipeline med övervaknings- och hanteringsappen</span><span class="sxs-lookup"><span data-stu-id="f98ef-346">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="f98ef-347">Du kan också använda övervaknings- och hanteringsprogrammet till att övervaka dina pipelines.</span><span class="sxs-lookup"><span data-stu-id="f98ef-347">You can also use Monitor & Manage application to monitor your pipelines.</span></span> <span data-ttu-id="f98ef-348">Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-348">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="f98ef-349">Klicka på ikonen Övervaka och hantera.</span><span class="sxs-lookup"><span data-stu-id="f98ef-349">Click Monitor & Manage tile.</span></span>

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. <span data-ttu-id="f98ef-351">Du bör se programmet Övervaka och hantera.</span><span class="sxs-lookup"><span data-stu-id="f98ef-351">You should see Monitor & Manage application.</span></span> <span data-ttu-id="f98ef-352">Ändra **Starttid** och **Sluttid** till att matcha starttid (04-01-2016 12:00) och sluttid (04-02-2016 12:00) i pipelinen. Klicka sedan på **Tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-352">Change the **Start time** and **End time** to match start (04-01-2016 12:00 AM) and end times (04-02-2016 12:00 AM) of your pipeline, and click **Apply**.</span></span>

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. <span data-ttu-id="f98ef-354">Om du vill visa information om ett aktivitetsfönster väljer du det i listan **Aktivitetsfönster**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-354">To see details about an activity window, select it in the **Activity Windows list** to see details about it.</span></span>
    <span data-ttu-id="f98ef-355">![Aktivitetsfönsterinformation](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="f98ef-355">![Activity window details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f98ef-356">Indatafilen tas bort när sektorn har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="f98ef-356">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="f98ef-357">Om du vill köra sektorn eller gå igenom självstudien igen laddar du därför upp indatafilen (input.log) till mappen `inputdata` i behållaren`adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-357">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the `inputdata` folder of the `adfgetstarted` container.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="f98ef-358">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="f98ef-358">Additional notes</span></span>
- <span data-ttu-id="f98ef-359">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="f98ef-359">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="f98ef-360">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="f98ef-360">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="f98ef-361">Till exempel, en kopieringsaktivitet som kopierar data från en källa till en måldatalagring och en HDInsight Hive-aktivitet som kör ett Hive-skript som omvandlar indata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-361">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data.</span></span> <span data-ttu-id="f98ef-362">I [stödda datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) står alla källor och mottagare som stöds av Kopiera aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-362">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="f98ef-363">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för att se listan över Compute Services som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f98ef-363">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory.</span></span>
- <span data-ttu-id="f98ef-364">Länkade tjänster länkar datalager eller beräkningstjänster till en Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f98ef-364">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="f98ef-365">I [stödda datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) står alla källor och mottagare som stöds av Kopiera aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-365">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="f98ef-366">Se [Länkade tjänster för Compute](data-factory-compute-linked-services.md) för att se listan över Compute Services som stöds av Data Factory och [transformeringsaktiviteter](data-factory-data-transformation-activities.md) som kan köras på dem.</span><span class="sxs-lookup"><span data-stu-id="f98ef-366">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory and [transformation activities](data-factory-data-transformation-activities.md) that can run on them.</span></span>
- <span data-ttu-id="f98ef-367">Se [Flytta data från/till Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) för mer information om JSON-egenskaper som används i definitionen för den länkade Azure Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-367">See [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used in the Azure Storage linked service definition.</span></span>
- <span data-ttu-id="f98ef-368">Du kan använda ditt eget HDInsight-kluster i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="f98ef-368">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="f98ef-369">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="f98ef-369">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>
-  <span data-ttu-id="f98ef-370">Data Factory skapar ett **Linux-baserat** HDInsight-kluster åt dig med ovanstående JSON.</span><span class="sxs-lookup"><span data-stu-id="f98ef-370">The Data Factory creates a **Linux-based** HDInsight cluster for you with the preceding JSON.</span></span> <span data-ttu-id="f98ef-371">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="f98ef-371">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
- <span data-ttu-id="f98ef-372">HDInsight-klustret skapar en **standardbehållare** i den bloblagring som du angav i JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="f98ef-372">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (linkedServiceName).</span></span> <span data-ttu-id="f98ef-373">HDInsight tar inte bort den här behållaren när klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="f98ef-373">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="f98ef-374">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="f98ef-374">This behavior is by design.</span></span> <span data-ttu-id="f98ef-375">Med den länkade tjänsten HDInsight på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="f98ef-375">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="f98ef-376">Klustret tas bort automatiskt när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-376">The cluster is automatically deleted when the processing is done.</span></span>
    
    <span data-ttu-id="f98ef-377">Allteftersom fler sektorer bearbetas kan du se mång behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f98ef-377">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="f98ef-378">Om du inte behöver dem för att felsöka jobb, kan du ta bort dem för att minska lagringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="f98ef-378">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="f98ef-379">Namnen på de här behållarna följer ett mönster: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="f98ef-379">The names of these containers follow a pattern: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span></span> <span data-ttu-id="f98ef-380">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) till att ta bort behållare i din Azure Blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="f98ef-380">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>
- <span data-ttu-id="f98ef-381">För närvarande är det utdatauppsättningen som skapar schemat. Därför måste du skapa en utdatauppsättning även om aktiviteten inte genererar några utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-381">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="f98ef-382">Om aktiviteten inte får några indata, kan du hoppa över att skapa indatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-382">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> 
- <span data-ttu-id="f98ef-383">Den här självstudiekursen visar inte hur du kopiera data med hjälp av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f98ef-383">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="f98ef-384">En självstudiekurs om hur du kopierar data med Azure Data Factory finns i [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Självstudie: Kopiera data från Blob Storage till SQL Database).</span><span class="sxs-lookup"><span data-stu-id="f98ef-384">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>


## <a name="use-server-explorer-to-view-data-factories"></a><span data-ttu-id="f98ef-385">Använda Server Explorer för att visa datafabriker</span><span class="sxs-lookup"><span data-stu-id="f98ef-385">Use Server Explorer to view data factories</span></span>
1. <span data-ttu-id="f98ef-386">I **Visual Studio** klickar du på **Visa** i menyn och därefter på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-386">In **Visual Studio**, click **View** on the menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="f98ef-387">I fönstret Server Explorer expanderar du **Azure** och **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-387">In the Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="f98ef-388">Om du ser **Logga in till Visual Studio** anger du det **konto** som är associerat med din Azure-prenumeration och klickar på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-388">If you see **Sign in to Visual Studio**, enter the **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="f98ef-389">Ange **lösenordet** och klicka på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-389">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="f98ef-390">Visual Studio försöker hämta information om alla Azure-datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f98ef-390">Visual Studio tries to get information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="f98ef-391">Du ser statusen för den här åtgärden i fönstret **Uppgiftslista för Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-391">You see the status of this operation in the **Data Factory Task List** window.</span></span>

    ![Server Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. <span data-ttu-id="f98ef-393">Du kan också högerklicka på en datafabrik och välja **Exportera Data Factory till nytt projekt** om du vill skapa ett Visual Studio-projekt som baseras på en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="f98ef-393">You can right-click a data factory, and select **Export Data Factory to New Project** to create a Visual Studio project based on an existing data factory.</span></span>

    ![Exportera datafabrik](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="f98ef-395">Uppdatera Data Factory-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f98ef-395">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="f98ef-396">Om du vill uppdatera Azure Data Factory-verktyg för Visual Studio går du igenom följande steg:</span><span class="sxs-lookup"><span data-stu-id="f98ef-396">To update Azure Data Factory tools for Visual Studio, do the following steps:</span></span>

1. <span data-ttu-id="f98ef-397">Klicka på **Verktyg** i menyn och välj **Tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-397">Click **Tools** on the menu and select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="f98ef-398">Välj **Uppdateringar** i den vänstra rutan och välj sedan **Visual Studio-galleriet**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-398">Select **Updates** in the left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="f98ef-399">Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-399">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="f98ef-400">Om du inte ser den här posten har du redan den senaste versionen av verktygen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-400">If you do not see this entry, you already have the latest version of the tools.</span></span>

## <a name="use-configuration-files"></a><span data-ttu-id="f98ef-401">Använda konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="f98ef-401">Use configuration files</span></span>
<span data-ttu-id="f98ef-402">Du kan använda konfigurationsfiler i Visual Studio för att konfigurera egenskaper för länkade tjänster/tabeller/pipelines olika för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="f98ef-402">You can use configuration files in Visual Studio to configure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="f98ef-403">Fundera på följande JSON-definition för en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="f98ef-403">Consider the following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="f98ef-404">Att ange **connectionString** med olika värden för accountname och accountkey baseras på vilken miljö (Utveckling/Test/Produktion) som du distribuerar Data Factory-entiteter till.</span><span class="sxs-lookup"><span data-stu-id="f98ef-404">To specify **connectionString** with different values for accountname and accountkey based on the environment (Dev/Test/Production) to which you are deploying Data Factory entities.</span></span> <span data-ttu-id="f98ef-405">Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="f98ef-405">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="f98ef-406">Lägga till en konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="f98ef-406">Add a configuration file</span></span>
<span data-ttu-id="f98ef-407">Lägg till en konfigurationsfil för varje miljö genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="f98ef-407">Add a configuration file for each environment by performing the following steps:</span></span>   

1. <span data-ttu-id="f98ef-408">Högerklicka på Data Factory-projektet i Visual Studio-lösningen, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-408">Right-click the Data Factory project in your Visual Studio solution, point to **Add**, and click **New item**.</span></span>
2. <span data-ttu-id="f98ef-409">Välj **Config** i listan med installerade mallar till vänster, välj **Konfigurationsfil**, ange ett **namn** för konfigurationsfilen och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-409">Select **Config** from the list of installed templates on the left, select **Configuration File**, enter a **name** for the configuration file, and click **Add**.</span></span>

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="f98ef-411">Lägg till konfigurationsparametrar och deras värden i följande format:</span><span class="sxs-lookup"><span data-stu-id="f98ef-411">Add configuration parameters and their values in the following format:</span></span>

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

    <span data-ttu-id="f98ef-412">Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="f98ef-412">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="f98ef-413">Observera att syntaxen för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="f98ef-413">Notice that the syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="f98ef-414">Om JSON har en egenskap med en värdematris enligt följande kod:</span><span class="sxs-lookup"><span data-stu-id="f98ef-414">If JSON has a property that has an array of values as shown in the following code:</span></span>  

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

    <span data-ttu-id="f98ef-415">Konfigurera egenskaper enligt följande konfigurationsfil (använd nollbaserad indexering):</span><span class="sxs-lookup"><span data-stu-id="f98ef-415">Configure properties as shown in the following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="f98ef-416">Egenskapsnamn med blanksteg</span><span class="sxs-lookup"><span data-stu-id="f98ef-416">Property names with spaces</span></span>
<span data-ttu-id="f98ef-417">Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser enligt följande exempel (databasservernamn):</span><span class="sxs-lookup"><span data-stu-id="f98ef-417">If a property name has spaces in it, use square brackets as shown in the following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="f98ef-418">Distribuera lösningen med en konfiguration</span><span class="sxs-lookup"><span data-stu-id="f98ef-418">Deploy solution using a configuration</span></span>
<span data-ttu-id="f98ef-419">När du publicerar Azure Data Factory-entiteter i VS, kan du ange den konfiguration som du vill använda för att publicera åtgärden.</span><span class="sxs-lookup"><span data-stu-id="f98ef-419">When you are publishing Azure Data Factory entities in VS, you can specify the configuration that you want to use for that publishing operation.</span></span>

<span data-ttu-id="f98ef-420">Publicera entiteter i Azure Data Factory-projekt med hjälp av en konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="f98ef-420">To publish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="f98ef-421">Högerklicka på Data Factory-projektet och visa dialogrutan **Publicera objekt** genom att klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-421">Right-click Data Factory project and click **Publish** to see the **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="f98ef-422">Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på sidan **Konfigurera datafabrik**. Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-422">Select an existing data factory or specify values for creating a data factory on the **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="f98ef-423">På sidan **Publicera objekt**: Här visas en listruta med tillgängliga konfigurationer för fältet **Välj distributionskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-423">On the **Publish Items** page: you see a drop-down list with available configurations for the **Select Deployment Config** field.</span></span>

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="f98ef-425">Välj den **konfigurationsfil** som du vill använda och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-425">Select the **configuration file** that you would like to use and click **Next**.</span></span>
5. <span data-ttu-id="f98ef-426">Kontrollera att du ser namnet på JSON-filen på sidan **Sammanfattning** och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-426">Confirm that you see the name of JSON file in the **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="f98ef-427">Klicka på **Slutför** när distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="f98ef-427">Click **Finish** after the deployment operation is finished.</span></span>

<span data-ttu-id="f98ef-428">När du distribuerar används värden från konfigurationsfilen till att ange värden för egenskaper i JSON-filerna innan entiteterna distribueras till Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-428">When you deploy, the values from the configuration file are used to set values for properties in the JSON files before the entities are deployed to Azure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="f98ef-429">Använda Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f98ef-429">Use Azure Key Vault</span></span>
<span data-ttu-id="f98ef-430">Det är inte tillrådligt och ofta mot säkerhetsprincipen införa känsliga data, till exempel anslutningssträngar, i kod-databasen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-430">It is not advisable and often against security policy to commit sensitive data such as connection strings to the code repository.</span></span> <span data-ttu-id="f98ef-431">Se exemplet [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub att lära dig om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory-entiteter.</span><span class="sxs-lookup"><span data-stu-id="f98ef-431">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub to learn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="f98ef-432">Secure Publish-tillägget för Visual Studio gör det möjligt att lagra hemligheter i Key Vault och endast specificera referenser till dessa i konfigurationer för länkade tjänster/distributioner.</span><span class="sxs-lookup"><span data-stu-id="f98ef-432">The Secure Publish extension for Visual Studio allows the secrets to be stored in Key Vault and only references to them are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="f98ef-433">Dessa referenser matchas när du publicerar Data Factory-entiteter i Azure.</span><span class="sxs-lookup"><span data-stu-id="f98ef-433">These references are resolved when you publish Data Factory entities to Azure.</span></span> <span data-ttu-id="f98ef-434">Dessa filer kan sedan skickas till källdatabasen utan visa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="f98ef-434">These files can then be committed to source repository without exposing any secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="f98ef-435">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f98ef-435">Summary</span></span>
<span data-ttu-id="f98ef-436">I den här självstudien skapade du en Azure-datafabrik som bearbetar data genom att köra ett Hive-skript i ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f98ef-436">In this tutorial, you created an Azure data factory to process data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="f98ef-437">Du utförde följande steg med hjälp av Data Factory-redigeraren i Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="f98ef-437">You used the Data Factory Editor in the Azure portal to do the following steps:</span></span>  

1. <span data-ttu-id="f98ef-438">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f98ef-438">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="f98ef-439">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="f98ef-439">Created two **linked services**:</span></span>
   1. <span data-ttu-id="f98ef-440">En länkad **Azure Storage-**tjänst som länkar din Azure Blob-lagring med in-/utdatafiler till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f98ef-440">**Azure Storage** linked service to link your Azure blob storage that holds input/output files to the data factory.</span></span>
   2. <span data-ttu-id="f98ef-441">En länkad **Azure HDInsight**-tjänst på begäran som länkar ett Hadoop-kluster i HDInsight på begäran till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="f98ef-441">**Azure HDInsight** on-demand linked service to link an on-demand HDInsight Hadoop cluster to the data factory.</span></span> <span data-ttu-id="f98ef-442">Azure Data Factory skapar ett Hadoop-kluster i HDInsight i rätt tid för att bearbeta indata och skapa utdata.</span><span class="sxs-lookup"><span data-stu-id="f98ef-442">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time to process input data and produce output data.</span></span>
3. <span data-ttu-id="f98ef-443">Du skapade två **datauppsättningar** som beskriver in- och utdata för Hive-aktiviteten för HDInsight i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-443">Created two **datasets**, which describe input and output data for HDInsight Hive activity in the pipeline.</span></span>
4. <span data-ttu-id="f98ef-444">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f98ef-444">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f98ef-445">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f98ef-445">Next Steps</span></span>
<span data-ttu-id="f98ef-446">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="f98ef-446">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="f98ef-447">Om du vill se hur du använder en kopieringsaktivitet till att kopiera data från en Azure-blobb till Azure SQL kan du läsa mer i [Självstudie: Kopiera data från en Azure-blobb till Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="f98ef-447">To see how to use a Copy Activity to copy data from an Azure Blob to Azure SQL, see [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="f98ef-448">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f98ef-448">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="f98ef-449">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="f98ef-449">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 


## <a name="see-also"></a><span data-ttu-id="f98ef-450">Se även</span><span class="sxs-lookup"><span data-stu-id="f98ef-450">See Also</span></span>
| <span data-ttu-id="f98ef-451">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="f98ef-451">Topic</span></span> | <span data-ttu-id="f98ef-452">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f98ef-452">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="f98ef-453">Pipelines</span><span class="sxs-lookup"><span data-stu-id="f98ef-453">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="f98ef-454">I den här artikeln beskriver vi pipelines och aktiviteter i Azure Data Factory och hur du kan använda dem för att konstruera datadrivna arbetsflöden för ditt scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="f98ef-454">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="f98ef-455">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="f98ef-455">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="f98ef-456">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f98ef-456">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="f98ef-457">Datatransformeringsaktiviteter</span><span class="sxs-lookup"><span data-stu-id="f98ef-457">Data Transformation Activities</span></span>](data-factory-data-transformation-activities.md) |<span data-ttu-id="f98ef-458">Den här artikeln innehåller en lista med de datatransformeringsaktiviteter (till exempel HDInsight Hive-transformeringen som du använde i självstudien) som stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f98ef-458">This article provides a list of data transformation activities (such as HDInsight Hive transformation you used in this tutorial) supported by Azure Data Factory.</span></span> |
| [<span data-ttu-id="f98ef-459">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="f98ef-459">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="f98ef-460">I den här artikeln beskrivs aspekter för schemaläggning och körning av Azure Data Factory-programmodellen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-460">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="f98ef-461">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="f98ef-461">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="f98ef-462">Den här artikeln beskriver hur du övervakar, hanterar och felsöker pipelines med övervaknings- och hanteringsappen.</span><span class="sxs-lookup"><span data-stu-id="f98ef-462">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |
