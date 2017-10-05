---
title: SQLServer lagrade Proceduraktiviteten
description: "Lär dig hur du kan använda lagrade Proceduraktiviteten för SQL Server för att anropa en lagrad procedur i en Azure SQL Database eller Azure SQL Data Warehouse från Data Factory-pipelinen."
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
ms.openlocfilehash: 6505d9aa2c7ae003bd928e2fa82cd923a9615394
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="a17cb-103">SQLServer lagrade Proceduraktiviteten</span><span class="sxs-lookup"><span data-stu-id="a17cb-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="a17cb-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="a17cb-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="a17cb-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="a17cb-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="a17cb-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="a17cb-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="a17cb-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="a17cb-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="a17cb-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="a17cb-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="a17cb-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="a17cb-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="a17cb-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="a17cb-114">Översikt</span><span class="sxs-lookup"><span data-stu-id="a17cb-114">Overview</span></span>
<span data-ttu-id="a17cb-115">Du kan använda data transformation aktiviteter i en Datafabrik [pipeline](data-factory-create-pipelines.md) att transformera och bearbeta rådata till förutsägelser och insikter.</span><span class="sxs-lookup"><span data-stu-id="a17cb-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) to transform and process raw data into predictions and insights.</span></span> <span data-ttu-id="a17cb-116">Den lagrade Proceduraktiviteten är en av omvandling av aktiviteter som har stöd för Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a17cb-116">The Stored Procedure Activity is one of the transformation activities that Data Factory supports.</span></span> <span data-ttu-id="a17cb-117">Den här artikeln bygger på den [data transformation aktiviteter](data-factory-data-transformation-activities.md) artikel som presenterar en allmän översikt över data transformation och stöds omvandling aktiviteter i Data Factory.</span><span class="sxs-lookup"><span data-stu-id="a17cb-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="a17cb-118">Du kan använda den lagrade Proceduraktiviteten för att anropa en lagrad procedur i någon av följande datalager i ditt företag eller på ett Azure-dator (VM):</span><span class="sxs-lookup"><span data-stu-id="a17cb-118">You can use the Stored Procedure Activity to invoke a stored procedure in one of the following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="a17cb-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a17cb-119">Azure SQL Database</span></span>
- <span data-ttu-id="a17cb-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="a17cb-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="a17cb-121">SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="a17cb-121">SQL Server Database.</span></span>  <span data-ttu-id="a17cb-122">Om du använder SQL Server, installera Data Management Gateway på samma dator som värd för databasen eller på en separat dator som har åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-122">If you are using SQL Server, install Data Management Gateway on the same machine that hosts the database or on a separate machine that has access to the database.</span></span> <span data-ttu-id="a17cb-123">Data Management Gateway är en komponent som ansluter datakällor på lokala/på virtuella Azure-datorn med molntjänster i en säker och hanterad sätt.</span><span class="sxs-lookup"><span data-stu-id="a17cb-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="a17cb-124">Se [Data Management Gateway](data-factory-data-management-gateway.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="a17cb-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a17cb-125">När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera den **SqlSink** i en Kopieringsaktivitet att anropa en lagrad procedur med hjälp av den **sqlWriterStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-125">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="a17cb-126">Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="a17cb-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="a17cb-127">Mer information om egenskapen Se följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a17cb-127">For details about the property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="a17cb-128">Anropa en lagrad procedur vid kopiering av data till en Azure SQL Data Warehouse med hjälp av en kopieringsaktiviteten stöds inte.</span><span class="sxs-lookup"><span data-stu-id="a17cb-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="a17cb-129">Men du kan använda aktiviteten lagrad procedur för att anropa en lagrad procedur i en SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a17cb-129">But, you can use the stored procedure activity to invoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="a17cb-130">När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i en Kopieringsaktivitet att anropa en lagrad procedur för att läsa data från källdatabasen med hjälp av den  **sqlReaderStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="a17cb-131">Mer information finns i följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="a17cb-131">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="a17cb-132">Den här genomgången använder aktiviteten lagrad procedur i en pipeline för att anropa en lagrad procedur i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="a17cb-132">The following walkthrough uses the Stored Procedure Activity in a pipeline to invoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="a17cb-133">Genomgång</span><span class="sxs-lookup"><span data-stu-id="a17cb-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="a17cb-134">Exempel på en tabell och lagrade procedurer</span><span class="sxs-lookup"><span data-stu-id="a17cb-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="a17cb-135">Skapa följande **tabellen** i Azure SQL-databasen med hjälp av SQL Server Management Studio eller andra verktyg som du är nöjd med.</span><span class="sxs-lookup"><span data-stu-id="a17cb-135">Create the following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="a17cb-136">Kolumnen datetimestamp är datum och tid när motsvarande ID har genererats.</span><span class="sxs-lookup"><span data-stu-id="a17cb-136">The datetimestamp column is the date and time when the corresponding ID is generated.</span></span>

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
    <span data-ttu-id="a17cb-137">ID är unikt identifieras och datetimestamp kolumnen är datum och tid när motsvarande ID har genererats.</span><span class="sxs-lookup"><span data-stu-id="a17cb-137">Id is the unique identified and the datetimestamp column is the date and time when the corresponding ID is generated.</span></span>
    
    ![Exempeldata](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="a17cb-139">I det här exemplet är den lagrade proceduren i en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a17cb-139">In this sample, the stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="a17cb-140">Om den lagrade proceduren finns i en Azure SQL Data Warehouse och SQL Server-databas, liknar tillvägagångssättet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-140">If the stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, the approach is similar.</span></span> <span data-ttu-id="a17cb-141">SQL Server-databas måste du installera en [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="a17cb-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="a17cb-142">Skapa följande **lagrade proceduren** som infogar data i den **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-142">Create the following **stored procedure** that inserts data in to the **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="a17cb-143">**Namnet** och **skiftläge** för parametern (DateTime i det här exemplet) måste matcha parameter som anges i pipeline/aktivitet JSON.</span><span class="sxs-lookup"><span data-stu-id="a17cb-143">**Name** and **casing** of the parameter (DateTime in this example) must match that of parameter specified in the pipeline/activity JSON.</span></span> <span data-ttu-id="a17cb-144">I den lagrade proceduren definitionen, kontrollerar du att  **@**  används som ett prefix för parametern.</span><span class="sxs-lookup"><span data-stu-id="a17cb-144">In the stored procedure definition, ensure that **@** is used as a prefix for the parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="a17cb-145">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="a17cb-145">Create a data factory</span></span>
1. <span data-ttu-id="a17cb-146">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a17cb-146">Log in to [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a17cb-147">Klicka på **ny** på den vänstra menyn klickar du på **Intelligence + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-147">Click **NEW** on the left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="a17cb-149">I den **nya datafabriken** bladet ange **SProcDF** för namnet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-149">In the **New data factory** blade, enter **SProcDF** for the Name.</span></span> <span data-ttu-id="a17cb-150">Azure Data Factory-namn är **globalt unika**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="a17cb-151">Du behöver Prefixnamn datafabriken med ditt namn, aktivera fabriken har skapats.</span><span class="sxs-lookup"><span data-stu-id="a17cb-151">You need to prefix the name of the data factory with your name, to enable the successful creation of the factory.</span></span>

   ![Nya data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="a17cb-153">Välj din **Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="a17cb-154">För **resursgruppen**, gör du något av följande steg:</span><span class="sxs-lookup"><span data-stu-id="a17cb-154">For **Resource Group**, do one of the following steps:</span></span>
   1. <span data-ttu-id="a17cb-155">Klicka på **Skapa nytt** och ange ett namn för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-155">Click **Create new** and enter a name for the resource group.</span></span>
   2. <span data-ttu-id="a17cb-156">Klicka på **använda befintliga** och välj en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a17cb-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="a17cb-157">Välj **plats** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="a17cb-157">Select the **location** for the data factory.</span></span>
7. <span data-ttu-id="a17cb-158">Välj **fäst på instrumentpanelen** så att du kan se datafabriken på instrumentpanelen nästa gång du loggar in.</span><span class="sxs-lookup"><span data-stu-id="a17cb-158">Select **Pin to dashboard** so that you can see the data factory on the dashboard next time you log in.</span></span>
8. <span data-ttu-id="a17cb-159">Klicka på **Skapa** på bladet **Ny datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-159">Click **Create** on the **New data factory** blade.</span></span>
9. <span data-ttu-id="a17cb-160">Du ser datafabriken som skapas i den **instrumentpanelen** på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-160">You see the data factory being created in the **dashboard** of the Azure portal.</span></span> <span data-ttu-id="a17cb-161">När datafabriken har skapats visas datafabrikssidan med innehållet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="a17cb-161">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>

   ![Data Factory-startsida](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="a17cb-163">Skapa en Azure SQL-länkade tjänst</span><span class="sxs-lookup"><span data-stu-id="a17cb-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="a17cb-164">Efter att datafabriken kan du skapa en Azure SQL länkade tjänst som länkar din Azure SQL-databasen, som innehåller sampletable tabellen och sp_sample lagrade proceduren till din data factory.</span><span class="sxs-lookup"><span data-stu-id="a17cb-164">After creating the data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains the sampletable table and sp_sample stored procedure, to your data factory.</span></span>

1. <span data-ttu-id="a17cb-165">Klicka på **författare och distribuera** på den **Datafabriken** bladet för **SProcDF** att starta Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a17cb-165">Click **Author and deploy** on the **Data Factory** blade for **SProcDF** to launch the Data Factory Editor.</span></span>
2. <span data-ttu-id="a17cb-166">Klicka på **Nytt datalager** på kommandoraden och välj **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-166">Click **New data store** on the command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="a17cb-167">Du bör se JSON-skript för att skapa en Azure SQL-länkade tjänst i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="a17cb-167">You should see the JSON script for creating an Azure SQL linked service in the editor.</span></span>

   ![Nytt datalager](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="a17cb-169">Gör följande ändringar i JSON-skript:</span><span class="sxs-lookup"><span data-stu-id="a17cb-169">In the JSON script, make the following changes:</span></span>

   1. <span data-ttu-id="a17cb-170">Ersätt `<servername>` med namnet på din Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="a17cb-170">Replace `<servername>` with the name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="a17cb-171">Ersätt `<databasename>` med den databas som du skapade i tabell och den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="a17cb-171">Replace `<databasename>` with the database in which you created the table and the stored procedure.</span></span>
   3. <span data-ttu-id="a17cb-172">Ersätt `<username@servername>` med det användarkonto som har åtkomst till databasen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-172">Replace `<username@servername>` with the user account that has access to the database.</span></span>
   4. <span data-ttu-id="a17cb-173">Ersätt `<password>` med lösenordet för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="a17cb-173">Replace `<password>` with the password for the user account.</span></span>

      ![Nytt datalager](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="a17cb-175">Om du vill distribuera den länkade tjänsten klickar du på **distribuera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-175">To deploy the linked service, click **Deploy** on the command bar.</span></span> <span data-ttu-id="a17cb-176">Bekräfta att du ser AzureSqlLinkedService i trädvyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="a17cb-176">Confirm that you see the AzureSqlLinkedService in the tree view on the left.</span></span>

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="a17cb-178">Skapa en datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="a17cb-178">Create an output dataset</span></span>
<span data-ttu-id="a17cb-179">Du måste ange en datamängd för utdata för en lagrad procedur aktivitet, även om den lagrade proceduren inte skapar några data.</span><span class="sxs-lookup"><span data-stu-id="a17cb-179">You must specify an output dataset for a stored procedure activity even if the stored procedure does not produce any data.</span></span> <span data-ttu-id="a17cb-180">Det beror på att den är datamängd för utdata som enheter schemat för aktiviteten (hur ofta aktiviteten körs - varje timme, varje dag, etc.).</span><span class="sxs-lookup"><span data-stu-id="a17cb-180">That's because it's the output dataset that drives the schedule of the activity (how often the activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="a17cb-181">Datamängd för utdata måste använda en **länkade tjänsten** som refererar till en Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill använda den lagrade proceduren för att köra.</span><span class="sxs-lookup"><span data-stu-id="a17cb-181">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <span data-ttu-id="a17cb-182">Datamängd för utdata kan fungera som ett sätt att skicka resultatet av den lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-182">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="a17cb-183">Dock skriver Data Factory inte automatiskt utdata från en lagrad procedur denna DataSet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-183">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="a17cb-184">Det är den lagrade proceduren som skriver till en SQLtabell som datamängd för utdata som pekar på.</span><span class="sxs-lookup"><span data-stu-id="a17cb-184">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <span data-ttu-id="a17cb-185">I vissa fall datamängd för utdata kan vara en **dummy dataset** (en datamängd som pekar på en tabell som inte innehåller utdata från den lagrade proceduren verkligen).</span><span class="sxs-lookup"><span data-stu-id="a17cb-185">In some cases, the output dataset can be a **dummy dataset** (a dataset that points to a table that does not really hold output of the stored procedure).</span></span> <span data-ttu-id="a17cb-186">Den här dummy datauppsättningen används bara för att ange schemat för att köra aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-186">This dummy dataset is used only to specify the schedule for running the stored procedure activity.</span></span> 

1. <span data-ttu-id="a17cb-187">Klicka på **... Flera** i verktygsfältet klickar du på **ny datamängd**, och klicka på **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-187">Click **... More** on the toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="a17cb-188">**Ny datamängd** i kommandofältet och välj **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-188">**New dataset** on the command bar and select **Azure SQL**.</span></span>

    ![trädvy för länkad tjänst](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="a17cb-190">Kopiera och klistra in följande JSON-skript i JSON-redigeringsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-190">Copy/paste the following JSON script in to the JSON editor.</span></span>

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
3. <span data-ttu-id="a17cb-191">Om du vill distribuera datauppsättningen klickar du på **distribuera** i kommandofältet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-191">To deploy the dataset, click **Deploy** on the command bar.</span></span> <span data-ttu-id="a17cb-192">Bekräfta att du ser dataset i trädvyn.</span><span class="sxs-lookup"><span data-stu-id="a17cb-192">Confirm that you see the dataset in the tree view.</span></span>

    ![Trädvy med länkade tjänster](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="a17cb-194">Skapa en pipeline med SqlServerStoredProcedure aktivitet</span><span class="sxs-lookup"><span data-stu-id="a17cb-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="a17cb-195">Nu ska vi skapa en pipeline med en lagrad procedur-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="a17cb-196">Observera följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="a17cb-196">Notice the following properties:</span></span> 

- <span data-ttu-id="a17cb-197">Den **typen** egenskap är inställd på **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-197">The **type** property is set to **SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="a17cb-198">Den **storedProcedureName** i typen egenskaper har angetts till **sp_sample** (namnet på den lagrade proceduren).</span><span class="sxs-lookup"><span data-stu-id="a17cb-198">The **storedProcedureName** in type properties is set to **sp_sample** (name of the stored procedure).</span></span>
- <span data-ttu-id="a17cb-199">Den **storedProcedureParameters** avsnittet innehåller en parameter med namnet **DataTime**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-199">The **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="a17cb-200">Namn och skiftläge för parametern i JSON måste matcha namnet och versaler och gemener i parametern i definitionen för lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="a17cb-200">Name and casing of the parameter in JSON must match the name and casing of the parameter in the stored procedure definition.</span></span> <span data-ttu-id="a17cb-201">Om du behöver lägga till null för en parameter, använder du syntax: `"param1": null` (gemener).</span><span class="sxs-lookup"><span data-stu-id="a17cb-201">If you need pass null for a parameter, use the syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="a17cb-202">Klicka på **... Flera** i kommandofältet och på **ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-202">Click **... More** on the command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="a17cb-203">Kopiera och klistra in följande kodutdrag JSON:</span><span class="sxs-lookup"><span data-stu-id="a17cb-203">Copy/paste the following JSON snippet:</span></span>   

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
3. <span data-ttu-id="a17cb-204">Om du vill distribuera pipeline, klickar du på **distribuera** i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-204">To deploy the pipeline, click **Deploy** on the toolbar.</span></span>  

### <a name="monitor-the-pipeline"></a><span data-ttu-id="a17cb-205">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="a17cb-205">Monitor the pipeline</span></span>
1. <span data-ttu-id="a17cb-206">Klicka på **X** för att stänga bladen i Data Factory-redigeraren och för att gå tillbaka till Data Factory-bladet och klicka på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="a17cb-206">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="a17cb-208">I **diagramvyn** visas en översikt över pipelines och datauppsättningar som används i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="a17cb-208">In the **Diagram View**, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="a17cb-210">Dubbelklicka på datauppsättningen i diagramvyn `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="a17cb-210">In the Diagram View, double-click the dataset `sprocsampleout`.</span></span> <span data-ttu-id="a17cb-211">Du ser segment i tillståndet Ready.</span><span class="sxs-lookup"><span data-stu-id="a17cb-211">You see the slices in Ready state.</span></span> <span data-ttu-id="a17cb-212">Det bör finnas fem segment eftersom ett segment skapas för varje timme mellan start- och sluttid från JSON.</span><span class="sxs-lookup"><span data-stu-id="a17cb-212">There should be five slices because a slice is produced for each hour between the start time and end time from the JSON.</span></span>

    ![Diagram över sida vid sida](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="a17cb-214">När ett segment är i **klar** tillstånd, kör en `select * from sampletable` frågan mot Azure SQL-databasen för att verifiera att data har infogas i tabellen av den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="a17cb-214">When a slice is in **Ready** state, run a `select * from sampletable` query against the Azure SQL database to verify that the data was inserted in to the table by the stored procedure.</span></span>

   ![utdata](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="a17cb-216">Se [övervaka pipeline](data-factory-monitor-manage-pipelines.md) detaljerad information om hur du övervakar Azure Data Factory pipelines.</span><span class="sxs-lookup"><span data-stu-id="a17cb-216">See [Monitor the pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="a17cb-217">Ange en inkommande datauppsättning</span><span class="sxs-lookup"><span data-stu-id="a17cb-217">Specify an input dataset</span></span>
<span data-ttu-id="a17cb-218">I den här genomgången har lagrade proceduraktiviteten inte några inkommande datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="a17cb-218">In the walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="a17cb-219">Om du anger en inkommande datauppsättning körs inte aktiviteten lagrad procedur tills segment av inkommande dataset är tillgänglig (i tillståndet Ready).</span><span class="sxs-lookup"><span data-stu-id="a17cb-219">If you specify an input dataset, the stored procedure activity does not run until the slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="a17cb-220">Dataset kan vara en externa datauppsättningen (som inte tillverkas av en annan aktivitet i samma rörledning) eller en intern datauppsättning som produceras av en överordnad aktivitet (den aktivitet som körs före den här aktiviteten).</span><span class="sxs-lookup"><span data-stu-id="a17cb-220">The dataset can be an external dataset (that is not produced by another activity in the same pipeline) or an internal dataset that is produced by an upstream activity (the activity that runs before this activity).</span></span> <span data-ttu-id="a17cb-221">Du kan ange flera indatauppsättningar för aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-221">You can specify multiple input datasets for the stored procedure activity.</span></span> <span data-ttu-id="a17cb-222">Om du gör det körs lagrade proceduraktiviteten bara när alla inkommande dataset-segment (i tillståndet Ready).</span><span class="sxs-lookup"><span data-stu-id="a17cb-222">If you do so, the stored procedure activity runs only when all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="a17cb-223">Inkommande dataset kan inte användas i den lagrade proceduren som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a17cb-223">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="a17cb-224">Den används endast för att kontrollera beroendet innan du startar aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-224">It is only used to check the dependency before starting the stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="a17cb-225">Länkning med andra aktiviteter</span><span class="sxs-lookup"><span data-stu-id="a17cb-225">Chaining with other activities</span></span>
<span data-ttu-id="a17cb-226">Ange utdata från den överordnade aktiviteten som indata för den här aktiviteten om du vill att en överordnad aktivitet med den här aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a17cb-226">If you want to chain an upstream activity with this activity, specify the output of the upstream activity as an input of this activity.</span></span> <span data-ttu-id="a17cb-227">När du gör det fungerar inte aktiviteten lagrad procedur tills den överordnade aktiviteten har slutförts och datamängd för utdata för den överordnade aktiviteten är tillgänglig (i redo status).</span><span class="sxs-lookup"><span data-stu-id="a17cb-227">When you do so, the stored procedure activity does not run until the upstream activity completes and the output dataset of the upstream activity is available (in Ready status).</span></span> <span data-ttu-id="a17cb-228">Du kan ange utdata-datauppsättningar på flera överordnade aktiviteter som indatauppsättningar för aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-228">You can specify output datasets of multiple upstream activities as input datasets of the stored procedure activity.</span></span> <span data-ttu-id="a17cb-229">När du gör det körs lagrade proceduraktiviteten bara när alla inkommande dataset-segment är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="a17cb-229">When you do so, the stored procedure activity runs only when all the input dataset slices are available.</span></span>  

<span data-ttu-id="a17cb-230">I följande exempel utdata från kopieringsaktiviteten är: OutputDataset som utgör indata för aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-230">In the following example, the output of the copy activity is: OutputDataset, which is an input of the stored procedure activity.</span></span> <span data-ttu-id="a17cb-231">Aktiviteten lagrad procedur körs därför inte förrän kopieringsaktiviteten har slutförts och OutputDataset sektorn är tillgänglig (i tillståndet Ready).</span><span class="sxs-lookup"><span data-stu-id="a17cb-231">Therefore, the stored procedure activity does not run until the copy activity completes and the OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="a17cb-232">Om du anger flera indatauppsättningar körs aktiviteten lagrad procedur inte förrän alla indata dataset-segment är tillgängliga (statusen Ready).</span><span class="sxs-lookup"><span data-stu-id="a17cb-232">If you specify multiple input datasets, the stored procedure activity does not run until all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="a17cb-233">Inkommande datauppsättningar kan inte användas direkt som parametrar för aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-233">The input datasets cannot be used directly as parameters to the stored procedure activity.</span></span> 

<span data-ttu-id="a17cb-234">Mer information om länkning aktiviteter finns [flera aktiviteter i en pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="a17cb-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
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

<span data-ttu-id="a17cb-235">På liknande sätt att länka en aktivitet för store-procedur med **underordnade aktiviteter** (aktiviteter som körs när aktiviteten lagrad procedur har slutförts), ange datamängd för utdata för aktiviteten lagrad procedur som indata för den underordnad aktivitet i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-235">Similarly, to link the store procedure activity with **downstream activities** (the activities that run after the stored procedure activity completes), specify the output dataset of the stored procedure activity as an input of the downstream activity in the pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a17cb-236">När du kopierar data till Azure SQL Database eller SQL Server, kan du konfigurera den **SqlSink** i en Kopieringsaktivitet att anropa en lagrad procedur med hjälp av den **sqlWriterStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-236">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="a17cb-237">Mer information finns i [anropa lagrade procedur från kopieringsaktiviteten](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="a17cb-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="a17cb-238">Mer information om egenskapen finns i följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="a17cb-238">For details about the property, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="a17cb-239">När du kopierar data från Azure SQL Database eller SQL Server eller Azure SQL Data Warehouse, kan du konfigurera **SqlSource** i en Kopieringsaktivitet att anropa en lagrad procedur för att läsa data från källdatabasen med hjälp av den  **sqlReaderStoredProcedureName** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="a17cb-240">Mer information finns i följande artiklar för koppling: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="a17cb-240">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="a17cb-241">JSON-format</span><span class="sxs-lookup"><span data-stu-id="a17cb-241">JSON format</span></span>
<span data-ttu-id="a17cb-242">Här är JSON-format för att definiera en lagrade Proceduraktiviteten:</span><span class="sxs-lookup"><span data-stu-id="a17cb-242">Here is the JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="a17cb-243">I följande tabell beskrivs egenskaperna JSON:</span><span class="sxs-lookup"><span data-stu-id="a17cb-243">The following table describes these JSON properties:</span></span>

| <span data-ttu-id="a17cb-244">Egenskap</span><span class="sxs-lookup"><span data-stu-id="a17cb-244">Property</span></span> | <span data-ttu-id="a17cb-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a17cb-245">Description</span></span> | <span data-ttu-id="a17cb-246">Krävs</span><span class="sxs-lookup"><span data-stu-id="a17cb-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a17cb-247">namn</span><span class="sxs-lookup"><span data-stu-id="a17cb-247">name</span></span> | <span data-ttu-id="a17cb-248">Namnet på aktiviteten</span><span class="sxs-lookup"><span data-stu-id="a17cb-248">Name of the activity</span></span> |<span data-ttu-id="a17cb-249">Ja</span><span class="sxs-lookup"><span data-stu-id="a17cb-249">Yes</span></span> |
| <span data-ttu-id="a17cb-250">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a17cb-250">description</span></span> |<span data-ttu-id="a17cb-251">Text som beskriver aktiviteten är det som används för</span><span class="sxs-lookup"><span data-stu-id="a17cb-251">Text describing what the activity is used for</span></span> |<span data-ttu-id="a17cb-252">Nej</span><span class="sxs-lookup"><span data-stu-id="a17cb-252">No</span></span> |
| <span data-ttu-id="a17cb-253">typ</span><span class="sxs-lookup"><span data-stu-id="a17cb-253">type</span></span> | <span data-ttu-id="a17cb-254">Måste anges till: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="a17cb-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="a17cb-255">Ja</span><span class="sxs-lookup"><span data-stu-id="a17cb-255">Yes</span></span> |
| <span data-ttu-id="a17cb-256">Indata</span><span class="sxs-lookup"><span data-stu-id="a17cb-256">inputs</span></span> | <span data-ttu-id="a17cb-257">Valfri.</span><span class="sxs-lookup"><span data-stu-id="a17cb-257">Optional.</span></span> <span data-ttu-id="a17cb-258">Om du anger en inkommande datauppsättning, måste det vara tillgänglig (statusen ”klar”) för aktiviteten lagrad procedur att köra.</span><span class="sxs-lookup"><span data-stu-id="a17cb-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for the stored procedure activity to run.</span></span> <span data-ttu-id="a17cb-259">Inkommande dataset kan inte användas i den lagrade proceduren som en parameter.</span><span class="sxs-lookup"><span data-stu-id="a17cb-259">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="a17cb-260">Den används endast för att kontrollera beroendet innan du startar aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-260">It is only used to check the dependency before starting the stored procedure activity.</span></span> |<span data-ttu-id="a17cb-261">Nej</span><span class="sxs-lookup"><span data-stu-id="a17cb-261">No</span></span> |
| <span data-ttu-id="a17cb-262">utdata</span><span class="sxs-lookup"><span data-stu-id="a17cb-262">outputs</span></span> | <span data-ttu-id="a17cb-263">Du måste ange en datamängd för utdata för en lagrad procedur-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="a17cb-264">Utdatauppsättningen anger den **schema** för aktiviteten lagrad procedur (varje timme, varje vecka, månad, etc.).</span><span class="sxs-lookup"><span data-stu-id="a17cb-264">Output dataset specifies the **schedule** for the stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="a17cb-265">Datamängd för utdata måste använda en **länkade tjänsten** som refererar till en Azure SQL Database eller ett Azure SQL Data Warehouse eller en SQL Server-databas som du vill använda den lagrade proceduren för att köra.</span><span class="sxs-lookup"><span data-stu-id="a17cb-265">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <br/><br/><span data-ttu-id="a17cb-266">Datamängd för utdata kan fungera som ett sätt att skicka resultatet av den lagrade proceduren för senare bearbetning av en annan aktivitet ([länkning aktiviteter](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-266">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="a17cb-267">Dock skriver Data Factory inte automatiskt utdata från en lagrad procedur denna DataSet.</span><span class="sxs-lookup"><span data-stu-id="a17cb-267">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="a17cb-268">Det är den lagrade proceduren som skriver till en SQLtabell som datamängd för utdata som pekar på.</span><span class="sxs-lookup"><span data-stu-id="a17cb-268">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <br/><br/><span data-ttu-id="a17cb-269">I vissa fall datamängd för utdata kan vara en **dummy dataset**, som används bara för att ange schemat för att köra aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-269">In some cases, the output dataset can be a **dummy dataset**, which is used only to specify the schedule for running the stored procedure activity.</span></span> |<span data-ttu-id="a17cb-270">Ja</span><span class="sxs-lookup"><span data-stu-id="a17cb-270">Yes</span></span> |
| <span data-ttu-id="a17cb-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="a17cb-271">storedProcedureName</span></span> |<span data-ttu-id="a17cb-272">Ange namnet på den lagrade proceduren i Azure SQL database eller Azure SQL Data Warehouse eller SQL Server-databas som representeras av den länkade tjänst som använder utdatatabellen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-272">Specify the name of the stored procedure in the Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by the linked service that the output table uses.</span></span> |<span data-ttu-id="a17cb-273">Ja</span><span class="sxs-lookup"><span data-stu-id="a17cb-273">Yes</span></span> |
| <span data-ttu-id="a17cb-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="a17cb-274">storedProcedureParameters</span></span> |<span data-ttu-id="a17cb-275">Ange värden för parametrarna för lagrade procedurer.</span><span class="sxs-lookup"><span data-stu-id="a17cb-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="a17cb-276">Om du måste överföra null för en parameter, använder du syntax: ”param1”: null (alla gemen).</span><span class="sxs-lookup"><span data-stu-id="a17cb-276">If you need to pass null for a parameter, use the syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="a17cb-277">Se följande exempel mer information om hur du använder den här egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a17cb-277">See the following sample to learn about using this property.</span></span> |<span data-ttu-id="a17cb-278">Nej</span><span class="sxs-lookup"><span data-stu-id="a17cb-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="a17cb-279">Skicka ett statiskt värde</span><span class="sxs-lookup"><span data-stu-id="a17cb-279">Passing a static value</span></span>
<span data-ttu-id="a17cb-280">Nu ska vi Överväg att lägga till en annan kolumn med namnet 'Scenariot' i tabellen som innehåller ett statiskt värde med namnet 'Dokumentera exempel'.</span><span class="sxs-lookup"><span data-stu-id="a17cb-280">Now, let’s consider adding another column named ‘Scenario’ in the table containing a static value called ‘Document sample’.</span></span>

![Exempeldata 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="a17cb-282">**Tabell:**</span><span class="sxs-lookup"><span data-stu-id="a17cb-282">**Table:**</span></span>

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

<span data-ttu-id="a17cb-283">**Lagrade proceduren:**</span><span class="sxs-lookup"><span data-stu-id="a17cb-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="a17cb-284">Nu kan skicka den **scenariot** och värdet från aktiviteten lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="a17cb-284">Now, pass the **Scenario** parameter and the value from the stored procedure activity.</span></span> <span data-ttu-id="a17cb-285">Den **typeProperties** avsnitt i föregående exempel ser ut som följande utdrag:</span><span class="sxs-lookup"><span data-stu-id="a17cb-285">The **typeProperties** section in the preceding sample looks like the following snippet:</span></span>

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

<span data-ttu-id="a17cb-286">**Data Factory datauppsättningen:**</span><span class="sxs-lookup"><span data-stu-id="a17cb-286">**Data Factory dataset:**</span></span>

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

<span data-ttu-id="a17cb-287">**Data Factory-pipelinen**</span><span class="sxs-lookup"><span data-stu-id="a17cb-287">**Data Factory pipeline**</span></span>

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