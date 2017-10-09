---
title: "aaaCopy data från Blob Storage tooSQL databasen - Azure | Microsoft Docs"
description: "Den här kursen visar hur toouse Kopieringsaktiviteten i ett Azure Data Factory pipeline toocopy data från Blob storage tooSQL databas."
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
ms.openlocfilehash: a2c3fb8a4ddd63b0b6b3e75903b7a7eaf188fda4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-copy-data-from-blob-storage-toosql-database-using-data-factory"></a><span data-ttu-id="c512d-104">Självstudier: Kopiera data från Blob Storage tooSQL databasen med hjälp av Data Factory</span><span class="sxs-lookup"><span data-stu-id="c512d-104">Tutorial: Copy data from Blob Storage tooSQL Database using Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c512d-105">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="c512d-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="c512d-106">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="c512d-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="c512d-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c512d-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="c512d-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c512d-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="c512d-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c512d-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="c512d-110">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c512d-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="c512d-111">REST API</span><span class="sxs-lookup"><span data-stu-id="c512d-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="c512d-112">.NET-API</span><span class="sxs-lookup"><span data-stu-id="c512d-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="c512d-113">I kursen får skapa du en datafabrik med en rörledning toocopy data från Blob storage tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="c512d-113">In this tutorial, you create a data factory with a pipeline toocopy data from Blob storage tooSQL database.</span></span>

<span data-ttu-id="c512d-114">Hej Kopieringsaktiviteten utför hello dataflyttning i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c512d-114">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="c512d-115">Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt.</span><span class="sxs-lookup"><span data-stu-id="c512d-115">It is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="c512d-116">Se [Data Movement aktiviteter](data-factory-data-movement-activities.md) artikeln för information om hello Kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c512d-116">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>  

> [!NOTE]
> <span data-ttu-id="c512d-117">En detaljerad översikt över hello Data Factory-tjänsten finns hello [introduktion tooAzure Data Factory](data-factory-introduction.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="c512d-117">For a detailed overview of hello Data Factory service, see hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article.</span></span>
>
>

## <a name="prerequisites-for-hello-tutorial"></a><span data-ttu-id="c512d-118">Förutsättningar för självstudiekursen hello</span><span class="sxs-lookup"><span data-stu-id="c512d-118">Prerequisites for hello tutorial</span></span>
<span data-ttu-id="c512d-119">Innan du börjar den här kursen måste du ha hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="c512d-119">Before you begin this tutorial, you must have hello following prerequisites:</span></span>

* <span data-ttu-id="c512d-120">**Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="c512d-120">**Azure subscription**.</span></span>  <span data-ttu-id="c512d-121">Om du inte har någon Azure-prenumeration kan du skapa ett kostnadsfritt konto på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="c512d-121">If you don't have a subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c512d-122">Se hello [kostnadsfri utvärderingsversion](http://azure.microsoft.com/pricing/free-trial/) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="c512d-122">See hello [Free Trial](http://azure.microsoft.com/pricing/free-trial/) article for details.</span></span>
* <span data-ttu-id="c512d-123">**Azure Storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="c512d-123">**Azure Storage Account**.</span></span> <span data-ttu-id="c512d-124">Du använder hello blob storage som en **källa** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-124">You use hello blob storage as a **source** data store in this tutorial.</span></span> <span data-ttu-id="c512d-125">Om du inte har ett Azure storage-konto finns hello [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) toocreate i steg en artikel.</span><span class="sxs-lookup"><span data-stu-id="c512d-125">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
* <span data-ttu-id="c512d-126">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="c512d-126">**Azure SQL Database**.</span></span> <span data-ttu-id="c512d-127">Du använder en Azure SQL-databas som en **mål** datalager i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-127">You use an Azure SQL database as a **destination** data store in this tutorial.</span></span> <span data-ttu-id="c512d-128">Om du inte har en Azure SQL-databas som du kan använda i hello självstudien finns [hur toocreate och konfigurera en Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate en.</span><span class="sxs-lookup"><span data-stu-id="c512d-128">If you don't have an Azure SQL database that you can use in hello tutorial, See [How toocreate and configure an Azure SQL Database](../sql-database/sql-database-get-started.md) toocreate one.</span></span>
* <span data-ttu-id="c512d-129">**2014-SQL Server 2012 eller Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="c512d-129">**SQL Server 2012/2014 or Visual Studio 2013**.</span></span> <span data-ttu-id="c512d-130">Du använder SQL Server Management Studio eller Visual Studio toocreate en exempeldatabas och tooview hello Resultatdata i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c512d-130">You use SQL Server Management Studio or Visual Studio toocreate a sample database and tooview hello result data in hello database.</span></span>  

## <a name="collect-blob-storage-account-name-and-key"></a><span data-ttu-id="c512d-131">Samla in blob storage-kontonamnet och nyckeln</span><span class="sxs-lookup"><span data-stu-id="c512d-131">Collect blob storage account name and key</span></span>
<span data-ttu-id="c512d-132">Du behöver hello konto namn och åtkomstnyckel för ditt Azure storage-konto toodo den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-132">You need hello account name and account key of your Azure storage account toodo this tutorial.</span></span> <span data-ttu-id="c512d-133">Notera **kontonamn** och **kontonyckel** för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="c512d-133">Note down **account name** and **account key** for your Azure storage account.</span></span>

1. <span data-ttu-id="c512d-134">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c512d-134">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c512d-135">Klicka på **fler tjänster** på hello vänster-menyn och välj **Lagringskonton**.</span><span class="sxs-lookup"><span data-stu-id="c512d-135">Click **More services** on hello left menu and select **Storage Accounts**.</span></span>

    ![Bläddra - Storage-konton](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. <span data-ttu-id="c512d-137">I hello **Lagringskonton** bladet, Välj hello **Azure storage-konto** som du vill toouse i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-137">In hello **Storage Accounts** blade, select hello **Azure storage account** that you want toouse in this tutorial.</span></span>
4. <span data-ttu-id="c512d-138">Välj **åtkomstnycklar** länken under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c512d-138">Select **Access keys** link under **SETTINGS**.</span></span>
5. <span data-ttu-id="c512d-139">Klicka på **kopiera** (bild) bredvid knappen för**lagringskontonamnet** text rutan och spara och klistra in den någonstans (till exempel: i en textfil).</span><span class="sxs-lookup"><span data-stu-id="c512d-139">Click **copy** (image) button next too**Storage account name** text box and save/paste it somewhere (for example: in a text file).</span></span>
6. <span data-ttu-id="c512d-140">Upprepa föregående steg toocopy för hello eller Skriv ner hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="c512d-140">Repeat hello previous step toocopy or note down hello **key1**.</span></span>

    ![Lagringsåtkomstnyckel](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. <span data-ttu-id="c512d-142">Stäng alla hello blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="c512d-142">Close all hello blades by clicking **X**.</span></span>

## <a name="collect-sql-server-database-user-names"></a><span data-ttu-id="c512d-143">Samla in SQLServer, databas, användarnamn</span><span class="sxs-lookup"><span data-stu-id="c512d-143">Collect SQL server, database, user names</span></span>
<span data-ttu-id="c512d-144">Hello namnen på Azure SQL server-databasen och användaren toodo måste den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-144">You need hello names of Azure SQL server, database, and user toodo this tutorial.</span></span> <span data-ttu-id="c512d-145">Skriv ner namnen på **server**, **databasen**, och **användaren** för din Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c512d-145">Note down names of **server**, **database**, and **user** for your Azure SQL database.</span></span>

1. <span data-ttu-id="c512d-146">I hello **Azure-portalen**, klickar du på **fler tjänster** på hello vänster och välj **SQL-databaser**.</span><span class="sxs-lookup"><span data-stu-id="c512d-146">In hello **Azure portal**, click **More services** on hello left and select **SQL databases**.</span></span>
2. <span data-ttu-id="c512d-147">I hello **SQL-databaser bladet**väljer hello **databasen** som du vill toouse i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-147">In hello **SQL databases blade**, select hello **database** that you want toouse in this tutorial.</span></span> <span data-ttu-id="c512d-148">Skriv ner hello **databasnamnet**.</span><span class="sxs-lookup"><span data-stu-id="c512d-148">Note down hello **database name**.</span></span>  
3. <span data-ttu-id="c512d-149">I hello **SQL-databas** bladet, klickar du på **egenskaper** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c512d-149">In hello **SQL database** blade, click **Properties** under **SETTINGS**.</span></span>
4. <span data-ttu-id="c512d-150">Skriv ner hello värden för **servernamn** och **inloggning för SERVERADMINISTRATÖR**.</span><span class="sxs-lookup"><span data-stu-id="c512d-150">Note down hello values for **SERVER NAME** and **SERVER ADMIN LOGIN**.</span></span>
5. <span data-ttu-id="c512d-151">Stäng alla hello blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="c512d-151">Close all hello blades by clicking **X**.</span></span>

## <a name="allow-azure-services-tooaccess-sql-server"></a><span data-ttu-id="c512d-152">Tillåt Azure-tjänster tooaccess SQLServer</span><span class="sxs-lookup"><span data-stu-id="c512d-152">Allow Azure services tooaccess SQL server</span></span>
<span data-ttu-id="c512d-153">Se till att **och ge åtkomst tooAzure tjänster** inställningen aktiverade **på** för Azure SQL-servern så att hello Data Factory-tjänsten har åtkomst till Azure SQL-servern.</span><span class="sxs-lookup"><span data-stu-id="c512d-153">Ensure that **Allow access tooAzure services** setting turned **ON** for your Azure SQL server so that hello Data Factory service can access your Azure SQL server.</span></span> <span data-ttu-id="c512d-154">tooverify och aktivera den här inställningen hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c512d-154">tooverify and turn on this setting, do hello following steps:</span></span>

1. <span data-ttu-id="c512d-155">Klicka på **fler tjänster** hubb hello vänster och klicka på **SQL-servrar**.</span><span class="sxs-lookup"><span data-stu-id="c512d-155">Click **More services** hub on hello left and click **SQL servers**.</span></span>
2. <span data-ttu-id="c512d-156">Välj din server och klicka på **Brandvägg** under **INSTÄLLNINGAR**.</span><span class="sxs-lookup"><span data-stu-id="c512d-156">Select your server, and click **Firewall** under **SETTINGS**.</span></span>
3. <span data-ttu-id="c512d-157">I hello **brandväggsinställningar** bladet, klickar du på **ON** för **och ge åtkomst tooAzure tjänster**.</span><span class="sxs-lookup"><span data-stu-id="c512d-157">In hello **Firewall settings** blade, click **ON** for **Allow access tooAzure services**.</span></span>
4. <span data-ttu-id="c512d-158">Stäng alla hello blad genom att klicka på **X**.</span><span class="sxs-lookup"><span data-stu-id="c512d-158">Close all hello blades by clicking **X**.</span></span>

## <a name="prepare-blob-storage-and-sql-database"></a><span data-ttu-id="c512d-159">Förbereda Blob Storage och SQL-databas</span><span class="sxs-lookup"><span data-stu-id="c512d-159">Prepare Blob Storage and SQL Database</span></span>
<span data-ttu-id="c512d-160">Nu kan förbereda Azure blob storage och Azure SQL-databas för hello kursen genom att utföra följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="c512d-160">Now, prepare your Azure blob storage and Azure SQL database for hello tutorial by performing hello following steps:</span></span>  

1. <span data-ttu-id="c512d-161">Öppna Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="c512d-161">Launch Notepad.</span></span> <span data-ttu-id="c512d-162">Kopiera hello följande text och spara den som **emp.txt** för**C:\ADFGetStarted** mapp på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="c512d-162">Copy hello following text and save it as **emp.txt** too**C:\ADFGetStarted** folder on your hard drive.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
2. <span data-ttu-id="c512d-163">Använd verktyg som [Azure Lagringsutforskaren](http://storageexplorer.com/) toocreate hello **adftutorial** behållare och tooupload hello **emp.txt** toohello filbehållare.</span><span class="sxs-lookup"><span data-stu-id="c512d-163">Use tools such as [Azure Storage Explorer](http://storageexplorer.com/) toocreate hello **adftutorial** container and tooupload hello **emp.txt** file toohello container.</span></span>

    ![Azure Lagringsutforskaren.](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. <span data-ttu-id="c512d-166">Använd hello följande SQL-skript toocreate hello **tomma** tabell i Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="c512d-166">Use hello following SQL script toocreate hello **emp** table in your Azure SQL Database.</span></span>  

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

    <span data-ttu-id="c512d-167">**Om du har SQL Server 2012 2014 installerad på datorn:** följer du anvisningarna från [hantera Azure SQL Database med SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server och kör hello SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="c512d-167">**If you have SQL Server 2012/2014 installed on your computer:** follow instructions from [Managing Azure SQL Database using SQL Server Management Studio](../sql-database/sql-database-manage-azure-ssms.md) tooconnect tooyour Azure SQL server and run hello SQL script.</span></span> <span data-ttu-id="c512d-168">Den här artikeln använder hello [klassiska Azure-portalen](http://manage.windowsazure.com), inte hello [nya Azure-portalen](https://portal.azure.com), tooconfigure Brandvägg för en Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="c512d-168">This article uses hello [classic Azure portal](http://manage.windowsazure.com), not hello [new Azure portal](https://portal.azure.com), tooconfigure firewall for an Azure SQL server.</span></span>

    <span data-ttu-id="c512d-169">Om klienten inte är tillåtet tooaccess hello Azure SQL-server behöver du tooconfigure Brandvägg för din Azure SQL server tooallow åtkomst från din dator (IP-adress).</span><span class="sxs-lookup"><span data-stu-id="c512d-169">If your client is not allowed tooaccess hello Azure SQL server, you need tooconfigure firewall for your Azure SQL server tooallow access from your machine (IP Address).</span></span> <span data-ttu-id="c512d-170">Se [i den här artikeln](../sql-database/sql-database-configure-firewall-settings.md) för steg tooconfigure hello-brandväggen för din Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="c512d-170">See [this article](../sql-database/sql-database-configure-firewall-settings.md) for steps tooconfigure hello firewall for your Azure SQL server.</span></span>

## <a name="create-a-data-factory"></a><span data-ttu-id="c512d-171">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="c512d-171">Create a data factory</span></span>
<span data-ttu-id="c512d-172">Du har slutfört hello krav.</span><span class="sxs-lookup"><span data-stu-id="c512d-172">You have completed hello prerequisites.</span></span> <span data-ttu-id="c512d-173">Du kan skapa en datafabrik med någon av följande sätt hello.</span><span class="sxs-lookup"><span data-stu-id="c512d-173">You can create a data factory using one of hello following ways.</span></span> <span data-ttu-id="c512d-174">Klicka på en hello alternativ i hello listrutan på hello top eller hello följande länkar tooperform hello kursen.</span><span class="sxs-lookup"><span data-stu-id="c512d-174">Click one of hello options in hello drop-down list at hello top or hello following links tooperform hello tutorial.</span></span>     

* [<span data-ttu-id="c512d-175">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="c512d-175">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
* [<span data-ttu-id="c512d-176">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c512d-176">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [<span data-ttu-id="c512d-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c512d-177">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [<span data-ttu-id="c512d-178">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c512d-178">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
* [<span data-ttu-id="c512d-179">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="c512d-179">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="c512d-180">REST API</span><span class="sxs-lookup"><span data-stu-id="c512d-180">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
* [<span data-ttu-id="c512d-181">.NET-API</span><span class="sxs-lookup"><span data-stu-id="c512d-181">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> <span data-ttu-id="c512d-182">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="c512d-182">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="c512d-183">Det inte transformera indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="c512d-183">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="c512d-184">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa din första pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="c512d-184">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>
> 
> <span data-ttu-id="c512d-185">Du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="c512d-185">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="c512d-186">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="c512d-186">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 
