---
title: "aaaMachine learning exempel med MLlib Spark på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse Spark MLlib toocreate en machine learning-app som analyserar en datauppsättning med hjälp av klassificering via logistic regression."
keywords: "Spark maskininlärning spark machine learning-exempel"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a>Använda Spark MLlib toobuild machine learning-program och analysera en datamängd

Lär dig hur toouse Spark **MLlib** toocreate en maskininlärning toodo enkel förutsägande analys av program på en öppen datauppsättning. Från Sparks inbyggda maskininlärning bibliotek, det här exemplet används *klassificering* via logistic regression. 

> [!TIP]
> Det här exemplet är också tillgängliga som en Jupyter-anteckningsbok på ett kluster med Spark (Linux) som du skapar i HDInsight. hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv. toofollow hello vägledningen i en bärbar dator, skapa ett Spark-klustret och starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Kör sedan hello anteckningsboken **Spark Machine Learning - förutsägbar analys på mat inspektion data med hjälp av MLlib.ipynb** under hello **Python** mapp.
>
>

MLlib är ett Spark core-bibliotek som innehåller många användbara verktyg för machine learning aktiviteter, inklusive verktyg som är lämpliga för:

* Klassificering
* Regression
* Klustring
* Avsnittet modellering
* Enda värde uppdelning (SVD) och viktigaste komponenten analys (PCA)
* Hypotesen testning och beräkna exempel statistik

## <a name="what-are-classification-and-logistic-regression"></a>Vad är klassificering och logistic regression?
*Klassificering*, populära machine learning-aktivitet, är hello process för inkommande data sorteras i kategorier. Det är hello jobb av en klassificering algoritmen toofigure reda på hur tooassign ”märker” tooinput data som du anger. Exempelvis kan du se en maskininlärningsalgoritmen som accepterar lager information som indata och dividerar hello börs i två kategorier: du bör säljer och lagren bör du tänka.

Logistic regression är hello-algoritm som du använder för klassificering. Sparks logistic regression API är användbart för *binär klassificering*, eller klassificera indata till en av två grupper. Läs mer om logistic regressioner [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Sammanfattningsvis hello sammankoppling av logistic regression leder till en *logistic funktionen* som kan vara används toopredict hello sannolikheten att en inkommande vector tillhör en grupp eller hello andra.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>Förutsägbar analys exempel på mat inspektion data
I det här exemplet du använder Spark tooperform vissa förutsägbar analys på mat inspektion data (**Food_Inspections1.csv**) som har köpts via hello [stad Chicago dataportalen](https://data.cityofchicago.org/). Den här datauppsättningen innehåller information om mat etablering kontroller som har utförts i Chicago, inklusive information om varje anläggning, hello överträdelser hittades (eventuella) och hello resultaten av hello kontroll. hello CSV-fil finns redan i hello storage-konto som är associerade med hello klustret på **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

I hello stegen nedan, utveckla en modell toosee vad som krävs för toopass eller misslyckas en mat-kontroll.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Börja skapa Spark MMLib machine learning-app
1. Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   
1. Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   > [!NOTE]
   > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Skapa en anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

    ![Skapa en Jupyter-anteckningsbok](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "skapa en ny Jupyter-anteckningsbok")
1. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.

    ![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "ange ett namn för anteckningsboken hello")
1. Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen. Du kan börja skapa ditt maskininlärning program genom att importera hello-typer som krävs för det här scenariot. toodo så markören hello i hello cell och trycka på **SKIFT + RETUR**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Skapa en inkommande dataframe
Vi kan använda `sqlContext` tooperform omformningar på strukturerade data. hello första uppgiften är tooload hello exempeldata ((**Food_Inspections1.csv**)) i en Spark SQL *dataframe*.

1. Eftersom hello rådata är CSV-format, måste toouse hello Spark kontexten toopull varje rad i hello-fil i minnet som Ostrukturerade text. sedan använder Python's CSV biblioteket tooparse varje rad individuellt.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. Nu har vi hello CSV-fil som en RDD.  toounderstand hello schemat för hello, vi hämtar en rad från hello RDD.

        inspections.take(1)

    Du bör se utdata som liknar hello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. hello ger föregående utdata oss en uppfattning om hello schemat för hello indatafilen. Den omfattar hello namnet på varje etablering, hello typ av etablering, hello adress, hello data för hello kontroller och hello plats, bland annat. Välj vi några kolumner som är användbara för våra förutsägbar analys och gruppera hello resultat som en dataframe som vi sedan använda toocreate en tillfällig tabell.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Nu har vi en *dataframe*, `df` som vi kan utföra vår analys. Vi har också en tillfällig tabell anropet **CountResults**. Innehåller fyra kolumner av intresse för hello dataframe: **id**, **namn**, **resultat**, och **överträdelser**.

    Det är dags ett litet antal hello data:

        df.show(5)

    Du bör se utdata som liknar hello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a>Förstå hello data
1. Låt oss börja tooget en uppfattning om vår datauppsättning innehåller. Vad är till exempel hello olika värden i hello **resultat** kolumnen?

        df.select('results').distinct().show()

    Du bör se utdata som liknar hello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. En snabb visualisering hjälper oss orsak om hello distribution av dessa resultat. Vi har redan hello data i en tillfällig tabell **CountResults**. Du kan köra följande SQL-fråga mot hello tabell tooget hello en bättre förståelse för hur hello resultat distribueras.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Hej `%%sql` magic följt av `-o countResultsdf` säkerställer att hello utdata från frågan hello sparas lokalt på hello Jupyter-servern (vanligtvis hello headnode hello klustret). hello utdata sparas som en [Pandas](http://pandas.pydata.org/) dataframe med hello angett namn **countResultsdf**.

    Du bör se utdata som liknar hello följande:

    ![SQL-frågan](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL frågeresultatet")

    Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. Du kan också använda Matplotlib, ett bibliotek används tooconstruct visualisering av data, toocreate ritning. Eftersom hello ritytans måste skapas från hello lokalt sparade **countResultsdf** dataframe, hello kodstycke måste börja med hello `%%local` Magiskt tal. Detta säkerställer att hello kod körs lokalt på hello Jupyter-servern.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Du bör se utdata som liknar hello följande:

    ![Spark maskininlärning programmet utdata - cirkeldiagram med fem olika resultaten](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark maskininlärning resultatet utdata")
1. Du kan se att det finns 5 distinkta resultat som kan ha en inspektion:

   * Företag som inte finns
   * Misslyckas
   * Skicka
   * PSS med villkor
   * Out-of-Business

     Låt oss utveckla en modell som kan gissa hello resultatet av en mat inspektion angivna hello överträdelser. Eftersom logistic regression är en klassificeringsmetod i binär, gör det klokt toogroup våra data i två kategorier: **misslyckas** och **skicka**. En ”skicka med villkor” är fortfarande ett steg, så vi Tänk hello när vi träna modellen hello två resulterar motsvarande. Data med hello andra resultat (”företag kan inte hitta” eller ”Out-of-Business”) är inte användbar så vi ta bort dem från våra träningsmängden. Detta bör vara bra eftersom dessa två kategorier utgör en liten andel av hello resultat ändå.
1. Låt oss gå vidare och konvertera vår befintliga dataframe (`df`) till en ny dataframe där varje inspektion representeras som ett par etikett överträdelser. I det här fallet en etikett för `0.0` representerar ett fel, en etikett för `1.0` representerar en lyckad och en etikett för `-1.0` representerar vissa resultat utöver de två. Vi kan filtrera dessa andra resultat ut när hello ny data ram.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    toosee vilka hello märkta data verkar vara, hämta vi en rad.

        labeledData.take(1)

    Du bör se utdata som liknar hello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a>Skapa en logistic regressionsmodell från hello inkommande dataframe
Vår sista aktivitet är tooconvert hello märkta data till ett format som kan analyseras av logistic regression. algoritmen för hello inkommande tooa logistic regression ska vara en uppsättning *etikett-funktionen vector par*, där hello ”funktionen vector” är en vektor med siffror som representerar hello indata. Vi behöver så tooconvert hello ”överträdelser” kolumn som är halvstrukturerade och innehåller många kommentarer i fritext, tooan matris av reellt tal som en dator lätt kan förstå.

En standard machine learning-metod för behandling av naturligt språk är tooassign varje distinkta ord ”index”, och sedan skicka en vector toohello maskininlärningsalgoritmen så att varje indexvärde innehåller hello ofta ordet i hello text sträng.

MLlib ger ett enkelt sätt tooperform den här åtgärden. Först ”tokenize” varje överträdelser sträng tooget hello enskilda ord i varje sträng. Använd sedan en `HashingTF` tooconvert varje uppsättning säkerhetstoken till en funktion vector som sedan kan skickade toohello logistic regression algoritmen tooconstruct en modell. Vi genomför alla stegen i ordning med hjälp av en ”pipeline”.

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a>Utvärdera hello modell på en separat testdata
Vi kan använda hello modellen som vi skapade tidigare för*förutsäga* vilka hello resultaten av nya kontroller kommer att vara, baserat på hello överträdelser som observerades. Vi har tränat modellen för hello datauppsättningen **Food_Inspections1.csv**. Låt oss använder en andra datauppsättningen **Food_Inspections2.csv**, för*utvärdera* hello styrkan hos den här modellen på nya data. Den här andra datamängden (**Food_Inspections2.csv**) bör redan vara i hello standardbehållare för lagring som associerats med hello kluster.

1. hello följande kodutdrag skapar en ny dataframe **predictionsDf** som innehåller hello förutsägelse som genereras av hello modell. hello fragment skapas också en tillfällig tabell som kallas **förutsägelser** baserat på hello dataframe.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Du bör se utdata som liknar hello följande:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. Titta på en av hello förutsägelser. Kör följande kodutdrag:

        predictionsDf.take(1)

   Det finns en förutsägelse för hello första posten i hello test-datauppsättningen.
1. Hej `model.transform()` metoden gäller hello samma omvandling tooany nya data med hello samma schema och kommer till en förutsägelse av hur tooclassify hello data. Vi kan göra några enkla statistik tooget en uppfattning om hur exakt våra förutsägelser var:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Det ser ut så hello följande hello utdata:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Med Spark logistic regression ger oss en korrekt modell för hello förhållandet mellan överträdelser beskrivningar på engelska och om ett visst företag skulle lyckat eller misslyckat en mat-kontroll.

## <a name="create-a-visual-representation-of-hello-prediction"></a>Skapa en bild av hello förutsägelse
Vi kan nu skapa en slutlig visualiseringen toohelp oss skäl om hello resultaten av det här testet.

1. Vi börjar med att extrahera hello olika förutsägelser och resultat från hello **förutsägelser** tillfällig tabell skapade tidigare. hello följande frågor separata hello utdata som *true_positive*, *false_positive*, *true_negative*, och *false_negative*. I hello frågor nedan, vi inaktivera visualisering med hjälp av `-q` och även spara hello utdata (med hjälp av `-o`) som dataframes som sedan kan användas med hello `%%local` Magiskt tal.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Använd slutligen hello följande fragment toogenerate hello ritytans med **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Du bör se hello följande utdata:

    ![Spark maskininlärning programinnehåll - cirkeldiagram procentandelar av misslyckade mat kontroller. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark maskininlärning resultatet utdata")

    I det här diagrammet refererar ”positivt” resultat toohello misslyckades mat kontroll, medan ett negativt resultat refererar tooa genomgått.

## <a name="shut-down-hello-notebook"></a>Stäng hello anteckningsboken
När du har kört programmet hello bör du stänga av hello anteckningsboken toorelease hello resurser. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**. Detta stängs av och stänger hello bärbar dator.

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
