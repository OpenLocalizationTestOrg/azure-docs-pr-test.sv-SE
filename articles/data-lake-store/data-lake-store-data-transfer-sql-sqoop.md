---
title: aaaCopy data mellan Data Lake Store och Azure SQL database med Sqoop | Microsoft Docs
description: "Använda Sqoop toocopy data mellan Azure SQL Database och Data Lake Store"
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
ms.openlocfilehash: f58483455f0ebe9544673a1d5c5884f2721c800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a><span data-ttu-id="2090c-103">Kopiera data mellan Data Lake Store och Azure SQL database med Sqoop</span><span class="sxs-lookup"><span data-stu-id="2090c-103">Copy data between Data Lake Store and Azure SQL database using Sqoop</span></span>
<span data-ttu-id="2090c-104">Lär dig hur toouse Apache Sqoop tooimport och exportera data mellan Azure SQL Database och Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2090c-104">Learn how toouse Apache Sqoop tooimport and export data between Azure SQL Database and Data Lake Store.</span></span>

## <a name="what-is-sqoop"></a><span data-ttu-id="2090c-105">Vad är Sqoop?</span><span class="sxs-lookup"><span data-stu-id="2090c-105">What is Sqoop?</span></span>
<span data-ttu-id="2090c-106">Stordataprogram är en fysisk val för bearbetning av Ostrukturerade och halvstrukturerade data, till exempel loggar och filer.</span><span class="sxs-lookup"><span data-stu-id="2090c-106">Big data applications are a natural choice for processing unstructured and semi-structured data, such as logs and files.</span></span> <span data-ttu-id="2090c-107">Men kan det också vara en behovet tooprocess strukturerade data som lagras i relationsdatabaser.</span><span class="sxs-lookup"><span data-stu-id="2090c-107">However, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="2090c-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) är ett verktyg tootransfer data mellan relationsdatabaser och en lagringsplats för stordata, till exempel Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2090c-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is a tool designed tootransfer data between  relational databases and a big data repository, such as Data Lake Store.</span></span> <span data-ttu-id="2090c-109">Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS), till exempel Azure SQL Database i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2090c-109">You can use it tooimport data from a relational database management system (RDBMS) such as Azure SQL Database into Data Lake Store.</span></span> <span data-ttu-id="2090c-110">Du kan omvandla och analysera hello data med hjälp av stordataarbetsbelastningar och sedan exportera hello data tillbaka till en RDBMS.</span><span class="sxs-lookup"><span data-stu-id="2090c-110">You can then transform and analyze hello data using big data workloads and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="2090c-111">I den här kursen använder du en Azure SQL Database som relationsdatabas tooimport/exporten från.</span><span class="sxs-lookup"><span data-stu-id="2090c-111">In this tutorial, you use an Azure SQL Database as your relational database tooimport/export from.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2090c-112">Krav</span><span class="sxs-lookup"><span data-stu-id="2090c-112">Prerequisites</span></span>
<span data-ttu-id="2090c-113">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="2090c-113">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="2090c-114">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="2090c-114">**An Azure subscription**.</span></span> <span data-ttu-id="2090c-115">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2090c-115">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2090c-116">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="2090c-116">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="2090c-117">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="2090c-117">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="2090c-118">**Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="2090c-118">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="2090c-119">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="2090c-119">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="2090c-120">Den här artikeln förutsätter att du har ett HDInsight Linux-kluster med Data Lake Store-åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2090c-120">This article assumes you have an HDInsight Linux cluster with Data Lake Store access.</span></span>
* <span data-ttu-id="2090c-121">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="2090c-121">**Azure SQL Database**.</span></span> <span data-ttu-id="2090c-122">Anvisningar för hur toocreate en, se [skapa en Azure SQL database](../sql-database/sql-database-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="2090c-122">For instructions on how toocreate one, see [Create an Azure SQL database](../sql-database/sql-database-get-started.md)</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="2090c-123">Lär du dig snabbt med videor?</span><span class="sxs-lookup"><span data-stu-id="2090c-123">Do you learn fast with videos?</span></span>
<span data-ttu-id="2090c-124">[Det här videoklippet](https://mix.office.com/watch/1butcdjxmu114) om hur toocopy data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.</span><span class="sxs-lookup"><span data-stu-id="2090c-124">[Watch this video](https://mix.office.com/watch/1butcdjxmu114) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="create-sample-tables-in-hello-azure-sql-database"></a><span data-ttu-id="2090c-125">Skapa Exempeltabeller i hello Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2090c-125">Create sample tables in hello Azure SQL Database</span></span>
1. <span data-ttu-id="2090c-126">toostart med, skapa två Exempeltabeller i hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2090c-126">toostart with, create two sample tables in hello Azure SQL Database.</span></span> <span data-ttu-id="2090c-127">Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio tooconnect toohello Azure SQL Database och sedan kör hello följande frågor.</span><span class="sxs-lookup"><span data-stu-id="2090c-127">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following queries.</span></span>

    <span data-ttu-id="2090c-128">**Skapa tabell 1**</span><span class="sxs-lookup"><span data-stu-id="2090c-128">**Create Table1**</span></span>

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

    <span data-ttu-id="2090c-129">**Skapa tabell2**</span><span class="sxs-lookup"><span data-stu-id="2090c-129">**Create Table2**</span></span>

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
2. <span data-ttu-id="2090c-130">I **tabell1**, lägga till exempeldata.</span><span class="sxs-lookup"><span data-stu-id="2090c-130">In **Table1**, add some sample data.</span></span> <span data-ttu-id="2090c-131">Lämna **tabell2** tom.</span><span class="sxs-lookup"><span data-stu-id="2090c-131">Leave **Table2** empty.</span></span> <span data-ttu-id="2090c-132">Vi kommer att importera data från **tabell1** i Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="2090c-132">We will import data from **Table1** into Data Lake Store.</span></span> <span data-ttu-id="2090c-133">Vi kommer sedan att exportera data från Data Lake Store i **tabell2**.</span><span class="sxs-lookup"><span data-stu-id="2090c-133">Then, we will export data from Data Lake Store into **Table2**.</span></span> <span data-ttu-id="2090c-134">Kör hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="2090c-134">Run hello following snippet.</span></span>

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a><span data-ttu-id="2090c-135">Använd Sqoop från ett HDInsight-kluster med åtkomst till tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="2090c-135">Use Sqoop from an HDInsight cluster with access tooData Lake Store</span></span>
<span data-ttu-id="2090c-136">Ett HDInsight-kluster har redan hello Sqoop paket som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="2090c-136">An HDInsight cluster already has hello Sqoop packages available.</span></span> <span data-ttu-id="2090c-137">Om du har konfigurerat hello HDInsight klustret toouse Data Lake Store som ett ytterligare lagringsutrymme, kan du använda Sqoop (utan några konfigurationsändringar) tooimport och exportera data mellan en relationsdatabas (i det här exemplet Azure SQL Database) och ett Data Lake Store konto.</span><span class="sxs-lookup"><span data-stu-id="2090c-137">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, you can use Sqoop (without any configuration changes) tooimport/export data between a relational database (in this example, Azure SQL Database) and a Data Lake Store account.</span></span>

1. <span data-ttu-id="2090c-138">Den här självstudiekursen förutsätter vi att du har skapat ett Linux-kluster, så du bör använda SSH tooconnect toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="2090c-138">For this tutorial, we assume you created a Linux cluster so you should use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="2090c-139">Se [Anslut tooa Linux-baserade HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2090c-139">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
2. <span data-ttu-id="2090c-140">Kontrollera om du kan komma åt hello Data Lake Store-konto från hello kluster.</span><span class="sxs-lookup"><span data-stu-id="2090c-140">Verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="2090c-141">Kör följande kommando från hello SSH prompten hello:</span><span class="sxs-lookup"><span data-stu-id="2090c-141">Run hello following command from hello SSH prompt:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    <span data-ttu-id="2090c-142">Detta bör ge en lista över filer/mappar i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="2090c-142">This should provide a list of files/folders in hello Data Lake Store account.</span></span>

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a><span data-ttu-id="2090c-143">Importera data från Azure SQL Database till Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2090c-143">Import data from Azure SQL Database into Data Lake Store</span></span>
1. <span data-ttu-id="2090c-144">Navigera toohello directory där Sqoop paket är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="2090c-144">Navigate toohello directory where Sqoop packages are available.</span></span> <span data-ttu-id="2090c-145">Vanligtvis detta kommer att vara `/usr/hdp/<version>/sqoop/bin`.</span><span class="sxs-lookup"><span data-stu-id="2090c-145">Typically, this will be at `/usr/hdp/<version>/sqoop/bin`.</span></span>
2. <span data-ttu-id="2090c-146">Importera hello data från **tabell1** till hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="2090c-146">Import hello data from **Table1** into hello Data Lake Store account.</span></span> <span data-ttu-id="2090c-147">Använd hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="2090c-147">Use hello following syntax:</span></span>

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    <span data-ttu-id="2090c-148">Observera att **sql-server-databasnamn** platshållaren hello namnet på hello servern där hello Azure SQL-databasen körs.</span><span class="sxs-lookup"><span data-stu-id="2090c-148">Note that **sql-database-server-name** placeholder represents hello name of hello server where hello Azure SQL database is running.</span></span> <span data-ttu-id="2090c-149">**SQL-databasnamnet** platshållaren hello faktiska databasnamnet.</span><span class="sxs-lookup"><span data-stu-id="2090c-149">**sql-database-name** placeholder represents hello actual database name.</span></span>

    <span data-ttu-id="2090c-150">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2090c-150">For example,</span></span>


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. <span data-ttu-id="2090c-151">Kontrollera att hello data har överförts toohello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="2090c-151">Verify that hello data has been transferred toohello Data Lake Store account.</span></span> <span data-ttu-id="2090c-152">Kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="2090c-152">Run hello following command:</span></span>

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    <span data-ttu-id="2090c-153">Du bör se hello följande utdata.</span><span class="sxs-lookup"><span data-stu-id="2090c-153">You should see hello following output.</span></span>


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    <span data-ttu-id="2090c-154">Varje **del-m -*** filen motsvarar tooa rad i hello källtabellen **tabell1**.</span><span class="sxs-lookup"><span data-stu-id="2090c-154">Each **part-m-*** file corresponds tooa row in hello source table, **Table1**.</span></span> <span data-ttu-id="2090c-155">Du kan visa hello innehållet i en del hello - m-* filer tooverify.</span><span class="sxs-lookup"><span data-stu-id="2090c-155">You can view hello contents of hello part-m-* files tooverify.</span></span>


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a><span data-ttu-id="2090c-156">Exportera data från Data Lake Store till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2090c-156">Export data from Data Lake Store into Azure SQL Database</span></span>
1. <span data-ttu-id="2090c-157">Exportera hello data från Data Lake Store-konto toohello tom tabell, **tabell2**, i hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="2090c-157">Export hello data from Data Lake Store account toohello empty table, **Table2**, in hello Azure SQL Database.</span></span> <span data-ttu-id="2090c-158">Använd följande syntax hello.</span><span class="sxs-lookup"><span data-stu-id="2090c-158">Use hello following syntax.</span></span>

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    <span data-ttu-id="2090c-159">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2090c-159">For example,</span></span>


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. <span data-ttu-id="2090c-160">Kontrollera att hello var data överförs toohello SQL-databastabell.</span><span class="sxs-lookup"><span data-stu-id="2090c-160">Verify that hello data was uploaded toohello SQL Database table.</span></span> <span data-ttu-id="2090c-161">Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio tooconnect toohello Azure SQL Database och sedan kör hello följande fråga.</span><span class="sxs-lookup"><span data-stu-id="2090c-161">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following query.</span></span>

        SELECT * FROM TABLE2

    <span data-ttu-id="2090c-162">Den har hello följande utdata.</span><span class="sxs-lookup"><span data-stu-id="2090c-162">This should have hello following output.</span></span>

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a><span data-ttu-id="2090c-163">Prestandaöverväganden när du använder Sqoop</span><span class="sxs-lookup"><span data-stu-id="2090c-163">Performance considerations while using Sqoop</span></span>

<span data-ttu-id="2090c-164">Prestandajustering din Sqoop jobbet toocopy Datasjölager för tooData, finns [Sqoop prestanda dokumentet](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span><span class="sxs-lookup"><span data-stu-id="2090c-164">For performance tuning your Sqoop job toocopy data tooData Lake Store, see [Sqoop performance document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span></span>

## <a name="see-also"></a><span data-ttu-id="2090c-165">Se även</span><span class="sxs-lookup"><span data-stu-id="2090c-165">See also</span></span>
* [<span data-ttu-id="2090c-166">Kopiera data från Azure Storage BLOB tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="2090c-166">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="2090c-167">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2090c-167">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="2090c-168">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2090c-168">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="2090c-169">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="2090c-169">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
