---
title: aaaCreate ett Apache Spark-klustret i Azure HDInsight | Microsoft Docs
description: "HDInsight Spark quickstart på hur toocreate ett Apache Spark-kluster i HDInsight."
keywords: "spark-snabbstart,interaktiv spark,interaktiv fråga,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 002f71b3cd4fb315d4a556cebc9263026515ec4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Skapa ett Apache Spark-kluster i Azure HDInsight

I den här artikeln lär du dig hur toocreate ett Apache Spark-klustret i Azure HDInsight. Mer information om Spark på HDInsight finns i [Översikt: Apache Spark på Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Diagram för Snabbstart som beskriver stegen toocreate ett Apache Spark-kluster i Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark Snabbstart med Apache Spark i HDInsight. Illustrerade steg: skapa ett kluster, kör interaktiv Spark-fråga")

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration. Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Skapa HDInsight Spark-kluster

I det här avsnittet skapar du ett Spark-kluster i HDInsight med en [Azure Resource Manager-mall](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Information om andra metoder för att skapa kluster finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Ange hello följande värden:

    ![Skapa HDInsight Spark-kluster med en Azure Resource Manager-mall](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Skapa Spark-kluster i HDInsight med en Azure Resource Manager-mall")

    * **Prenumeration**: Välj din Azure-prenumeration för det här klustret.
    * **Resursgrupp**: Skapa en resursgrupp eller välj en befintlig. Resursgruppen är används toomanage Azure-resurser för dina projekt.
    * **Plats**: Välj en plats för hello resursgrupp. hello-mallen använder den här platsen för att skapa hello kluster även för hello standard klusterlagringen.
    * **Klusternamn**: Ange ett namn för hello HDInsight-kluster som du vill toocreate.
    * **Spark version**: Välj **2.0** som hello-version som du vill tooinstall på hello klustret.
    * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är administratör.
    * **SSH-användarnamn och lösenord**.

   Anteckna dessa värden.  Du behöver dem senare i självstudiekursen hello.

3. Välj **acceptera toohello villkoren ovan**väljer **PIN-kod toodashboard**, och klicka sedan på **inköp**. En ny panel visas med rubriken Skicka distribution för malldistribution. Det tar cirka 20 minuter toocreate hello klustret.

Om du stöter på ett problem med att skapa HDInsight-kluster kan bero det på att du inte har hello rätt behörigheter toodo så. Mer information finns i [åtkomstkravkontrollen](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Den här artikeln skapar ett Spark-kluster som använder [Azure Storage-Blobbar som hello kluster lagring](hdinsight-hadoop-use-blob-storage.md). Du kan också skapa ett Spark-kluster som använder [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) som hello standardlagring. Instruktioner finns i [Skapa ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="run-a-hive-query-using-spark-sql"></a>Köra en Hive-fråga med Spark SQL

När du använder en Jupyter-anteckningsbok som konfigurerats för HDInsight Spark-kluster kan du få en förinställning `sqlContext` som du kan använda toorun Hive-frågor med Spark SQL. I det här avsnittet lär du dig hur toostart en Jupyter-anteckningsbok och kör sedan en grundläggande Hive-fråga.

1. Öppna hello [Azure-portalen](https://portal.azure.com/).

2. Om du har valt toopin hello klusterinstrumentpanel toohello Klicka hello klustret panelen från hello instrumentpanelen toolaunch hello klusterbladet.

    Om du inte fäster hello klustret toohello instrumentpanelen från hello till vänster och klicka på **HDInsight-kluster**, och klicka sedan på hello-klustret som du skapade.

3. I **Snabblänkar** klickar du på **Klusterinstrumentpaneler** och sedan på **Jupyter Notebook**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

   ![Öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")

   > [!NOTE]
   > Du kan också komma åt hello Jupyter notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Skapa en anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.

   ![Skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")

   En ny anteckningsbok skapas och öppnas med hello namnet Untitled(Untitled.pynb).

4. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn om du vill.

    ![Ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från")

5.  Klistra in hello följande kod i en tom cell och tryck sedan på **SKIFT + RETUR** toorun hello kod. I hello koden nedan `%%sql` (kallas hello sql magic) anger Jupyter-anteckningsbok toouse hello förinställningen `sqlContext` toorun hello Hive-fråga. hello fråga hämtar hello översta 10 rader från en Hive-tabell (**hivesampletable**) som är tillgängligt som standard på alla HDInsight-kluster.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Hive-fråga i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-fråga i HDInsight Spark")

    Mer information om hello `%%sql` magic och hello förinställningen kontexter, se [Jupyter kernlar som är tillgängliga för ett HDInsight-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Varje gång du kör en fråga i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel. Du också se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet. När hello jobbet har slutförts ändras tooa tom cirkel.
    >
    >
    
6. hello-skärmen bör uppdatera tooshow hello frågeresultatet.

    ![Hive-frågeresultatet i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-frågeresultatet i HDInsight Spark")

7. Stäng hello anteckningsboken toorelease hello klusterresurser när du har kört programmet hello. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.

8. Om du planerar toocomplete hello nästa steg vid ett senare tillfälle kan du kontrollera att du tar bort hello HDInsight-kluster som du skapade i den här artikeln. 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Nästa steg 

I den här artikeln som du har lärt dig hur toocreate ett HDInsight Spark-kluster och kör en grundläggande Spark SQL-frågan. Avancera toohello nästa artikel toolearn hur toouse ett HDInsight Spark-kluster toorun interaktiva frågor på exempeldata.

> [!div class="nextstepaction"]
>[Köra interaktiva frågor på ett HDInsight Spark-kluster](hdinsight-apache-spark-load-data-run-query.md)



