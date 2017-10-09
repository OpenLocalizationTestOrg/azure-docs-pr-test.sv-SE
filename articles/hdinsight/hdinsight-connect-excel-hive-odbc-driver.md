---
title: aaaConnect Excel tooHadoop med hello ODBC-drivrutinen Hive - Azure HDInsight | Microsoft Docs
description: "Lär dig hur tooset in och använda hello Microsoft Hive ODBC-drivrutin för Excel tooquery data i HDInsight-kluster från Microsoft Excel."
keywords: hadoop excel, hive excel, hive odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
tags: azure-portal
editor: cgronlun
ms.assetid: a7665a14-0211-4521-b3e7-3b26e8029cc0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/22/2017
ms.author: jgao
ms.openlocfilehash: f01f89e7d4203c739d56079dc589fc11f4aa2174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-in-azure-hdinsight-with-hello-microsoft-hive-odbc-driver"></a>Ansluta Excel tooHadoop i Azure HDInsight med hello Microsoft Hive ODBC-drivrutin

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsofts Stordata lösningen integreras Microsoft Business Intelligence (BI) komponenter med Apache Hadoop-kluster som har distribuerats av hello Azure HDInsight. Ett exempel på den här integreringen är hello möjlighet tooconnect Excel toohello Hive-datalagret i ett Hadoop-kluster i HDInsight med hello Microsoft Hive öppna ODBC (Database Connectivity) drivrutin.

Det är också möjligt tooconnect hello data som är associerade med ett HDInsight-kluster och andra datakällor, inklusive andra (icke-HDInsight) Hadoop-kluster, från Excel med hjälp av hello Microsoft Power Query-tillägg för Excel. Information om att installera och använda Power Query finns [ansluta Excel tooHDInsight med Power Query][hdinsight-power-query].

> [!NOTE]
> Medan hello stegen i den här artikeln kan användas med antingen en Linux eller Windows-baserade HDInsight-kluster Windows krävs för hello klientens arbetsstation.
> 
> 

**Krav för**:

Innan du börjar den här artikeln, måste du ha hello följande objekt:

* **Ett HDInsight-kluster**. toocreate, se [komma igång med Azure HDInsight][hdinsight-get-started].
* **En arbetsstation** med Office 2013 Professional Plus Office 365 Pro Plus, fristående för Excel 2013 eller Office Professional Plus 2010.

## <a name="install-microsoft-hive-odbc-driver"></a>Installera Microsoft Hive ODBC-drivrutin
Hämta och installera Microsoft Hive ODBC-drivrutinen från hello [Download Center][hive-odbc-driver-download].

Den här drivrutinen kan installeras på 32-bitars eller 64-bitars versioner av Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 och Windows Server 2012. hello drivrutinen tillåter anslutning tooAzure HDInsight (version 1.6 och senare) och Azure HDInsight-emulatorn (v.1.0.0.0 och senare). Du bör installera hello-version som matchar hello version av hello program där du använder hello ODBC-drivrutinen. I den här självstudien används hello drivrutinen från Office Excel.

## <a name="create-hive-odbc-data-source"></a>Skapa Hive ODBC-datakälla
hello följande steg visar hur toocreate en Hive ODBC-datakällan.

1. Tryck på startskärmen för hello Windows viktiga tooopen hello från Windows 8 eller Windows 10 och skriv sedan **datakällor**.
2. Klicka på **installera ODBC-datakällor (32-bitars)** eller **ställa in ODBC-datakällor (64-bitars)** beroende på dina Office-version. Om du använder Windows 7, väljer du **ODBC-datakällor (32 bitar)** eller **ODBC-datakällor (64-bitars)** från **Administrationsverktyg**. Du bör se hello **ODBC Data Source Administrator** dialogrutan.
   
    ![Datakälla för OBDC](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "konfigurera DSN med ODBC Data Source Administrator")

3. Användaren DNS, klicka på **Lägg till** tooopen hello **Skapa ny datakälla** guiden.
4. Välj **Microsoft Hive ODBC-drivrutinen**, och klicka sedan på **Slutför**. Du bör se hello **Microsoft Hive ODBC-drivrutinen DNS-inställningar för** dialogrutan.
5. Ange eller välj hello följande värden:
   
   | Egenskap | Beskrivning |
   | --- | --- |
   |  Namn på datakälla |Ge en namnet tooyour-datakälla |
   |  Värd |Ange &lt;HDInsightClusterName>.azurehdinsight.net. Till exempel myHDICluster.azurehdinsight.net |
   |  Port |Använd <strong>443</strong>. (Den här porten har ändrats från 563 too443.) |
   |  Databas |Använd <strong>Standard</strong>. |
   |  Mekanism |Välj <strong>Azure HDInsight-tjänst</strong> |
   |  Användarnamn |Ange ett HDInsight-kluster HTTP användaren användarnamn. hello Standardanvändarnamnet är <strong>admin</strong>. |
   |  Lösenord |Ange användarlösenord för HDInsight-kluster. |
   
    </table>
   
    Det finns vissa viktiga parametrar toobe medveten om när du klickar på **avancerade alternativ**:
   
   | Parameter | Beskrivning |
   | --- | --- |
   |  Använda en intern fråga |När den är markerad försöker hello ODBC-drivrutinen inte tooconvert TSQL i HiveQL. Du kan använda den om du är 100% säker på att du skickar ren HiveQL-instruktioner. När du ansluter tooSQL Server eller Azure SQL Database, bör den vara avmarkerad. |
   |  Rader som hämtas per block |När du hämtar ett stort antal poster kan inställning av den här parametern vara nödvändiga tooensure optimala prestanda. |
   |  Standard kolumnen stränglängd, binära kolumnlängden, Decimal kolumnskalan |hello datatypen längder och Precision-datorerna kan påverka hur data returneras. De orsakar felaktig information toobe returnerade på grund av tooloss av precision och/eller trunkering. |

    ![Avancerade alternativ](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "DSN avancerade konfigurationsalternativ")

1. Klicka på **Test** tootest hello-datakälla. Hello-datakällan är korrekt konfigurerad, visas *TESTERNA har SLUTFÖRTS!*.
2. Klicka på **OK** tooclose hello Test dialogrutan. hello ny datakälla skall anges i hello **ODBC Data Source Administrator**.
3. Klicka på **OK** tooexit hello guiden.

## <a name="import-data-into-excel-from-hdinsight"></a>Importera data till Excel från HDInsight
hello följande steg beskriver hello sätt tooimport data från en Hive-tabell till en Excel-arbetsbok med hello ODBC-datakällan som du skapade i föregående avsnitt i hello.

1. Öppna en ny eller befintlig arbetsbok i Excel.
2. Från hello **Data** klickar du på **hämta Data**, klickar du på **från andra källor**, och klicka sedan på **från ODBC** toolaunch hello  **Guiden**.
   
    ![Öppna guiden](./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "öppna guiden")
4. Välj hello data datakällans namn som du skapade i hello sista avsnittet och klicka sedan på **OK**.
5. Ange Hadoop-användarnamn (hello standardnamnet är admin) hello lösenord och klicka sedan på **Anslut**.
6. Expandera på Navigator **HIVE**, expandera **standard**, klickar du på **hivesampletable**, och klicka sedan på **belastningen**. Det tar några sekunder innan data hämtar importerade tooExcel.

    ![HDInsight Hive ODBC navigator](./media/hdinsight-connect-excel-hive-ODBC-driver/hdinsight.hive.odbc.navigator.png "öppna guiden")


## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse hello Microsoft Hive ODBC-drivrutinen tooretrieve data från hello HDInsight Service till Excel. På liknande sätt kan du hämta data från hello HDInsight Service i SQL-databasen. Det är också möjligt tooupload data till ett HDInsight Service. Det finns fler toolearn:

* [Analysera svarta fördröjning data med HDInsight][hdinsight-analyze-flight-data]
* [Ladda upp Data tooHDInsight][hdinsight-upload-data]
* [Använda Sqoop med HDInsight][hdinsight-use-sqoop]

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


