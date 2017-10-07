---
title: "aaaOverview av datavetenskap med Spark på Azure HDInsight | Microsoft Docs"
description: "hello Spark MLlib toolkit ger betydande maskininlärning modeling funktioner toohello distribuerade HDInsight miljö."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a4e1de99-a554-4240-9647-2c6d669593c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 515705684a46917c2741bf063d439b1cda016abb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Översikt över datavetenskap med Spark på Azure HDInsight
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Den här sviten avsnitt visar hur toouse HDInsight Spark toocomplete vanliga datavetenskap aktiviteter, till exempel datapåfyllning, funktionen tekniker, modellering och utvärdering av modellen. hello-data som används är ett exempel på hello 2013 NYC taxi resa och avgiften dataset. hello modeller inbyggda inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd. hello avsnitt visas också hur toostore dessa modeller i Azure blob storage (WASB) och hur tooscore och utvärdera deras förutsägbar prestanda. Mer avancerade avsnitt beskriver hur modeller kan vara tränas med omfattande korsvalidering och hyper-parametern. Det här översiktsavsnittet refererar även till hello avsnitt som beskriver hur tooset in hello Spark-kluster som du behöver toocomplete hello stegen i hello-instruktionerna. 

## <a name="spark-and-mllib"></a>Spark och MLlib
[Spark](http://spark.apache.org/) är ett ramverk för parallellbearbetning med öppen källkod som stöder minnesintern bearbetning tooboost hello prestanda hos ett stort program för stordataanalys. Hej bearbetningsmotorn i Spark är byggd för hastighet, enkel användning och avancerade analyser. Sparks funktioner för beräkning för distribuerade i minnet gör det ett bra alternativ för hello iterativa algoritmer i machine learning och grafberäkningar. [MLlib](http://spark.apache.org/mllib/) är Sparks skalbara machine learning bibliotek som låter dig hantera hello algoritmbaserade modeling funktioner toothis distribuerad miljö. 

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) hello Azure finns erbjudande för öppen källkod Spark. Den omfattar också stöd för **Jupyter PySpark-anteckningsböcker** på hello Spark-kluster som kan köra interaktiva Spark SQL-frågor för att omvandla, filtrering och visualisera data som lagras i Azure BLOB (WASB). PySpark är hello Python-API för Spark. hello kodstycken som tillhandahåller hello lösningar och visa hello relevanta använder toovisualize hello data här köras i Jupyter-anteckningsböcker som är installerad på hello Spark-kluster. hello modellering stegen i följande avsnitt innehåller kod som visar hur tootrain, utvärdera, spara och använda varje typ av modellen. 

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Installationsprogrammet: Spark-kluster och Jupyter-anteckningsböcker
Konfigurationsstegen och kod finns i den här genomgången för att använda ett HDInsight Spark 1.6. Men Jupyter-anteckningsböcker som har angetts för både HDInsight Spark 1.6 och 2.0 Spark-kluster. En beskrivning av hello anteckningsböcker och länkar toothem finns i hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för hello GitHub-lagringsplats som innehåller dessa. Dessutom hello koden här hello länkade anteckningsböcker är generisk och bör fungera på Spark-kluster. Om du inte använder HDInsight Spark hello konfiguration och hantering av steg kan vara skiljer sig från vad som anges här. För enkelhetens skull följer hello länkar toohello Jupyter-anteckningsböcker för Spark 1.6 (toobe köras i hello pySpark-kerneln av hello Jupyter-anteckningsbok server) och Spark 2.0 (toobe kör i hello pySpark3 kernel av hello Jupyter-anteckningsbok server):

### <a name="spark-16-notebooks"></a>Spark 1.6 bärbara datorer
Dessa datorer är toobe som körs i hello pySpark-kerneln Jupyter-anteckningsbok Server.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): innehåller information om hur tooperform datagranskning modellering och bedömningen med flera olika algoritmer.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): innehåller avsnitt i anteckningsboken #1 och modellen utveckling med hjälp av hyperparameter justering och korsvalidering.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): Visar hur toooperationalize en sparad modell med Python på HDInsight-kluster.

### <a name="spark-20-notebooks"></a>Spark 2.0 bärbara datorer
Dessa datorer är toobe kör i hello pySpark3 kernel Jupyter-anteckningsbok Server.

- [Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): den här filen innehåller information om hur tooperform datagranskning modellering och bedömningen i Spark 2.0-kluster med hello NYC Taxi resa och avgiften-datamängd beskrivs [här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Den här anteckningsboken kan vara en bra utgångspunkt för att snabbt utforska hello koden som vi har angetts för Spark 2.0. En mer detaljerad anteckningsbok analyserar hello NYC Taxi data, finns hello nästa anteckningsboken i den här listan. Se hello anteckningar efter den här listan som jämför dessa datorer. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello NYC Taxi resa och avgiften datauppsättning beskrivs [ här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello välkända flygbolag i tid avvikelse DataSet från 2011 och 2012. Vi integrerad hello flygbolag dataset med hello flygplats väder data (t.ex. vindhastigheten, temperatur, höjd etc.) tidigare toomodeling, så att dessa väder-funktioner kan ingå i hello modellen.

<!-- -->

> [!NOTE]
> hello flygbolag datamängden har lagts till toohello Spark 2.0 anteckningsböcker toobetter illustrera hello användningen av algoritmer för klassificering. Se följande länkar för information om flygbolag i tid avvikelse dataset och väder datamängd hello:

>- Flygbolag i tid avvikelse data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Flygplats väder data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
hello Spark 2.0 anteckningsböcker på hello NYC taxi och flygbolag svarta fördröjning-datauppsättningar kan ta 10 minuter eller mer toorun (beroende på hello storleken på ditt HDI-kluster). hello första anteckningsboken i hello ovanför listan visar flera olika aspekter av hello datagranskning, visualisering och ML-modell utbildning i en bärbar dator som tar mindre tid toorun med ned provtagning NYC datamängd, i vilken hello taxi och avgiften filer har redan anslutits: [ Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) anteckningsboken tar en mycket kortare tid toofinish (2-3 minuter) och kan vara en bra utgångspunkt för snabbt utforska hello koden har vi föreskrivs Spark 2.0. 

<!-- -->

Vägledning om hello operationalization av en Spark 2.0 och modellen förbrukningen för bedömningen finns hello [Spark 1.6 dokumentet på förbrukningen](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) exempel beskriver hello steg som krävs. toouse på Spark 2.0, Ersätt hello Python kodfilen med [filen](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Krav
hello följande procedurer är relaterade tooSpark 1.6. Hello Spark 2.0 version, Använd hello anteckningsböcker beskrivs och länka toopreviously. 

1. Du måste ha en Azure-prenumeration. Om du inte redan har en, se [hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. måste toocomplete en 1.6 Spark-kluster i den här genomgången. toocreate, se hello instruktionerna i [komma igång: skapa Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). typ av hello kluster och version har angetts från hello **Välj typ av kluster** menyn. 

![Konfigurera klustret](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Ett avsnitt som visar hur toouse Scala i stället för Python toocomplete aktiviteter för en slutpunkt till slutpunkt datavetenskap process finns hello [datavetenskap med hjälp av Scala med Spark på Azure](machine-learning-data-science-process-scala-walkthrough.md).
> 
> 

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

## <a name="hello-nyc-2013-taxi-data"></a>hello NYC 2013 Taxi data
hello NYC Taxi resa data är cirka 20 GB komprimerad fil med kommaavgränsade värden (CSV)-filer (~ 48 GB okomprimerade), som består av fler än 173 miljoner enskilda resor och hello priser för varje resa. Hello plocka upp och Samlingsbibliotek plats och tid, anonymiserade hackare (drivrutin) licensnummer och antalet medallion (taxi's unikt id) innehåller varje resa-post. hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:

1. hello 'trip_data' CSV-filer innehåller resa information, till exempel antalet passagerare, hämta och dropoff pekar utlösas varaktighet och resa längd. Här följer några Exempelposter:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello 'trip_fare' CSV-filer innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald. Här följer några Exempelposter:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Vi har tagit en 0,1% exempel på dessa filer och kopplade hello resa\_data och resa\_färdavgiften CSV-filer i en enda dataset toouse som hello inkommande datamängden för den här genomgången. hello Unik nyckel toojoin resa\_data och resa\_avgiften består av hello fält: medallion hackare\_licensen och hämtning\_datetime. Varje post i hello dataset innehåller följande attribut som representerar en NYC Taxi resa hello:

| Fält | Kort beskrivning |
| --- | --- |
| medallion |Anonymiserade taxi medallion (taxi unikt id) |
| hack_license |Anonymiserade Hackney transport licensnummer |
| vendor_id |Taxi leverantörs-id |
| rate_code |NYC taxi frekvensen avgiften |
| store_and_fwd_flag |Lagra och vidarebefordra flaggan |
| pickup_datetime |Hämta datum och tid |
| dropoff_datetime |Dropoff datum och tid |
| pickup_hour |Hämta timme |
| pickup_week |Hämta veckan på året hello |
| veckodag |Veckodag (mellan 1-7) |
| passenger_count |Passagerare i en taxi resa |
| trip_time_in_secs |Resa tiden i sekunder |
| trip_distance |Resa avstånd som obligatoriska i mil |
| pickup_longitude |Hämta longitud |
| pickup_latitude |Hämta latitud |
| dropoff_longitude |Dropoff longitud |
| dropoff_latitude |Dropoff latitud |
| direct_distance |Dirigera avståndet mellan plocka upp och dropoff platser |
| payment_type |Betalningssätt (CA: er, kreditkortsnummer osv.) |
| fare_amount |Avgiften beloppet i |
| Tillägg |Tillägg |
| mta_tax |MTA skatt |
| tip_amount |Tips belopp |
| tolls_amount |Vägtullar belopp |
| total_amount |Totalmängd |
| lutad |Lutad (0-1 för Nej eller Ja) |
| tip_class |Tips klass (0: 0, 1: $0-5, 2: $6 – 10, 3: $11-20, 4: > 20) |

## <a name="execute-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>Köra kod från en Jupyter-anteckningsbok på hello Spark-kluster
Du kan starta hello Jupyter-anteckningsbok från hello Azure-portalen. Spark-kluster på instrumentpanelen och klicka på den sidan för hantering av tooenter för klustret. tooopen hello bärbar dator som är associerade med hello Spark-kluster, klicka på **Klusterinstrumentpaneler** -> **Jupyter-anteckningsbok** .

![Klustret instrumentpaneler](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Du kan även bläddra för***https://CLUSTERNAME.azurehdinsight.net/jupyter*** tooaccess hello Jupyter-anteckningsböcker. Ersätt hello KLUSTERNAMN en del av denna URL med hello namnet på ditt eget kluster. Du behöver hello lösenord för din admin konto tooaccess hello bärbara datorer.

![Bläddra Jupyter-anteckningsböcker](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Välj PySpark toosee en katalog som innehåller några exempel på förhand packade bärbara datorer som använder hello PySpark API.hello bärbara datorer som innehåller hello kodexempel för den här serien Spark-avsnittet finns på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

Du kan ladda upp hello bärbara datorer direkt från [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) toohello Jupyter-anteckningsbok server på Spark-kluster. Klicka på hello startsidan för din Jupyter hello **överför** knappen hello höger tillhör hello-skärmen. En Utforskaren öppnas. Här kan du klistra in hello GitHub (rådata innehåll) URL hello bärbara datorer och klicka på **öppna**. 

Du ser hello filnamn i listan Jupyter-fil med en **överför** igen. Klicka här **överför** knappen. Nu har du importerat hello bärbar dator. Upprepa dessa steg tooupload hello andra datorer från den här genomgången.

> [!TIP]
> Du kan högerklicka på hello länkar i webbläsaren och välj **Kopiera länk** tooget hello rå innehåll github-URL. Du kan klistra in URL: en i hello Jupyter överför filen dialogrutan explorer.
> 
> 

Nu kan du:

* Se koden för hello genom att klicka på hello bärbar dator.
* Köra varje cell genom att trycka på **SKIFT-ange**.
* Kör hello anteckningsboken genom att klicka på **Cell** -> **kör**.
* Använd hello automatisk visualisering av frågor.

> [!TIP]
> Hej PySpark-kerneln visualizes automatiskt hello utdata för SQL (HiveQL)-frågor. Du ges hello alternativet tooselect mellan flera olika typer av grafik (register, cirkeldiagram, rad, område eller fältet) med hjälp av hello **typen** menyn knapparna i hello anteckningsbok:
> 
> 

![Logistic regression ROC-kurvan generisk metod](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Nästa steg
Nu när du har konfigurerat ett HDInsight Spark-kluster och har överfört hello Jupyter-anteckningsböcker är klar toowork via hello avsnitt som motsvarar toohello tre PySpark bärbara datorer. De visar hur tooexplore dina data och sedan hur toocreate och använda modeller. hello avancerade datagranskning och modellering anteckningsboken visar hur tooinclude korsvalidering, hyper-parameter omfattande och utvärdering av modellen. 

**Datagranskning och modellering med Spark:** utforska hello datauppsättningen och skapa poängsätta och utvärdera hello machine learning-modeller genom att utföra hello [skapa binär klassificering och regression modeller för data med hello Spark MLlib toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet.

**Modellen förbrukning:** toolearn hur tooscore hello klassificering och regression modeller som skapats i det här avsnittet finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).

**Korsvalidering och hyperparameter omfattande**: se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parameter

