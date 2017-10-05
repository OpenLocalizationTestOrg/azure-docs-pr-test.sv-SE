---
title: "Kopiera data från Blob Storage till SQL Database - Azure | Microsoft Docs"
description: "Den här kursen visar hur du använder Kopieringsaktiviteten i ett Azure Data Factory-pipelinen för att kopiera data från Blob storage till SQL-databas."
keywords: BLOB-sql, blob storage, kopiering av data
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 730140d15f4dec7ddc1280c2e4da1d247902fe4a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a><span data-ttu-id="42d46-104">Självstudier: Kopiera data från Blob Storage till SQL-databas med hjälp av Data Factory</span><span class="sxs-lookup"><span data-stu-id="42d46-104">Tutorial: Copy data from Blob Storage to SQL Database using Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="42d46-105">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="42d46-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="42d46-106">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="42d46-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="42d46-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42d46-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="42d46-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42d46-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="42d46-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42d46-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="42d46-110">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="42d46-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="42d46-111">REST API</span><span class="sxs-lookup"><span data-stu-id="42d46-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="42d46-112">.NET-API</span><span class="sxs-lookup"><span data-stu-id="42d46-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="42d46-113">I kursen får skapa du en datafabrik med en rörledning för att kopiera data från Blob storage till SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="42d46-113">In this tutorial, you create a data factory with a pipeline to copy data from Blob storage to SQL database.</span></span>

<span data-ttu-id="42d46-114">Kopieringsaktiviteten utför dataflyttningen i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="42d46-114">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="42d46-115">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="42d46-115">It is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="42d46-116">Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="42d46-116">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>  

> [!NOTE]
> <span data-ttu-id="42d46-117">En detaljerad översikt över Data Factory-tjänsten finns i [introduktion till Azure Data Factory](data-factory-introduction.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="42d46-117">For a detailed overview of the Data Factory service, see the [Introduction to Azure Data Factory](data-factory-introduction.md) article.</span></span>
>
>

## <a name="prerequisites-for-the-tutorial"></a><span data-ttu-id="42d46-118">Förutsättningar för självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="42d46-118">Prerequisites for the tutorial</span></span>
<span data-ttu-id="42d46-119">Innan du påbörjar den här självstudien måste du ha följande krav:</span><span class="sxs-lookup"><span data-stu-id="42d46-119">Before you begin this tutorial, you must have the following prerequisites:</span></span>

* <span data-ttu-id="42d46-120">**Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="42d46-120">**Azure subscription**.</span></span>  <span data-ttu-id="42d46-121">Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="42d46-121">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="42d46-122">Finns det [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="42d46-122">See the [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="42d46-123">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="42d46-123">**Azure Storage Account**.</span></span> <span data-ttu-id="42d46-124">Du använder blobblagring som en **källa** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-124">You use the blob storage as a **source** data store in this tutorial.</span></span> <span data-ttu-id="42d46-125">Om du inte har ett Azure storage-konto finns i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) artikel steg för att skapa en.</span><span class="sxs-lookup"><span data-stu-id="42d46-125">if you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps to create one.</span></span>
* <span data-ttu-id="42d46-126">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="42d46-126">**Azure SQL Database**.</span></span> <span data-ttu-id="42d46-127">Du använder en Azure SQL-databas som en **mål** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-127">You use an Azure SQL database as a **destination** data store in this tutorial.</span></span> <span data-ttu-id="42d46-128">Om du inte har en Azure SQL-databas som du kan använda i kursen, se [hur du skapar och konfigurerar en Azure SQL Database](../sql-database/sql-database-get-started.md) att skapa en.</span><span class="sxs-lookup"><span data-stu-id="42d46-128">If you don't have an Azure SQL database that you can use in the tutorial, See [How to create and configure an Azure SQL Database](../sql-database/sql-database-get-started.md) to create one.</span></span>
* <span data-ttu-id="42d46-129">**2014-SQL Server 2012 eller Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="42d46-129">**SQL Server 2012/2014 or Visual Studio 2013**.</span></span> <span data-ttu-id="42d46-130">Du använder SQL Server Management Studio eller Visual Studio för att skapa en exempeldatabas och visa resulterande data i databasen.</span><span class="sxs-lookup"><span data-stu-id="42d46-130">You use SQL Server Management Studio or Visual Studio to create a sample database and to view the result data in the database.</span></span>  

## <a name="collect-blob-storage-account-name-and-key"></a><span data-ttu-id="42d46-131">Samla in blob storage-kontonamnet och nyckeln</span><span class="sxs-lookup"><span data-stu-id="42d46-131">Collect blob storage account name and key</span></span>
<span data-ttu-id="42d46-132">Du behöver kontonamnet och kontonyckel av Azure storage-konto för att göra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-132">You need the account name and account key of your Azure storage account to do this tutorial.</span></span> <span data-ttu-id="42d46-133">Notera **kontonamn** och **kontonyckel** för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="42d46-133">Note down **account name** and **account key** for your Azure storage account.</span></span>

1. <span data-ttu-id="42d46-134">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="42d46-134">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="42d46-135">Klicka på **fler tjänster** på den vänstra menyn och välj **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="42d46-135">Click **More services** on the left menu and select **Storage Accounts**.</span></span>

    ![Bläddra - Storage-konton](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. <span data-ttu-id="42d46-137">I den **Lagringskonton** bladet väljer den **Azure storage-konto** som du vill använda i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-137">In the **Storage Accounts** blade, select the **Azure storage account** that you want to use in this tutorial.</span></span>
4. <span data-ttu-id="42d46-138">Välj **åtkomstnycklar** länken under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="42d46-138">Select **Access keys** link under **SETTINGS**.</span></span>
5. <span data-ttu-id="42d46-139">Klicka på **kopiera** (bild)-knappen bredvid **lagringskontonamnet** text rutan och spara och klistra in den någonstans (till exempel: i en textfil).</span><span class="sxs-lookup"><span data-stu-id="42d46-139">Click **copy** (image) button next to **Storage account name** text box and save/paste it somewhere (for example: in a text file).</span></span>
6. <span data-ttu-id="42d46-140">Upprepa det föregående steget för att kopiera eller Skriv ned den **key1**.</span><span class="sxs-lookup"><span data-stu-id="42d46-140">Repeat the previous step to copy or note down the **key1**.</span></span>

    ![Lagringsåtkomstnyckel](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. <span data-ttu-id="42d46-142">Stäng alla blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="42d46-142">Close all the blades by clicking **X**.</span></span>

## <a name="collect-sql-server-database-user-names"></a><span data-ttu-id="42d46-143">Samla in SQLServer, databas, användarnamn</span><span class="sxs-lookup"><span data-stu-id="42d46-143">Collect SQL server, database, user names</span></span>
<span data-ttu-id="42d46-144">Du måste namnen på Azure SQL server-databasen och användaren att göra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-144">You need the names of Azure SQL server, database, and user to do this tutorial.</span></span> <span data-ttu-id="42d46-145">Skriv ner namnen på **server**, **databasen**, och **användaren** för din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="42d46-145">Note down names of **server**, **database**, and **user** for your Azure SQL database.</span></span>

1. <span data-ttu-id="42d46-146">I den **Azure-portalen**, klickar du på **fler tjänster** till vänster och välj **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="42d46-146">In the **Azure portal**, click **More services** on the left and select **SQL databases**.</span></span>
2. <span data-ttu-id="42d46-147">I den **SQL-databaser bladet**, Välj den **databasen** som du vill använda i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-147">In the **SQL databases blade**, select the **database** that you want to use in this tutorial.</span></span> <span data-ttu-id="42d46-148">Notera den **databasnamnet**.</span><span class="sxs-lookup"><span data-stu-id="42d46-148">Note down the **database name**.</span></span>  
3. <span data-ttu-id="42d46-149">I den **SQL-databas** bladet, klickar du på **egenskaper** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="42d46-149">In the **SQL database** blade, click **Properties** under **SETTINGS**.</span></span>
4. <span data-ttu-id="42d46-150">Skriv ner värdena för **servernamn** och **inloggning för SERVERADMINISTRATÖR**.</span><span class="sxs-lookup"><span data-stu-id="42d46-150">Note down the values for **SERVER NAME** and **SERVER ADMIN LOGIN**.</span></span>
5. <span data-ttu-id="42d46-151">Stäng alla blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="42d46-151">Close all the blades by clicking **X**.</span></span>

## <a name="allow-azure-services-to-access-sql-server"></a><span data-ttu-id="42d46-152">Ge Azure-tjänster åtkomst till SQLServer</span><span class="sxs-lookup"><span data-stu-id="42d46-152">Allow Azure services to access SQL server</span></span>
<span data-ttu-id="42d46-153">Se till att **Tillåt åtkomst till Azure-tjänster** inställningen aktiverade **på** för Azure SQL-servern så att Data Factory-tjänsten har åtkomst till Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="42d46-153">Ensure that **Allow access to Azure services** setting turned **ON** for your Azure SQL server so that the Data Factory service can access your Azure SQL server.</span></span> <span data-ttu-id="42d46-154">För att kontrollera och aktivera den här inställningen gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="42d46-154">To verify and turn on this setting, do the following steps:</span></span>

1. <span data-ttu-id="42d46-155">Klicka på **fler tjänster** hubb till vänster och klicka på **SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="42d46-155">Click **More services** hub on the left and click **SQL servers**.</span></span>
2. <span data-ttu-id="42d46-156">Markera din server och klicka på **brandväggen** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="42d46-156">Select your server, and click **Firewall** under **SETTINGS**.</span></span>
3. <span data-ttu-id="42d46-157">På bladet **Brandväggsinställningar** klickar du på **På** för **Tillåt åtkomst till Azure-tjänster**.</span><span class="sxs-lookup"><span data-stu-id="42d46-157">In the **Firewall settings** blade, click **ON** for **Allow access to Azure services**.</span></span>
4. <span data-ttu-id="42d46-158">Stäng alla blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="42d46-158">Close all the blades by clicking **X**.</span></span>

## <a name="prepare-blob-storage-and-sql-database"></a><span data-ttu-id="42d46-159">Förbereda Blob Storage och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="42d46-159">Prepare Blob Storage and SQL Database</span></span>
<span data-ttu-id="42d46-160">Nu kan förbereda Azure blob storage och Azure SQL-databas för kursen genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="42d46-160">Now, prepare your Azure blob storage and Azure SQL database for the tutorial by performing the following steps:</span></span>  

1. <span data-ttu-id="42d46-161">Öppna Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="42d46-161">Launch Notepad.</span></span> <span data-ttu-id="42d46-162">Kopiera följande text och spara den som **emp.txt** till **C:\ADFGetStarted** mapp på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="42d46-162">Copy the following text and save it as **emp.txt** to **C:\ADFGetStarted** folder on your hard drive.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
2. <span data-ttu-id="42d46-163">Använd verktyg som t.ex. [Azure Lagringsutforskaren](http://storageexplorer.com/) till att skapa behållaren **adftutorial** och för att ladda upp filen **emp.txt** till behållaren.</span><span class="sxs-lookup"><span data-stu-id="42d46-163">Use tools such as [Azure Storage Explorer](http://storageexplorer.com/) to create the **adftutorial** container and to upload the **emp.txt** file to the container.</span></span>

    ![Azure Lagringsutforskaren.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. <span data-ttu-id="42d46-166">Använd följande SQL-skript för att skapa tabellen **emp** i din Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="42d46-166">Use the following SQL script to create the **emp** table in your Azure SQL Database.</span></span>  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    <span data-ttu-id="42d46-167">**Om du har SQL Server 2012 2014 installerad på datorn:** följer du anvisningarna från [hantera Azure SQL Database med SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) att ansluta till din Azure SQL-server och köra SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="42d46-167">**If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) to connect to your Azure SQL server and run the SQL script.</span></span> <span data-ttu-id="42d46-168">Den här artikeln används den [klassiska Azure-portalen](http://manage.windowsazure.com), inte den [nya Azure-portalen](https://portal.azure.com), för att konfigurera brandväggen för en Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="42d46-168">This article uses the [classic Azure portal](http://manage.windowsazure.com), not the [new Azure portal](https://portal.azure.com), to configure firewall for an Azure SQL server.</span></span>

    <span data-ttu-id="42d46-169">Om klienten inte har åtkomst till Azure SQL-servern måste du konfigurera brandväggen för din Azure SQL-server och tillåta åtkomst från din dator (IP-adress).</span><span class="sxs-lookup"><span data-stu-id="42d46-169">If your client is not allowed to access the Azure SQL server, you need to configure firewall for your Azure SQL server to allow access from your machine (IP Address).</span></span> <span data-ttu-id="42d46-170">Anvisningar för hur du konfigurerar brandväggen för Azure SQL-servern finns i [den här artikeln](../sql-database/sql-database-configure-firewall-settings.md).</span><span class="sxs-lookup"><span data-stu-id="42d46-170">See [this article](../sql-database/sql-database-configure-firewall-settings.md) for steps to configure the firewall for your Azure SQL server.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="42d46-171">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="42d46-171">Create a data factory</span></span>
<span data-ttu-id="42d46-172">Du har slutfört de nödvändiga förutsättningarna.</span><span class="sxs-lookup"><span data-stu-id="42d46-172">You have completed the prerequisites.</span></span> <span data-ttu-id="42d46-173">Du kan skapa en datafabrik som använder något av följande sätt.</span><span class="sxs-lookup"><span data-stu-id="42d46-173">You can create a data factory using one of the following ways.</span></span> <span data-ttu-id="42d46-174">Klicka på något av alternativen i listrutan längst upp eller följande länkar för att genomföra kursen.</span><span class="sxs-lookup"><span data-stu-id="42d46-174">Click one of the options in the drop-down list at the top or the following links to perform the tutorial.</span></span>     

* [<span data-ttu-id="42d46-175">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="42d46-175">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
* [<span data-ttu-id="42d46-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="42d46-176">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [<span data-ttu-id="42d46-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42d46-177">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [<span data-ttu-id="42d46-178">PowerShell</span><span class="sxs-lookup"><span data-stu-id="42d46-178">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
* [<span data-ttu-id="42d46-179">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="42d46-179">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="42d46-180">REST API</span><span class="sxs-lookup"><span data-stu-id="42d46-180">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
* [<span data-ttu-id="42d46-181">.NET-API</span><span class="sxs-lookup"><span data-stu-id="42d46-181">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> <span data-ttu-id="42d46-182">Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager.</span><span class="sxs-lookup"><span data-stu-id="42d46-182">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="42d46-183">Det transformerar inte indata för att generera utdata.</span><span class="sxs-lookup"><span data-stu-id="42d46-183">It does not transform input data to produce output data.</span></span> <span data-ttu-id="42d46-184">Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg din första pipeline för att omvandla data med Hadoop-kluster).</span><span class="sxs-lookup"><span data-stu-id="42d46-184">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>
> 
> <span data-ttu-id="42d46-185">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="42d46-185">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="42d46-186">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="42d46-186">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 
