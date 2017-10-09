---
title: aaaSQL Server lagrade Proceduraktiviteten
description: "Lär dig hur du kan använda hello lagrade Proceduraktiviteten i SQL Server-tooinvoke en lagrad procedur i en Azure SQL Database eller Azure SQL Data Warehouse från Data Factory-pipelinen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="0395f-103">SQLServer lagrade Proceduraktiviteten</span><span class="sxs-lookup"><span data-stu-id="0395f-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="0395f-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="0395f-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="0395f-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="0395f-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="0395f-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="0395f-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="0395f-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="0395f-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="0395f-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="0395f-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="0395f-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="0395f-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="0395f-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="0395f-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="0395f-114">Overview</span></span>
<span data-ttu-id="0395f-115">Du kan använda data transformation aktiviteter i en Datafabrik [pipeline](data-factory-create-pipelines.md) tootransform och bearbeta rådata i förutsägelser och insikter.</span><span class="sxs-lookup"><span data-stu-id="0395f-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) tootransform and process raw data into predictions and insights.</span></span> <span data-ttu-id="0395f-116">hello är lagrade Proceduraktiviteten en av hello omvandling av aktiviteter som har stöd för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0395f-116">hello Stored Procedure Activity is one of hello transformation activities that Data Factory supports.</span></span> <span data-ttu-id="0395f-117">Den här artikeln bygger på hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och hello stöd för omvandling av aktiviteter i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="0395f-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="0395f-118">Du kan använda hello lagrade Proceduraktiviteten tooinvoke en lagrad procedur i någon av hello följande data lagras i företaget, eller på en Azure-dator (VM):</span><span class="sxs-lookup"><span data-stu-id="0395f-118">You can use hello Stored Procedure Activity tooinvoke a stored procedure in one of hello following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="0395f-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="0395f-119">Azure SQL Database</span></span>
- <span data-ttu-id="0395f-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="0395f-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="0395f-121">SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="0395f-121">SQL Server Database.</span></span>  <span data-ttu-id="0395f-122">Om du använder SQL Server, installera Data Management Gateway på hello samma datorn att värdar hello databas eller på en separat dator som har åtkomst till toohello databas.</span><span class="sxs-lookup"><span data-stu-id="0395f-122">If you are using SQL Server, install Data Management Gateway on hello same machine that hosts hello database or on a separate machine that has access toohello database.</span></span> <span data-ttu-id="0395f-123">Data Management Gateway är en komponent som ansluter datakällor på lokala/på virtuella Azure-datorn med molntjänster i en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="0395f-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="0395f-124">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="0395f-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0395f-125">När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur med hjälp av hello **sqlWriterStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0395f-125">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="0395f-126">Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="0395f-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="0395f-127">Information om hello egenskapen finns i följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0395f-127">For details about hello property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="0395f-128">Anropa en lagrad procedur vid kopiering av data till en Azure SQL Data Warehouse med hjälp av en kopieringsaktiviteten stöds inte.</span><span class="sxs-lookup"><span data-stu-id="0395f-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="0395f-129">Men du kan använda hello lagrade proceduren aktiviteten tooinvoke en lagrad procedur i en SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0395f-129">But, you can use hello stored procedure activity tooinvoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="0395f-130">När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i kopiera aktiviteten tooinvoke en lagrad procedur tooread data från hello källdatabasen med hjälp av hello  **sqlReaderStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0395f-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="0395f-131">Mer information finns i hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="0395f-131">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="0395f-132">hello följande genomgången använder hello lagrade Proceduraktiviteten i pipeline-tooinvoke en lagrad procedur i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0395f-132">hello following walkthrough uses hello Stored Procedure Activity in a pipeline tooinvoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="0395f-133">Genomgång</span><span class="sxs-lookup"><span data-stu-id="0395f-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="0395f-134">Exempel på en tabell och lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="0395f-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="0395f-135">Skapa följande hello **tabellen** i Azure SQL-databasen med hjälp av SQL Server Management Studio eller andra verktyg som du är nöjd med.</span><span class="sxs-lookup"><span data-stu-id="0395f-135">Create hello following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="0395f-136">Hej datetimestamp kolumnen är hello datum och tid när motsvarande hello-ID har genererats.</span><span class="sxs-lookup"><span data-stu-id="0395f-136">hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    <span data-ttu-id="0395f-137">ID är hello unika identifieras och hello datetimestamp kolumnen är hello datum och tid när motsvarande hello-ID har genererats.</span><span class="sxs-lookup"><span data-stu-id="0395f-137">Id is hello unique identified and hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>
    
    ![Exempeldata](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="0395f-139">I det här exemplet är hello lagrad procedur i en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0395f-139">In this sample, hello stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="0395f-140">Om hello lagrade proceduren finns i en Azure SQL Data Warehouse och SQL Server-databas, liknar hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="0395f-140">If hello stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, hello approach is similar.</span></span> <span data-ttu-id="0395f-141">SQL Server-databas måste du installera en [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="0395f-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="0395f-142">Skapa följande hello **lagrade proceduren** som infogar data i toohello **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="0395f-142">Create hello following **stored procedure** that inserts data in toohello **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="0395f-143">**Namnet** och **skiftläge** av hello parametern (DateTime i det här exemplet) måste matcha parameter som anges i hello pipeline/aktivitets-JSON.</span><span class="sxs-lookup"><span data-stu-id="0395f-143">**Name** and **casing** of hello parameter (DateTime in this example) must match that of parameter specified in hello pipeline/activity JSON.</span></span> <span data-ttu-id="0395f-144">I hello lagrade Procedurdefinition, se till att  **@**  används som ett prefix för hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="0395f-144">In hello stored procedure definition, ensure that **@** is used as a prefix for hello parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="0395f-145">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="0395f-145">Create a data factory</span></span>
1. <span data-ttu-id="0395f-146">Logga in för[Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0395f-146">Log in too[Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="0395f-147">Klicka på **ny** på hello vänstra menyn **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="0395f-147">Click **NEW** on hello left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="0395f-149">I hello **nya datafabriken** bladet ange **SProcDF** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="0395f-149">In hello **New data factory** blade, enter **SProcDF** for hello Name.</span></span> <span data-ttu-id="0395f-150">Azure Data Factory-namn är **globalt unika**.</span><span class="sxs-lookup"><span data-stu-id="0395f-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="0395f-151">Du måste tooprefix hello namn i hello data factory med ditt namn, tooenable hello har skapats av hello fabriken.</span><span class="sxs-lookup"><span data-stu-id="0395f-151">You need tooprefix hello name of hello data factory with your name, tooenable hello successful creation of hello factory.</span></span>

   ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="0395f-153">Välj din **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="0395f-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="0395f-154">För **resursgruppen**, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="0395f-154">For **Resource Group**, do one of hello following steps:</span></span>
   1. <span data-ttu-id="0395f-155">Klicka på **Skapa nytt** och ange ett namn för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0395f-155">Click **Create new** and enter a name for hello resource group.</span></span>
   2. <span data-ttu-id="0395f-156">Klicka på **använda befintliga** och välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0395f-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="0395f-157">Välj hello **plats** för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="0395f-157">Select hello **location** for hello data factory.</span></span>
7. <span data-ttu-id="0395f-158">Välj **PIN-kod toodashboard** så att du kan se hello data factory på hello instrumentpanelen nästa gång du loggar in.</span><span class="sxs-lookup"><span data-stu-id="0395f-158">Select **Pin toodashboard** so that you can see hello data factory on hello dashboard next time you log in.</span></span>
8. <span data-ttu-id="0395f-159">Klicka på **skapa** på hello **nya data factory** bladet.</span><span class="sxs-lookup"><span data-stu-id="0395f-159">Click **Create** on hello **New data factory** blade.</span></span>
9. <span data-ttu-id="0395f-160">Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0395f-160">You see hello data factory being created in hello **dashboard** of hello Azure portal.</span></span> <span data-ttu-id="0395f-161">När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="0395f-161">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![Data Factory-startsida](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="0395f-163">Skapa en Azure SQL-länkade tjänst</span><span class="sxs-lookup"><span data-stu-id="0395f-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="0395f-164">När du har skapat hello data factory, kan du skapa en Azure SQL-länkade tjänst som länkar din Azure SQL-databas som innehåller hello sampletable tabell och sp_sample lagrad procedur, tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="0395f-164">After creating hello data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains hello sampletable table and sp_sample stored procedure, tooyour data factory.</span></span>

1. <span data-ttu-id="0395f-165">Klicka på **författare och distribuera** på hello **Datafabriken** bladet för **SProcDF** toolaunch hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0395f-165">Click **Author and deploy** on hello **Data Factory** blade for **SProcDF** toolaunch hello Data Factory Editor.</span></span>
2. <span data-ttu-id="0395f-166">Klicka på **Nytt datalager** hello kommandofältet och välj **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="0395f-166">Click **New data store** on hello command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="0395f-167">Du bör se hello JSON-skript för att skapa en Azure SQL länkade tjänsten i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="0395f-167">You should see hello JSON script for creating an Azure SQL linked service in hello editor.</span></span>

   ![Nytt datalager](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="0395f-169">Gör följande ändringar hello i hello JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="0395f-169">In hello JSON script, make hello following changes:</span></span>

   1. <span data-ttu-id="0395f-170">Ersätt `<servername>` med hello namnet på din Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="0395f-170">Replace `<servername>` with hello name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="0395f-171">Ersätt `<databasename>` med hello-databas som du skapade hello tabell och hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="0395f-171">Replace `<databasename>` with hello database in which you created hello table and hello stored procedure.</span></span>
   3. <span data-ttu-id="0395f-172">Ersätt `<username@servername>` med hello-användarkonto som har åtkomst till toohello databas.</span><span class="sxs-lookup"><span data-stu-id="0395f-172">Replace `<username@servername>` with hello user account that has access toohello database.</span></span>
   4. <span data-ttu-id="0395f-173">Ersätt `<password>` med hello lösenordet för användarkontot för hello.</span><span class="sxs-lookup"><span data-stu-id="0395f-173">Replace `<password>` with hello password for hello user account.</span></span>

      ![Nytt datalager](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="0395f-175">toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="0395f-175">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="0395f-176">Bekräfta att du ser hello AzureSqlLinkedService i hello trädet visa hello vänster.</span><span class="sxs-lookup"><span data-stu-id="0395f-176">Confirm that you see hello AzureSqlLinkedService in hello tree view on hello left.</span></span>

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="0395f-178">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="0395f-178">Create an output dataset</span></span>
<span data-ttu-id="0395f-179">Du måste ange en datamängd för utdata för en lagrad procedur aktivitet även om hello lagrade proceduren inte skapar några data.</span><span class="sxs-lookup"><span data-stu-id="0395f-179">You must specify an output dataset for a stored procedure activity even if hello stored procedure does not produce any data.</span></span> <span data-ttu-id="0395f-180">Det beror på att dess hello utdatauppsättningen som enheter hello schemat för hello aktivitet (hur ofta hello aktiviteten körs - varje timme, varje dag, etc.).</span><span class="sxs-lookup"><span data-stu-id="0395f-180">That's because it's hello output dataset that drives hello schedule of hello activity (how often hello activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="0395f-181">Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun.</span><span class="sxs-lookup"><span data-stu-id="0395f-181">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="0395f-182">Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="0395f-182">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="0395f-183">Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset.</span><span class="sxs-lookup"><span data-stu-id="0395f-183">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="0395f-184">Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="0395f-184">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="0395f-185">I vissa fall hello utdatauppsättningen kan vara en **dummy dataset** (en datamängd som pekar tooa tabell som inte innehåller utdata från hello verkligen lagrade proceduren).</span><span class="sxs-lookup"><span data-stu-id="0395f-185">In some cases, hello output dataset can be a **dummy dataset** (a dataset that points tooa table that does not really hold output of hello stored procedure).</span></span> <span data-ttu-id="0395f-186">Den här dummy datauppsättningen används bara för toospecify hello schema för att köra hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-186">This dummy dataset is used only toospecify hello schedule for running hello stored procedure activity.</span></span> 

1. <span data-ttu-id="0395f-187">Klicka på **... Flera** på hello verktygsfältet **ny datamängd**, och klicka på **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="0395f-187">Click **... More** on hello toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="0395f-188">**Ny datamängd** på hello kommandot och välj **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="0395f-188">**New dataset** on hello command bar and select **Azure SQL**.</span></span>

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="0395f-190">Kopiera och klistra in hello följande JSON-skript i toohello JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="0395f-190">Copy/paste hello following JSON script in toohello JSON editor.</span></span>

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="0395f-191">toodeploy Hej dataset, klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="0395f-191">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="0395f-192">Bekräfta att du ser hello datauppsättning i hello trädvyn.</span><span class="sxs-lookup"><span data-stu-id="0395f-192">Confirm that you see hello dataset in hello tree view.</span></span>

    ![Trädvy med länkade tjänster](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="0395f-194">Skapa en pipeline med SqlServerStoredProcedure aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="0395f-195">Nu ska vi skapa en pipeline med en lagrad procedur-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0395f-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="0395f-196">Observera hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0395f-196">Notice hello following properties:</span></span> 

- <span data-ttu-id="0395f-197">Hej **typen** egenskapen för**SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="0395f-197">hello **type** property is set too**SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="0395f-198">Hej **storedProcedureName** i typen ange egenskaper för**sp_sample** (namnet på hello lagrade proceduren).</span><span class="sxs-lookup"><span data-stu-id="0395f-198">hello **storedProcedureName** in type properties is set too**sp_sample** (name of hello stored procedure).</span></span>
- <span data-ttu-id="0395f-199">Hej **storedProcedureParameters** avsnittet innehåller en parameter med namnet **DataTime**.</span><span class="sxs-lookup"><span data-stu-id="0395f-199">hello **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="0395f-200">Namn och skiftläge för hello-parametern i JSON måste matcha hello namn och versaler och gemener i hello-parametern i hello lagrade Procedurdefinition.</span><span class="sxs-lookup"><span data-stu-id="0395f-200">Name and casing of hello parameter in JSON must match hello name and casing of hello parameter in hello stored procedure definition.</span></span> <span data-ttu-id="0395f-201">Om du behöver lägga till null för en parameter, använder hello syntax: `"param1": null` (gemener).</span><span class="sxs-lookup"><span data-stu-id="0395f-201">If you need pass null for a parameter, use hello syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="0395f-202">Klicka på **... Flera** hello kommandofältet och klicka på **ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="0395f-202">Click **... More** on hello command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="0395f-203">Kopiera och klistra in följande JSON-fragment hello:</span><span class="sxs-lookup"><span data-stu-id="0395f-203">Copy/paste hello following JSON snippet:</span></span>   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. <span data-ttu-id="0395f-204">toodeploy hello pipeline, klickar du på **distribuera** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="0395f-204">toodeploy hello pipeline, click **Deploy** on hello toolbar.</span></span>  

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="0395f-205">Övervakaren hello pipeline</span><span class="sxs-lookup"><span data-stu-id="0395f-205">Monitor hello pipeline</span></span>
1. <span data-ttu-id="0395f-206">Klicka på **X** tooclose Data Factory-redigeraren blad toonavigate tillbaka toohello Data Factory-bladet och på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="0395f-206">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="0395f-208">I hello **diagramvyn**du se en översikt över hello pipelines och datauppsättningar som används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0395f-208">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="0395f-210">Dubbelklicka på hello dataset i hello diagramvyn, `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="0395f-210">In hello Diagram View, double-click hello dataset `sprocsampleout`.</span></span> <span data-ttu-id="0395f-211">Du ser hello segment i tillståndet Ready.</span><span class="sxs-lookup"><span data-stu-id="0395f-211">You see hello slices in Ready state.</span></span> <span data-ttu-id="0395f-212">Det bör finnas fem segment eftersom ett segment skapas för varje timme mellan hello starttid och sluttid från hello JSON.</span><span class="sxs-lookup"><span data-stu-id="0395f-212">There should be five slices because a slice is produced for each hour between hello start time and end time from hello JSON.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="0395f-214">När ett segment är i **klar** tillstånd, kör en `select * from sampletable` frågan mot hello Azure SQL database tooverify som hello data infogades i tabellen toohello av hello lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="0395f-214">When a slice is in **Ready** state, run a `select * from sampletable` query against hello Azure SQL database tooverify that hello data was inserted in toohello table by hello stored procedure.</span></span>

   ![utdata](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="0395f-216">Se [övervakaren hello pipeline](data-factory-monitor-manage-pipelines.md) detaljerad information om hur du övervakar Azure Data Factory pipelines.</span><span class="sxs-lookup"><span data-stu-id="0395f-216">See [Monitor hello pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="0395f-217">Ange en inkommande datauppsättning</span><span class="sxs-lookup"><span data-stu-id="0395f-217">Specify an input dataset</span></span>
<span data-ttu-id="0395f-218">I hello genomgången har lagrade proceduraktiviteten inte några inkommande datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="0395f-218">In hello walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="0395f-219">Om du anger en inkommande datauppsättning hello lagrade proceduraktiviteten körs inte förrän hello segment av inkommande dataset är tillgänglig (i tillståndet Ready).</span><span class="sxs-lookup"><span data-stu-id="0395f-219">If you specify an input dataset, hello stored procedure activity does not run until hello slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="0395f-220">hello dataset kan vara en externa datauppsättningen (som inte tillverkas av en annan aktivitet i hello samma rörledning) eller en intern datauppsättning som produceras av en överordnad aktivitet (hello aktivitet som körs före den här aktiviteten).</span><span class="sxs-lookup"><span data-stu-id="0395f-220">hello dataset can be an external dataset (that is not produced by another activity in hello same pipeline) or an internal dataset that is produced by an upstream activity (hello activity that runs before this activity).</span></span> <span data-ttu-id="0395f-221">Du kan ange flera indatauppsättningar för hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-221">You can specify multiple input datasets for hello stored procedure activity.</span></span> <span data-ttu-id="0395f-222">Om du gör det hello lagrade proceduraktiviteten körs bara när alla hello inkommande dataset-segment är tillgängliga (statusen Ready).</span><span class="sxs-lookup"><span data-stu-id="0395f-222">If you do so, hello stored procedure activity runs only when all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="0395f-223">hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter.</span><span class="sxs-lookup"><span data-stu-id="0395f-223">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="0395f-224">Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-224">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="0395f-225">Länkning med andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="0395f-225">Chaining with other activities</span></span>
<span data-ttu-id="0395f-226">Ange hello utdata från hello uppströmsaktivitet som indata för den här aktiviteten om du vill toochain en överordnad aktivitet med den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-226">If you want toochain an upstream activity with this activity, specify hello output of hello upstream activity as an input of this activity.</span></span> <span data-ttu-id="0395f-227">När du gör det hello lagrade proceduraktiviteten körs inte förrän hello överordnade aktiviteten har slutförts och hello utdatauppsättningen hello överordnad aktivitet är tillgänglig (i redo status).</span><span class="sxs-lookup"><span data-stu-id="0395f-227">When you do so, hello stored procedure activity does not run until hello upstream activity completes and hello output dataset of hello upstream activity is available (in Ready status).</span></span> <span data-ttu-id="0395f-228">Du kan ange utdata-datauppsättningar på flera överordnade aktiviteter som inkommande datauppsättningar på hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-228">You can specify output datasets of multiple upstream activities as input datasets of hello stored procedure activity.</span></span> <span data-ttu-id="0395f-229">När du gör det hello lagrade proceduraktiviteten körs bara när alla hello inkommande dataset-segment är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="0395f-229">When you do so, hello stored procedure activity runs only when all hello input dataset slices are available.</span></span>  

<span data-ttu-id="0395f-230">I följande exempel hello, hello utdata från hello kopieringsaktiviteten är: OutputDataset som utgör indata för hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-230">In hello following example, hello output of hello copy activity is: OutputDataset, which is an input of hello stored procedure activity.</span></span> <span data-ttu-id="0395f-231">Därför hello lagrade proceduraktiviteten körs inte förrän hello kopieringsaktiviteten har slutförts och hello OutputDataset sektorn är tillgänglig (i tillståndet Ready).</span><span class="sxs-lookup"><span data-stu-id="0395f-231">Therefore, hello stored procedure activity does not run until hello copy activity completes and hello OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="0395f-232">Om du anger flera indatauppsättningar hello lagrade proceduraktiviteten körs inte förrän alla hello inkommande dataset-segment är tillgängliga (statusen Ready).</span><span class="sxs-lookup"><span data-stu-id="0395f-232">If you specify multiple input datasets, hello stored procedure activity does not run until all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="0395f-233">Hej indatauppsättningar kan inte användas direkt som parametrar toohello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-233">hello input datasets cannot be used directly as parameters toohello stored procedure activity.</span></span> 

<span data-ttu-id="0395f-234">Mer information om länkning aktiviteter finns [flera aktiviteter i en pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="0395f-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

<span data-ttu-id="0395f-235">På liknande sätt toolink hello store proceduraktiviteten med **underordnade aktiviteter** (hello aktiviteter som körs efter hello lagrade proceduraktiviteten har slutförts), ange hello datamängd för utdata för hello lagrade proceduraktiviteten som en Ange hello underordnad aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="0395f-235">Similarly, toolink hello store procedure activity with **downstream activities** (hello activities that run after hello stored procedure activity completes), specify hello output dataset of hello stored procedure activity as an input of hello downstream activity in hello pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0395f-236">När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera hello **SqlSink** i kopiera aktiviteten tooinvoke en lagrad procedur med hjälp av hello **sqlWriterStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0395f-236">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="0395f-237">Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="0395f-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="0395f-238">Mer information om hello egenskap finns hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="0395f-238">For details about hello property, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="0395f-239">När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i kopiera aktiviteten tooinvoke en lagrad procedur tooread data från hello källdatabasen med hjälp av hello  **sqlReaderStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0395f-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="0395f-240">Mer information finns i hello följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="0395f-240">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="0395f-241">JSON-format</span><span class="sxs-lookup"><span data-stu-id="0395f-241">JSON format</span></span>
<span data-ttu-id="0395f-242">Här är hello JSON-format för att definiera en lagrade Proceduraktiviteten:</span><span class="sxs-lookup"><span data-stu-id="0395f-242">Here is hello JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="0395f-243">hello följande tabell beskrivs egenskaperna JSON:</span><span class="sxs-lookup"><span data-stu-id="0395f-243">hello following table describes these JSON properties:</span></span>

| <span data-ttu-id="0395f-244">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0395f-244">Property</span></span> | <span data-ttu-id="0395f-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0395f-245">Description</span></span> | <span data-ttu-id="0395f-246">Krävs</span><span class="sxs-lookup"><span data-stu-id="0395f-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0395f-247">namn</span><span class="sxs-lookup"><span data-stu-id="0395f-247">name</span></span> | <span data-ttu-id="0395f-248">Namnet på hello-aktivitet</span><span class="sxs-lookup"><span data-stu-id="0395f-248">Name of hello activity</span></span> |<span data-ttu-id="0395f-249">Ja</span><span class="sxs-lookup"><span data-stu-id="0395f-249">Yes</span></span> |
| <span data-ttu-id="0395f-250">description</span><span class="sxs-lookup"><span data-stu-id="0395f-250">description</span></span> |<span data-ttu-id="0395f-251">Text som beskriver vilka hello-aktivitet används för</span><span class="sxs-lookup"><span data-stu-id="0395f-251">Text describing what hello activity is used for</span></span> |<span data-ttu-id="0395f-252">Nej</span><span class="sxs-lookup"><span data-stu-id="0395f-252">No</span></span> |
| <span data-ttu-id="0395f-253">typ</span><span class="sxs-lookup"><span data-stu-id="0395f-253">type</span></span> | <span data-ttu-id="0395f-254">Måste anges till: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="0395f-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="0395f-255">Ja</span><span class="sxs-lookup"><span data-stu-id="0395f-255">Yes</span></span> |
| <span data-ttu-id="0395f-256">Indata</span><span class="sxs-lookup"><span data-stu-id="0395f-256">inputs</span></span> | <span data-ttu-id="0395f-257">Valfri.</span><span class="sxs-lookup"><span data-stu-id="0395f-257">Optional.</span></span> <span data-ttu-id="0395f-258">Om du anger en inkommande datauppsättning, måste det vara tillgänglig (statusen ”klar”) för hello lagrade proceduren aktiviteten toorun.</span><span class="sxs-lookup"><span data-stu-id="0395f-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="0395f-259">hello inkommande dataset kan inte användas i hello lagrade procedur som en parameter.</span><span class="sxs-lookup"><span data-stu-id="0395f-259">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="0395f-260">Det är bara använda toocheck hello beroende innan start hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-260">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> |<span data-ttu-id="0395f-261">Nej</span><span class="sxs-lookup"><span data-stu-id="0395f-261">No</span></span> |
| <span data-ttu-id="0395f-262">utdata</span><span class="sxs-lookup"><span data-stu-id="0395f-262">outputs</span></span> | <span data-ttu-id="0395f-263">Du måste ange en datamängd för utdata för en lagrad procedur-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0395f-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="0395f-264">Utdatauppsättningen anger hello **schema** för hello lagrade proceduraktiviteten (varje timme, varje vecka, månad, etc.).</span><span class="sxs-lookup"><span data-stu-id="0395f-264">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="0395f-265">Hej utdatauppsättningen måste använda en **länkade tjänsten** som refererar tooan Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill hello lagrade proceduren toorun.</span><span class="sxs-lookup"><span data-stu-id="0395f-265">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <br/><br/><span data-ttu-id="0395f-266">Hej utdatauppsättningen kan fungera som ett sätt toopass hello resultat hello lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="0395f-266">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="0395f-267">Data Factory kan dock inte automatiskt skriva hello utdata från en lagrad procedur toothis dataset.</span><span class="sxs-lookup"><span data-stu-id="0395f-267">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="0395f-268">Det är hello lagrad procedur som skrivningar tooa SQL-tabell som hello utdata dataset pekar på.</span><span class="sxs-lookup"><span data-stu-id="0395f-268">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <br/><br/><span data-ttu-id="0395f-269">I vissa fall hello utdatauppsättningen kan vara en **dummy dataset**, som används för endast toospecify hello schema för att köra hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-269">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span> |<span data-ttu-id="0395f-270">Ja</span><span class="sxs-lookup"><span data-stu-id="0395f-270">Yes</span></span> |
| <span data-ttu-id="0395f-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="0395f-271">storedProcedureName</span></span> |<span data-ttu-id="0395f-272">Ange hello namnet på hello lagrade procedur i hello Azure SQL database eller Azure SQL Data Warehouse eller SQL Server-databas som representeras av hello länkade tjänst som hello utdata tabellen använder.</span><span class="sxs-lookup"><span data-stu-id="0395f-272">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="0395f-273">Ja</span><span class="sxs-lookup"><span data-stu-id="0395f-273">Yes</span></span> |
| <span data-ttu-id="0395f-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="0395f-274">storedProcedureParameters</span></span> |<span data-ttu-id="0395f-275">Ange värden för parametrarna för lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="0395f-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="0395f-276">Om du behöver toopass null för en parameter, Använd hello syntax: ”param1”: null (alla gemen).</span><span class="sxs-lookup"><span data-stu-id="0395f-276">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="0395f-277">Se följande exempel toolearn om hur du använder den här egenskapen hello.</span><span class="sxs-lookup"><span data-stu-id="0395f-277">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="0395f-278">Nej</span><span class="sxs-lookup"><span data-stu-id="0395f-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="0395f-279">Skicka ett statiskt värde</span><span class="sxs-lookup"><span data-stu-id="0395f-279">Passing a static value</span></span>
<span data-ttu-id="0395f-280">Nu ska vi Överväg att lägga till en annan kolumn med namnet 'Scenariot' i hello tabell som innehåller ett statiskt värde med namnet 'Dokumentera exempel'.</span><span class="sxs-lookup"><span data-stu-id="0395f-280">Now, let’s consider adding another column named ‘Scenario’ in hello table containing a static value called ‘Document sample’.</span></span>

![Exempeldata 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="0395f-282">**Tabell:**</span><span class="sxs-lookup"><span data-stu-id="0395f-282">**Table:**</span></span>

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

<span data-ttu-id="0395f-283">**Lagrade proceduren:**</span><span class="sxs-lookup"><span data-stu-id="0395f-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="0395f-284">Nu kan skicka hello **scenariot** värde för parametern och hello från hello lagrade proceduraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="0395f-284">Now, pass hello **Scenario** parameter and hello value from hello stored procedure activity.</span></span> <span data-ttu-id="0395f-285">Hej **typeProperties** avsnitt i föregående exempel ser ut som följande fragment hello hello:</span><span class="sxs-lookup"><span data-stu-id="0395f-285">hello **typeProperties** section in hello preceding sample looks like hello following snippet:</span></span>

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

<span data-ttu-id="0395f-286">**Data Factory datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="0395f-286">**Data Factory dataset:**</span></span>

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="0395f-287">**Data Factory-pipelinen**</span><span class="sxs-lookup"><span data-stu-id="0395f-287">**Data Factory pipeline**</span></span>

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```