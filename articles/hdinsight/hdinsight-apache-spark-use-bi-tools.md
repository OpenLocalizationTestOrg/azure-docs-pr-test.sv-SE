---
title: "aaaSpark BI med hjälp av verktyg för visualisering av data på Azure HDInsight | Microsoft Docs"
description: "Använd verktyg för visualisering av data för analytics med hjälp av BI för Apache Spark i HDInsight-kluster"
keywords: "Apache spark bi, spark bi, spark datavisualisering Väck affärsinformation"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: ba4bfff737ce80ffca5c24f1c2c82a1447f467fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI med hjälp av verktyg för visualisering av data med Azure HDInsight

Lär dig hur toouse datavisualisering verktyg, till exempel Power BI och Tableau tooanalyze raw data exempeldata med Apache Spark BI på HDInsight-kluster.

> [!NOTE]
> Anslutning med BI-verktyg som beskrivs i denna artikel stöds inte på 2.1 Spark på Azure HDInsight 3,6 Preview. Endast Spark version 1.6 och 2.0 (HDInsight 3.4, 3.5 respektive) stöds.
>

Den här kursen är också tillgängliga som en Jupyter-anteckningsbok på ett HDInsight Spark-kluster. hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv. tooperform hello vägledningen i en bärbar dator, skapa ett Spark-kluster, starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), och kör sedan hello anteckningsboken **Använd BI-verktyg med Apache Spark i HDInsight.ipynb** under hello **Python**  mapp.

## <a name="prerequisites"></a>Krav

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Förbered data för Spark datavisualisering

I det här avsnittet ska vi använda hello [Jupyter](https://jupyter.org) bärbar dator från ett HDInsight Spark-kluster toorun jobb som bearbetar dina rådata exempeldata och spara den som en tabell. hello exempeldata är en CSV-fil (hvac.csv) tillgängliga på alla kluster som standard. När du har sparat dina data som en tabell i nästa avsnitt om hello vi använder BI verktyg tooconnect toohello tabell och utföra datavisualiseringar.

> [!NOTE]
> Om du utför hello stegen i den här artikeln när du har slutfört hello instruktionerna i [köra interaktiva frågor på ett HDInsight Spark-kluster](hdinsight-apache-spark-load-data-run-query.md), kan du hoppa över tooStep 8 nedan.
>

1. Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   

2. Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   > [!NOTE]
   > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Skapa en anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

    ![Skapa en Jupyter-anteckningsbok för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "skapa en Jupyter-anteckningsbok för Apache Spark BI")

4. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.

    ![Ange ett namn för anteckningsboken hello för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "ange ett namn för anteckningsboken hello för Apache Spark BI")

5. Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen. Du kan starta genom att importera hello-typer som krävs för det här scenariot. toodo så markören hello i hello cell och trycka på **SKIFT + RETUR**.

        from pyspark.sql import *

6. Läs in exempeldata i en tillfällig tabell. När du skapar ett Spark-kluster i HDInsight, hello exempeldatafil **hvac.csv**, är kopierade toohello associerade lagringskontot under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Klistra in följande hello i en tom cell fragment och tryck på **SKIFT + RETUR**. Den här fragment registrerar hello data i en tabell som kallas **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Kontrollera att hello-tabellen har skapats. Du kan använda hello `%%sql` magiska toorun Hive-frågor direkt. Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Du se utdata som visas nedan:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Endast hello tabeller som har falskt under hello **isTemporary** kolumnen är hive-tabeller som är lagrade på hello metastore och kan nås från hello BI-verktyg. I den här självstudiekursen kommer vi ansluta toohello **hvac** tabellen som vi skapade.

8. Kontrollera hello tabellen innehåller hello avsedda data. I en tom cell i hello anteckningsbok kopiera hello följande fragment och tryck på **SKIFT + RETUR**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Stäng hello anteckningsboken toorelease hello resurser. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.

## <a name="powerbi"></a>Använd Power BI för Spark datavisualisering

> [!NOTE]
> Det här avsnittet gäller endast för 1.6 Spark på HDInsight 3.4 och 2.0 Spark på HDInsight 3.5.
>
>

När du har sparat hello data som en tabell kan du använda Power BI tooconnect toohello data och visualisera den toocreate rapporter instrumentpaneler, osv.

1. Kontrollera att du har åtkomst tooPower BI. Du kan hämta en kostnadsfri förhandsversion prenumeration på Power BI från [http://www.powerbi.com/](http://www.powerbi.com/).

2. Logga in för[Power BI](http://www.powerbi.com/).

3. Hello längst ned i hello vänstra rutan klickar du på **hämta Data**.

4. På hello **hämta Data** sidan under **importera eller Anslut tooData**, för **databaser**, klickar du på **hämta**.

    ![Hämta data till Power BI för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "hämta data till Power BI för Apache Spark BI")

5. Klicka på nästa skärm hello **Spark på Azure HDInsight** och klicka sedan på **Anslut**. När du uppmanas att ange hello kluster-URL (`mysparkcluster.azurehdinsight.net`) och hello autentiseringsuppgifter tooconnect toohello kluster.

    ![Ansluta tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "ansluta tooApache Spark BI")

    Efter hello anslutningen har upprättats startar Power BI import av data från hello Spark-kluster i HDInsight.

6. Powerbi importerar hello data och lägger till en **Spark** dataset under hello **datauppsättningar** rubrik. Klicka på hello datauppsättning tooopen nya toovisualize hello kalkylbladsdata. Du kan också spara hello kalkylbladet som en rapport. toosave ett kalkylblad från hello **filen** -menyn klickar du på **spara**.

    ![Apache Spark BI-panelen på Power BI-instrumentpanel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI-panelen på Power BI-instrumentpanel")
7. Observera att hello **fält** listan hello höger visar hello **hvac** tabellen som du skapade tidigare. Expandera hello toosee hello fält i hello tabell, som du har definierat i anteckningsbok tidigare.

      ![Visa en lista med tabeller på Apache Spark BI-instrumentpanel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "lista tabeller på Apache Spark BI-instrumentpanel")

8. Skapa en visualiseringen tooshow hello variansen mellan target temperatur- och faktiska temperatur för varje byggnad. Välj toovisualize med data **ytdiagram** (visas med röd ruta). toodefine hello axeln kan dra och släpp hello **BuildingID** fältet under **axel**, och **ActualTemp**/**TargetTemp** fält **värdet**.

    ![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "skapa Spark datavisualiseringar med Apache Spark BI")

9. Som standard visar hello visualiseringen hello summan för **ActualTemp** och **TargetTemp**. För båda hello fält från hello listrutan, Välj **genomsnittlig** tooget genomsnitt faktiska och mål temperaturer för båda byggnader.

    ![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "skapa Spark datavisualiseringar med Apache Spark BI")

10. Visualisering av data ska vara liknande toohello något i hello skärmbilden. Flytta markören över hello visualiseringen tooget verktygstips med relevanta data.

    ![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "skapa Spark datavisualiseringar med Apache Spark BI")

11. Klicka på **spara** från hello översta menyn och ange ett rapportnamn. Du kan även fästa hello visual. När du fäster en visualisering lagras den på din instrumentpanel så att du kan spåra hello senaste värdet i korthet.

   Du kan lägga till så många visualiseringar som du vill använda för hello samma datamängd och fästa dem toohello instrumentpanel för en ögonblicksbild av dina data. Spark-kluster i HDInsight är också ansluten tooPower BI med direct connect. Detta säkerställer att Power BI har alltid hello de flesta uppdaterade data från ditt kluster så du inte behöver tooschedule uppdateringar för hello dataset.

## <a name="tableau"></a>Använda Tableau skrivbordet för Spark datavisualisering

> [!NOTE]
> Det här avsnittet gäller endast för Spark 1.5.2 kluster som skapas i Azure HDInsight.
>
>

1. Installera [Tableau Desktop](http://www.tableau.com/products/desktop) på hello dator där du kör den här kursen om Apache Spark BI.

2. Kontrollera att datorn har även Microsoft Spark ODBC-drivrutinen ska installeras. Du kan installera hello drivrutin från [här](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Starta Tableau skrivbordet. Klicka på hello vänster hello listan över server tooconnect, **Spark SQL**. Om inte Spark SQL visas som standard i hello vänster hittar du den genom att klicka på **fler servrar**.
2. Ange hello värden hello Spark SQL-anslutningen i dialogrutan som visas i skärmbilden hello och klicka sedan på **OK**.

    ![Anslut tooa kluster för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Anslut tooa kluster för Apache Spark BI")

    Hej autentisering listrutorna **Microsoft Azure HDInsight-tjänst** som ett alternativ, endast om du har installerat hello [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) på hello-dator.
3. På nästa hello-skärmen, från hello **schemat** listrutan, klickar du på hello **hitta** ikonen och klickar sedan på **standard**.

    ![Hitta schema för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "hitta schemat för Apache Spark BI")
4. För hello **tabell** klickar hello **hitta** ikonen igen toolist alla hello Hive tabeller som är tillgängliga i hello kluster. Du bör se hello **hvac** tabellen som du skapat tidigare med hello bärbar dator.

    ![Hitta tabellen för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "hitta tabellen för Apache Spark BI")
5. Dra och släpp hello tabell toohello översta rutan på hello rätt. Tableau importerar hello data och visar hello schemat som markerade av hello röd ruta.

    ![Lägga till tabeller tooTableau för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "lägga till tabeller tooTableau för Apache Spark BI")
6. Klicka på hello **Blad1** högst hello längst ned till vänster. Gör en visualisering som visar hello genomsnittlig mål och faktiska temperaturer för alla byggnader för varje datum. Dra **datum** och **byggnad ID** för**kolumner** och **faktiska Temp**/**mål Temp**för**rader**. Under **märken**väljer **området** toouse en området mappning för Spark datavisualisering.

     ![Lägga till fält i Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "lägga till fält i Spark datavisualisering")
7. Som standard hello temperatur fält visas som aggregat. Om du vill tooshow hello genomsnittlig temperaturer i stället kan göra du det från hello nedrullningsbara enligt nedan.

    ![Ta medelvärde för temperaturen för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "ta medelvärde för temperaturen för Spark datavisualisering")

8. Du kan också super-införa en temperatur mappning över hello andra tooget en bättre känsla skillnaden mellan mål- och faktiska temperaturer. Flytta hello musen toohello hörn hello lägre området kartan tills du ser hello referensen formen markerade med en röd cirkel. Dra hello mappa toohello andra kartan på hello upp och version hello musen när du ser hello formen markerat i rött rektangel.

    ![Sammanfoga för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge mappar för Spark datavisualisering")

     Ändra din datavisualisering som visas i skärmbilden hello:

    ![Tableau utdata för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau utdata för Spark datavisualisering")
9. Klicka på **spara** toosave hello kalkylblad. Du kan skapa instrumentpaneler och lägga till en eller flera blad tooit.

## <a name="next-steps"></a>Nästa steg

Så länge du lärt dig hur toocreate ett kluster, skapa Spark data ramar tooquery data och komma åt dessa data från BI-verktyg. Du kan nu se instruktioner för hur toomanage hello klusterresurser och felsöka jobb som körs i ett HDInsight Spark-kluster.

* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

