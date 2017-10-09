---
title: "Självstudier: Skapa ett Azure Data Factory pipeline toocopy data (Azure portal) | Microsoft Docs"
description: "I den här kursen använder du Azure portal toocreate ett Azure Data Factory-pipelinen med en Kopieringsaktiviteten toocopy data från Azure blob storage tooan Azure SQL-databas."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fadd840fe9a15cd8831cdb25dccbd10ac42fa161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-portal-toocreate-a-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="573c8-103">Självstudier: Använda Azure portal toocreate Data Factory pipeline toocopy data</span><span class="sxs-lookup"><span data-stu-id="573c8-103">Tutorial: Use Azure portal toocreate a Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="573c8-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="573c8-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="573c8-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="573c8-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="573c8-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="573c8-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="573c8-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="573c8-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="573c8-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="573c8-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="573c8-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="573c8-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="573c8-110">REST API</span><span class="sxs-lookup"><span data-stu-id="573c8-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="573c8-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="573c8-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="573c8-112">I den här artikeln får du lära dig hur toouse [Azure-portalen](https://portal.azure.com) toocreate en datafabrik med en rörledning som kopierar data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-112">In this article, you learn how toouse [Azure portal](https://portal.azure.com) toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="573c8-113">Om du är ny tooAzure Data Factory, Läs igenom hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="573c8-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="573c8-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="573c8-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="573c8-115">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="573c8-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="573c8-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="573c8-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="573c8-117">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="573c8-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="573c8-118">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="573c8-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="573c8-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="573c8-120">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="573c8-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="573c8-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="573c8-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="573c8-122">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="573c8-122">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="573c8-123">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-123">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="573c8-124">Krav</span><span class="sxs-lookup"><span data-stu-id="573c8-124">Prerequisites</span></span>
<span data-ttu-id="573c8-125">Slutföra förutsättningar som anges i hello [förutsättningar för självstudien](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) artikel innan du utför den här kursen.</span><span class="sxs-lookup"><span data-stu-id="573c8-125">Complete prerequisites listed in hello [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="steps"></a><span data-ttu-id="573c8-126">Steg</span><span class="sxs-lookup"><span data-stu-id="573c8-126">Steps</span></span>
<span data-ttu-id="573c8-127">Här är hello steg du utför som en del av den här kursen:</span><span class="sxs-lookup"><span data-stu-id="573c8-127">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="573c8-128">Skapa en Azure-**datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="573c8-128">Create an Azure **data factory**.</span></span> <span data-ttu-id="573c8-129">I det här steget skapar du en datafabrik med namnet ADFTutorialDataFactory.</span><span class="sxs-lookup"><span data-stu-id="573c8-129">In this step, you create a data factory named ADFTutorialDataFactory.</span></span> 
2. <span data-ttu-id="573c8-130">Skapa **länkade tjänster** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-130">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="573c8-131">I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-131">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="573c8-132">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-132">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="573c8-133">Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-133">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="573c8-134">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-134">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="573c8-135">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="573c8-135">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="573c8-136">Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-136">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="573c8-137">Skapa ingående och utgående **datauppsättningar** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-137">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="573c8-138">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="573c8-138">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="573c8-139">Och hello inkommande blobbdatauppsättning anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="573c8-139">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="573c8-140">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-140">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="573c8-141">Och hello utdatauppsättningen SQL tabellen anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.</span><span class="sxs-lookup"><span data-stu-id="573c8-141">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
4. <span data-ttu-id="573c8-142">Skapa en **pipeline** i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-142">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="573c8-143">I det här steget kan du skapa en pipeline med en kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="573c8-143">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="573c8-144">Hej kopieringsaktiviteten kopierar data från en blobb i hello Azure blob storage tooa tabellen i hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-144">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="573c8-145">Du kan använda en kopia aktivitet i en pipeline toocopy data från alla stöds källan tooany stöds målet.</span><span class="sxs-lookup"><span data-stu-id="573c8-145">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="573c8-146">I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds.</span><span class="sxs-lookup"><span data-stu-id="573c8-146">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="573c8-147">Övervakaren hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="573c8-147">Monitor hello pipeline.</span></span> <span data-ttu-id="573c8-148">I det här steget du **övervakaren** hello segment av inkommande och utgående datamängder med hjälp av Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="573c8-148">In this step, you **monitor** hello slices of input and output datasets by using Azure portal.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="573c8-149">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="573c8-149">Create data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="573c8-150">Fullständig [förutsättningar för självstudiekursen hello](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) om du inte redan gjort det..</span><span class="sxs-lookup"><span data-stu-id="573c8-150">Complete [prerequisites for hello tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="573c8-151">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="573c8-151">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="573c8-152">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="573c8-152">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="573c8-153">Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata.</span><span class="sxs-lookup"><span data-stu-id="573c8-153">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="573c8-154">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="573c8-154">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="573c8-155">När du loggar in toohello [Azure-portalen](https://portal.azure.com/), klickar du på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="573c8-155">After logging in toohello [Azure portal](https://portal.azure.com/), click **New** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span> 
   
   ![Nytt->DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. <span data-ttu-id="573c8-157">I hello **nya data factory** bladet:</span><span class="sxs-lookup"><span data-stu-id="573c8-157">In hello **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="573c8-158">Ange **ADFTutorialDataFactory** för hello **namn**.</span><span class="sxs-lookup"><span data-stu-id="573c8-158">Enter **ADFTutorialDataFactory** for hello **name**.</span></span> 
      
         ![Bladet Ny datafabrik](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       <span data-ttu-id="573c8-160">hello hello Azure data factory måste vara **globalt unika**.</span><span class="sxs-lookup"><span data-stu-id="573c8-160">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="573c8-161">Om du får följande fel hello ändra hello namn hello data factory (till exempel yournameADFTutorialDataFactory) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="573c8-161">If you receive hello following error, change hello name of hello data factory (for example, yournameADFTutorialDataFactory) and try creating again.</span></span> <span data-ttu-id="573c8-162">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="573c8-162">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Datafabriksnamnet är inte tillgängligt](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. <span data-ttu-id="573c8-164">Välj din Azure **prenumeration** som du vill toocreate hello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-164">Select your Azure **subscription** in which you want toocreate hello data factory.</span></span> 
   3. <span data-ttu-id="573c8-165">För hello **resursgruppen**, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="573c8-165">For hello **Resource Group**, do one of hello following steps:</span></span>
      
      - <span data-ttu-id="573c8-166">Välj **använda befintliga**, och välj en befintlig resursgrupp hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="573c8-166">Select **Use existing**, and select an existing resource group from hello drop-down list.</span></span> 
      - <span data-ttu-id="573c8-167">Välj **Skapa nytt**, och ange hello namnet på en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="573c8-167">Select **Create new**, and enter hello name of a resource group.</span></span>   
         
          <span data-ttu-id="573c8-168">Några av hello stegen i den här självstudiekursen förutsätts att du använder hello namn: **ADFTutorialResourceGroup** för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="573c8-168">Some of hello steps in this tutorial assume that you use hello name: **ADFTutorialResourceGroup** for hello resource group.</span></span> <span data-ttu-id="573c8-169">toolearn om resursgrupper finns [resursnamnet grupper toomanage resurserna i Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-169">toolearn about resource groups, see [Using resource groups toomanage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>  
   4. <span data-ttu-id="573c8-170">Välj hello **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-170">Select hello **location** for hello data factory.</span></span> <span data-ttu-id="573c8-171">Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="573c8-171">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
   5. <span data-ttu-id="573c8-172">Välj **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="573c8-172">Select **Pin toodashboard**.</span></span>     
   6. <span data-ttu-id="573c8-173">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="573c8-173">Click **Create**.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="573c8-174">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="573c8-174">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
      > 
      > <span data-ttu-id="573c8-175">hello namnet på hello data factory kan registreras som en DNS-namnet i hello framtida och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="573c8-175">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>                
      > 
      > 
3. <span data-ttu-id="573c8-176">Hello instrumentpanelen visas hello följande panelen med status: **distribuera data factory**.</span><span class="sxs-lookup"><span data-stu-id="573c8-176">On hello dashboard, you see hello following tile with status: **Deploying data factory**.</span></span> 

    ![panelen distribuerar datafabrik](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. <span data-ttu-id="573c8-178">När hello har skapats visas hello **Data Factory** bladet enligt hello bild.</span><span class="sxs-lookup"><span data-stu-id="573c8-178">After hello creation is complete, you see hello **Data Factory** blade as shown in hello image.</span></span>
   
   ![Datafabrikens startsida](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a><span data-ttu-id="573c8-180">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="573c8-180">Create linked services</span></span>
<span data-ttu-id="573c8-181">Du kan skapa länkade tjänster i en data factory toolink dina data lagras och beräkna services toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-181">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="573c8-182">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="573c8-182">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="573c8-183">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="573c8-183">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="573c8-184">Därför kan du skapa två länkade tjänster som heter AzureStorageLinkedService och AzureSqlLinkedService av typerna: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="573c8-184">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="573c8-185">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-185">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="573c8-186">Det här lagringskontot är hello något som du skapade en behållare och överföra hello data som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-186">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="573c8-187">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-187">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="573c8-188">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="573c8-188">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="573c8-189">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-189">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="573c8-190">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="573c8-190">Create Azure Storage linked service</span></span>
<span data-ttu-id="573c8-191">I det här steget kan länka du din Azure storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-191">In this step, you link your Azure storage account tooyour data factory.</span></span> <span data-ttu-id="573c8-192">Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="573c8-192">You specify hello name and key of your Azure storage account in this section.</span></span>  

1. <span data-ttu-id="573c8-193">I hello **Datafabriken** bladet, klickar du på **författare och distribuera** panelen.</span><span class="sxs-lookup"><span data-stu-id="573c8-193">In hello **Data Factory** blade, click **Author and deploy** tile.</span></span>
   
   ![Ikonen Författare och distribution](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. <span data-ttu-id="573c8-195">Du ser hello **Data Factory-redigeraren** som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="573c8-195">You see hello **Data Factory Editor** as shown in hello following image:</span></span> 

    ![Data Factory-redigeraren](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. <span data-ttu-id="573c8-197">I Redigeraren för hello, klickar du på **Nytt datalager** hello verktygsfältet och välj knappen **Azure storage** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-197">In hello editor, click **New data store** button on hello toolbar and select **Azure storage** from hello drop-down menu.</span></span> <span data-ttu-id="573c8-198">Du bör se hello JSON-mall för att skapa en länkad Azure storage-tjänst i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="573c8-198">You should see hello JSON template for creating an Azure storage linked service in hello right pane.</span></span> 
   
    ![Redigerarens knapp Nytt datalager](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. <span data-ttu-id="573c8-200">Ersätt `<accountname>` och `<accountkey>` med hello konto namn och värden för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="573c8-200">Replace `<accountname>` and `<accountkey>` with hello account name and account key values for your Azure storage account.</span></span> 
   
    ![JSON för redigerarens blobblagring](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. <span data-ttu-id="573c8-202">Klicka på **distribuera** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="573c8-202">Click **Deploy** on hello toolbar.</span></span> <span data-ttu-id="573c8-203">Du bör se hello distribueras **AzureStorageLinkedService** i trädet hello nu visa.</span><span class="sxs-lookup"><span data-stu-id="573c8-203">You should see hello deployed **AzureStorageLinkedService** in hello tree view now.</span></span> 
   
    ![Distribuera redigerarens blobblagring](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    <span data-ttu-id="573c8-205">Läs mer om JSON-egenskaper i länkade hello tjänstdefinitionen [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) artikel.</span><span class="sxs-lookup"><span data-stu-id="573c8-205">For more information about JSON properties in hello linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-a-linked-service-for-hello-azure-sql-database"></a><span data-ttu-id="573c8-206">Skapa en länkad tjänst för hello Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="573c8-206">Create a linked service for hello Azure SQL Database</span></span>
<span data-ttu-id="573c8-207">I det här steget kan länka du din Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-207">In this step, you link your Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="573c8-208">Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="573c8-208">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> 

1. <span data-ttu-id="573c8-209">I hello **Data Factory-redigeraren**, klickar du på **Nytt datalager** hello verktygsfältet och välj knappen **Azure SQL Database** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-209">In hello **Data Factory Editor**, click **New data store** button on hello toolbar and select **Azure SQL Database** from hello drop-down menu.</span></span> <span data-ttu-id="573c8-210">Du bör se hello JSON-mall för att skapa hello Azure SQL-länkade tjänsten i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="573c8-210">You should see hello JSON template for creating hello Azure SQL linked service in hello right pane.</span></span>
2. <span data-ttu-id="573c8-211">Ersätt `<servername>`, `<databasename>`, `<username>@<servername>` och `<password>` med namnen på din Azure SQL-server, databas, ditt användarkonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="573c8-211">Replace `<servername>`, `<databasename>`, `<username>@<servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span> 
3. <span data-ttu-id="573c8-212">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="573c8-212">Click **Deploy** on hello toolbar toocreate and deploy hello **AzureSqlLinkedService**.</span></span>
4. <span data-ttu-id="573c8-213">Bekräfta att du ser **AzureSqlLinkedService** i hello trädvy under **länkade tjänster**.</span><span class="sxs-lookup"><span data-stu-id="573c8-213">Confirm that you see **AzureSqlLinkedService** in hello tree view under **Linked services**.</span></span>  

    <span data-ttu-id="573c8-214">Mer information om de här JSON-egenskaperna finns i [Anslutningsapp för Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="573c8-214">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

## <a name="create-datasets"></a><span data-ttu-id="573c8-215">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="573c8-215">Create datasets</span></span>
<span data-ttu-id="573c8-216">I föregående steg hello Skapa länkade tjänster toolink dina Azure Storage-konto och Azure SQL database tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-216">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="573c8-217">I det här steget definierar du två datamängder som heter InputDataset och OutputDataset som representerar indata och utdata som är lagrad i hello datalager som anges av AzureStorageLinkedService och AzureSqlLinkedService respektive.</span><span class="sxs-lookup"><span data-stu-id="573c8-217">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="573c8-218">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="573c8-218">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="573c8-219">Och hello inkommande blobbdatauppsättning (InputDataset) anger hello-behållaren och hello-mapp som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="573c8-219">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="573c8-220">På samma sätt anger hello Azure SQL Database länkad tjänst hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-220">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="573c8-221">Och hello utgående SQL tabell datauppsättningen (OututDataset) anger hello tabellen i hello databasen toowhich hello kopieras data från hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="573c8-221">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="573c8-222">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="573c8-222">Create input dataset</span></span>
<span data-ttu-id="573c8-223">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar tooa blob-fil (emp.txt) i blob-behållaren (adftutorial) hello rotmapp i hello Azure Storage som representeras av hello AzureStorageLinkedService länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="573c8-223">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="573c8-224">Om du inte ange ett värde för hello filnamn (eller hoppa över den), är data från alla blobbar i hello inkommande mappen kopierade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="573c8-224">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="573c8-225">I kursen får ange du ett värde för hello filnamnet.</span><span class="sxs-lookup"><span data-stu-id="573c8-225">In this tutorial, you specify a value for hello fileName.</span></span> 

1. <span data-ttu-id="573c8-226">I hello **Editor** hello Data Factory, klickar du på **... Flera**, klickar du på **ny datamängd**, och klicka på **Azure Blob storage** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-226">In hello **Editor** for hello Data Factory, click **... More**, click **New dataset**, and click **Azure Blob storage** from hello drop-down menu.</span></span> 
   
    ![Menyn Ny datauppsättning](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. <span data-ttu-id="573c8-228">Ersätt JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="573c8-228">Replace JSON in hello right pane with hello following JSON snippet:</span></span> 
   
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
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
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

    <span data-ttu-id="573c8-229">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="573c8-229">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="573c8-230">Egenskap</span><span class="sxs-lookup"><span data-stu-id="573c8-230">Property</span></span> | <span data-ttu-id="573c8-231">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="573c8-231">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="573c8-232">typ</span><span class="sxs-lookup"><span data-stu-id="573c8-232">type</span></span> | <span data-ttu-id="573c8-233">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="573c8-233">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="573c8-234">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="573c8-234">linkedServiceName</span></span> | <span data-ttu-id="573c8-235">Refererar toohello **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="573c8-235">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="573c8-236">folderPath</span><span class="sxs-lookup"><span data-stu-id="573c8-236">folderPath</span></span> | <span data-ttu-id="573c8-237">Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar.</span><span class="sxs-lookup"><span data-stu-id="573c8-237">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="573c8-238">I den här självstudiekursen adftutorial är hello blob-behållaren och mappen är hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="573c8-238">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="573c8-239">fileName</span><span class="sxs-lookup"><span data-stu-id="573c8-239">fileName</span></span> | <span data-ttu-id="573c8-240">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="573c8-240">This property is optional.</span></span> <span data-ttu-id="573c8-241">Om du utesluter den här egenskapen har alla filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="573c8-241">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="573c8-242">I den här självstudiekursen **emp.txt** har angetts för hello filnamn, så att endast filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="573c8-242">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="573c8-243">format -> typ</span><span class="sxs-lookup"><span data-stu-id="573c8-243">format -> type</span></span> |<span data-ttu-id="573c8-244">hello indatafilen är i hello-format, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="573c8-244">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="573c8-245">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="573c8-245">columnDelimiter</span></span> | <span data-ttu-id="573c8-246">hello kolumner i hello indatafilen avgränsas med **kommatecken tecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="573c8-246">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="573c8-247">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="573c8-247">frequency/interval</span></span> | <span data-ttu-id="573c8-248">hello frekvens har angetts för**timme** och intervallet anges för**1**, vilket innebär att hello indata segment är tillgängliga **varje timme**.</span><span class="sxs-lookup"><span data-stu-id="573c8-248">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="573c8-249">Med andra ord hello Data Factory-tjänsten söker efter indata varje timme i hello rotmapp blob-behållaren (**adftutorial**) du har angett.</span><span class="sxs-lookup"><span data-stu-id="573c8-249">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="573c8-250">Det ser ut för hello data inom hello pipeline start- och tider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="573c8-250">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="573c8-251">extern</span><span class="sxs-lookup"><span data-stu-id="573c8-251">external</span></span> | <span data-ttu-id="573c8-252">Den här egenskapen anges för**SANT** om hello data inte genereras av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="573c8-252">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="573c8-253">hello inkommande data i den här självstudiekursen har hello emp.txt fil, som genereras av denna pipeline, så vi ställa in den här egenskapen tootrue.</span><span class="sxs-lookup"><span data-stu-id="573c8-253">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="573c8-254">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="573c8-254">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>      
3. <span data-ttu-id="573c8-255">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **InputDataset** dataset.</span><span class="sxs-lookup"><span data-stu-id="573c8-255">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset** dataset.</span></span> <span data-ttu-id="573c8-256">Bekräfta att du ser hello **InputDataset** i hello trädvy.</span><span class="sxs-lookup"><span data-stu-id="573c8-256">Confirm that you see hello **InputDataset** in hello tree view.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="573c8-257">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="573c8-257">Create output dataset</span></span>
<span data-ttu-id="573c8-258">hello Azure SQL Database länkade tjänsten anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-258">hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="573c8-259">hello SQL tabell utdatauppsättningen (OututDataset) du skapar i det här steget anger hello tabellen i hello toowhich hello databasdata från hello blob storage kopieras.</span><span class="sxs-lookup"><span data-stu-id="573c8-259">hello output SQL table dataset (OututDataset) you create in this step specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

1. <span data-ttu-id="573c8-260">I hello **Editor** hello Data Factory, klickar du på **... Flera**, klickar du på **ny datamängd**, och klicka på **Azure SQL** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-260">In hello **Editor** for hello Data Factory, click **... More**, click **New dataset**, and click **Azure SQL** from hello drop-down menu.</span></span> 
2. <span data-ttu-id="573c8-261">Ersätt JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="573c8-261">Replace JSON in hello right pane with hello following JSON snippet:</span></span>

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
        "linkedServiceName": "AzureSqlLinkedService",
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

    <span data-ttu-id="573c8-262">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="573c8-262">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="573c8-263">Egenskap</span><span class="sxs-lookup"><span data-stu-id="573c8-263">Property</span></span> | <span data-ttu-id="573c8-264">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="573c8-264">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="573c8-265">typ</span><span class="sxs-lookup"><span data-stu-id="573c8-265">type</span></span> | <span data-ttu-id="573c8-266">hello typegenskapen har ställts in för**AzureSqlTable** eftersom data är kopierade tooa tabell i Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-266">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="573c8-267">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="573c8-267">linkedServiceName</span></span> | <span data-ttu-id="573c8-268">Refererar toohello **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="573c8-268">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="573c8-269">tableName</span><span class="sxs-lookup"><span data-stu-id="573c8-269">tableName</span></span> | <span data-ttu-id="573c8-270">Angivna hello **tabell** toowhich hello data kopieras.</span><span class="sxs-lookup"><span data-stu-id="573c8-270">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="573c8-271">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="573c8-271">frequency/interval</span></span> | <span data-ttu-id="573c8-272">hello frekvens har angetts för**timme** och är **1**, vilket innebär att hello utdata segment produceras **varje timme** mellan hello pipeline start och slut tider inte före eller När dessa tider.</span><span class="sxs-lookup"><span data-stu-id="573c8-272">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="573c8-273">Det finns tre kolumner – **ID**, **Förnamn**, och **efternamn** – i hello tomma tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="573c8-273">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="573c8-274">ID: T är en identitetskolumn, så du behöver bara toospecify **Förnamn** och **efternamn** här.</span><span class="sxs-lookup"><span data-stu-id="573c8-274">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="573c8-275">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="573c8-275">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="573c8-276">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **OutputDataset** dataset.</span><span class="sxs-lookup"><span data-stu-id="573c8-276">Click **Deploy** on hello toolbar toocreate and deploy hello **OutputDataset** dataset.</span></span> <span data-ttu-id="573c8-277">Bekräfta att du ser hello **OutputDataset** i hello trädvy under **datauppsättningar**.</span><span class="sxs-lookup"><span data-stu-id="573c8-277">Confirm that you see hello **OutputDataset** in hello tree view under **Datasets**.</span></span> 

## <a name="create-pipeline"></a><span data-ttu-id="573c8-278">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="573c8-278">Create pipeline</span></span>
<span data-ttu-id="573c8-279">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="573c8-279">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="573c8-280">Datamängd för utdata är för närvarande vilka enheter hello schema.</span><span class="sxs-lookup"><span data-stu-id="573c8-280">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="573c8-281">I den här kursen är utdatauppsättningen konfigurerade tooproduce ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="573c8-281">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="573c8-282">hello pipeline har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="573c8-282">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="573c8-283">Därför produceras 24 segment för utdatauppsättningen av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="573c8-283">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="573c8-284">I hello **Editor** hello Data Factory, klickar du på **... More (Mer)** och sedan på **Ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="573c8-284">In hello **Editor** for hello Data Factory, click **... More**, and click **New pipeline**.</span></span> <span data-ttu-id="573c8-285">Du kan också högerklicka på **Pipelines** i hello trädvyn och klicka på **ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="573c8-285">Alternatively, you can right-click **Pipelines** in hello tree view and click **New pipeline**.</span></span>
2. <span data-ttu-id="573c8-286">Ersätt JSON i hello högra med hello följande JSON-fragment:</span><span class="sxs-lookup"><span data-stu-id="573c8-286">Replace JSON in hello right pane with hello following JSON snippet:</span></span> 

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
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```   
    
    <span data-ttu-id="573c8-287">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="573c8-287">Note hello following points:</span></span>
   
    - <span data-ttu-id="573c8-288">Under hello aktiviteter är bara en aktivitet vars **typen** har angetts för**kopiera**.</span><span class="sxs-lookup"><span data-stu-id="573c8-288">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="573c8-289">Läs mer om hello kopieringsaktiviteten [data movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-289">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="573c8-290">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="573c8-291">Indata för hello aktiviteten är inställd för**InputDataset** och utdata för hello aktiviteten är inställd för**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="573c8-291">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="573c8-292">I hello **typeProperties** avsnittet **BlobSource** har angetts som hello källtypen och **SqlSink** har angetts som hello Mottagartypen.</span><span class="sxs-lookup"><span data-stu-id="573c8-292">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="573c8-293">En fullständig lista över datakällor som stöds av hello kopieringsaktiviteten som källor och sänkor finns [stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="573c8-293">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="573c8-294">toolearn hur toouse en specifik stöds data lagras som en källor/mottagare, klicka länken hello i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="573c8-294">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>
    - <span data-ttu-id="573c8-295">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="573c8-295">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="573c8-296">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="573c8-296">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="573c8-297">Hej **end** tid är valfritt, men vi använda den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="573c8-297">hello **end** time is optional, but we use it in this tutorial.</span></span> <span data-ttu-id="573c8-298">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="573c8-298">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="573c8-299">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="573c8-299">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="573c8-300">I föregående exempel hello, finns det 24 datasektorer som varje datasektorn produceras per timma.</span><span class="sxs-lookup"><span data-stu-id="573c8-300">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="573c8-301">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-301">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="573c8-302">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-302">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="573c8-303">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-303">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="573c8-304">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="573c8-304">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
3. <span data-ttu-id="573c8-305">Klicka på **distribuera** hello verktygsfältet toocreate och distribuera hello **ADFTutorialPipeline**.</span><span class="sxs-lookup"><span data-stu-id="573c8-305">Click **Deploy** on hello toolbar toocreate and deploy hello **ADFTutorialPipeline**.</span></span> <span data-ttu-id="573c8-306">Bekräfta att du ser hello pipeline i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-306">Confirm that you see hello pipeline in hello tree view.</span></span> 
4. <span data-ttu-id="573c8-307">Stäng nu hello **Editor** bladet genom att klicka på **X**. Klicka på **X** igen toosee hello **Datafabriken** startsidan för hello **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="573c8-307">Now, close hello **Editor** blade by clicking **X**. Click **X** again toosee hello **Data Factory** home page for hello **ADFTutorialDataFactory**.</span></span>

<span data-ttu-id="573c8-308">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="573c8-308">**Congratulations!**</span></span> <span data-ttu-id="573c8-309">Du har skapat ett Azure data factory med en rörledning toocopy data från Azure blob storage tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-309">You have successfully created an Azure data factory with a pipeline toocopy data from an Azure blob storage tooan Azure SQL database.</span></span> 


## <a name="monitor-pipeline"></a><span data-ttu-id="573c8-310">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="573c8-310">Monitor pipeline</span></span>
<span data-ttu-id="573c8-311">I det här steget använder du hello Azure portal toomonitor vad som händer i ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-311">In this step, you use hello Azure portal toomonitor what’s going on in an Azure data factory.</span></span>    

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="573c8-312">Övervaka pipeline med övervaknings- och hanteringsappen</span><span class="sxs-lookup"><span data-stu-id="573c8-312">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="573c8-313">hello visar följande steg hur toomonitor rörledningar i din data factory med hello övervaka och hantera program:</span><span class="sxs-lookup"><span data-stu-id="573c8-313">hello following steps show you how toomonitor pipelines in your data factory by using hello Monitor & Manage application:</span></span> 

1. <span data-ttu-id="573c8-314">Klicka på **övervaka och hantera** panelen på hello startsidan för din data factory.</span><span class="sxs-lookup"><span data-stu-id="573c8-314">Click **Monitor & Manage** tile on hello home page for your data factory.</span></span>
   
    ![Ikonen Övervaka och hantera](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. <span data-ttu-id="573c8-316">Du bör se programmet **Övervaka och hantera** i en separat flik.</span><span class="sxs-lookup"><span data-stu-id="573c8-316">You should see **Monitor & Manage application** in a separate tab.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="573c8-317">Om du ser att hello webbläsare har ”auktorisera...”, gör du något av följande hello: Rensa hello **blockerar cookies från tredje part och platsdata** kryssrutan (eller) skapa ett undantag för **login.microsoftonline.com**, och försök sedan tooopen hello appen igen.</span><span class="sxs-lookup"><span data-stu-id="573c8-317">If you see that hello web browser is stuck at "Authorizing...", do one of hello following: clear hello **Block third-party cookies and site data** check box (or) create an exception for **login.microsoftonline.com**, and then try tooopen hello app again.</span></span>

    ![Appen Övervaka och hantera](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. <span data-ttu-id="573c8-319">Ändra hello **starttid** och **sluttiden** tooinclude start (2017-05-11) och sluttider (2017-05-12) för din pipeline och på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="573c8-319">Change hello **Start time** and **End time** tooinclude start (2017-05-11) and end times (2017-05-12) of your pipeline, and click **Apply**.</span></span>     
3. <span data-ttu-id="573c8-320">Du ser hello **aktivitet windows** som är associerade med varje timme mellan pipeline start och slut gånger i listan för hello hello mellersta rutan.</span><span class="sxs-lookup"><span data-stu-id="573c8-320">You see hello **activity windows** associated with each hour between pipeline start and end times in hello list in hello middle pane.</span></span> 
4. <span data-ttu-id="573c8-321">toosee information om en aktivitetsfönstret väljer hello aktivitetsfönstret i hello **aktivitet Windows** lista.</span><span class="sxs-lookup"><span data-stu-id="573c8-321">toosee details about an activity window, select hello activity window in hello **Activity Windows** list.</span></span> 
    <span data-ttu-id="573c8-322">![Aktivitetsfönsterinformation](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="573c8-322">![Activity window details](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)</span></span>

    <span data-ttu-id="573c8-323">Aktiviteten fönstret Utforskaren på hello rätt visas i den hello sektorer toohello aktuella UTC-tid (8:12:00) bearbetas (med grön färg).</span><span class="sxs-lookup"><span data-stu-id="573c8-323">In Activity Window Explorer on hello right, you see that hello slices up toohello current UTC time (8:12 PM) are all processed (in green color).</span></span> <span data-ttu-id="573c8-324">hello 8 – 9 PM, 9-10 PM, 10-11 PM, 11 PM - 12: 00 segment bearbetas inte ännu.</span><span class="sxs-lookup"><span data-stu-id="573c8-324">hello 8-9 PM, 9 - 10 PM, 10 - 11 PM, 11 PM - 12 AM slices are not processed yet.</span></span>

    <span data-ttu-id="573c8-325">Hej **försök** avsnitt i hello högra fönstret innehåller information om hello aktiviteten kör för hello datasektorn.</span><span class="sxs-lookup"><span data-stu-id="573c8-325">hello **Attempts** section in hello right pane provides information about hello activity run for hello data slice.</span></span> <span data-ttu-id="573c8-326">Om ett fel uppstod, innehåller information om hello-fel.</span><span class="sxs-lookup"><span data-stu-id="573c8-326">If there was an error, it provides details about hello error.</span></span> <span data-ttu-id="573c8-327">Till exempel om hello indata mapp eller behållare inte finns och hello sektor bearbetningen misslyckas, visas ett felmeddelande om behållaren hello eller mappen finns inte.</span><span class="sxs-lookup"><span data-stu-id="573c8-327">For example, if hello input folder or container does not exist and hello slice processing fails, you see an error message stating that hello container or folder does not exist.</span></span>

    ![Aktivitetskörningsförsök](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. <span data-ttu-id="573c8-329">Starta **SQL Server Management Studio**, ansluta toohello Azure SQL Database och kontrollera att hello rader infogas i toohello **tomma** tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="573c8-329">Launch **SQL Server Management Studio**, connect toohello Azure SQL Database, and verify that hello rows are inserted in toohello **emp** table in hello database.</span></span>
    
    ![sql-frågeresultat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

<span data-ttu-id="573c8-331">Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="573c8-331">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="573c8-332">Övervaka pipeline med diagramvyn</span><span class="sxs-lookup"><span data-stu-id="573c8-332">Monitor pipeline using Diagram View</span></span>
<span data-ttu-id="573c8-333">Du kan också övervaka data pipelines med hjälp av hello diagramvyn.</span><span class="sxs-lookup"><span data-stu-id="573c8-333">You can also monitor data pipelines by using hello diagram view.</span></span>  

1. <span data-ttu-id="573c8-334">I hello **Datafabriken** bladet, klickar du på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="573c8-334">In hello **Data Factory** blade, click **Diagram**.</span></span>
   
    ![Data Factory-bladet – Diagramikon](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. <span data-ttu-id="573c8-336">Du bör se hello diagram liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="573c8-336">You should see hello diagram similar toohello following image:</span></span> 
   
    ![Diagramvy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. <span data-ttu-id="573c8-338">Dubbelklicka i hello diagramvyn **InputDataset** toosee segment för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="573c8-338">In hello diagram view, double-click **InputDataset** toosee slices for hello dataset.</span></span>  
   
    ![Datauppsättningar med InputDataset valt](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. <span data-ttu-id="573c8-340">Klicka på **se mer** länka toosee alla hello datasegment.</span><span class="sxs-lookup"><span data-stu-id="573c8-340">Click **See more** link toosee all hello data slices.</span></span> <span data-ttu-id="573c8-341">Du ser 24 timsegment mellan pipelinens start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="573c8-341">You see 24 hourly slices between pipeline start and end times.</span></span> 
   
    ![Alla indatasektorer](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    <span data-ttu-id="573c8-343">Alla hello datasektorer toohello aktuella UTC-tid är **klar** eftersom hello **emp.txt** filen finns alla hello-tid i hello blob-behållaren: **adftutorial\input**.</span><span class="sxs-lookup"><span data-stu-id="573c8-343">Notice that all hello data slices up toohello current UTC time are **Ready** because hello **emp.txt** file exists all hello time in hello blob container: **adftutorial\input**.</span></span> <span data-ttu-id="573c8-344">hello segment för hello framtida gånger ännu inte i tillståndet ready.</span><span class="sxs-lookup"><span data-stu-id="573c8-344">hello slices for hello future times are not in ready state yet.</span></span> <span data-ttu-id="573c8-345">Bekräfta att ingen segment visas i hello **nyligen misslyckade datasektorer** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="573c8-345">Confirm that no slices show up in hello **Recently failed slices** section at hello bottom.</span></span>
6. <span data-ttu-id="573c8-346">Stäng hello blad tills du se hello diagram visa (eller) rulla vänstra toosee hello diagram visar.</span><span class="sxs-lookup"><span data-stu-id="573c8-346">Close hello blades until you see hello diagram view (or) scroll left toosee hello diagram view.</span></span> <span data-ttu-id="573c8-347">Dubbelklicka sedan på **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="573c8-347">Then, double-click **OutputDataset**.</span></span> 
8. <span data-ttu-id="573c8-348">Klicka på **se mer** länk på hello **tabell** bladet för **OutputDataset** toosee alla hello segment.</span><span class="sxs-lookup"><span data-stu-id="573c8-348">Click **See more** link on hello **Table** blade for **OutputDataset** toosee all hello slices.</span></span>

    ![datasektorblad](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. <span data-ttu-id="573c8-350">Observera att alla hello segment toohello aktuella UTC-tid flytta från **väntande körning** tillstånd = > **pågår** ==> **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="573c8-350">Notice that all hello slices up toohello current UTC time move from **pending execution** state => **In progress** ==> **Ready** state.</span></span> <span data-ttu-id="573c8-351">Hej från hello senaste segment (före aktuell tid) bearbetas från senaste toooldest som standard.</span><span class="sxs-lookup"><span data-stu-id="573c8-351">hello slices from hello past (before current time) are processed from latest toooldest by default.</span></span> <span data-ttu-id="573c8-352">Till exempel bearbetas hello aktuell tid är 8:12:00 UTC, hello segment för 7 PM - 20: 00 i hello 18: 00 - 7 PM sektorn.</span><span class="sxs-lookup"><span data-stu-id="573c8-352">For example, if hello current time is 8:12 PM UTC, hello slice for 7 PM - 8 PM is processed ahead of hello 6 PM - 7 PM slice.</span></span> <span data-ttu-id="573c8-353">hello 20: 00 - 21: 00 sektorn behandlas hello slutet av hello tidsintervall som standard som inträffar efter 21: 00.</span><span class="sxs-lookup"><span data-stu-id="573c8-353">hello 8 PM - 9 PM slice is processed at hello end of hello time interval by default, that is after 9 PM.</span></span>  
10. <span data-ttu-id="573c8-354">Klicka på någon datasektorn hello listan och du bör se hello **datasektorn** bladet.</span><span class="sxs-lookup"><span data-stu-id="573c8-354">Click any data slice from hello list and you should see hello **Data slice** blade.</span></span> <span data-ttu-id="573c8-355">En datauppsättning som associeras med ett aktivitetsfönster kallas ett segment.</span><span class="sxs-lookup"><span data-stu-id="573c8-355">A piece of data associated with an activity window is called a slice.</span></span> <span data-ttu-id="573c8-356">Ett segment kan ha en eller flera filer.</span><span class="sxs-lookup"><span data-stu-id="573c8-356">A slice can be one file or multiple files.</span></span>  
    
     ![datasektorblad](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     <span data-ttu-id="573c8-358">Om hello segment inte hello **klar** tillstånd, som du kan se hello överordnade sektorer som inte är redo och blockerar hello aktuellt segment från att köras i hello **sektorer uppströms som inte är redo** lista.</span><span class="sxs-lookup"><span data-stu-id="573c8-358">If hello slice is not in hello **Ready** state, you can see hello upstream slices that are not Ready and are blocking hello current slice from executing in hello **Upstream slices that are not ready** list.</span></span>
11. <span data-ttu-id="573c8-359">I hello **DATASEKTORN** bladet bör du se alla aktiviteter som körs i hello lista längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="573c8-359">In hello **DATA SLICE** blade, you should see all activity runs in hello list at hello bottom.</span></span> <span data-ttu-id="573c8-360">Klicka på en **aktiviteten kör** toosee hello **aktivitet köras information** bladet.</span><span class="sxs-lookup"><span data-stu-id="573c8-360">Click an **activity run** toosee hello **Activity run details** blade.</span></span> 
    
    ![Aktivitetskörningsinformation](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    <span data-ttu-id="573c8-362">I det här bladet kommer du se hur lång hello kopieringsåtgärden tog vilka dataflöde, hur många byte data har läs- och skriftliga, kör starttiden körs sluttid osv.</span><span class="sxs-lookup"><span data-stu-id="573c8-362">In this blade, you see how long hello copy operation took, what throughput is, how many bytes of data were read and written, run start time, run end time etc.</span></span>  
12. <span data-ttu-id="573c8-363">Klicka på **X** tooclose alla hello blad tills du komma toohello hem bladet för hello **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="573c8-363">Click **X** tooclose all hello blades until you get back toohello home blade for hello **ADFTutorialDataFactory**.</span></span>
13. <span data-ttu-id="573c8-364">(valfritt) Klicka på hello **datauppsättningar** panelen eller **Pipelines** panelen tooget hello blad som du har sett hello ovanstående steg.</span><span class="sxs-lookup"><span data-stu-id="573c8-364">(optional) click hello **Datasets** tile or **Pipelines** tile tooget hello blades you have seen hello preceding steps.</span></span> 
14. <span data-ttu-id="573c8-365">Starta **SQL Server Management Studio**, ansluta toohello Azure SQL Database och kontrollera att hello rader infogas i toohello **tomma** tabellen i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="573c8-365">Launch **SQL Server Management Studio**, connect toohello Azure SQL Database, and verify that hello rows are inserted in toohello **emp** table in hello database.</span></span>
    
    ![sql-frågeresultat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a><span data-ttu-id="573c8-367">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="573c8-367">Summary</span></span>
<span data-ttu-id="573c8-368">I kursen får skapat du ett Azure data factory toocopy data från Azure blob tooan Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="573c8-368">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="573c8-369">Du har använt hello Azure portal toocreate hello data factory, länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="573c8-369">You used hello Azure portal toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="573c8-370">Här följer hello anvisningar som du utförde i den här kursen:</span><span class="sxs-lookup"><span data-stu-id="573c8-370">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="573c8-371">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="573c8-371">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="573c8-372">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="573c8-372">Created **linked services**:</span></span>
   1. <span data-ttu-id="573c8-373">En **Azure Storage** länkade tjänsten toolink ditt Azure Storage-konto som innehåller data som indata.</span><span class="sxs-lookup"><span data-stu-id="573c8-373">An **Azure Storage** linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="573c8-374">En **Azure SQL** länkade tjänsten toolink din Azure SQL-databas som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="573c8-374">An **Azure SQL** linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="573c8-375">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="573c8-375">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="573c8-376">Du skapade en **pipeline** med en **kopieringsaktivitet** med **BlobSource** som källa och **SqlSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="573c8-376">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="573c8-377">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="573c8-377">Next steps</span></span>
<span data-ttu-id="573c8-378">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="573c8-378">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="573c8-379">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="573c8-379">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="573c8-380">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="573c8-380">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
