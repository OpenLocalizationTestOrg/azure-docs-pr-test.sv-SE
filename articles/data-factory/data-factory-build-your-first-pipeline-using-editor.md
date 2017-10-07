---
title: "aaaBuild din första data factory (Azure portal) | Microsoft Docs"
description: "I den här självstudiekursen skapar du en pipeline för Azure Data Factory av exemplet med Data Factory-redigeraren i hello Azure-portalen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="3dc77-103">Självstudier: Skapa din första Azure-datafabrik med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3dc77-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3dc77-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="3dc77-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="3dc77-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3dc77-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="3dc77-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3dc77-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="3dc77-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3dc77-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="3dc77-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="3dc77-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="3dc77-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="3dc77-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="3dc77-110">I den här artikeln får du lära dig hur toouse [Azure-portalen](https://portal.azure.com/) toocreate din första Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-110">In this article, you learn how toouse [Azure portal](https://portal.azure.com/) toocreate your first Azure data factory.</span></span> <span data-ttu-id="3dc77-111">toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="3dc77-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span> 

<span data-ttu-id="3dc77-112">hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="3dc77-113">Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="3dc77-114">hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="3dc77-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="3dc77-115">hello data pipeline i den här handledningen omvandlar indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="3dc77-116">En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3dc77-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="3dc77-117">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="3dc77-118">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3dc77-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="3dc77-119">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="3dc77-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3dc77-120">Krav</span><span class="sxs-lookup"><span data-stu-id="3dc77-120">Prerequisites</span></span>
1. <span data-ttu-id="3dc77-121">Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="3dc77-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
2. <span data-ttu-id="3dc77-122">Den här artikeln innehåller inte en översikt över hello Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-122">This article does not provide a conceptual overview of hello Azure Data Factory service.</span></span> <span data-ttu-id="3dc77-123">Vi rekommenderar att du går igenom [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel en detaljerad översikt av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-123">We recommend that you go through [Introduction tooAzure Data Factory](data-factory-introduction.md) article for a detailed overview of hello service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="3dc77-124">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="3dc77-124">Create data factory</span></span>
<span data-ttu-id="3dc77-125">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="3dc77-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="3dc77-126">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="3dc77-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="3dc77-127">Till exempel ange en Kopieringsaktiviteten toocopy data från ett dataarkiv som källa tooa mål och en HDInsight Hive aktiviteten toorun tootransform en Hive-skript data tooproduct utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-127">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="3dc77-128">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="3dc77-128">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="3dc77-129">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3dc77-129">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="3dc77-130">Klicka på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-130">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![Bladet Skapa](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="3dc77-132">I hello **nya datafabriken** bladet ange **GetStartedDF** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="3dc77-132">In hello **New data factory** blade, enter **GetStartedDF** for hello Name.</span></span>

   ![Bladet Ny datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="3dc77-134">hello hello Azure data factory måste vara **globalt unika**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-134">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="3dc77-135">Om felmeddelandet hello: **datafabriksnamnet ”GetStartedDF” är inte tillgänglig**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-135">If you receive hello error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="3dc77-136">Ändra hello namn i hello data factory (till exempel yournameGetStartedDF) och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="3dc77-136">Change hello name of hello data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="3dc77-137">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="3dc77-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="3dc77-138">hello namn i hello data factory får registreras som en **DNS** namn i hello framtiden och därför bli synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="3dc77-138">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="3dc77-139">Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="3dc77-139">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="3dc77-140">Välj befintlig **resursgrupp** eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="3dc77-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="3dc77-141">Hello självstudiekurs skapar du en resursgrupp med namnet: **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-141">For hello tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="3dc77-142">Välj hello **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-142">Select hello **location** for hello data factory.</span></span> <span data-ttu-id="3dc77-143">Endast de regioner som stöds av hello Data Factory-tjänsten som visas i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="3dc77-143">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
7. <span data-ttu-id="3dc77-144">Välj **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-144">Select **Pin toodashboard**.</span></span> 
8. <span data-ttu-id="3dc77-145">Klicka på **skapa** på hello **nya data factory** bladet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-145">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="3dc77-146">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="3dc77-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="3dc77-147">Hello instrumentpanelen visas hello följande panelen med status: distribuera data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-147">On hello dashboard, you see hello following tile with status: Deploying data factory.</span></span>    

   ![Skapar datafabrikstatus](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="3dc77-149">Grattis!</span><span class="sxs-lookup"><span data-stu-id="3dc77-149">Congratulations!</span></span> <span data-ttu-id="3dc77-150">Du har skapat din första datafabrik.</span><span class="sxs-lookup"><span data-stu-id="3dc77-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="3dc77-151">När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-151">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>     

    ![Bladet Datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="3dc77-153">Innan du skapar en pipeline i hello data factory, måste toocreate några Data Factory-entiteter först.</span><span class="sxs-lookup"><span data-stu-id="3dc77-153">Before creating a pipeline in hello data factory, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="3dc77-154">Du måste först skapa länkade tjänster toolink data lagrar/beräknar tooyour data lagras, definiera indata och utdata datauppsättningar toorepresent in-/ utdata i länkade datalager och sedan skapa hello pipeline med en aktivitet som använder dessa data.</span><span class="sxs-lookup"><span data-stu-id="3dc77-154">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="3dc77-155">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="3dc77-155">Create linked services</span></span>
<span data-ttu-id="3dc77-156">I det här steget kan länka du ditt Azure Storage-konto och en på begäran Azure HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="3dc77-157">hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-157">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="3dc77-158">hello länkad HDInsight-tjänst är används toorun en Hive-skript som angetts i hello aktivitet för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-158">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="3dc77-159">Identifiera vad [datalagret](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) används i din situation och länka dessa tjänster toohello data factory genom att skapa länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services toohello data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="3dc77-160">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="3dc77-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="3dc77-161">I det här steget kan länka du din Azure Storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-161">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="3dc77-162">I den här kursen använder du hello samma Azure Storage-konto toostore i/o-data och hello HQL skriptfil.</span><span class="sxs-lookup"><span data-stu-id="3dc77-162">In this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="3dc77-163">Klicka på **författare och distribuera** på hello **DATAFABRIKEN** bladet för **GetStartedDF**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-163">Click **Author and deploy** on hello **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="3dc77-164">Du bör se hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3dc77-164">You should see hello Data Factory Editor.</span></span>

   ![Ikonen Författare och distribution](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="3dc77-166">Klicka på **Nytt datalager** och välj **Azure-lagring**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Ny datalagring - Azure Storage - meny](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="3dc77-168">Du bör se hello JSON-skript för att skapa ett Azure Storage länkade tjänsten i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3dc77-168">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![Länkad Azure-lagringstjänst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="3dc77-170">Ersätt **kontonamn** med hello namnet på ditt Azure storage-konto och **kontonyckel** med hello snabbtangent av hello Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3dc77-170">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="3dc77-171">toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3dc77-171">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="3dc77-172">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-172">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![Knappen Distribuera](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="3dc77-174">När hello länkade tjänsten har distribuerats korrekt hello **utkast 1** fönstret bör försvinner och du ser **AzureStorageLinkedService** i hello trädvyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-174">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

    ![Länkad lagringstjänst i menyn](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="3dc77-176">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="3dc77-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="3dc77-177">I det här steget kan länka du ett på begäran HDInsight-kluster tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-177">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="3dc77-178">Hej HDInsight-kluster skapas under körning och tas bort när den är klar bearbetning och inaktiv för hello angiven tidsperiod automatiskt.</span><span class="sxs-lookup"><span data-stu-id="3dc77-178">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span>

1. <span data-ttu-id="3dc77-179">I hello **Data Factory-redigeraren**, klickar du på **... Fler**, klicka på **Ny beräkning**, och välj **På begäran HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-179">In hello **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![Ny beräkning](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="3dc77-181">Kopiera och klistra in följande kodutdrag toohello hello **utkast 1** fönster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-181">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="3dc77-182">hello JSON fragment beskriver hello egenskaper används toocreate hello HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="3dc77-182">hello JSON snippet describes hello properties that are used toocreate hello HDInsight cluster on-demand.</span></span>

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    <span data-ttu-id="3dc77-183">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="3dc77-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="3dc77-184">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3dc77-184">Property</span></span> | <span data-ttu-id="3dc77-185">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3dc77-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="3dc77-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="3dc77-186">ClusterSize</span></span> |<span data-ttu-id="3dc77-187">Anger hello storleken på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="3dc77-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="3dc77-188">TimeToLive</span></span> | <span data-ttu-id="3dc77-189">Anger den inaktiva tiden hello för hello HDInsight-kluster, innan den tas bort.</span><span class="sxs-lookup"><span data-stu-id="3dc77-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="3dc77-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3dc77-190">linkedServiceName</span></span> | <span data-ttu-id="3dc77-191">Anger hello storage-konto som används toostore hello loggar som genereras av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3dc77-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="3dc77-192">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="3dc77-192">Note hello following points:</span></span>

   * <span data-ttu-id="3dc77-193">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello JSON.</span><span class="sxs-lookup"><span data-stu-id="3dc77-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="3dc77-194">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3dc77-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="3dc77-195">Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="3dc77-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="3dc77-196">Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3dc77-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="3dc77-197">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="3dc77-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="3dc77-198">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="3dc77-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="3dc77-199">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="3dc77-199">This behavior is by design.</span></span> <span data-ttu-id="3dc77-200">Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="3dc77-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="3dc77-201">hello klustret tas bort automatiskt när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="3dc77-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="3dc77-202">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3dc77-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="3dc77-203">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="3dc77-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="3dc77-204">hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="3dc77-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="3dc77-205">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="3dc77-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="3dc77-206">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3dc77-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="3dc77-207">Klicka på **distribuera** på hello i kommandofältet toodeploy hello länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-207">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![Distribuera på begäran länkad HDInsight-tjänst](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="3dc77-209">Bekräfta att du ser både **AzureStorageLinkedService** och **HDInsightOnDemandLinkedService** i hello trädvyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in hello tree view on hello left.</span></span>

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="3dc77-211">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="3dc77-211">Create datasets</span></span>
<span data-ttu-id="3dc77-212">I det här steget Skapa datauppsättningar toorepresent hello indata och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3dc77-212">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="3dc77-213">De här datauppsättningarna finns toohello **AzureStorageLinkedService** du har skapat tidigare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3dc77-213">These datasets refer toohello **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="3dc77-214">Hej länkade tjänsten punkter tooan Azure Storage-konto och datauppsättningar anger behållare, mapp, filnamn i hello lagring som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-214">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="3dc77-215">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="3dc77-215">Create input dataset</span></span>
1. <span data-ttu-id="3dc77-216">I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-216">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![Ny datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="3dc77-218">Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3dc77-218">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="3dc77-219">Hello JSON fragment du skapar en datauppsättning som kallas **AzureBlobInput** som representerar indata för en aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-219">In hello JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="3dc77-220">Dessutom kan du ange att hello indata finns i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-220">In addition, you specify that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
    <span data-ttu-id="3dc77-221">hello innehåller följande tabell beskrivningar för hello JSON egenskaper som används i hello fragment:</span><span class="sxs-lookup"><span data-stu-id="3dc77-221">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="3dc77-222">Egenskap</span><span class="sxs-lookup"><span data-stu-id="3dc77-222">Property</span></span> | <span data-ttu-id="3dc77-223">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3dc77-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="3dc77-224">typ</span><span class="sxs-lookup"><span data-stu-id="3dc77-224">type</span></span> |<span data-ttu-id="3dc77-225">hello typegenskapen har ställts in för**AzureBlob** eftersom data finns i ett Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="3dc77-225">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="3dc77-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="3dc77-226">linkedServiceName</span></span> |<span data-ttu-id="3dc77-227">Refererar toohello **AzureStorageLinkedService** du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="3dc77-227">Refers toohello **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="3dc77-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="3dc77-228">folderPath</span></span> | <span data-ttu-id="3dc77-229">Anger hello blob **behållare** och hello **mappen** som innehåller inkommande blobbar.</span><span class="sxs-lookup"><span data-stu-id="3dc77-229">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="3dc77-230">fileName</span><span class="sxs-lookup"><span data-stu-id="3dc77-230">fileName</span></span> |<span data-ttu-id="3dc77-231">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="3dc77-231">This property is optional.</span></span> <span data-ttu-id="3dc77-232">Om du utesluter den här egenskapen har alla hello-filer från hello folderPath plockats.</span><span class="sxs-lookup"><span data-stu-id="3dc77-232">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="3dc77-233">I den här självstudiekursen bara hello **input.log** bearbetas.</span><span class="sxs-lookup"><span data-stu-id="3dc77-233">In this tutorial, only hello **input.log** is processed.</span></span> |
   | <span data-ttu-id="3dc77-234">typ</span><span class="sxs-lookup"><span data-stu-id="3dc77-234">type</span></span> |<span data-ttu-id="3dc77-235">hello loggfilerna i textformat, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-235">hello log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="3dc77-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="3dc77-236">columnDelimiter</span></span> |<span data-ttu-id="3dc77-237">kolumner i hello loggfiler avgränsas med **kommatecken tecken (`,`)**</span><span class="sxs-lookup"><span data-stu-id="3dc77-237">columns in hello log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="3dc77-238">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="3dc77-238">frequency/interval</span></span> |<span data-ttu-id="3dc77-239">Ange frekvensen för**månad** och är **1**, vilket innebär att hello indata segment som är tillgängliga varje månad.</span><span class="sxs-lookup"><span data-stu-id="3dc77-239">frequency set too**Month** and interval is **1**, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="3dc77-240">extern</span><span class="sxs-lookup"><span data-stu-id="3dc77-240">external</span></span> | <span data-ttu-id="3dc77-241">Den här egenskapen anges för**SANT** om hello indata inte genereras av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-241">This property is set too**true** if hello input data is not generated by this pipeline.</span></span> <span data-ttu-id="3dc77-242">I den här självstudiekursen genereras hello input.log inte av denna pipeline, så vi ställa in hello egenskapen tootrue.</span><span class="sxs-lookup"><span data-stu-id="3dc77-242">In this tutorial, hello input.log file is not generated by this pipeline, so we set hello property tootrue.</span></span> |

    <span data-ttu-id="3dc77-243">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="3dc77-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="3dc77-244">Klicka på **distribuera** på hello i kommandofältet toodeploy hello nyskapad dataset.</span><span class="sxs-lookup"><span data-stu-id="3dc77-244">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span> <span data-ttu-id="3dc77-245">Du bör se hello datauppsättning i hello trädvyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-245">You should see hello dataset in hello tree view on hello left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="3dc77-246">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="3dc77-246">Create output dataset</span></span>
<span data-ttu-id="3dc77-247">Nu kan skapa du hello utdata dataset toorepresent hello utgående data som lagras i hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3dc77-247">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="3dc77-248">I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-248">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="3dc77-249">Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3dc77-249">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="3dc77-250">Hello JSON fragment du skapar en datauppsättning som kallas **AzureBlobOutput**, och ange hello strukturen för hello data som produceras av hello Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="3dc77-250">In hello JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying hello structure of hello data that is produced by hello Hive script.</span></span> <span data-ttu-id="3dc77-251">Dessutom kan du ange att hello resultat lagras i hello blob-behållaren som kallas **adfgetstarted** och hello mapp som heter **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-251">In addition, you specify that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="3dc77-252">Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje månad.</span><span class="sxs-lookup"><span data-stu-id="3dc77-252">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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
    <span data-ttu-id="3dc77-253">Se **skapa hello inkommande dataset** avsnitt beskrivs dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3dc77-253">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="3dc77-254">Du anger inte externa hello-egenskapen på en datamängd för utdata som hello dataset produceras av hello Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-254">You do not set hello external property on an output dataset as hello dataset is produced by hello Data Factory service.</span></span>
3. <span data-ttu-id="3dc77-255">Klicka på **distribuera** på hello i kommandofältet toodeploy hello nyskapad dataset.</span><span class="sxs-lookup"><span data-stu-id="3dc77-255">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span>
4. <span data-ttu-id="3dc77-256">Kontrollera att hello datauppsättningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="3dc77-256">Verify that hello dataset is created successfully.</span></span>

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="3dc77-258">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="3dc77-258">Create pipeline</span></span>
<span data-ttu-id="3dc77-259">I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="3dc77-260">Inkommande segmentet är tillgängliga månad (frekvens: månad, intervall: 1), utdata segment skapas varje månad och hello scheduler för hello aktiviteten också egenskapen toomonthly.</span><span class="sxs-lookup"><span data-stu-id="3dc77-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="3dc77-261">hello inställningar för hello utdatauppsättningen och hello aktivitet Schemaläggaren måste matcha.</span><span class="sxs-lookup"><span data-stu-id="3dc77-261">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="3dc77-262">Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-262">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="3dc77-263">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="3dc77-263">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="3dc77-264">hello-egenskaper som används i följande JSON hello beskrivs hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-264">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="3dc77-265">I hello **Data Factory-redigeraren**, klickar du på **ellips (...) Fler kommandon** och klicka sedan på **ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-265">In hello **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![knappen ny pipeline](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="3dc77-267">Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret.</span><span class="sxs-lookup"><span data-stu-id="3dc77-267">Copy and paste hello following snippet toohello Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="3dc77-268">Ersätt **storageaccountname** med hello namnet på ditt lagringskonto i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="3dc77-268">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
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
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    <span data-ttu-id="3dc77-269">Hello JSON kodutdrag skapar du en pipeline som består av en enskild aktivitet som använder Hive tooprocess Data på ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-269">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="3dc77-270">hello Hive-skriptfil, **partitionweblogs.hql**, lagras i hello Azure storage-konto (anges av hello scriptLinkedService, kallas **AzureStorageLinkedService**), och i  **skriptet** mapp i hello behållaren **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-270">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="3dc77-271">Hej **definierar** avsnitt är används toospecify hello runtime inställningarna som överförs toohello hive-skript som Hive konfigurationsvärden (t.ex. ${hiveconf: inputtable}, ${hiveconf:partitionedtable}).</span><span class="sxs-lookup"><span data-stu-id="3dc77-271">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="3dc77-272">Hej **starta** och **end** egenskaper för hello pipeline anger hello aktiva perioden för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-272">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="3dc77-273">I hello aktivitets-JSON, anger du den hello Hive-skript som körs på hello beräkning som anges av hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-273">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3dc77-274">Se ”Pipeline-JSON” i [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md) mer information om JSON-egenskaper som används i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="3dc77-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello example.</span></span>
   >
   >
3. <span data-ttu-id="3dc77-275">Bekräfta hello följande:</span><span class="sxs-lookup"><span data-stu-id="3dc77-275">Confirm hello following:</span></span>

   1. <span data-ttu-id="3dc77-276">**Input.log** filen finns på hello **inputdata** för hello **adfgetstarted** behållare i hello Azure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="3dc77-276">**input.log** file exists in hello **inputdata** folder of hello **adfgetstarted** container in hello Azure blob storage</span></span>
   2. <span data-ttu-id="3dc77-277">**partitionweblogs.hql** filen finns på hello **skriptet** för hello **adfgetstarted** behållare i hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="3dc77-277">**partitionweblogs.hql** file exists in hello **script** folder of hello **adfgetstarted** container in hello Azure blob storage.</span></span> <span data-ttu-id="3dc77-278">Fullständig hello nödvändiga steg i hello [kursen översikt](data-factory-build-your-first-pipeline.md) om du inte ser de här filerna.</span><span class="sxs-lookup"><span data-stu-id="3dc77-278">Complete hello prerequisite steps in hello [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="3dc77-279">Bekräfta att du har ersatt **storageaccountname** med hello namnet på ditt lagringskonto i hello pipeline-JSON.</span><span class="sxs-lookup"><span data-stu-id="3dc77-279">Confirm that you replaced **storageaccountname** with hello name of your storage account in hello pipeline JSON.</span></span>
4. <span data-ttu-id="3dc77-280">Klicka på **distribuera** på hello i kommandofältet toodeploy hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-280">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span> <span data-ttu-id="3dc77-281">Eftersom hello **starta** och **end** gånger ställs i hello senaste och **isPaused** är uppsättningen toofalse, hello pipeline (aktivitet i pipelinen hello) körs omedelbart när du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="3dc77-281">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="3dc77-282">Bekräfta att du ser hello pipeline i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="3dc77-282">Confirm that you see hello pipeline in hello tree view.</span></span>

    ![Trädvy med pipeline](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="3dc77-284">Grattis, du har skapat din första pipeline!</span><span class="sxs-lookup"><span data-stu-id="3dc77-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="3dc77-285">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="3dc77-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="3dc77-286">Övervaka pipeline med diagramvyn</span><span class="sxs-lookup"><span data-stu-id="3dc77-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="3dc77-287">Klicka på **X** tooclose Data Factory-redigeraren blad toonavigate tillbaka toohello Data Factory-bladet och på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-287">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="3dc77-289">I hello diagramvyn visas en översikt över hello pipelines och datauppsättningar som används i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="3dc77-289">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="3dc77-291">tooview alla aktiviteter i hello pipeline, högerklicka på pipeline i hello diagram och klicka på Öppna Pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-291">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="3dc77-293">Bekräfta att du ser hello HDInsightHive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-293">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="3dc77-295">toonavigate bakifrån toohello tidigare, klicka på **datafabriken** i hello dynamiska menyn hello överst.</span><span class="sxs-lookup"><span data-stu-id="3dc77-295">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
5. <span data-ttu-id="3dc77-296">I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-296">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="3dc77-297">Kontrollera att hello-segment i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3dc77-297">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="3dc77-298">Det kan ta några minuter för hello sektorn tooshow i tillståndet Ready.</span><span class="sxs-lookup"><span data-stu-id="3dc77-298">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="3dc77-299">Om det inte sker när du vänta ett tag, kan du se om du har hello indatafilen (input.log) placerade i rätt hello-behållaren (adfgetstarted) och mappen (inputdata).</span><span class="sxs-lookup"><span data-stu-id="3dc77-299">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="3dc77-301">Klicka på **X** tooclose **AzureBlobInput** bladet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-301">Click **X** tooclose **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="3dc77-302">I hello **diagramvyn**, dubbelklicka på hello dataset **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-302">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="3dc77-303">Du ser att hello-segment som håller på att behandlas.</span><span class="sxs-lookup"><span data-stu-id="3dc77-303">You see that hello slice that is currently being processed.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="3dc77-305">När bearbetningen är klar visas hello sektor i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3dc77-305">When processing is done, you see hello slice in **Ready** state.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="3dc77-307">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="3dc77-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="3dc77-308">Därför förvänta sig hello pipeline för ta **cirka 30 minuter** tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="3dc77-308">Therefore, expect hello pipeline too      take **approximately 30 minutes** tooprocess hello slice.</span></span>
   >
   >

9. <span data-ttu-id="3dc77-309">När hello sektorn är i **klar** tillstånd, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="3dc77-309">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

   ![utdata](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="3dc77-311">Klicka på hello sektorn toosee information om den i en **datasektorn** bladet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-311">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

   ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="3dc77-313">Klicka på en aktivitet som körs i hello **aktiviteten körs listan** toosee information om en aktivitet kör (Hive aktivitet i vårt scenario) i en **aktivitet köras information** fönster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-313">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="3dc77-315">Du kan se hello Hive-fråga som har utförts och statusinformation från hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="3dc77-315">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="3dc77-316">Dessa loggar är användbara vid felsökning av eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="3dc77-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="3dc77-317">Se artikeln [Övervaka och hantera pipelines med Azure-portalblad](data-factory-monitor-manage-pipelines.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3dc77-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3dc77-318">hello indatafilen hämtar bort när hello segment har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="3dc77-318">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="3dc77-319">Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="3dc77-319">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="3dc77-320">Övervaka pipeline med övervaknings- och hanteringsappen</span><span class="sxs-lookup"><span data-stu-id="3dc77-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="3dc77-321">Du kan också använda Övervakare och hantera program toomonitor din pipelines.</span><span class="sxs-lookup"><span data-stu-id="3dc77-321">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="3dc77-322">Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="3dc77-323">Klicka på **övervaka och hantera** panelen på hello startsidan för din data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-323">Click **Monitor & Manage** tile on hello home page for your data factory.</span></span>

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="3dc77-325">Du bör se **Övervakaren och hantera program**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="3dc77-326">Ändra hello **starttid** och **sluttiden** toomatch starta sluttider för din pipeline och på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-326">Change hello **Start time** and **End time** toomatch start and end times of your pipeline, and click **Apply**.</span></span>

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="3dc77-328">Välj en aktivitetsfönstret i hello **aktivitet Windows** listan toosee information om den.</span><span class="sxs-lookup"><span data-stu-id="3dc77-328">Select an activity window in hello **Activity Windows** list toosee details about it.</span></span>

    ![Information om aktivitetsfönstret](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="3dc77-330">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3dc77-330">Summary</span></span>
<span data-ttu-id="3dc77-331">I kursen får skapat du ett Azure data factory tooprocess data genom att köra Hive-skript på ett HDInsight hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3dc77-331">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="3dc77-332">Du har använt hello Data Factory-redigeraren i hello Azure portal toodo hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3dc77-332">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="3dc77-333">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="3dc77-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="3dc77-334">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="3dc77-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="3dc77-335">**Azure Storage** länkade tjänsten toolink din Azure blob-lagring som innehåller in-/ utdata filer toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-335">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="3dc77-336">**Azure HDInsight** på begäran länkade tjänsten toolink en på begäran HDInsight Hadoop-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-336">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="3dc77-337">Azure Data Factory skapar ett HDInsight Hadoop klustret just-in-time tooprocess indata och utdata för produkten.</span><span class="sxs-lookup"><span data-stu-id="3dc77-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="3dc77-338">Skapa två **datauppsättningar**, som beskriver inkommande och utgående data för HDInsight Hive aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="3dc77-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="3dc77-339">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3dc77-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3dc77-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3dc77-340">Next Steps</span></span>
<span data-ttu-id="3dc77-341">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="3dc77-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="3dc77-342">hur toouse en Kopieringsaktiviteten toocopy data från ett Azure Blob-tooAzure SQL, se toosee [Självstudier: kopiera data från ett Azure blob-tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="3dc77-342">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="3dc77-343">Se även</span><span class="sxs-lookup"><span data-stu-id="3dc77-343">See Also</span></span>
| <span data-ttu-id="3dc77-344">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="3dc77-344">Topic</span></span> | <span data-ttu-id="3dc77-345">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3dc77-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="3dc77-346">Pipelines</span><span class="sxs-lookup"><span data-stu-id="3dc77-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="3dc77-347">Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="3dc77-347">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="3dc77-348">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="3dc77-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="3dc77-349">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="3dc77-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="3dc77-350">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="3dc77-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="3dc77-351">Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="3dc77-351">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="3dc77-352">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="3dc77-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="3dc77-353">Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="3dc77-353">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
