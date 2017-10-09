---
title: aaaConnect Excel tooHadoop med Power Query - Azure HDInsight | Microsoft Docs
description: "Lär dig hur tootake nytta av business intelligence-komponenter och använda Power Query för Excel tooaccess data lagras i Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jgao
ms.openlocfilehash: 1213849f1bc04e0ed206750b80766ff2268664b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-toohadoop-by-using-power-query"></a>Ansluta Excel tooHadoop med Power Query
En nyckelfunktion i hello Microsoft stordata lösning är hello integrering av Microsoft business intelligence (BI)-komponenter med Hadoop-kluster i Azure HDInsight. En primär exempel är hello möjlighet tooconnect Excel toohello Azure Storage-konto som innehåller hello data som är associerade med Hadoop-kluster med hjälp av hello Microsoft Power Query för Excel-tillägg. Den här artikeln får du veta hur tooset in och använda Power Query tooquery data som är associerade med ett Hadoop-kluster hanteras med HDInsight.

### <a name="prerequisites"></a>Krav
Innan du börjar den här artikeln, måste du ha hello följande objekt:

* **Ett HDInsight-kluster**. tooconfigure, se [komma igång med Azure HDInsight][hdinsight-get-started].
* **En arbetsstation** som kör Windows 7, Windows Server 2008 R2 eller senare operativsystem.
* **Office 2016, Office 2013 Professional Plus Office 365 ProPlus, fristående för Excel 2013 eller Office Professional Plus 2010**.

## <a name="install-power-query"></a>Installera Power Query
Power Query kan importera data som har tagits ut eller som har genererats av ett Hadoop-jobb som körs på ett HDInsight-kluster.

Power Query har integrerats i hello Data menyfliksområdet under hello Get & Transform-avsnitt i Excel 2016. För äldre versioner av Excel, ladda ned Microsoft Power Query för Excel från hello [Microsoft Download Center] [ powerquery-download] och installera den.

## <a name="import-hdinsight-data-into-excel"></a>Importera HDInsight data till Excel
hello Power Query-tillägget för Excel gör det enkelt tooimport data från ditt HDInsight-kluster i Excel där BI-verktyg som PowerPivot och Power mappar kan använda tooinspect, analysera och presentera hello data.

**tooimport data från ett HDInsight-kluster**

1. Öppna Excel.
2. Skapa en ny tom arbetsbok.
3. Utför följande utifrån hello Excel version hello:

    - Excel 2016

        - Klicka på hello **Data** -menyn klickar du på **hämta Data** från hello **Hämta & transformera Data** band klickar du på **från Azure**, och klicka sedan på  **Från Azure HDInsight(HDFS)**.

        ![HDI. PowerQuery.SelectHdiSource](./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Excel 2013/2010

        - Klicka på hello **Power Query** -menyn klickar du på **från Azure**, och klicka sedan på **från Microsoft Azure HDInsight**.
   
        ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Obs:** om du inte ser hello **Power Query** menyn Gå för**filen** > **alternativ** > **tillägg**, och välj **COM-tillägg** hello listrutan **hantera** rutan på hello hello sidans nederkant. Välj hello **gå...**  knappen och kontrollera hello rutan hello Power Query för Excel-tillägget har kontrollerats.
       
        **Obs:** Power Query kan du också tooimport data från HDFS genom att klicka på **från andra källor**.
4. För **kontonamn**anger hello namnet på hello Azure Blob storage-konto som är associerade med klustret och klicka sedan på **OK**. Det här kontot kan vara hello [standard lagringskonto](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) eller ett länkade storage-konto.  hello-formatet är *https://&lt;StorageAccountName >.blob.core.windows.net/*.
5. För **Kontonyckel**RETUR hello hello Blob storage-konto och klicka sedan på **spara**. (Du måste tooenter hello konto information endast hello första gången du öppnar den här butiken.)
6. I hello **Navigator** rutan på hello vänsterkant hello frågeredigeraren genom att dubbelklicka på hello Blob storage-behållarnamn. Som standard är hello behållarnamn hello samma namn som hello klusternamnet.
7. Leta upp **HiveSampleData.txt** i hello **namn** kolumn (hello mappsökväg **... / hive/datalager/hivesampletable/**), och klicka sedan på **binära** hello till vänster i HiveSampleData.txt. HiveSampleData.txt innehåller alla hello-klustret. Du kan använda en egen fil.
   
    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Om du vill kan du byta namn på hello kolumnnamn. När du är klar klickar du på **Stäng & ladda**.  hello data har lästs in tooyour arbetsboken:
   
    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse Power Query tooretrieve data från HDInsight till Excel. På liknande sätt kan du hämta data från HDInsight till Azure SQL Database. Det är också möjligt tooupload data till HDInsight. toolearn se fler hello följande artiklar:

* [Ansluta Excel tooHDInsight med hello Microsoft Hive ODBC-drivrutinen][hdinsight-ODBC]
* [Ladda upp Data tooHDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
