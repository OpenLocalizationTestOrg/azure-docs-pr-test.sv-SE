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
ms.openlocfilehash: f90b158e45a3679210685765b23c8299eb76ed50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a><span data-ttu-id="8ffac-103">Självstudie: Skapa en pipeline med en kopieringsaktivitet med hjälp av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ffac-103">Tutorial: Create a pipeline with Copy Activity using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8ffac-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="8ffac-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="8ffac-105">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="8ffac-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="8ffac-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8ffac-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="8ffac-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ffac-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="8ffac-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ffac-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="8ffac-109">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="8ffac-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="8ffac-110">REST API</span><span class="sxs-lookup"><span data-stu-id="8ffac-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="8ffac-111">.NET-API</span><span class="sxs-lookup"><span data-stu-id="8ffac-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="8ffac-112">I den här artikeln får du lära dig hur du använder Microsoft Visual Studio för att skapa en datafabrik med en pipeline som kopierar data från en Azure-bloblagring till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-112">In this article, you learn how to use the Microsoft Visual Studio to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="8ffac-113">Om du inte har använt Azure Data Factory, bör du läsa igenom artikeln [Introduktion till Azure Data Factory](data-factory-introduction.md) innan du genomför den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="8ffac-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="8ffac-114">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="8ffac-115">Kopieringsaktiviteten kopierar data från källans datalager till mottagarens datalager.</span><span class="sxs-lookup"><span data-stu-id="8ffac-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="8ffac-116">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="8ffac-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="8ffac-117">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="8ffac-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="8ffac-118">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8ffac-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="8ffac-119">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="8ffac-120">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8ffac-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="8ffac-121">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="8ffac-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE] 
> <span data-ttu-id="8ffac-122">Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager.</span><span class="sxs-lookup"><span data-stu-id="8ffac-122">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="8ffac-123">Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster).</span><span class="sxs-lookup"><span data-stu-id="8ffac-123">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ffac-124">Krav</span><span class="sxs-lookup"><span data-stu-id="8ffac-124">Prerequisites</span></span>
1. <span data-ttu-id="8ffac-125">Läs igenom artikeln [Självstudier – översikt](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och slutför de **nödvändiga** stegen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-125">Read through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article and complete the **prerequisite** steps.</span></span>       
2. <span data-ttu-id="8ffac-126">Om du vill skapa Data Factory-instanser måste du vara medlem i [Data Factory-deltagarrollen](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) på gruppnivå/resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="8ffac-126">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
3. <span data-ttu-id="8ffac-127">Du måste ha följande installerat på datorn:</span><span class="sxs-lookup"><span data-stu-id="8ffac-127">You must have the following installed on your computer:</span></span> 
   * <span data-ttu-id="8ffac-128">Visual Studio 2013 eller Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="8ffac-128">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="8ffac-129">Hämta Azure SDK för Visual Studio 2013 eller Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="8ffac-129">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="8ffac-130">Gå till [Azures hämtningssida](https://azure.microsoft.com/downloads/) och klicka på **VS 2013** eller **VS 2015** i **.NET**-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-130">Navigate to [Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in the **.NET** section.</span></span>
   * <span data-ttu-id="8ffac-131">Hämta det senaste Azure Data Factory-plugin-programmet för Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) eller [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="8ffac-131">Download the latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="8ffac-132">Du kan även uppdatera plugin-programmet genom att göra följande: På menyn klickar du på **Verktyg** -> **Tillägg och uppdateringar** -> **Online** -> **Visual Studio-galleriet** -> **Microsoft Azure Data Factory-verktyg för Visual Studio** -> **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-132">You can also update the plugin by doing the following steps: On the menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

## <a name="steps"></a><span data-ttu-id="8ffac-133">Steg</span><span class="sxs-lookup"><span data-stu-id="8ffac-133">Steps</span></span>
<span data-ttu-id="8ffac-134">Här är de steg du utför som en del av de här självstudierna:</span><span class="sxs-lookup"><span data-stu-id="8ffac-134">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="8ffac-135">Skapa **länkade tjänster** i den här datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-135">Create **linked services** in the data factory.</span></span> <span data-ttu-id="8ffac-136">I det här steget kan du skapa två länkade tjänster: Azure Storage och Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-136">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="8ffac-137">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-137">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="8ffac-138">Du har skapat en behållare och överfört data till det här lagringskontot som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-138">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="8ffac-139">AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-139">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="8ffac-140">Data som kopieras från blob-lagringen sparas i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-140">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="8ffac-141">Du har skapat den SQL-tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-141">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>     
2. <span data-ttu-id="8ffac-142">Skapa **datauppsättningar** för indata och utdata i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-142">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="8ffac-143">Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8ffac-143">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="8ffac-144">Och en Azure Blob-datauppsättning anger vilken blobbehållare och mapp som innehåller data.</span><span class="sxs-lookup"><span data-stu-id="8ffac-144">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="8ffac-145">Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-145">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="8ffac-146">Och utdatauppsättningen för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-146">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
3. <span data-ttu-id="8ffac-147">Skapa en **pipeline** i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-147">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="8ffac-148">I det här steget kan du skapa en pipeline med en kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-148">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="8ffac-149">Kopieringsaktiviteten kopierar data från en Azure-blob till en tabell i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-149">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="8ffac-150">Du kan använda en kopieringsaktivitet i en pipeline för att kopiera data från alla datakällor som stöds till ett mål som stöds.</span><span class="sxs-lookup"><span data-stu-id="8ffac-150">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="8ffac-151">I avsnittet [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md#supported-data-stores-and-formats) finns en lista över datalager som stöds.</span><span class="sxs-lookup"><span data-stu-id="8ffac-151">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
4. <span data-ttu-id="8ffac-152">Skapa en Azure-**datafabrik** när du distribuerar Data Factory-enheter (länkade tjänster, datauppsättningar och tabeller och pipelines).</span><span class="sxs-lookup"><span data-stu-id="8ffac-152">Create an Azure **data factory** when deploying Data Factory entities (linked services, datasets/tables, and pipelines).</span></span> 

## <a name="create-visual-studio-project"></a><span data-ttu-id="8ffac-153">Skapa Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="8ffac-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="8ffac-154">Starta **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-154">Launch **Visual Studio 2015**.</span></span> <span data-ttu-id="8ffac-155">Klicka på **Arkiv**, peka på **Nytt** och klicka på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-155">Click **File**, point to **New**, and click **Project**.</span></span> <span data-ttu-id="8ffac-156">Dialogrutan **Nytt projekt** visas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-156">You should see the **New Project** dialog box.</span></span>  
2. <span data-ttu-id="8ffac-157">I dialogrutan **Nytt projekt** väljer du mallen **DataFactory** och klickar på **Tomt Data Factory-projekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-157">In the **New Project** dialog, select the **DataFactory** template, and click **Empty Data Factory Project**.</span></span>  
   
    ![Dialogrutan Nytt projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. <span data-ttu-id="8ffac-159">Ange namnet på projektet, platsen för lösningen och namnet för lösningen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-159">Specify the name of the project, location for the solution, and name of the solution, and then click **OK**.</span></span>
   
    ![Solution Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a><span data-ttu-id="8ffac-161">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="8ffac-161">Create linked services</span></span>
<span data-ttu-id="8ffac-162">Du kan skapa länkade tjänster i en datafabrik för att länka ditt datalager och beräkna datafabrik-tjänster.</span><span class="sxs-lookup"><span data-stu-id="8ffac-162">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="8ffac-163">I den här självstudiekursen kommer använder du inte någon beräkningstjänst, till exempel Azure HDInsight eller Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="8ffac-163">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="8ffac-164">Du använder två datalager av typen Azure Storage (källa) och Azure SQL Database (mål).</span><span class="sxs-lookup"><span data-stu-id="8ffac-164">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="8ffac-165">Därför kan du skapa två länkade tjänster: AzureStorage och AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="8ffac-165">Therefore, you create two linked services of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="8ffac-166">AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-166">The Azure Storage linked service links your Azure storage account to the data factory.</span></span> <span data-ttu-id="8ffac-167">Använd det lagringskonto i vilket du skapade en behållare och laddade upp data under [förberedelsestegen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-167">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="8ffac-168">AzureSQLLinkedService länkar din Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-168">Azure SQL linked service links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="8ffac-169">Data som kopieras från blob-lagringen sparas i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-169">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="8ffac-170">Du har skapat den tomma tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-170">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="8ffac-171">Länkade tjänster länkar datalager eller beräkningstjänster till en Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8ffac-171">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="8ffac-172">I [stödda datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) står alla källor och mottagare som stöds av Kopiera aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-172">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="8ffac-173">Se [Beräkna länkade tjänster](data-factory-compute-linked-services.md) för att se listan över Compute Services som stöds av Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8ffac-173">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory.</span></span> <span data-ttu-id="8ffac-174">I den här självstudiekursen använder du ingen tjänst för beräkning.</span><span class="sxs-lookup"><span data-stu-id="8ffac-174">In this tutorial, you do not use any compute service.</span></span> 

### <a name="create-the-azure-storage-linked-service"></a><span data-ttu-id="8ffac-175">Skapa den länkade Azure Storage-tjänsten</span><span class="sxs-lookup"><span data-stu-id="8ffac-175">Create the Azure Storage linked service</span></span>
1. <span data-ttu-id="8ffac-176">I **Solution Explorer** högerklickar du på **Länkade tjänster**, pekar på **Lägg till** och klickar på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-176">In **Solution Explorer**, right-click **Linked Services**, point to **Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="8ffac-177">I dialogrutan **Lägg till nytt objekt** väljer du **Länkad Azure Storage-tjänst** i listan och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-177">In the **Add New Item** dialog box, select **Azure Storage Linked Service** from the list, and click **Add**.</span></span> 
   
    ![Ny länkad tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. <span data-ttu-id="8ffac-179">Ersätt `<accountname>` och `<accountkey>`* med namnet på ditt Azure Storage-konto och dess nyckel.</span><span class="sxs-lookup"><span data-stu-id="8ffac-179">Replace `<accountname>` and `<accountkey>`* with the name of your Azure storage account and its key.</span></span> 
   
    ![Länkad Azure Storage-tjänst](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. <span data-ttu-id="8ffac-181">Spara filen **AzureStorageLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-181">Save the **AzureStorageLinkedService1.json** file.</span></span>

    <span data-ttu-id="8ffac-182">Läs mer om JSON-egenskaper i den länkade tjänstdefinitionen i artikeln [Anslutningsapp för Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8ffac-182">For more information about JSON properties in the linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-the-azure-sql-linked-service"></a><span data-ttu-id="8ffac-183">Skapa den länkade Azure SQL-tjänsten</span><span class="sxs-lookup"><span data-stu-id="8ffac-183">Create the Azure SQL linked service</span></span>
1. <span data-ttu-id="8ffac-184">Högerklicka på noden **Länkade tjänster** i **Solution Explorer** igen, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-184">Right-click on **Linked Services** node in the **Solution Explorer** again, point to **Add**, and click **New Item**.</span></span> 
2. <span data-ttu-id="8ffac-185">Den här gången väljer du **Länkad Azure SQL-tjänst** och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-185">This time, select **Azure SQL Linked Service**, and click **Add**.</span></span> 
3. <span data-ttu-id="8ffac-186">I filen **AzureSqlLinkedService1.json file** ersätter du `<servername>`, `<databasename>`, `<username@servername>` och `<password>` med namnen på din Azure SQL-server, databas, ditt användarkonto och lösenord.</span><span class="sxs-lookup"><span data-stu-id="8ffac-186">In the **AzureSqlLinkedService1.json file**, replace `<servername>`, `<databasename>`, `<username@servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span>    
4. <span data-ttu-id="8ffac-187">Spara filen **AzureSqlLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-187">Save the **AzureSqlLinkedService1.json** file.</span></span> 
    
    <span data-ttu-id="8ffac-188">Mer information om de här JSON-egenskaperna finns i [Anslutningsapp för Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8ffac-188">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>


## <a name="create-datasets"></a><span data-ttu-id="8ffac-189">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="8ffac-189">Create datasets</span></span>
<span data-ttu-id="8ffac-190">I det föregående steget skapade du kopplade tjänster för att länka ett Azure-lagringskonto och en Azure SQL-databas till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-190">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="8ffac-191">I det här steget definierar du två datauppsättningar – InputDataset och OutputDataset – som visar in- och utdata som lagras i de datalager som refereras till av AzureStorageLinkedService1 och AzureSqlLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="8ffac-191">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService1 and AzureSqlLinkedService1 respectively.</span></span>

<span data-ttu-id="8ffac-192">Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8ffac-192">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="8ffac-193">Och en indatauppsättning anger vilken blobbehållare och mapp som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="8ffac-193">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="8ffac-194">Den länkade Azure SQL-databasen anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-194">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="8ffac-195">Och utdatauppsättningen (OutputDataset) för SQL-tabellen anger tabellen i databasen som data kopieras till från blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-195">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="8ffac-196">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="8ffac-196">Create input dataset</span></span>
<span data-ttu-id="8ffac-197">I det här steget skapar du en datauppsättning med namnet InputDataset som pekar på en blobfil (emp.ext) i rotmappen i en blobbehållare (adftutorial) i Azure Storage som representeras av den länkade tjänsten AzureStorageLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="8ffac-197">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService1 linked service.</span></span> <span data-ttu-id="8ffac-198">Om du inte anger ett värde för filnamnet (eller hoppar över det), kommer data från alla blobbar i indatamappen att kopieras till målet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-198">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="8ffac-199">I den här kursen anger du ett värde för filnamnet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-199">In this tutorial, you specify a value for the fileName.</span></span> 

<span data-ttu-id="8ffac-200">Här kan använda du termen ”tabeller” i stället för ”datauppsättningar”.</span><span class="sxs-lookup"><span data-stu-id="8ffac-200">Here, you use the term "tables" rather than "datasets".</span></span> <span data-ttu-id="8ffac-201">En tabell är en rektangulär datauppsättning och är den enda typen av datauppsättning som stöds för tillfället.</span><span class="sxs-lookup"><span data-stu-id="8ffac-201">A table is a rectangular dataset and is the only type of dataset supported right now.</span></span> 

1. <span data-ttu-id="8ffac-202">Högerklicka på **Tabeller** i **Solution Explorer**, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-202">Right-click **Tables** in the **Solution Explorer**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="8ffac-203">I dialogrutan **Lägg till nytt objekt** väljer du **Azure-blobb** och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-203">In the **Add New Item** dialog box, select **Azure Blob**, and click **Add**.</span></span>   
3. <span data-ttu-id="8ffac-204">Ersätt texten JSON med följande text och spara filen **AzureBlobLocation1.json**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-204">Replace the JSON text with the following text and save the **AzureBlobLocation1.json** file.</span></span> 

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
    <span data-ttu-id="8ffac-205">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="8ffac-205">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="8ffac-206">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8ffac-206">Property</span></span> | <span data-ttu-id="8ffac-207">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8ffac-207">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="8ffac-208">typ</span><span class="sxs-lookup"><span data-stu-id="8ffac-208">type</span></span> | <span data-ttu-id="8ffac-209">Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-209">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="8ffac-210">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="8ffac-210">linkedServiceName</span></span> | <span data-ttu-id="8ffac-211">Refererar till **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8ffac-211">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="8ffac-212">folderPath</span><span class="sxs-lookup"><span data-stu-id="8ffac-212">folderPath</span></span> | <span data-ttu-id="8ffac-213">Anger vilken **blobbehållare** och **mapp** som innehåller indatablobbar.</span><span class="sxs-lookup"><span data-stu-id="8ffac-213">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="8ffac-214">I den här självstudiekursen adftutorial är blob-behållaren och -mappen rotmappen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-214">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="8ffac-215">fileName</span><span class="sxs-lookup"><span data-stu-id="8ffac-215">fileName</span></span> | <span data-ttu-id="8ffac-216">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="8ffac-216">This property is optional.</span></span> <span data-ttu-id="8ffac-217">Om du tar bort egenskapen kommer alla filer från folderPath hämtas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-217">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="8ffac-218">I den här självstudiekursen har angetts **emp.txt** som filnamn så att endast den filen hämtas för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="8ffac-218">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="8ffac-219">format -> typ</span><span class="sxs-lookup"><span data-stu-id="8ffac-219">format -> type</span></span> |<span data-ttu-id="8ffac-220">Indatafilen är i textformat, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-220">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="8ffac-221">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="8ffac-221">columnDelimiter</span></span> | <span data-ttu-id="8ffac-222">Kolumner i loggfilerna avgränsas med **kommatecken (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-222">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="8ffac-223">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="8ffac-223">frequency/interval</span></span> | <span data-ttu-id="8ffac-224">Frekvensen är **timme** och intervallet är **1**, vilket innebär att indatasektorerna är tillgängliga en gång i **timmen**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-224">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="8ffac-225">Det betyder att tjänsten Data Factory söker efter indata varje timme i rotmappen för den angivna blobbehållaren (**adftutorial**).</span><span class="sxs-lookup"><span data-stu-id="8ffac-225">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="8ffac-226">Den söker data i pipelinens start- och sluttider och inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="8ffac-226">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="8ffac-227">extern</span><span class="sxs-lookup"><span data-stu-id="8ffac-227">external</span></span> | <span data-ttu-id="8ffac-228">Den här egenskapen anges som **true** om indatan inte skapades av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="8ffac-228">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="8ffac-229">Inkommande data i den här självstudien finns i filen emp.txt som genereras av denna pipeline, så vi ställer in den här egenskapen på true.</span><span class="sxs-lookup"><span data-stu-id="8ffac-229">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="8ffac-230">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8ffac-230">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>   

### <a name="create-output-dataset"></a><span data-ttu-id="8ffac-231">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="8ffac-231">Create output dataset</span></span>
<span data-ttu-id="8ffac-232">I det här steget ska du skapa en utdatauppsättning med namnet **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-232">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="8ffac-233">Den här datauppsättningen pekar på en SQL-tabell i Azure SQL-databasen som representeras av **AzureSqlLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-233">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService1**.</span></span> 

1. <span data-ttu-id="8ffac-234">Högerklicka på **Tabeller** i **Solution Explorer** igen, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-234">Right-click **Tables** in the **Solution Explorer** again, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="8ffac-235">I dialogrutan **Lägg till nytt objekt** väljer du **Azure SQL** och klickar på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-235">In the **Add New Item** dialog box, select **Azure SQL**, and click **Add**.</span></span> 
3. <span data-ttu-id="8ffac-236">Ersätt texten JSON med följande JSON och spara filen **AzureSqlTableLocation1.json**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-236">Replace the JSON text with the following JSON and save the **AzureSqlTableLocation1.json** file.</span></span>

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
    <span data-ttu-id="8ffac-237">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="8ffac-237">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="8ffac-238">Egenskap</span><span class="sxs-lookup"><span data-stu-id="8ffac-238">Property</span></span> | <span data-ttu-id="8ffac-239">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="8ffac-239">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="8ffac-240">typ</span><span class="sxs-lookup"><span data-stu-id="8ffac-240">type</span></span> | <span data-ttu-id="8ffac-241">Typegenskapen är **AzureSqlTable** eftersom data kopieras till en tabell i en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-241">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="8ffac-242">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="8ffac-242">linkedServiceName</span></span> | <span data-ttu-id="8ffac-243">Refererar till **AzureSqlLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8ffac-243">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="8ffac-244">tableName</span><span class="sxs-lookup"><span data-stu-id="8ffac-244">tableName</span></span> | <span data-ttu-id="8ffac-245">Ange **tabellen** dit data kopieras.</span><span class="sxs-lookup"><span data-stu-id="8ffac-245">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="8ffac-246">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="8ffac-246">frequency/interval</span></span> | <span data-ttu-id="8ffac-247">Frekvensen är inställd på **timme** och intervallet är **1**, vilket innebär att utdatasegment produceras **varje timme** mellan pipelinens start- och sluttider, inte före eller efter dessa tider.</span><span class="sxs-lookup"><span data-stu-id="8ffac-247">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="8ffac-248">Det finns tre kolumner – **ID**, **FirstName** och **LastName** – i emp-tabellen i databasen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-248">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="8ffac-249">ID är en identitetskolumn, så du anger bara **FirstName** och **LastName** här.</span><span class="sxs-lookup"><span data-stu-id="8ffac-249">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="8ffac-250">Mer information om de här JSON-egenskaperna finns i artikeln [Azure SQL-anslutningsapp](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8ffac-250">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

## <a name="create-pipeline"></a><span data-ttu-id="8ffac-251">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="8ffac-251">Create pipeline</span></span>
<span data-ttu-id="8ffac-252">I det här steget ska du skapa en pipeline med en **kopieringsaktivitet** som använder **InputDataset** som indata och **OutputDataset** som utdata.</span><span class="sxs-lookup"><span data-stu-id="8ffac-252">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="8ffac-253">Schemat styrs för närvarande av utdatamängd.</span><span class="sxs-lookup"><span data-stu-id="8ffac-253">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="8ffac-254">I den här självstudiekursen är datamängden för utdata konfigurerad för att skapa ett segment en gång i timmen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-254">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="8ffac-255">Pipelinen har en starttid och sluttid som är en dag från varandra, vilket är 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="8ffac-255">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="8ffac-256">Därför produceras 24 segment för utdatauppsättningen av pipeline.</span><span class="sxs-lookup"><span data-stu-id="8ffac-256">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 

1. <span data-ttu-id="8ffac-257">Högerklicka på **Pipelines** i **Solution Explorer**, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-257">Right-click **Pipelines** in the **Solution Explorer**, point to **Add**, and click **New Item**.</span></span>  
2. <span data-ttu-id="8ffac-258">Välj **Kopiera datapipeline** i dialogrutan **Lägg till nytt objekt** och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-258">Select **Copy Data Pipeline** in the **Add New Item** dialog box and click **Add**.</span></span> 
3. <span data-ttu-id="8ffac-259">Ersätt JSON med följande JSON och spara filen **CopyActivity1.json**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-259">Replace the JSON with the following JSON and save the **CopyActivity1.json** file.</span></span>

  ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob to Azure SQL table",
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
    - <span data-ttu-id="8ffac-260">I avsnittet Aktiviteter finns det bara en aktivitet vars **typ** anges till **Kopia**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-260">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="8ffac-261">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8ffac-261">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="8ffac-262">I Data Factory-lösningar, kan du också använda [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-262">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="8ffac-263">Indata för aktiviteten är inställd på **InputDataset** och utdata för aktiviteten är inställd på **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-263">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="8ffac-264">I avsnittet för **typeProperties** har **BlobSource** angetts som källtyp och **SqlSink** har angetts som mottagartyp.</span><span class="sxs-lookup"><span data-stu-id="8ffac-264">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="8ffac-265">En fullständig lista över datakällor som stöds av kopieringsaktiviteten som källor och mottagare finns i [Datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="8ffac-265">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="8ffac-266">Klicka på länken i tabellen om du vill veta hur du använder ett visst datalager som stöds som källa/mottagare.</span><span class="sxs-lookup"><span data-stu-id="8ffac-266">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
     
    <span data-ttu-id="8ffac-267">Ersätt värdet i **start**egenskapen med den aktuella dagen och **slut**värdet med nästa dag.</span><span class="sxs-lookup"><span data-stu-id="8ffac-267">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="8ffac-268">Du kan ange endast datumdelen och hoppa över tidsvärdet.</span><span class="sxs-lookup"><span data-stu-id="8ffac-268">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="8ffac-269">Till exempel ”2016-02-03” som motsvarar ”2016-02-03T00:00:00Z”</span><span class="sxs-lookup"><span data-stu-id="8ffac-269">For example, "2016-02-03", which is equivalent to "2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="8ffac-270">Både start- och slutdatum måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="8ffac-270">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="8ffac-271">Exempel: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="8ffac-271">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="8ffac-272">**Sluttiden** är valfri, men vi använder den i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-272">The **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="8ffac-273">Om du inte anger värdet för **slut**egenskapen, beräknas det som ”**start + 48 timmar**”.</span><span class="sxs-lookup"><span data-stu-id="8ffac-273">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="8ffac-274">Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-274">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="8ffac-275">I det föregående exemplet finns det 24 datasektorer eftersom varje datasektor skapas varje timme.</span><span class="sxs-lookup"><span data-stu-id="8ffac-275">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="8ffac-276">Beskrivningar av JSON-egenskaper i en pipeline-definition finns i artikeln [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-276">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="8ffac-277">Beskrivningar av JSON-egenskaper i en kopieringsaktivitet-definition finns i artikeln [aktiviteter för dataflyttning](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-277">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="8ffac-278">Beskrivningar av JSON-egenskaper som stöds av BlobSource finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-278">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="8ffac-279">Beskrivningar av JSON-egenskaper som stöds av SqlSink finns i artikeln [Azure SQL Database-anslutningsapp](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="8ffac-279">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="8ffac-280">Publicera/distribuera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="8ffac-280">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="8ffac-281">I det här steget publicerar du Data Factory-enheter (länkade tjänster, datauppsättningar och pipeline) som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="8ffac-281">In this step, you publish Data Factory entities (linked services, datasets, and pipeline) you created earlier.</span></span> <span data-ttu-id="8ffac-282">Du kan även ange namnet på en ny datafabrik som ska skapas för dessa enheter.</span><span class="sxs-lookup"><span data-stu-id="8ffac-282">You also specify the name of the new data factory to be created to hold these entities.</span></span>  

1. <span data-ttu-id="8ffac-283">I Solution Explorer högerklickar du på projektet och klickar sedan på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-283">Right-click project in the Solution Explorer, and click **Publish**.</span></span> 
2. <span data-ttu-id="8ffac-284">Om du ser dialogrutan **Logga in på ditt Microsoft-konto**, anger du dina autentiseringsuppgifter för det konto som har Azure-prenumerationen. Klicka sedan på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-284">If you see **Sign in to your Microsoft account** dialog box, enter your credentials for the account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="8ffac-285">Du bör se följande dialogruta:</span><span class="sxs-lookup"><span data-stu-id="8ffac-285">You should see the following dialog box:</span></span>
   
   ![Dialogrutan Publicera](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. <span data-ttu-id="8ffac-287">På sidan Konfigurera datafabrik går du igenom följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ffac-287">In the Configure data factory page, do the following steps:</span></span> 
   
   1. <span data-ttu-id="8ffac-288">välj alternativet **Skapa ny Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-288">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="8ffac-289">Ange **VSTutorialFactory** som **namn**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-289">Enter **VSTutorialFactory** for **Name**.</span></span>  
      
      > [!IMPORTANT]
      > <span data-ttu-id="8ffac-290">Namnet på Azure Data Factory måste vara globalt unikt.</span><span class="sxs-lookup"><span data-stu-id="8ffac-290">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="8ffac-291">Ändra namnet på datafabriken (till exempel yournameVSTutorialFactory) och försök att publicera igen, om du får ett felmeddelande om namnet på datafabriken när du publicerar.</span><span class="sxs-lookup"><span data-stu-id="8ffac-291">If you receive an error about the name of data factory when publishing, change the name of the data factory (for example, yournameVSTutorialFactory) and try publishing again.</span></span> <span data-ttu-id="8ffac-292">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="8ffac-292">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>        
      > 
      > 
   3. <span data-ttu-id="8ffac-293">Välj din Azure-prenumeration i fältet **Prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-293">Select your Azure subscription for the **Subscription** field.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="8ffac-294">Om du inte ser någon prenumeration kontrollerar du att du har loggat in med ett konto som är en administratör eller en medadministratör för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-294">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of the subscription.</span></span>  
      > 
      > 
   4. <span data-ttu-id="8ffac-295">Välj **resursgrupp** för datafabriken som ska skapas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-295">Select the **resource group** for the data factory to be created.</span></span> 
   5. <span data-ttu-id="8ffac-296">Välj **region** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="8ffac-296">Select the **region** for the data factory.</span></span> <span data-ttu-id="8ffac-297">Endast regioner som stöds av tjänsten Data Factory visas i listrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffac-297">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
   6. <span data-ttu-id="8ffac-298">Klicka på **Nästa** för att växla till sidan **Publicera objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-298">Click **Next** to switch to the **Publish Items** page.</span></span>
      
       ![Konfigurera Data Factory-sida](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. <span data-ttu-id="8ffac-300">På sidan **Publicera objekt** kontrollerar du att alla datafabriksentiteter har valts. Klicka på **Nästa** för att växla till sidan **Sammanfattning**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-300">In the **Publish Items** page, ensure that all the Data Factories entities are selected, and click **Next** to switch to the **Summary** page.</span></span>
   
   ![Sidan Publish items (Publicera objekt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. <span data-ttu-id="8ffac-302">Granska sammanfattningen och klicka på **Nästa** för att starta distributionsprocessen och visa **Distributionsstatus**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-302">Review the summary and click **Next** to start the deployment process and view the **Deployment Status**.</span></span>
   
   ![Sidan Publish summary (Publicera översikt)](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. <span data-ttu-id="8ffac-304">På sidan **Distributionsstatus** bör du se statusen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-304">In the **Deployment Status** page, you should see the status of the deployment process.</span></span> <span data-ttu-id="8ffac-305">Klicka på Slutför när distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="8ffac-305">Click Finish after the deployment is done.</span></span>
 
   ![Statussida för distribution](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

<span data-ttu-id="8ffac-307">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="8ffac-307">Note the following points:</span></span> 

* <span data-ttu-id="8ffac-308">Om du får felet: ”Den här prenumerationen har inte registrerats för användning av namnområdet Microsoft.DataFactory” gör du något av följande och försöker att publicera igen:</span><span class="sxs-lookup"><span data-stu-id="8ffac-308">If you receive the error: "This subscription is not registered to use namespace Microsoft.DataFactory", do one of the following and try publishing again:</span></span> 
  
  * <span data-ttu-id="8ffac-309">I Azure PowerShell kör du följande kommando för att registrera Data Factory-providern.</span><span class="sxs-lookup"><span data-stu-id="8ffac-309">In Azure PowerShell, run the following command to register the Data Factory provider.</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="8ffac-310">Du kan köra följande kommando om du vill kontrollera att Data Factory-providern är registrerad.</span><span class="sxs-lookup"><span data-stu-id="8ffac-310">You can run the following command to confirm that the Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="8ffac-311">Logga in med Azure-prenumerationen i [Azure Portal](https://portal.azure.com) och navigera till ett Data Factory-blad (eller) skapa en datafabrik i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8ffac-311">Login using the Azure subscription into the [Azure portal](https://portal.azure.com) and navigate to a Data Factory blade (or) create a data factory in the Azure portal.</span></span> <span data-ttu-id="8ffac-312">Med den här åtgärden registreras providern automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8ffac-312">This action automatically registers the provider for you.</span></span>
* <span data-ttu-id="8ffac-313">Namnet på datafabriken kan komma att registreras som ett DNS-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="8ffac-313">The name of the data factory may be registered as a DNS name in the future and hence become publically visible.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ffac-314">Om du vill skapa Data Factory-instanser måste du vara administratör/medadministratör för Azure-prenumerationen</span><span class="sxs-lookup"><span data-stu-id="8ffac-314">To create Data Factory instances, you need to be a admin/co-admin of the Azure subscription</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="8ffac-315">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="8ffac-315">Monitor pipeline</span></span>
<span data-ttu-id="8ffac-316">Gå till startsidan för din datafabrik:</span><span class="sxs-lookup"><span data-stu-id="8ffac-316">Navigate to the home page for your data factory:</span></span>

1. <span data-ttu-id="8ffac-317">Logga in på [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8ffac-317">Log in to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8ffac-318">Klicka på **Fler tjänster** i den vänstra menyn och på **Datafabriker**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-318">Click **More services** on the left menu, and click **Data factories**.</span></span>

    ![Bläddra igenom datafabrikerna](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. <span data-ttu-id="8ffac-320">Börja skriva in namnet på din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8ffac-320">Start typing the name of your data factory.</span></span>

    ![Namnet på datafabriken](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. <span data-ttu-id="8ffac-322">Klicka på din datafabrik i resultatlistan för att visa startsidan för din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8ffac-322">Click your data factory in the results list to see the home page for your data factory.</span></span>

    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. <span data-ttu-id="8ffac-324">Se [Övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) för instruktioner om hur övervakar en pipeline och datauppsättningar som du har skapat i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="8ffac-324">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) to monitor the pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="8ffac-325">Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines.</span><span class="sxs-lookup"><span data-stu-id="8ffac-325">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span> 

## <a name="summary"></a><span data-ttu-id="8ffac-326">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="8ffac-326">Summary</span></span>
<span data-ttu-id="8ffac-327">I den här självstudien har du skapat en Azure-datafabrik som kopierar data från en Azure-blobb till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="8ffac-327">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="8ffac-328">Du använde Visual Studio till att skapa datafabriken, länkade tjänster, datauppsättningar och en pipeline.</span><span class="sxs-lookup"><span data-stu-id="8ffac-328">You used Visual Studio to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="8ffac-329">Här är de avancerade steg som du utförde i självstudierna:</span><span class="sxs-lookup"><span data-stu-id="8ffac-329">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="8ffac-330">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-330">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="8ffac-331">Du skapade **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="8ffac-331">Created **linked services**:</span></span>
   1. <span data-ttu-id="8ffac-332">En länkad **Azure Storage-**tjänst som länkar Azure Storage-kontot som innehåller indata.</span><span class="sxs-lookup"><span data-stu-id="8ffac-332">An **Azure Storage** linked service to link your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="8ffac-333">En länkad **Azure SQL**-tjänst som länkar din Azure SQL-databas som innehåller utdata.</span><span class="sxs-lookup"><span data-stu-id="8ffac-333">An **Azure SQL** linked service to link your Azure SQL database that holds the output data.</span></span> 
3. <span data-ttu-id="8ffac-334">Du skapade **datauppsättningar** som beskriver indata och utdata för pipelines.</span><span class="sxs-lookup"><span data-stu-id="8ffac-334">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="8ffac-335">Du skapade en **pipeline** med en **kopieringsaktivitet** med **BlobSource** som källa och **SqlSink** som mottagare.</span><span class="sxs-lookup"><span data-stu-id="8ffac-335">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span> 

<span data-ttu-id="8ffac-336">Om du vill se en självstudie som visar hur du omvandlar data med en HDInsight Hive-aktivitet i Azure HDInsight-klustret går du till [Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg din första pipeline för att omvandla data med Hadoop-kluster).</span><span class="sxs-lookup"><span data-stu-id="8ffac-336">To see how to use a HDInsight Hive Activity to transform data by using Azure HDInsight cluster, see [ Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="8ffac-337">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="8ffac-337">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="8ffac-338">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="8ffac-338">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 

## <a name="view-all-data-factories-in-server-explorer"></a><span data-ttu-id="8ffac-339">Visa datafabriker i Server Explorer</span><span class="sxs-lookup"><span data-stu-id="8ffac-339">View all data factories in Server Explorer</span></span>
<span data-ttu-id="8ffac-340">Det här avsnittet beskrivs hur du använder Server Explorer i Visual Studio för att visa alla datafabriker i din Azure-prenumeration och skapa ett Visual Studio-projekt utifrån en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8ffac-340">This section describes how to use the Server Explorer in Visual Studio to view all the data factories in your Azure subscription and create a Visual Studio project based on an existing data factory.</span></span> 

1. <span data-ttu-id="8ffac-341">I **Visual Studio** klickar du på **Visa** i menyn och därefter på **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-341">In **Visual Studio**, click **View** on the menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="8ffac-342">I fönstret Server Explorer expanderar du **Azure** och **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-342">In the Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="8ffac-343">Om du ser **Logga in till Visual Studio** anger du det **konto** som är associerat med din Azure-prenumeration och klickar på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-343">If you see **Sign in to Visual Studio**, enter the **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="8ffac-344">Ange **lösenordet** och klicka på **Logga in**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-344">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="8ffac-345">Visual Studio försöker hämta information om alla Azure-datafabriker i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8ffac-345">Visual Studio tries to get information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="8ffac-346">Du ser statusen för den här åtgärden i fönstret **Uppgiftslista för Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-346">You see the status of this operation in the **Data Factory Task List** window.</span></span>

    ![Server Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a><span data-ttu-id="8ffac-348">Skapa ett Visual Studio-projekt för en befintlig datafabrik</span><span class="sxs-lookup"><span data-stu-id="8ffac-348">Create a Visual Studio project for an existing data factory</span></span>

- <span data-ttu-id="8ffac-349">Högerklicka på en datafabrik i Server Explorer och välj **Exportera Data Factory till nytt projekt** om du vill skapa ett Visual Studio-projekt som baseras på en befintlig datafabrik.</span><span class="sxs-lookup"><span data-stu-id="8ffac-349">Right-click a data factory in Server Explorer, and select **Export Data Factory to New Project** to create a Visual Studio project based on an existing data factory.</span></span>

    ![Exportera datafabrik till ett VS-projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="8ffac-351">Uppdatera Data Factory-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8ffac-351">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="8ffac-352">Om du vill uppdatera Azure Data Factory-verktyg för Visual Studio går du igenom följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ffac-352">To update Azure Data Factory tools for Visual Studio, do the following steps:</span></span>

1. <span data-ttu-id="8ffac-353">Klicka på **Verktyg** i menyn och välj **Tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-353">Click **Tools** on the menu and select **Extensions and Updates**.</span></span> 
2. <span data-ttu-id="8ffac-354">Välj **Uppdateringar** i den vänstra rutan och välj sedan **Visual Studio-galleriet**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-354">Select **Updates** in the left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="8ffac-355">Välj **Azure Data Factory-verktyg för Visual Studio** och klicka på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-355">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="8ffac-356">Om du inte ser den här posten har du redan den senaste versionen av verktygen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-356">If you do not see this entry, you already have the latest version of the tools.</span></span> 

## <a name="use-configuration-files"></a><span data-ttu-id="8ffac-357">Använda konfigurationsfiler</span><span class="sxs-lookup"><span data-stu-id="8ffac-357">Use configuration files</span></span>
<span data-ttu-id="8ffac-358">Du kan använda konfigurationsfiler i Visual Studio för att konfigurera egenskaper för länkade tjänster/tabeller/pipelines olika för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="8ffac-358">You can use configuration files in Visual Studio to configure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="8ffac-359">Fundera på följande JSON-definition för en länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="8ffac-359">Consider the following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="8ffac-360">Att ange **connectionString** med olika värden för accountname och accountkey baseras på vilken miljö (Utveckling/Test/Produktion) som du distribuerar Data Factory-entiteter till.</span><span class="sxs-lookup"><span data-stu-id="8ffac-360">To specify **connectionString** with different values for accountname and accountkey based on the environment (Dev/Test/Production) to which you are deploying Data Factory entities.</span></span> <span data-ttu-id="8ffac-361">Du kan göra detta genom att använda separata konfigurationsfiler för varje miljö.</span><span class="sxs-lookup"><span data-stu-id="8ffac-361">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="8ffac-362">Lägga till en konfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="8ffac-362">Add a configuration file</span></span>
<span data-ttu-id="8ffac-363">Lägg till en konfigurationsfil för varje miljö genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ffac-363">Add a configuration file for each environment by performing the following steps:</span></span>   

1. <span data-ttu-id="8ffac-364">Högerklicka på Data Factory-projektet i Visual Studio-lösningen, peka på **Lägg till** och klicka på **Nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-364">Right-click the Data Factory project in your Visual Studio solution, point to **Add**, and click **New item**.</span></span>
2. <span data-ttu-id="8ffac-365">Välj **Config** i listan med installerade mallar till vänster, välj **Konfigurationsfil**, ange ett **namn** för konfigurationsfilen och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-365">Select **Config** from the list of installed templates on the left, select **Configuration File**, enter a **name** for the configuration file, and click **Add**.</span></span>

    ![Lägga till en konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="8ffac-367">Lägg till konfigurationsparametrar och deras värden i följande format:</span><span class="sxs-lookup"><span data-stu-id="8ffac-367">Add configuration parameters and their values in the following format:</span></span>

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

    <span data-ttu-id="8ffac-368">Det här exemplet anger egenskapen connectionString för en länkad Azure Storage-tjänst och en Azure SQL-länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="8ffac-368">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="8ffac-369">Observera att syntaxen för att ange namnet är [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="8ffac-369">Notice that the syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="8ffac-370">Om JSON har en egenskap med en värdematris enligt följande kod:</span><span class="sxs-lookup"><span data-stu-id="8ffac-370">If JSON has a property that has an array of values as shown in the following code:</span></span>  

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

    <span data-ttu-id="8ffac-371">Konfigurera egenskaper enligt följande konfigurationsfil (använd nollbaserad indexering):</span><span class="sxs-lookup"><span data-stu-id="8ffac-371">Configure properties as shown in the following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="8ffac-372">Egenskapsnamn med blanksteg</span><span class="sxs-lookup"><span data-stu-id="8ffac-372">Property names with spaces</span></span>
<span data-ttu-id="8ffac-373">Om ett egenskapsnamn innehåller blanksteg, använder du hakparenteser enligt följande exempel (databasservernamn):</span><span class="sxs-lookup"><span data-stu-id="8ffac-373">If a property name has spaces in it, use square brackets as shown in the following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="8ffac-374">Distribuera lösningen med en konfiguration</span><span class="sxs-lookup"><span data-stu-id="8ffac-374">Deploy solution using a configuration</span></span>
<span data-ttu-id="8ffac-375">När du publicerar Azure Data Factory-entiteter i VS, kan du ange den konfiguration som du vill använda för att publicera åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8ffac-375">When you are publishing Azure Data Factory entities in VS, you can specify the configuration that you want to use for that publishing operation.</span></span>

<span data-ttu-id="8ffac-376">Publicera entiteter i Azure Data Factory-projekt med hjälp av en konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="8ffac-376">To publish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="8ffac-377">Högerklicka på Data Factory-projektet och visa dialogrutan **Publicera objekt** genom att klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-377">Right-click Data Factory project and click **Publish** to see the **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="8ffac-378">Välj en befintlig datafabrik eller ange värden för att skapa en datafabrik på sidan **Konfigurera datafabrik**. Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-378">Select an existing data factory or specify values for creating a data factory on the **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="8ffac-379">På sidan **Publicera objekt**: Här visas en listruta med tillgängliga konfigurationer för fältet **Välj distributionskonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-379">On the **Publish Items** page: you see a drop-down list with available configurations for the **Select Deployment Config** field.</span></span>

    ![Välj konfigurationsfil](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="8ffac-381">Välj den **konfigurationsfil** som du vill använda och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-381">Select the **configuration file** that you would like to use and click **Next**.</span></span>
5. <span data-ttu-id="8ffac-382">Kontrollera att du ser namnet på JSON-filen på sidan **Sammanfattning** och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8ffac-382">Confirm that you see the name of JSON file in the **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="8ffac-383">Klicka på **Slutför** när distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="8ffac-383">Click **Finish** after the deployment operation is finished.</span></span>

<span data-ttu-id="8ffac-384">När du distribuerar används värden från konfigurationsfilen till att ange värden för egenskaper i JSON-filerna innan entiteterna distribueras till Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8ffac-384">When you deploy, the values from the configuration file are used to set values for properties in the JSON files before the entities are deployed to Azure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="8ffac-385">Använda Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="8ffac-385">Use Azure Key Vault</span></span>
<span data-ttu-id="8ffac-386">Det är inte tillrådligt och ofta mot säkerhetsprincipen införa känsliga data, till exempel anslutningssträngar, i kod-databasen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-386">It is not advisable and often against security policy to commit sensitive data such as connection strings to the code repository.</span></span> <span data-ttu-id="8ffac-387">Se exemplet [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) på GitHub att lära dig om att lagra känslig information i Azure Key Vault och använda den när du publicerar Data Factory-entiteter.</span><span class="sxs-lookup"><span data-stu-id="8ffac-387">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub to learn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="8ffac-388">Secure Publish-tillägget för Visual Studio gör det möjligt att lagra hemligheter i Key Vault och endast specificera referenser till dessa i konfigurationer för länkade tjänster/distributioner.</span><span class="sxs-lookup"><span data-stu-id="8ffac-388">The Secure Publish extension for Visual Studio allows the secrets to be stored in Key Vault and only references to them are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="8ffac-389">Dessa referenser matchas när du publicerar Data Factory-entiteter i Azure.</span><span class="sxs-lookup"><span data-stu-id="8ffac-389">These references are resolved when you publish Data Factory entities to Azure.</span></span> <span data-ttu-id="8ffac-390">Dessa filer kan sedan skickas till källdatabasen utan visa hemligheter.</span><span class="sxs-lookup"><span data-stu-id="8ffac-390">These files can then be committed to source repository without exposing any secrets.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8ffac-391">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8ffac-391">Next steps</span></span>
<span data-ttu-id="8ffac-392">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="8ffac-392">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="8ffac-393">Följande tabell innehåller en lista över datalager som stöds som källor och mål av kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="8ffac-393">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="8ffac-394">För mer information om hur du kopierar data till/från ett datalager klickar du på länken för datalagret i tabellen.</span><span class="sxs-lookup"><span data-stu-id="8ffac-394">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>