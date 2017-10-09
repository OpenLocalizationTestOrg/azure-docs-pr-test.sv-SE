---
title: "aaaHDInsight Spark genomgång med hjälp av PySpark och Scala i Azure | Microsoft Docs"
description: "Exempel på hello Team datavetenskap Process som går igenom hello användning av PySpark och Scala på ett Azure HDInsight Spark toodo förutsägelseanalys."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: f8b41a8cae414586570761ba4b4eb4c239cbb981
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hdinsight-spark-data-science-walkthroughs-using-pyspark-and-scala-on-azure"></a>Med hjälp av PySpark och Scala på Azure HDInsight Spark datavetenskap genomgång

Dessa genomgång använda PySpark och Scala i förutsägelseanalyser en Azure Spark-kluster toodo. De följer hello steg som beskrivs i hello Team datavetenskap Process. En översikt över hello Team datavetenskap Process, se [datavetenskap Process](data-science-process-overview.md). En översikt över Spark i HDInsight, se [introduktion tooSpark på HDInsight](../hdinsight/hdinsight-apache-spark-overview.md).

Ytterligare datavetenskap genomgångar som kör hello Team av vetenskapliga data är grupperade efter hello **plattform** som de använder: 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]

## <a name="predict-taxi-tips-using-pyspark-on-azure-spark"></a>Förutsäga taxi tips med hjälp av PySpark på Azure Spark

Hej [använda Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md) genomgången använder data från New York taxibilar toopredict om ett tips är betald och hello mängd belopp förväntas toobe betald. Den använder hello Team datavetenskap Process i ett scenario med en [Azure HDInsight Spark-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska, och funktionen tekniker data från hello offentligt tillgängliga NYC taxi resa och färdavgiften dataset. Det här översiktsavsnittet ställer du in med ett HDInsight Spark-kluster och hello Jupyter PySpark anteckningsböcker som används i hello resten av hello genomgången. Dessa anteckningsböcker visar hur tooexplore dina data och sedan hur toocreate och använda modeller. hello avancerade datagranskning och modellering anteckningsboken visar hur tooinclude korsvalidering, hyper-parameter omfattande och utvärdering av modellen.

### <a name="data-exploration-and-modeling-with-spark"></a>Datagranskning och modellering med Spark 
Utforska hello datauppsättningen och skapa poängsätta och utvärdera hello machine learning-modeller genom att utföra hello [skapa binär klassificering och regression modeller för data med hello Spark MLlib toolkit](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet.

### <a name="model-consumption"></a>Modell-förbrukning
toolearn hur tooscore hello klassificering och regression modeller som skapats i det här avsnittet finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).

### <a name="cross-validation-and-hyperparameter-sweeping"></a>Korsvalidering och hyperparameter omfattande
Se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parametern.


## <a name="predict-taxi-tips-using-scala-on-azure-spark"></a>Förutsäga taxi tips på Azure Spark Scala

Hej [Använd Scala med Spark på Azure](machine-learning-data-science-process-scala-walkthrough.md) genomgången använder data från New York taxibilar toopredict om ett tips är betald och hello mängd belopp förväntas toobe betald. Den visar hur toouse Scala för övervakade machine learning-aktiviteter med hello Spark machine learning-biblioteket (MLlib) och SparkML paket i ett Azure HDInsight Spark-kluster. Den vägleder dig genom hello aktiviteter som utgör hello [datavetenskap Process](http://aka.ms/datascienceprocess): datapåfyllning och undersökning, visualisering, funktionen tekniker, modellering och förbrukning av modellen. hello modeller inbyggda inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd.


## <a name="next-steps"></a>Nästa steg

En beskrivning av hello viktiga komponenter som utgör hello Team datavetenskap Process finns [Team datavetenskap processöversikt](data-science-process-overview.md).

En beskrivning av hello Team datavetenskap Process livscykel som du kan använda toostructure datavetenskap projekt, se [Team datavetenskap Process livscykel](data-science-process-lifecycle.md). hello livscykel beskrivs hello steg från start toofinish som projekt följer vanligtvis när de utförs. 

