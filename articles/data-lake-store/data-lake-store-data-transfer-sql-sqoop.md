---
title: Kopiera data mellan Data Lake Store och Azure SQL database med Sqoop | Microsoft Docs
description: "Använd Sqoop för att kopiera data mellan Azure SQL Database och Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 53bf33f6027f1f365bd92251490d5c851fb83f8b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a><span data-ttu-id="00aaa-103">Kopiera data mellan Data Lake Store och Azure SQL database med Sqoop</span><span class="sxs-lookup"><span data-stu-id="00aaa-103">Copy data between Data Lake Store and Azure SQL database using Sqoop</span></span>
<span data-ttu-id="00aaa-104">Lär dig hur du använder Apache Sqoop för att importera och exportera data mellan Azure SQL Database och Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="00aaa-104">Learn how to use Apache Sqoop to import and export data between Azure SQL Database and Data Lake Store.</span></span>

## <a name="what-is-sqoop"></a><span data-ttu-id="00aaa-105">Vad är Sqoop?</span><span class="sxs-lookup"><span data-stu-id="00aaa-105">What is Sqoop?</span></span>
<span data-ttu-id="00aaa-106">Stordataprogram är en fysisk val för bearbetning av Ostrukturerade och halvstrukturerade data, till exempel loggar och filer.</span><span class="sxs-lookup"><span data-stu-id="00aaa-106">Big data applications are a natural choice for processing unstructured and semi-structured data, such as logs and files.</span></span> <span data-ttu-id="00aaa-107">Men kan det också vara nödvändigt att bearbeta strukturerade data som lagras i relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="00aaa-107">However, there may also be a need to process structured data that is stored in relational databases.</span></span>

<span data-ttu-id="00aaa-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) är ett verktyg som utformats för att överföra data mellan relationsdatabaser och en lagringsplats för stordata, till exempel Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="00aaa-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is a tool designed to transfer data between  relational databases and a big data repository, such as Data Lake Store.</span></span> <span data-ttu-id="00aaa-109">Du kan använda den för att importera data från ett relationella databashanteringssystem (RDBMS), till exempel Azure SQL Database till Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="00aaa-109">You can use it to import data from a relational database management system (RDBMS) such as Azure SQL Database into Data Lake Store.</span></span> <span data-ttu-id="00aaa-110">Du kan sedan transformera och analysera data med stordataarbetsbelastningar och exportera data till en RDBMS.</span><span class="sxs-lookup"><span data-stu-id="00aaa-110">You can then transform and analyze the data using big data workloads and then export the data back into an RDBMS.</span></span> <span data-ttu-id="00aaa-111">I den här kursen använder du en Azure SQL Database som relationell databas för att importera och exportera från.</span><span class="sxs-lookup"><span data-stu-id="00aaa-111">In this tutorial, you use an Azure SQL Database as your relational database to import/export from.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00aaa-112">Krav</span><span class="sxs-lookup"><span data-stu-id="00aaa-112">Prerequisites</span></span>
<span data-ttu-id="00aaa-113">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="00aaa-113">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="00aaa-114">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="00aaa-114">**An Azure subscription**.</span></span> <span data-ttu-id="00aaa-115">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00aaa-115">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="00aaa-116">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="00aaa-116">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="00aaa-117">Anvisningar om hur du skapar en finns [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="00aaa-117">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="00aaa-118">**Azure HDInsight-kluster** med åtkomst till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="00aaa-118">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="00aaa-119">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="00aaa-119">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="00aaa-120">Den här artikeln förutsätter att du har ett HDInsight Linux-kluster med Data Lake Store-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="00aaa-120">This article assumes you have an HDInsight Linux cluster with Data Lake Store access.</span></span>
* <span data-ttu-id="00aaa-121">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="00aaa-121">**Azure SQL Database**.</span></span> <span data-ttu-id="00aaa-122">Anvisningar om hur du skapar en finns [skapa en Azure SQL database](../sql-database/sql-database-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="00aaa-122">For instructions on how to create one, see [Create an Azure SQL database](../sql-database/sql-database-get-started.md)</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="00aaa-123">Lär du dig snabbt med videor?</span><span class="sxs-lookup"><span data-stu-id="00aaa-123">Do you learn fast with videos?</span></span>
<span data-ttu-id="00aaa-124">[Det här videoklippet](https://mix.office.com/watch/1butcdjxmu114) om hur du kopierar data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.</span><span class="sxs-lookup"><span data-stu-id="00aaa-124">[Watch this video](https://mix.office.com/watch/1butcdjxmu114) on how to copy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="create-sample-tables-in-the-azure-sql-database"></a><span data-ttu-id="00aaa-125">Skapa Exempeltabeller i Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="00aaa-125">Create sample tables in the Azure SQL Database</span></span>
1. <span data-ttu-id="00aaa-126">Börja med, skapa två Exempeltabeller i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="00aaa-126">To start with, create two sample tables in the Azure SQL Database.</span></span> <span data-ttu-id="00aaa-127">Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio för att ansluta till Azure SQL-databasen och kör sedan följande frågor.</span><span class="sxs-lookup"><span data-stu-id="00aaa-127">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio to connect to the Azure SQL Database and then run the following queries.</span></span>

    <span data-ttu-id="00aaa-128">**Skapa tabell 1**</span><span class="sxs-lookup"><span data-stu-id="00aaa-128">**Create Table1**</span></span>

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    <span data-ttu-id="00aaa-129">**Skapa tabell2**</span><span class="sxs-lookup"><span data-stu-id="00aaa-129">**Create Table2**</span></span>

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. <span data-ttu-id="00aaa-130">I **tabell1**, lägga till exempeldata.</span><span class="sxs-lookup"><span data-stu-id="00aaa-130">In **Table1**, add some sample data.</span></span> <span data-ttu-id="00aaa-131">Lämna **tabell2** tom.</span><span class="sxs-lookup"><span data-stu-id="00aaa-131">Leave **Table2** empty.</span></span> <span data-ttu-id="00aaa-132">Vi kommer att importera data från **tabell1** i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="00aaa-132">We will import data from **Table1** into Data Lake Store.</span></span> <span data-ttu-id="00aaa-133">Vi kommer sedan att exportera data från Data Lake Store i **tabell2**.</span><span class="sxs-lookup"><span data-stu-id="00aaa-133">Then, we will export data from Data Lake Store into **Table2**.</span></span> <span data-ttu-id="00aaa-134">Kör följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="00aaa-134">Run the following snippet.</span></span>

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a><span data-ttu-id="00aaa-135">Använda Sqoop från ett HDInsight-kluster med åtkomst till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-135">Use Sqoop from an HDInsight cluster with access to Data Lake Store</span></span>
<span data-ttu-id="00aaa-136">Ett HDInsight-kluster har redan de Sqoop paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="00aaa-136">An HDInsight cluster already has the Sqoop packages available.</span></span> <span data-ttu-id="00aaa-137">Om du har konfigurerat HDInsight-klustret för att använda Data Lake Store som ett ytterligare lagringsutrymme, du kan använda Sqoop (utan några konfigurationsändringar) för att importera och exportera data mellan en relationsdatabas (i det här exemplet Azure SQL Database) och ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="00aaa-137">If you have configured the HDInsight cluster to use Data Lake Store as an additional storage, you can use Sqoop (without any configuration changes) to import/export data between a relational database (in this example, Azure SQL Database) and a Data Lake Store account.</span></span>

1. <span data-ttu-id="00aaa-138">Den här självstudiekursen förutsätter vi att du har skapat ett Linux-kluster, så du bör använda SSH för att ansluta till klustret.</span><span class="sxs-lookup"><span data-stu-id="00aaa-138">For this tutorial, we assume you created a Linux cluster so you should use SSH to connect to the cluster.</span></span> <span data-ttu-id="00aaa-139">Se [Anslut till ett Linux-baserat HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="00aaa-139">See [Connect to a Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
2. <span data-ttu-id="00aaa-140">Kontrollera om du har åtkomst till Data Lake Store-konto från klustret.</span><span class="sxs-lookup"><span data-stu-id="00aaa-140">Verify whether you can access the Data Lake Store account from the cluster.</span></span> <span data-ttu-id="00aaa-141">Kör följande kommando från SSH-prompten:</span><span class="sxs-lookup"><span data-stu-id="00aaa-141">Run the following command from the SSH prompt:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    <span data-ttu-id="00aaa-142">Detta bör ge en lista över filer/mappar i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="00aaa-142">This should provide a list of files/folders in the Data Lake Store account.</span></span>

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a><span data-ttu-id="00aaa-143">Importera data från Azure SQL Database till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-143">Import data from Azure SQL Database into Data Lake Store</span></span>
1. <span data-ttu-id="00aaa-144">Gå till den katalog där Sqoop paket är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="00aaa-144">Navigate to the directory where Sqoop packages are available.</span></span> <span data-ttu-id="00aaa-145">Vanligtvis detta kommer att vara `/usr/hdp/<version>/sqoop/bin`.</span><span class="sxs-lookup"><span data-stu-id="00aaa-145">Typically, this will be at `/usr/hdp/<version>/sqoop/bin`.</span></span>
2. <span data-ttu-id="00aaa-146">Importera data från **tabell1** i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="00aaa-146">Import the data from **Table1** into the Data Lake Store account.</span></span> <span data-ttu-id="00aaa-147">Använd följande syntax:</span><span class="sxs-lookup"><span data-stu-id="00aaa-147">Use the following syntax:</span></span>

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    <span data-ttu-id="00aaa-148">Observera att **sql-server-databasnamn** platshållare representerar namnet på den server där Azure SQL-databasen körs.</span><span class="sxs-lookup"><span data-stu-id="00aaa-148">Note that **sql-database-server-name** placeholder represents the name of the server where the Azure SQL database is running.</span></span> <span data-ttu-id="00aaa-149">**SQL-databasnamnet** platshållare representerar det faktiska databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="00aaa-149">**sql-database-name** placeholder represents the actual database name.</span></span>

    <span data-ttu-id="00aaa-150">Exempel:</span><span class="sxs-lookup"><span data-stu-id="00aaa-150">For example,</span></span>


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. <span data-ttu-id="00aaa-151">Kontrollera att data har överförts till Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="00aaa-151">Verify that the data has been transferred to the Data Lake Store account.</span></span> <span data-ttu-id="00aaa-152">Kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="00aaa-152">Run the following command:</span></span>

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    <span data-ttu-id="00aaa-153">Du bör se följande utdata.</span><span class="sxs-lookup"><span data-stu-id="00aaa-153">You should see the following output.</span></span>


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    <span data-ttu-id="00aaa-154">Varje **del-m -*** fil motsvarar en rad i tabellen källa **tabell1**.</span><span class="sxs-lookup"><span data-stu-id="00aaa-154">Each **part-m-*** file corresponds to a row in the source table, **Table1**.</span></span> <span data-ttu-id="00aaa-155">Du kan visa innehållet i en del - m-* verifiera.</span><span class="sxs-lookup"><span data-stu-id="00aaa-155">You can view the contents of the part-m-* files to verify.</span></span>


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a><span data-ttu-id="00aaa-156">Exportera data från Data Lake Store till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="00aaa-156">Export data from Data Lake Store into Azure SQL Database</span></span>
1. <span data-ttu-id="00aaa-157">Exportera data från Data Lake Store-konto till tom tabell **tabell2**, i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="00aaa-157">Export the data from Data Lake Store account to the empty table, **Table2**, in the Azure SQL Database.</span></span> <span data-ttu-id="00aaa-158">Använd följande syntax.</span><span class="sxs-lookup"><span data-stu-id="00aaa-158">Use the following syntax.</span></span>

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    <span data-ttu-id="00aaa-159">Exempel:</span><span class="sxs-lookup"><span data-stu-id="00aaa-159">For example,</span></span>


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. <span data-ttu-id="00aaa-160">Kontrollera att data överfördes till SQL-databastabell.</span><span class="sxs-lookup"><span data-stu-id="00aaa-160">Verify that the data was uploaded to the SQL Database table.</span></span> <span data-ttu-id="00aaa-161">Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio för att ansluta till Azure SQL-databasen och kör sedan följande fråga.</span><span class="sxs-lookup"><span data-stu-id="00aaa-161">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio to connect to the Azure SQL Database and then run the following query.</span></span>

        SELECT * FROM TABLE2

    <span data-ttu-id="00aaa-162">Detta bör ha följande utdata.</span><span class="sxs-lookup"><span data-stu-id="00aaa-162">This should have the following output.</span></span>

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a><span data-ttu-id="00aaa-163">Prestandaöverväganden när du använder Sqoop</span><span class="sxs-lookup"><span data-stu-id="00aaa-163">Performance considerations while using Sqoop</span></span>

<span data-ttu-id="00aaa-164">Prestandajustering Sqoop jobbet för att kopiera data till Data Lake Store, se [Sqoop prestanda dokumentet](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span><span class="sxs-lookup"><span data-stu-id="00aaa-164">For performance tuning your Sqoop job to copy data to Data Lake Store, see [Sqoop performance document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span></span>

## <a name="see-also"></a><span data-ttu-id="00aaa-165">Se även</span><span class="sxs-lookup"><span data-stu-id="00aaa-165">See also</span></span>
* [<span data-ttu-id="00aaa-166">Kopiera data från Azure Storage-Blobbar till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-166">Copy data from Azure Storage Blobs to Data Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="00aaa-167">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-167">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="00aaa-168">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-168">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="00aaa-169">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="00aaa-169">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
