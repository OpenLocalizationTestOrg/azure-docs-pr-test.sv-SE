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
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopiera data mellan Data Lake Store och Azure SQL database med Sqoop
Lär dig hur toouse Apache Sqoop tooimport och exportera data mellan Azure SQL Database och Data Lake Store.

## <a name="what-is-sqoop"></a>Vad är Sqoop?
Stordataprogram är en fysisk val för bearbetning av Ostrukturerade och halvstrukturerade data, till exempel loggar och filer. Men kan det också vara en behovet tooprocess strukturerade data som lagras i relationsdatabaser.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) är ett verktyg tootransfer data mellan relationsdatabaser och en lagringsplats för stordata, till exempel Data Lake Store. Du kan använda den tooimport data från ett relationella databashanteringssystem (RDBMS), till exempel Azure SQL Database i Data Lake Store. Du kan omvandla och analysera hello data med hjälp av stordataarbetsbelastningar och sedan exportera hello data tillbaka till en RDBMS. I den här kursen använder du en Azure SQL Database som relationsdatabas tooimport/exporten från.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Den här artikeln förutsätter att du har ett HDInsight Linux-kluster med Data Lake Store-åtkomst.
* **Azure SQL Database**. Anvisningar för hur toocreate en, se [skapa en Azure SQL database](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Lär du dig snabbt med videor?
[Det här videoklippet](https://mix.office.com/watch/1butcdjxmu114) om hur toocopy data mellan Azure Storage-Blobbar och Data Lake Store med hjälp av DistCp.

## <a name="create-sample-tables-in-hello-azure-sql-database"></a>Skapa Exempeltabeller i hello Azure SQL Database
1. toostart med, skapa två Exempeltabeller i hello Azure SQL Database. Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio tooconnect toohello Azure SQL Database och sedan kör hello följande frågor.

    **Skapa tabell 1**

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

    **Skapa tabell2**

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
2. I **tabell1**, lägga till exempeldata. Lämna **tabell2** tom. Vi kommer att importera data från **tabell1** i Data Lake Store. Vi kommer sedan att exportera data från Data Lake Store i **tabell2**. Kör hello följande kodavsnitt.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a>Använd Sqoop från ett HDInsight-kluster med åtkomst till tooData Datasjölager
Ett HDInsight-kluster har redan hello Sqoop paket som finns tillgängliga. Om du har konfigurerat hello HDInsight klustret toouse Data Lake Store som ett ytterligare lagringsutrymme, kan du använda Sqoop (utan några konfigurationsändringar) tooimport och exportera data mellan en relationsdatabas (i det här exemplet Azure SQL Database) och ett Data Lake Store konto.

1. Den här självstudiekursen förutsätter vi att du har skapat ett Linux-kluster, så du bör använda SSH tooconnect toohello klustret. Se [Anslut tooa Linux-baserade HDInsight-kluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Kontrollera om du kan komma åt hello Data Lake Store-konto från hello kluster. Kör följande kommando från hello SSH prompten hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    Detta bör ge en lista över filer/mappar i hello Data Lake Store-konto.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Importera data från Azure SQL Database till Data Lake Store
1. Navigera toohello directory där Sqoop paket är tillgängliga. Vanligtvis detta kommer att vara `/usr/hdp/<version>/sqoop/bin`.
2. Importera hello data från **tabell1** till hello Data Lake Store-konto. Använd hello följande syntax:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Observera att **sql-server-databasnamn** platshållaren hello namnet på hello servern där hello Azure SQL-databasen körs. **SQL-databasnamnet** platshållaren hello faktiska databasnamnet.

    Exempel:


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Kontrollera att hello data har överförts toohello Data Lake Store-konto. Kör följande kommando hello:

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Du bör se hello följande utdata.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Varje **del-m -*** filen motsvarar tooa rad i hello källtabellen **tabell1**. Du kan visa hello innehållet i en del hello - m-* filer tooverify.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Exportera data från Data Lake Store till Azure SQL Database
1. Exportera hello data från Data Lake Store-konto toohello tom tabell, **tabell2**, i hello Azure SQL Database. Använd följande syntax hello.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Exempel:


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Kontrollera att hello var data överförs toohello SQL-databastabell. Använd [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) eller Visual Studio tooconnect toohello Azure SQL Database och sedan kör hello följande fråga.

        SELECT * FROM TABLE2

    Den har hello följande utdata.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Prestandaöverväganden när du använder Sqoop

Prestandajustering din Sqoop jobbet toocopy Datasjölager för tooData, finns [Sqoop prestanda dokumentet](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Se även
* [Kopiera data från Azure Storage BLOB tooData Datasjölager](data-lake-store-copy-data-azure-storage-blob.md)
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
