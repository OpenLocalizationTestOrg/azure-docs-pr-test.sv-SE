---
title: aaaInstall Jupyter lokalt & ansluta tooan Azure HDInsight Spark-kluster | Microsoft Docs
description: "Lär dig hur tooinstall Jupyter-anteckningsbok lokalt på din dator och koppla den tooan Apache Spark-kluster i Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 95c052110b84b677fd23048597af9511365cacfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-tooapache-spark-on-hdinsight"></a>Installera Jupyter-anteckningsbok på datorn och ansluta tooApache Spark i HDInsight

I den här artikeln lär du dig hur tooinstall Jupyter notebook med hello anpassade PySpark (för Python) och Spark (för Scala) kärnor med Väck magic och ansluta hello anteckningsboken tooan HDInsight-kluster. Det kan finnas flera orsaker tooinstall Jupyter på den lokala datorn och det kan finnas några utmaningar samt. Mer information om det här avsnittet hello [Varför bör jag installera Jupyter på datorn](#why-should-i-install-jupyter-on-my-computer) hello slutet av den här artikeln.

Det finns tre viktiga steg ingår i installera Jupyter och hello Spark Magiskt tal på datorn.

* Installera Jupyter-anteckningsbok
* Installera hello PySpark och Spark kärnor med hello Spark Magiskt tal
* Konfigurera Spark magiskt tooaccess Spark-kluster i HDInsight

Mer information om hello anpassade kärnor och hello Spark Magiskt tal för Jupyter-anteckningsböcker med HDInsight-kluster finns [kernlar som är tillgängliga för Jupyter-anteckningsböcker med Apache Spark Linux-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Krav
hello förutsättningar som anges här är inte för att installera Jupyter. Det här är för anslutande hello Jupyter-anteckningsbok tooan HDInsight-kluster när hello anteckningsboken har installerats.

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Installera Jupyter-anteckningsbok på datorn

Du måste installera Python innan du kan installera Jupyter-anteckningsböcker. Både Python och Jupyter är tillgängliga som en del av hello [Anaconda distribution](https://www.continuum.io/downloads). När du installerar Anaconda, installerar du en distribution av Python. När Anaconda har installerats kan du lägga till hello Jupyter installationen genom att köra lämpliga kommandon.

1. Hämta hello [Anaconda installer](https://www.continuum.io/downloads) för din plattform och kör hello inställningar. Kontrollera att du väljer hello alternativet tooadd Anaconda tooyour sökvägsvariabeln när körs hello-installationsguiden.
2. Kör följande kommando tooinstall Jupyter hello.

        conda install jupyter

    Läs mer om hur du installerar Jupyter [installera Jupyter med Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-hello-kernels-and-spark-magic"></a>Installera hello kärnor och Spark Magiskt tal

Anvisningar för hur tooinstall hello Spark magic, hello PySpark och Spark kärnor följer hello installationsanvisningarna i hello [sparkmagic dokumentationen](https://github.com/jupyter-incubator/sparkmagic#installation) på GitHub. hello frågar första steget i hello Spark magiskt dokumentationen tooinstall Spark Magiskt tal. Ersätt det första steget i hello länken med hello följande kommandon, beroende på hello version av hello HDInsight-kluster som du ska ansluta till. Följ hello återstående steg i magiskt hello Spark-dokumentationen. Om du vill tooinstall hello olika kärnor, måste du utföra steg3 i hello Spark magiskt instruktioner installationsavsnittet.

* Installera sparkmagic 0.2.3 för kluster v3.4 genom att köra`pip install sparkmagic==0.2.3`

* För kluster v3.5 och v3.6, installera sparkmagic 0.11.2 genom att köra`pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-tooconnect-toohdinsight-spark-cluster"></a>Konfigurera Spark magiskt tooconnect tooHDInsight Spark-kluster

I det här avsnittet kan du konfigurera hello Spark Magiskt tal som du har installerat tidigare tooconnect tooan Apache Spark-kluster som du har redan skapat i Azure HDInsight.

1. Hej Jupyter konfigurationsinformation lagras vanligtvis i hello användare hemkatalog. toolocate arbetskatalogen på alla OS-plattformar, typen hello följande kommandon.

    Starta hello Python-gränssnittet. På ett kommandofönster, skriver du hello följande:

        python

    Ange hello efter kommandot toofind ut hello arbetskatalog på hello Python-gränssnittet.

        import os
        print(os.path.expanduser('~'))

2. Navigera toohello arbetskatalog och skapa en mapp med namnet **.sparkmagic** om den inte redan finns.
3. Skapa en fil med namnet i hello mappen **config.json** och Lägg till följande JSON-fragment inuti hello.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Ersätt **{USERNAME}**, **{CLUSTERDNSNAME}**, och **{BASE64ENCODEDPASSWORD}** med lämpliga värden. Du kan använda ett antal verktyg i din favorit programmeringsspråk eller online toogenerate en base64-kodade lösenordet för det faktiska lösenordet.

5. Konfigurera hello pulsslag inställningar i `config.json`. Du bör lägga till de här inställningarna på samma nivå som hello hello `kernel_python_credentials` och `kernel_scala_credentials` kodavsnitt din lagts till tidigare. Ett exempel på hur och var tooadd hello inställningar för pulsslag finns [exempel config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * För `sparkmagic 0.2.3` (kluster v3.4) inkluderar:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * För `sparkmagic 0.11.2` (kluster v3.5 och v3.6) inkluderar:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Tooensure att sessioner inte sprids sänds pulsslag. När en dator ska toosleep eller är avstängd, pulsslag hello skickas inte, vilket resulterar i hello sessionen som rensas. För kluster v3.4 om du inte vill toodisable problemet, kan du ange hello Livius config `livy.server.interactive.heartbeat.timeout` för`0` från hello Ambari UI. För kluster v3.5 om du inte anger hello 3.5 configuration ovan tas hello sessionen inte bort.

6. Starta Jupyter. Använd följande kommando från kommandoraden hello hello.

        jupyter notebook

7. Kontrollera att du kan ansluta toohello kluster med hjälp av hello Jupyter-anteckningsbok och att du kan använda hello Spark magic tillgängliga med hello kärnor. Utför följande steg hello.

    a. Skapa en ny anteckningsbok. Hello högra hörnet och klicka på **ny**. Du bör se hello standardkernel **Python2** och hello två nya kernlar som du installerar **PySpark** och **Spark**. Klicka på **PySpark**.

    ![Kernlar som är i Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "kärnor i Jupyter-anteckningsbok")

    b. Kör hello följande kodavsnitt.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Om du kan hämta hello utdata, testas anslutningen toohello HDInsight-kluster.

    >[!TIP]
    >Om du vill tooupdate hello anteckningsboken configuration tooconnect tooa annat kluster, måste du uppdatera hello config.json med hello ny uppsättning värden, som visas i steg3 ovan.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Varför ska jag installera Jupyter på min dator?
Det kan finnas flera orsaker varför du kanske vill tooinstall Jupyter på datorn och anslut den tooa Spark-kluster i HDInsight.

* Även om Jupyter-anteckningsböcker finns redan på hello Spark-kluster i Azure HDInsight, innehåller installera Jupyter på datorn hello alternativet toocreate dina anteckningsböcker lokalt, testa programmet mot ett kluster som körs och sedan ladda upp hello anteckningsböcker toohello kluster. tooupload hello anteckningsböcker toohello klustret, du kan antingen ladda upp dem med hjälp av hello Jupyter-anteckningsbok som körs eller hello kluster eller spara dem toohello /HdiNotebooks mapp i hello storage-konto som är associerade med hello-kluster. Mer information om hur bärbara datorer lagras på hello klustret finns [där lagras Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Med hello bärbara datorer som är tillgängliga lokalt, kan du ansluta baserat på din programkrav toodifferent Spark-kluster.
* Du kan använda GitHub tooimplement ett system för källa och har versionskontroll för hello bärbara datorer. Du kan också ha en gemensam miljö där flera användare kan arbeta med hello samma bärbar dator.
* Du kan arbeta med anteckningsböcker lokalt utan att behöva ett kluster även. Du behöver bara en kluster-tootest dina anteckningsböcker mot, inte toomanually hantera dina anteckningsböcker eller en utvecklingsmiljö.
* Det kan vara enklare tooconfigure egna lokala utvecklingsmiljö än tooconfigure hello Jupyter installation på hello klustret.  Du kan dra nytta av alla hello-programvara som du har installerat lokalt utan att konfigurera ett eller flera fjärranslutna kluster.

> [!WARNING]
> Med Jupyter installerat på den lokala datorn, kan flera användare kör hello samma anteckningsbok på hello samma Spark-kluster på hello samtidigt. I detta fall skapas flera Livius sessioner. Om du stöter på ett problem och vill att den kommer att vara en komplicerad uppgift tootrack vilka Livius session hör toowhich användaren toodebug.
>
>

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
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

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
