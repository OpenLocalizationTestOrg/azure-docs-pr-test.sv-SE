---
title: aaaApache Sqoop med Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Sqoop tooimport och exportera mellan Hadoop i HDInsight och en Azure SQL Database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: b256285659bbcf18ff05e220ccdf51c21eb8fbf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Använd Apache Sqoop tooimport och exportera data mellan Hadoop i HDInsight och SQL-databas

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Lär dig hur toouse Apache Sqoop tooimport och exportera mellan ett Hadoop-kluster i Azure HDInsight och Azure SQL Database eller Microsoft SQL Server-databasen. hello stegen i det här dokumentet används hello `sqoop` direkt från hello headnode av hello Hadoop-kluster. Du använder SSH tooconnect toohello huvudnod och kör hello kommandon i det här dokumentet.

> [!IMPORTANT]
> hello fungerar stegen i det här dokumentet endast med HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="install-freetds"></a>Installera FreeTDS

1. Använda SSH tooconnect toohello HDInsight-kluster. Till exempel följande kommando hello ansluter toohello primära headnode i ett kluster med namnet `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd följande kommando tooinstall FreeTDS hello:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    FreeTDS används i flera steg tooconnect tooSQL databas.

## <a name="create-hello-table-in-sql-database"></a>Skapa hello tabell i SQL-databas

> [!IMPORTANT]
> Om du använder hello HDInsight-kluster och SQL-databas som skapats i [skapa klustret och SQL database](hdinsight-use-sqoop.md), hoppa över hello steg i det här avsnittet. hello databas och tabell skapats som en del av hello stegen i hello [skapa klustret och SQL database](hdinsight-use-sqoop.md) dokumentet.

1. Använd hello efter kommandot tooconnect toohello SQL Database-server från hello SSH-sessionen.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Du får utdata liknande toohello följande text:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. Vid hello `1>` uppmanar, ange hello följande fråga:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    När hello `GO` uttryck har angetts, hello tidigare rapporter utvärderas. Först hello **mobiledata** tabell skapas och sedan ett grupperat index läggs tooit (obligatoriskt för SQL-databas).

    Använd hello följande fråga tooverify som hello tabellen har skapats:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Du kan se utdata liknande toohello följande text:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Ange `exit` på hello `1>` fråga tooexit hello tsql-verktyget.

## <a name="sqoop-export"></a>Sqoop export

1. Använd hello följande kommando från hello SSH-anslutning toohello kluster tooverify att Sqoop kan se din SQL-databas:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    När du uppmanas du ange hello lösenord för hello inloggningen för SQL-databas.

    Det här kommandot returnerar en lista över databaser, inklusive hello **sqooptest** databasen som du skapade tidigare.

2. tooexport data från **hivesampletable** toohello **mobiledata** tabell använder hello följande kommando:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    Det här kommandot instruerar Sqoop tooconnect toohello **sqooptest** databas. Sqoop sedan exporterar data från **wasb: / / / hive/datalager/hivesampletable** toohello **mobiledata** tabell.

    > [!IMPORTANT]
    > Använd `wasb:///` om hello standardlagring för klustret är ett Azure Storage-konto. Använd `adl:///` om det är ett Azure Data Lake Store.

3. När hello-kommandot har slutförts kan du använda följande kommando tooconnect toohello databasen med TSQL hello:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    När du är ansluten, Använd hello följande instruktioner tooverify som hello data var exporterade toohello **mobiledata** tabell:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Du bör se en lista över data i hello tabell. Typen `exit` tooexit hello tsql-verktyget.

## <a name="sqoop-import"></a>Sqoop import

1. Använd hello följande kommando tooimport data från hello **mobiledata** tabell i SQL-databas, toohello **wasb: / / / självstudier/usesqoop/importeddata** på HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    hello fälten i hello data avgränsas med ett tabbtecken och hello rader avslutas med ett tecken för ny rad.

2. När hello importen har slutförts använder du hello efter kommandot toolist ut hello data i hello ny katalog:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Använda SQLServer

Du kan också använda Sqoop tooimport och exportera data från SQL Server i ditt datacenter eller på en virtuell dator i Azure. hello skillnaderna mellan att använda SQL Database och SQL Server är:

* HDInsight- och SQL Server måste vara på hello samma virtuella Azure-nätverk.

    Ett exempel finns hello [ansluta HDInsight tooyour lokalt nätverk](./connect-on-premises-network.md) dokumentet.

    Mer information om hur du använder HDInsight med ett Azure Virtual Network finns hello [utöka HDInsight med Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet. Mer information i Azure Virtual Network finns hello [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md) dokumentet.

* SQL Server måste vara konfigurerade tooallow SQL-autentisering. Mer information finns i hello [välja ett autentiseringsläge](https://msdn.microsoft.com/ms144284.aspx) dokumentet.

* Du kan ha SQL Server tooconfigure tooaccept fjärranslutningar. Mer information finns i hello [hur tootroubleshoot anslutande toohello SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentet.

* Skapa hello **sqooptest** databas i SQL Server med hjälp av ett verktyg som till exempel **SQL Server Management Studio** eller **tsql**. hello steg för att använda hello Azure CLI fungerar bara för Azure SQL Database.

    Använd hello följande Transact-SQL-instruktioner toocreate hello **mobiledata** tabell:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* När du ansluter toohello SQL Server från HDInsight kan kanske du toouse hello IP-adressen för hello SQL Server. Exempel:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Begränsningar

* Masskapat export - med Linux-baserat HDInsight, hello Sqoop koppling som används tooexport data tooMicrosoft SQL Server eller Azure SQL Database stöder för närvarande inte bulkinfogningar.

* Batchbearbetning - med Linux-baserat HDInsight när du använder hello `-batch` växeln när du utför infogningar, Sqoop gör flera infogningar i stället för batchbearbetning hello infogningsåtgärder.

## <a name="next-steps"></a>Nästa steg

Nu har du fått veta hur toouse Sqoop. Det finns fler toolearn:

* [Använda Oozie med HDInsight][hdinsight-use-oozie]: Använd Sqoop åtgärd i ett arbetsflöde för Oozie.
* [Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]: använda Hive tooanalyze svarta fördröjning data och sedan använda Sqoop tooexport data tooan Azure SQL-databas.
* [Ladda upp data tooHDInsight][hdinsight-upload-data]: hitta andra metoder för att överföra data tooHDInsight/Azure Blob storage.

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
