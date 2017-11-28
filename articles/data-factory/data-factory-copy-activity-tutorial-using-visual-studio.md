---
title: "Självstudier: Skapa en pipeline med en kopieringsaktivitet med hjälp av Visual Studio | Microsoft Docs"
description: "I de här självstudierna skapar du en Azure Data Factory-pipeline med en kopieringsaktivitet genom att använda Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a><span data-ttu-id="7dc94-103">Självstudie: Skapa en pipeline med en kopieringsaktivitet med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc94-103">Tutorial: Create a pipeline with Copy Activity using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7dc94-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="7dc94-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="7dc94-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="7dc94-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="7dc94-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7dc94-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="7dc94-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc94-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="7dc94-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7dc94-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="7dc94-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="7dc94-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="7dc94-110">REST API</span><span class="sxs-lookup"><span data-stu-id="7dc94-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="7dc94-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="7dc94-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="7dc94-112">I den här artikeln får du lära dig hur hello toouse Microsoft Visual Studio toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-112">In this article, you learn how toouse hello Microsoft Visual Studio toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="7dc94-113">Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="7dc94-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="7dc94-115">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="7dc94-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="7dc94-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="7dc94-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="7dc94-117">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="7dc94-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="7dc94-118">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="7dc94-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="7dc94-120">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="7dc94-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="7dc94-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE] 
> <span data-ttu-id="7dc94-122">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="7dc94-122">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="7dc94-123">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-123">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dc94-124">Krav</span><span class="sxs-lookup"><span data-stu-id="7dc94-124">Prerequisites</span></span>
1. <span data-ttu-id="7dc94-125">Läs igenom [kursen översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="7dc94-125">Read through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article and complete hello **prerequisite** steps.</span></span>       
2. <span data-ttu-id="7dc94-126">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="7dc94-126">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
3. <span data-ttu-id="7dc94-127">Du måste ha hello följande installerat på datorn:</span><span class="sxs-lookup"><span data-stu-id="7dc94-127">You must have hello following installed on your computer:</span></span> 
   * <span data-ttu-id="7dc94-128">Visual Studio 2013 eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="7dc94-128">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="7dc94-129">Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="7dc94-129">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="7dc94-130">Navigera för[Azure-hämtningssida](https://azure.microsoft.com/downloads/) och på **VS 2013** eller **VS 2015** i hello **.NET** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7dc94-130">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="7dc94-131">Hämta hello senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="7dc94-131">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="7dc94-132">Du kan också uppdatera hello plugin-programmet genom att göra följande hello: på menyn hello **verktyg** -> **tillägg och uppdateringar** -> **Online**  ->  **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-132">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

## <a name="steps"></a><span data-ttu-id="7dc94-133">Steg</span><span class="sxs-lookup"><span data-stu-id="7dc94-133">Steps</span></span>
<span data-ttu-id="7dc94-134">Här är hello steg du utför som en del av den här kursen:</span><span class="sxs-lookup"><span data-stu-id="7dc94-134">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="7dc94-135">Skapa **länkade tjänster** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-135">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="7dc94-136">I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-136">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="7dc94-137">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-137">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="7dc94-138">Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-138">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="7dc94-139">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-139">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="7dc94-140">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-140">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="7dc94-141">Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-141">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>     
2. <span data-ttu-id="7dc94-142">Skapa ingående och utgående **datauppsättningar** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-142">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="7dc94-143">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7dc94-143">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="7dc94-144">Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="7dc94-144">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="7dc94-145">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-145">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="7dc94-146">Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.</span><span class="sxs-lookup"><span data-stu-id="7dc94-146">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
3. <span data-ttu-id="7dc94-147">Skapa en **pipeline** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-147">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="7dc94-148">I det här steget kan du skapa en pipeline med en kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-148">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="7dc94-149">Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-149">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="7dc94-150">Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-150">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="7dc94-151">I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds.</span><span class="sxs-lookup"><span data-stu-id="7dc94-151">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
4. <span data-ttu-id="7dc94-152">Skapa en Azure-**datafabrik** när du distribuerar Data Factory-enheter (länkade tjänster, datauppsättningar och tabeller och pipelines).</span><span class="sxs-lookup"><span data-stu-id="7dc94-152">Create an Azure **data factory** when deploying Data Factory entities (linked services, datasets/tables, and pipelines).</span></span> 

## <a name="create-visual-studio-project"></a><span data-ttu-id="7dc94-153">Skapa Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="7dc94-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="7dc94-154">Starta **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-154">Launch **Visual Studio 2015**.</span></span> <span data-ttu-id="7dc94-155">Klicka på **filen**, peka för**ny**, och klicka på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="7dc94-156">Du bör se hello **nytt projekt** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dc94-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="7dc94-157">I hello **nytt projekt** dialogrutan, Välj hello **DataFactory** mall och klicka på **tomt Data Factory projekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>  
   
    ![Dialogrutan Nytt projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. <span data-ttu-id="7dc94-159">Ange hello av hello projekt, platsen för hello lösningen och namn på hello lösningen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-159">Specify hello name of hello project, location for hello solution, and name of hello solution, and then click **OK**.</span></span>
   
    ![Solution Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a><span data-ttu-id="7dc94-161">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="7dc94-161">Create linked services</span></span>
<span data-ttu-id="7dc94-162">Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-162">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="7dc94-163">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="7dc94-163">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="7dc94-164">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="7dc94-164">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="7dc94-165">Därför kan du skapa två länkade tjänster: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="7dc94-165">Therefore, you create two linked services of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="7dc94-166">hello Azure Storage länkade tjänsten länkar till din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-166">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="7dc94-167">Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-167">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="7dc94-168">Azure SQL länkade tjänsten länkar till din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-168">Azure SQL linked service links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="7dc94-169">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-169">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="7dc94-170">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-170">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="7dc94-171">Länkade tjänster länka datalager eller compute services tooan Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-171">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="7dc94-172">Se [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) för alla hello källor och sänkor som stöds av hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7dc94-172">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="7dc94-173">Se [compute länkade tjänster](data-factory-compute-linked-services.md) hello lista över compute-tjänster som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-173">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span> <span data-ttu-id="7dc94-174">I den här självstudiekursen använder du ingen tjänst för beräkning.</span><span class="sxs-lookup"><span data-stu-id="7dc94-174">In this tutorial, you do not use any compute service.</span></span> 

### <a name="create-hello-azure-storage-linked-service"></a><span data-ttu-id="7dc94-175">Skapa hello länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="7dc94-175">Create hello Azure Storage linked service</span></span>
1. <span data-ttu-id="7dc94-176">I **Solution Explorer**, högerklicka på **länkade tjänster**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-176">In **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="7dc94-177">I hello **Lägg till nytt objekt** dialogrutan **länkade Azure-lagringstjänsten** hello listan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-177">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span> 
   
    ![Ny länkad tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. <span data-ttu-id="7dc94-179">Ersätt `<accountname>` och `<accountkey>`* med hello namnet på ditt Azure storage-konto och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="7dc94-179">Replace `<accountname>` and `<accountkey>`* with hello name of your Azure storage account and its key.</span></span> 
   
    ![Länkad Azure Storage-tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. <span data-ttu-id="7dc94-181">Spara hello **AzureStorageLinkedService1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="7dc94-181">Save hello **AzureStorageLinkedService1.json** file.</span></span>

    <span data-ttu-id="7dc94-182">Läs mer om JSON-egenskaper i länkade hello tjänstdefinitionen [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="7dc94-182">For more information about JSON properties in hello linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-hello-azure-sql-linked-service"></a><span data-ttu-id="7dc94-183">Skapa hello Azure SQL-länkade tjänst</span><span class="sxs-lookup"><span data-stu-id="7dc94-183">Create hello Azure SQL linked service</span></span>
1. <span data-ttu-id="7dc94-184">Högerklicka på **länkade tjänster** nod i hello **Solution Explorer** igen och peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-184">Right-click on **Linked Services** node in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span> 
2. <span data-ttu-id="7dc94-185">Den här gången väljer du **Länkad Azure SQL-tjänst** och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-185">This time, select **Azure SQL Linked Service**, and click **Add**.</span></span> 
3. <span data-ttu-id="7dc94-186">I hello **AzureSqlLinkedService1.json filen**, Ersätt `<servername>`, `<databasename>`, `<username@servername>`, och `<password>` med namn på din Azure SQL server, databas, användarkonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7dc94-186">In hello **AzureSqlLinkedService1.json file**, replace `<servername>`, `<databasename>`, `<username@servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span>    
4. <span data-ttu-id="7dc94-187">Spara hello **AzureSqlLinkedService1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="7dc94-187">Save hello **AzureSqlLinkedService1.json** file.</span></span> 
    
    <span data-ttu-id="7dc94-188">Mer information om de här JSON-egenskaperna finns i [Anslutningsapp för Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="7dc94-188">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>


## <a name="create-datasets"></a><span data-ttu-id="7dc94-189">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="7dc94-189">Create datasets</span></span>
<span data-ttu-id="7dc94-190">I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-190">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="7dc94-191">I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService1 och AzureSqlLinkedService1 respektive.</span><span class="sxs-lookup"><span data-stu-id="7dc94-191">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService1 and AzureSqlLinkedService1 respectively.</span></span>

<span data-ttu-id="7dc94-192">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7dc94-192">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="7dc94-193">Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="7dc94-193">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="7dc94-194">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-194">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="7dc94-195">Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="7dc94-195">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="7dc94-196">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="7dc94-196">Create input dataset</span></span>
<span data-ttu-id="7dc94-197">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService1 länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7dc94-197">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService1 linked service.</span></span> <span data-ttu-id="7dc94-198">Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="7dc94-198">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="7dc94-199">I kursen får ange du ett värde för hello filnamnet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-199">In this tutorial, you specify a value for hello fileName.</span></span> 

<span data-ttu-id="7dc94-200">Här kan använda du hello termen ”tabeller” i stället för ”datauppsättningar”.</span><span class="sxs-lookup"><span data-stu-id="7dc94-200">Here, you use hello term "tables" rather than "datasets".</span></span> <span data-ttu-id="7dc94-201">En tabell är en rektangulär datamängd och hello enda typen av datauppsättning stöds just nu.</span><span class="sxs-lookup"><span data-stu-id="7dc94-201">A table is a rectangular dataset and is hello only type of dataset supported right now.</span></span> 

1. <span data-ttu-id="7dc94-202">Högerklicka på **tabeller** i hello **Solution Explorer**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-202">Right-click **Tables** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="7dc94-203">I hello **Lägg till nytt objekt** dialogrutan **Azure Blob**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-203">In hello **Add New Item** dialog box, select **Azure Blob**, and click **Add**.</span></span>   
3. <span data-ttu-id="7dc94-204">Ersätt hello JSON-text med hello följande text och spara hello **AzureBlobLocation1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="7dc94-204">Replace hello JSON text with hello following text and save hello **AzureBlobLocation1.json** file.</span></span> 

  ```json   
  {
    "name": "InputDataset",
    "properties": {
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
      "type": "AzureBlob",
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
        "format": {
          "type": "TextFormat",
          "columnDelimiter": ","
        }
      },
      "external": true,
      "availability": {
        "frequency": "Hour",
        "interval": 1
      }
    }
  }
  ``` 
    <span data-ttu-id="7dc94-205">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="7dc94-205">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="7dc94-206">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7dc94-206">Property</span></span> | <span data-ttu-id="7dc94-207">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7dc94-207">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="7dc94-208">typ</span><span class="sxs-lookup"><span data-stu-id="7dc94-208">type</span></span> | <span data-ttu-id="7dc94-209">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="7dc94-209">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="7dc94-210">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="7dc94-210">linkedServiceName</span></span> | <span data-ttu-id="7dc94-211">Refererar toohello **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7dc94-211">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="7dc94-212">folderPath</span><span class="sxs-lookup"><span data-stu-id="7dc94-212">folderPath</span></span> | <span data-ttu-id="7dc94-213">Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar.</span><span class="sxs-lookup"><span data-stu-id="7dc94-213">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="7dc94-214">I den här självstudiekursen adftutorial är hello blob-behållaren och mappen är hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-214">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="7dc94-215">fileName</span><span class="sxs-lookup"><span data-stu-id="7dc94-215">fileName</span></span> | <span data-ttu-id="7dc94-216">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="7dc94-216">This property is optional.</span></span> <span data-ttu-id="7dc94-217">Om du utesluter den här egenskapen har alla filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="7dc94-217">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="7dc94-218">I den här självstudiekursen **emp.txt** har angetts för hello filnamn, så att endast filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7dc94-218">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="7dc94-219">format -> typ</span><span class="sxs-lookup"><span data-stu-id="7dc94-219">format -> type</span></span> |<span data-ttu-id="7dc94-220">hello indatafilen är i hello-format, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-220">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="7dc94-221">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="7dc94-221">columnDelimiter</span></span> | <span data-ttu-id="7dc94-222">hello kolumner i hello indatafilen avgränsas med **kommatecken tecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-222">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="7dc94-223">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="7dc94-223">frequency/interval</span></span> | <span data-ttu-id="7dc94-224">hello frekvens har angetts för**timme** och intervallet anges för**1**, vilket innebär att hello indata segment är tillgängliga **varje timme**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-224">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="7dc94-225">Med andra ord hello Data Factory-tjänsten söker efter indata varje timme i hello rotmapp blob-behållaren (**adftutorial**) du har angett.</span><span class="sxs-lookup"><span data-stu-id="7dc94-225">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="7dc94-226">Det ser ut för hello data inom hello pipeline start- och tider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="7dc94-226">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="7dc94-227">extern</span><span class="sxs-lookup"><span data-stu-id="7dc94-227">external</span></span> | <span data-ttu-id="7dc94-228">Den här egenskapen anges för**SANT** om hello data inte genereras av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="7dc94-228">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="7dc94-229">hello inkommande data i den här självstudiekursen har hello emp.txt fil, som genereras av denna pipeline, så vi ställa in den här egenskapen tootrue.</span><span class="sxs-lookup"><span data-stu-id="7dc94-229">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="7dc94-230">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7dc94-230">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>   

### <a name="create-output-dataset"></a><span data-ttu-id="7dc94-231">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="7dc94-231">Create output dataset</span></span>
<span data-ttu-id="7dc94-232">I det här steget ska du skapa en utdatauppsättning med namnet **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-232">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="7dc94-233">Denna dataset pekar tooa SQL-tabellen i hello Azure SQL-databas som representeras av **AzureSqlLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-233">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService1**.</span></span> 

1. <span data-ttu-id="7dc94-234">Högerklicka på **tabeller** i hello **Solution Explorer** igen och peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-234">Right-click **Tables** in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="7dc94-235">I hello **Lägg till nytt objekt** dialogrutan **Azure SQL**, och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-235">In hello **Add New Item** dialog box, select **Azure SQL**, and click **Add**.</span></span> 
3. <span data-ttu-id="7dc94-236">Ersätt hello JSON-text med hello följande JSON och spara hello **AzureSqlTableLocation1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="7dc94-236">Replace hello JSON text with hello following JSON and save hello **AzureSqlTableLocation1.json** file.</span></span>

  ```json
    {
     "name": "OutputDataset",
     "properties": {
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
       "type": "AzureSqlTable",
       "linkedServiceName": "AzureSqlLinkedService1",
       "typeProperties": {
         "tableName": "emp"
       },
       "availability": {
         "frequency": "Hour",
         "interval": 1
       }
     }
    }
    ```
    <span data-ttu-id="7dc94-237">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="7dc94-237">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="7dc94-238">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7dc94-238">Property</span></span> | <span data-ttu-id="7dc94-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7dc94-239">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="7dc94-240">typ</span><span class="sxs-lookup"><span data-stu-id="7dc94-240">type</span></span> | <span data-ttu-id="7dc94-241">hello typegenskapen har ställts in för**AzureSqlTable** eftersom data är kopierade tooa tabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-241">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="7dc94-242">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="7dc94-242">linkedServiceName</span></span> | <span data-ttu-id="7dc94-243">Refererar toohello **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7dc94-243">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="7dc94-244">tableName</span><span class="sxs-lookup"><span data-stu-id="7dc94-244">tableName</span></span> | <span data-ttu-id="7dc94-245">Angivna hello **tabell** toowhich hello data kopieras.</span><span class="sxs-lookup"><span data-stu-id="7dc94-245">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="7dc94-246">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="7dc94-246">frequency/interval</span></span> | <span data-ttu-id="7dc94-247">hello frekvens har angetts för**timme** och är **1**, vilket innebär att hello utdata segment produceras **varje timme** mellan hello pipeline start och slut tider inte före eller När dessa tider.</span><span class="sxs-lookup"><span data-stu-id="7dc94-247">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="7dc94-248">Det finns tre kolumner – **ID**, **Förnamn**, och **efternamn** – i hello tomma tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-248">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="7dc94-249">ID: T är en identitetskolumn, så du behöver bara toospecify **Förnamn** och **efternamn** här.</span><span class="sxs-lookup"><span data-stu-id="7dc94-249">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="7dc94-250">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="7dc94-250">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

## <a name="create-pipeline"></a><span data-ttu-id="7dc94-251">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="7dc94-251">Create pipeline</span></span>
<span data-ttu-id="7dc94-252">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="7dc94-252">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="7dc94-253">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="7dc94-253">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="7dc94-254">I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-254">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="7dc94-255">hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="7dc94-255">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="7dc94-256">Därför produceras 24 segment för utdatauppsättningen av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="7dc94-256">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="7dc94-257">Högerklicka på **Pipelines** i hello **Solution Explorer**, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-257">Right-click **Pipelines** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>  
2. <span data-ttu-id="7dc94-258">Välj **kopiera Data Pipeline** i hello **Lägg till nytt objekt** dialogrutan och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-258">Select **Copy Data Pipeline** in hello **Add New Item** dialog box and click **Add**.</span></span> 
3. <span data-ttu-id="7dc94-259">Ersätt hello JSON med hello följande JSON och spara hello **CopyActivity1.json** fil.</span><span class="sxs-lookup"><span data-stu-id="7dc94-259">Replace hello JSON with hello following JSON and save hello **CopyActivity1.json** file.</span></span>

  ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob tooAzure SQL table",
       "activities": [
         {
           "name": "CopyFromBlobToSQL",
           "type": "Copy",
           "inputs": [
             {
               "name": "InputDataset"
             }
           ],
           "outputs": [
             {
               "name": "OutputDataset"
             }
           ],
           "typeProperties": {
             "source": {
               "type": "BlobSource"
             },
             "sink": {
               "type": "SqlSink",
               "writeBatchSize": 10000,
               "writeBatchTimeout": "60:00:00"
             }
           },
           "Policy": {
             "concurrency": 1,
             "executionPriorityOrder": "NewestFirst",
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - <span data-ttu-id="7dc94-260">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-260">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="7dc94-261">Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-261">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="7dc94-262">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-262">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="7dc94-263">Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-263">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="7dc94-264">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-264">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="7dc94-265">En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="7dc94-265">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="7dc94-266">toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="7dc94-266">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="7dc94-267">Ersätt hello värdet för hello **starta** egenskap med hello aktuell dag och **end** värdet med hello nästa dag.</span><span class="sxs-lookup"><span data-stu-id="7dc94-267">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="7dc94-268">Du kan ange endast hello DatumDel och hoppa över hello time-delen av hello datum och tid.</span><span class="sxs-lookup"><span data-stu-id="7dc94-268">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="7dc94-269">Till exempel ”2016-02-03”, vilket motsvarar för ”2016-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="7dc94-269">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="7dc94-270">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="7dc94-270">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="7dc94-271">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="7dc94-271">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="7dc94-272">Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-272">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="7dc94-273">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="7dc94-273">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="7dc94-274">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-274">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="7dc94-275">I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.</span><span class="sxs-lookup"><span data-stu-id="7dc94-275">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="7dc94-276">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-276">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="7dc94-277">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-277">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="7dc94-278">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-278">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="7dc94-279">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-279">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="7dc94-280">Publicera/distribuera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="7dc94-280">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="7dc94-281">I det här steget publicerar du Data Factory-enheter (länkade tjänster, datauppsättningar och pipeline) som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7dc94-281">In this step, you publish Data Factory entities (linked services, datasets, and pipeline) you created earlier.</span></span> <span data-ttu-id="7dc94-282">Du kan också ange hello namn för hello nya data factory toobe skapade toohold dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-282">You also specify hello name of hello new data factory toobe created toohold these entities.</span></span>  

1. <span data-ttu-id="7dc94-283">Högerklicka på projektet i hello Solution Explorer och klicka på **publicera**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-283">Right-click project in hello Solution Explorer, and click **Publish**.</span></span> 
2. <span data-ttu-id="7dc94-284">Om du ser **logga in tooyour Microsoft-konto** dialogrutan, ange dina autentiseringsuppgifter för hello-konto med Azure-prenumeration och på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-284">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="7dc94-285">Du bör se följande dialogrutan hello:</span><span class="sxs-lookup"><span data-stu-id="7dc94-285">You should see hello following dialog box:</span></span>
   
   ![Dialogrutan Publicera](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. <span data-ttu-id="7dc94-287">Hello konfigurera data factory på sidan hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dc94-287">In hello Configure data factory page, do hello following steps:</span></span> 
   
   1. <span data-ttu-id="7dc94-288">välj alternativet **Skapa ny Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-288">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="7dc94-289">Ange **VSTutorialFactory** som **namn**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-289">Enter **VSTutorialFactory** for **Name**.</span></span>  
      
      > [!IMPORTANT]
      > <span data-ttu-id="7dc94-290">hello namn i hello Azure data factory måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="7dc94-290">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="7dc94-291">Om du får ett felmeddelande om hello namnet på data factory vid publicering, ändra hello namn hello data factory (till exempel yournameVSTutorialFactory) och försök att publicera igen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-291">If you receive an error about hello name of data factory when publishing, change hello name of hello data factory (for example, yournameVSTutorialFactory) and try publishing again.</span></span> <span data-ttu-id="7dc94-292">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-292">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>        
      > 
      > 
   3. <span data-ttu-id="7dc94-293">Välj din Azure-prenumeration för hello **prenumeration** fältet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-293">Select your Azure subscription for hello **Subscription** field.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="7dc94-294">Om du inte ser någon prenumeration, kontrollera att du har loggat in med ett konto som är en administratör eller medadministratör för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7dc94-294">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>  
      > 
      > 
   4. <span data-ttu-id="7dc94-295">Välj hello **resursgruppen** för hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-295">Select hello **resource group** for hello data factory toobe created.</span></span> 
   5. <span data-ttu-id="7dc94-296">Välj hello **region** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-296">Select hello **region** for hello data factory.</span></span> <span data-ttu-id="7dc94-297">Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="7dc94-297">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
   6. <span data-ttu-id="7dc94-298">Klicka på **nästa** tooswitch toohello **publicera objekt** sidan.</span><span class="sxs-lookup"><span data-stu-id="7dc94-298">Click **Next** tooswitch toohello **Publish Items** page.</span></span>
      
       ![Konfigurera Data Factory-sida](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. <span data-ttu-id="7dc94-300">I hello **publicera objekt** , se till att alla hello Datafabriker entiteter är markerade och klickar på **nästa** tooswitch toohello **sammanfattning** sidan.</span><span class="sxs-lookup"><span data-stu-id="7dc94-300">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>
   
   ![Sidan Publish items (Publicera objekt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. <span data-ttu-id="7dc94-302">Granska hello sammanfattning och klicka på **nästa** toostart hello distribution processen och visa hello **Distributionsstatus**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-302">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>
   
   ![Sidan Publish summary (Publicera översikt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. <span data-ttu-id="7dc94-304">I hello **Distributionsstatus** sida, bör du se hello status hello distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-304">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="7dc94-305">Klicka på Slutför när hello distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="7dc94-305">Click Finish after hello deployment is done.</span></span>
 
   ![Statussida för distribution](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

<span data-ttu-id="7dc94-307">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="7dc94-307">Note hello following points:</span></span> 

* <span data-ttu-id="7dc94-308">Om felmeddelandet hello: ”den här prenumerationen är inte registrerad toouse namnområde Microsoft.DataFactory”, gör du något av följande hello och försök att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="7dc94-308">If you receive hello error: "This subscription is not registered toouse namespace Microsoft.DataFactory", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="7dc94-309">Kör hello efter kommandot tooregister hello Data Factory-providern i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7dc94-309">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="7dc94-310">Du kan köra följande kommando tooconfirm hello som hello Data Factory providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="7dc94-310">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="7dc94-311">Logga in med hjälp av hello Azure-prenumeration i hello [Azure-portalen](https://portal.azure.com) och navigera tooa Data Factory bladet (eller) skapa en datafabrik i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-311">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="7dc94-312">Den här åtgärden registrerar automatiskt hello-providern för dig.</span><span class="sxs-lookup"><span data-stu-id="7dc94-312">This action automatically registers hello provider for you.</span></span>
* <span data-ttu-id="7dc94-313">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="7dc94-313">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7dc94-314">toocreate Data Factory instanser måste toobe en admin/medadministratör av hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7dc94-314">toocreate Data Factory instances, you need toobe a admin/co-admin of hello Azure subscription</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="7dc94-315">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="7dc94-315">Monitor pipeline</span></span>
<span data-ttu-id="7dc94-316">Navigera toohello startsidan för din data factory:</span><span class="sxs-lookup"><span data-stu-id="7dc94-316">Navigate toohello home page for your data factory:</span></span>

1. <span data-ttu-id="7dc94-317">Logga in för[Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7dc94-317">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7dc94-318">Klicka på **fler tjänster** på hello vänstra menyn och klickar på **datafabriker**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-318">Click **More services** on hello left menu, and click **Data factories**.</span></span>

    ![Bläddra igenom datafabrikerna](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. <span data-ttu-id="7dc94-320">Börja skriva hello namnet på din data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-320">Start typing hello name of your data factory.</span></span>

    ![Namnet på datafabriken](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. <span data-ttu-id="7dc94-322">Klicka på din data factory i hello resultat listan toosee hello startsidan för din data factory.</span><span class="sxs-lookup"><span data-stu-id="7dc94-322">Click your data factory in hello results list toosee hello home page for your data factory.</span></span>

    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. <span data-ttu-id="7dc94-324">Följ anvisningarna från [övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-324">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="7dc94-325">Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines.</span><span class="sxs-lookup"><span data-stu-id="7dc94-325">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span> 

## <a name="summary"></a><span data-ttu-id="7dc94-326">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7dc94-326">Summary</span></span>
<span data-ttu-id="7dc94-327">I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7dc94-327">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="7dc94-328">Du har använt Visual Studio toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="7dc94-328">You used Visual Studio toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="7dc94-329">Här följer hello anvisningar som du utförde i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="7dc94-329">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="7dc94-330">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-330">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="7dc94-331">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="7dc94-331">Created **linked services**:</span></span>
   1. <span data-ttu-id="7dc94-332">En **Azure Storage** länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.</span><span class="sxs-lookup"><span data-stu-id="7dc94-332">An **Azure Storage** linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="7dc94-333">En **Azure SQL** länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="7dc94-333">An **Azure SQL** linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="7dc94-334">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="7dc94-334">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="7dc94-335">Du skapade en **pipeline** med en **kopieringsaktivitet** med **BlobSource** som källa och **SqlSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="7dc94-335">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span> 

<span data-ttu-id="7dc94-336">hur toouse en HDInsight Hive tootransform aktivitetsdata med hjälp av Azure HDInsight-kluster, se toosee [ Självstudier: skapa din första pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="7dc94-336">toosee how toouse a HDInsight Hive Activity tootransform data by using Azure HDInsight cluster, see [ Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="7dc94-337">Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-337">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="7dc94-338">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="7dc94-338">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 

## <a name="view-all-data-factories-in-server-explorer"></a><span data-ttu-id="7dc94-339">Visa datafabriker i Server Explorer</span><span class="sxs-lookup"><span data-stu-id="7dc94-339">View all data factories in Server Explorer</span></span>
<span data-ttu-id="7dc94-340">Det här avsnittet beskrivs hur toouse hello Server Explorer i Visual Studio tooview alla hello datafabriker i din Azure-prenumeration och skapa ett Visual Studio-projekt utifrån en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="7dc94-340">This section describes how toouse hello Server Explorer in Visual Studio tooview all hello data factories in your Azure subscription and create a Visual Studio project based on an existing data factory.</span></span> 

1. <span data-ttu-id="7dc94-341">I **Visual Studio**, klickar du på **visa** på hello menyn och klickar på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-341">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="7dc94-342">I hello Server Explorer, expandera **Azure** och expandera **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-342">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="7dc94-343">Om du ser **logga in tooVisual Studio**, ange hello **konto** som är associerade med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-343">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="7dc94-344">Ange **lösenordet** och klicka på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-344">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="7dc94-345">Visual Studio försöker tooget information om alla Azure datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7dc94-345">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="7dc94-346">Du ser hello status för den här åtgärden i hello **Data Factory uppgiftslista** fönster.</span><span class="sxs-lookup"><span data-stu-id="7dc94-346">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a><span data-ttu-id="7dc94-348">Skapa ett Visual Studio-projekt för en befintlig datafabrik</span><span class="sxs-lookup"><span data-stu-id="7dc94-348">Create a Visual Studio project for an existing data factory</span></span>

- <span data-ttu-id="7dc94-349">Högerklicka på en datafabrik i Server Explorer och välj **exportera Data Factory tooNew projekt** toocreate Visual Studio-projekt utifrån en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="7dc94-349">Right-click a data factory in Server Explorer, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Exportera data factory tooa VS-projektet](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="7dc94-351">Uppdatera Data Factory-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc94-351">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="7dc94-352">tooupdate Azure Data Factory tools för Visual Studio hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dc94-352">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="7dc94-353">Klicka på **verktyg** på hello-menyn och välj **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-353">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span> 
2. <span data-ttu-id="7dc94-354">Välj **uppdateringar** i hello till vänster och välj sedan **Visual Studio-galleriet**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-354">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="7dc94-355">Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-355">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="7dc94-356">Om du inte ser den här posten har du redan hello senaste versionen av hello verktyg.</span><span class="sxs-lookup"><span data-stu-id="7dc94-356">If you do not see this entry, you already have hello latest version of hello tools.</span></span> 

## <a name="use-configuration-files"></a><span data-ttu-id="7dc94-357">Använda konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="7dc94-357">Use configuration files</span></span>
<span data-ttu-id="7dc94-358">Du kan använda konfigurationsfiler i Visual Studio tooconfigure egenskaper för länkade tjänster/tabeller/pipelines på olika sätt för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="7dc94-358">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="7dc94-359">Överväg att hello JSON-definitionen för en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="7dc94-359">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="7dc94-360">toospecify **connectionString** med olika värden för accountname eller accountkey baserat på hello miljö (Dev/Test/produktion) toowhich du distribuerar Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-360">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="7dc94-361">Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="7dc94-361">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="7dc94-362">Lägga till en konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="7dc94-362">Add a configuration file</span></span>
<span data-ttu-id="7dc94-363">Lägg till en konfigurationsfil för varje miljö genom att utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dc94-363">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="7dc94-364">Högerklicka på hello Data Factory-projekt i Visual Studio-lösning, peka för**Lägg till**, och klicka på **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-364">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="7dc94-365">Välj **Config** hello listan över installerade mallar hello vänster och välj **konfigurationsfilen**, ange en **namn** för konfiguration av hello fil och klicka på **Lägga till**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-365">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="7dc94-367">Lägg till konfigurationsparametrar och deras värden i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="7dc94-367">Add configuration parameters and their values in hello following format:</span></span>

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

    <span data-ttu-id="7dc94-368">Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="7dc94-368">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="7dc94-369">Observera att hello syntax för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="7dc94-369">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="7dc94-370">Om JSON har en egenskap som innehåller en matris med värden som visas i följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="7dc94-370">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

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

    <span data-ttu-id="7dc94-371">Konfigurera egenskaperna som visas i hello följande konfigurationsfilen (Använd Nollbaserad indexering):</span><span class="sxs-lookup"><span data-stu-id="7dc94-371">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="7dc94-372">Egenskapsnamn med blanksteg</span><span class="sxs-lookup"><span data-stu-id="7dc94-372">Property names with spaces</span></span>
<span data-ttu-id="7dc94-373">Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser som visas i följande exempel (Databasservernamnet) hello:</span><span class="sxs-lookup"><span data-stu-id="7dc94-373">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="7dc94-374">Distribuera lösningen med en konfiguration</span><span class="sxs-lookup"><span data-stu-id="7dc94-374">Deploy solution using a configuration</span></span>
<span data-ttu-id="7dc94-375">När du publicerar Azure Data Factory-entiteter i VS anger du hello-konfiguration som du vill toouse för publishing operationen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-375">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="7dc94-376">toopublish entiteter i ett Azure Data Factory-projekt med hjälp av konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="7dc94-376">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="7dc94-377">Högerklicka på Data Factory-projektet och klicka på **publicera** toosee hello **publicera objekt** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dc94-377">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="7dc94-378">Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på hello **konfigurera datafabriken** och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-378">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="7dc94-379">På hello **publicera objekt** sida: du kan se en lista med tillgängliga konfigurationer för hello **Välj distributionskonfiguration** fältet.</span><span class="sxs-lookup"><span data-stu-id="7dc94-379">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="7dc94-381">Välj hello **konfigurationsfilen** som du skulle t.ex. toouse och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-381">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="7dc94-382">Bekräfta att du ser hello namnet på JSON-fil i hello **sammanfattning** och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="7dc94-382">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="7dc94-383">Klicka på **Slutför** när hello Distributionsåtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7dc94-383">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="7dc94-384">När du distribuerar är hello värden från konfigurationsfilen hello används tooset värden för egenskaper i hello JSON-filer innan hello entiteter som är distribuerade tooAzure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7dc94-384">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="7dc94-385">Använda Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="7dc94-385">Use Azure Key Vault</span></span>
<span data-ttu-id="7dc94-386">Det är inte tillrådligt och ofta mot säkerhet princip toocommit känsliga data, till exempel anslutning strängar toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="7dc94-386">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="7dc94-387">Se [ADF Secure publicera](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub toolearn om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-387">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="7dc94-388">hello Secure publicera tillägget för Visual Studio kan hello hemligheter toobe lagras i Nyckelvalvet och endast referenser toothem har angetts i länkade tjänster / distributionskonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="7dc94-388">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="7dc94-389">När du publicerar Data Factory entiteter tooAzure matchas dessa referenser.</span><span class="sxs-lookup"><span data-stu-id="7dc94-389">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="7dc94-390">De här filerna kan sedan vara allokerat toosource databasen utan att exponera alla hemligheter.</span><span class="sxs-lookup"><span data-stu-id="7dc94-390">These files can then be committed toosource repository without exposing any secrets.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7dc94-391">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7dc94-391">Next steps</span></span>
<span data-ttu-id="7dc94-392">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="7dc94-392">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="7dc94-393">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="7dc94-393">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="7dc94-394">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="7dc94-394">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
