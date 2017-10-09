---
title: aaaInstall Presto i Azure HDInsight Linux-kluster | Microsoft Docs
description: "Lär dig hur tooinstall Presto och Airpal på Linux-baserade HDInsight Hadoop-kluster med hjälp av skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Installera och använda Presto på HDInsight Hadoop-kluster

I det här avsnittet får du lära dig hur tooinstall Presto på HDInsight Hadoop-kluster med hjälp av skriptåtgärder. Du också lära dig hur tooinstall Airpal på ett befintligt Presto HDInsight-kluster.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver en **HDInsight 3.5 Hadoop-kluster** som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-versioner](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Vad är Presto?
[Presto](https://prestodb.io/overview.html) är en snabb distribuerade SQL-frågemotor för stordata. Presto är lämplig för interaktiva frågor till petabyte data. Mer information om hello komponenter av Presto och hur de fungerar finns i [Presto begrepp](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut och Microsoft Support kommer att tooisolate och lösa problem relaterade toothese komponenter.
> 
> Anpassade komponenter, till exempel Presto kan ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning. Detta kan resultera i att lösa problemet hello eller be tooengage tillgängliga kanaler för hello öppnas teknikerna där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Installera Presto med skriptåtgärder

Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar ett nytt kluster med hjälp av hello Azure-portalen. 

1. Börja etablera ett kluster med hjälp av hello stegen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md). Se till att du skapar hello-kluster med hjälp av hello **anpassad** klustret skapas flödet. Du måste se till att hello-kluster som du skapar uppfyller följande krav hello.

    a. Det måste vara ett Hadoop-kluster med HDInsight version 3.5.

    b. Den måste använda Azure Storage som hello datalager. Med hjälp av Presto på ett kluster som använder Azure Data Lake Store som hello lagringsalternativ stöds inte ännu. 

    ![HDInsight-kluster med anpassade alternativ](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. På hello **avancerade inställningar** bladet väljer **skriptåtgärder**, och ange hello informationen nedan:
   
   * **NAMNET**: Ange ett eget namn för hello skriptåtgärder.
   * **Bash-skript-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Markera det här alternativet
   * **WORKER**: Markera det här alternativet
   * **ZOOKEEPER**: avmarkerar du kryssrutan
   * **PARAMETRARNA**: lämna fältet tomt


3. Längst ned hello hello **skriptåtgärder** bladet, klickar du på hello **Välj** toosave hello konfiguration. Klicka slutligen på hello **Välj** knappen längst ned hello hello **avancerade inställningar** bladet toosave hello konfigurationsinformation.

4. Fortsätta etablering hello klustret enligt beskrivningen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK eller Azure Resource Manager-mallar kan också vara används tooapply skriptåtgärder. Du kan också använda skriptet åtgärder tooalready kluster som körs. Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Använda Presto med HDInsight

Utföra hello följande steg toouse Presto i ett HDInsight-kluster när du har installerat den med hjälp av hello stegen som beskrivs ovan.

1. Anslut toohello HDInsight-kluster med SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).
     

2. Starta hello Presto shell med hello följande kommando.
   
        presto --schema default

3. Köra en fråga på en exempeltabell **hivesampletable**, som är tillgänglig i alla HDInsight-kluster som standard.
   
        select count (*) from hivesampletable;
   
    Som standard [Hive](https://prestodb.io/docs/current/connector/hive.html) och [TPCH](https://prestodb.io/docs/current/connector/tpch.html) kopplingar för Presto har redan konfigurerats. Hive connector är konfigurerade toouse hello installerat Hive standardinstallation, så alla hello tabeller från Hive visas automatiskt i Presto.

    En detaljerad beskrivning av hur du kan använda Presto finns [Presto dokumentationen](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Använda Airpal med Presto

[Airpal](https://github.com/airbnb/airpal#airpal) är ett gränssnitt för öppen källkod webbaserade frågan för Presto. Mer information om Airpal finns [Airpal dokumentationen](https://github.com/airbnb/airpal#airpal).

I det här avsnittet ska vi titta på hello steg för**installera Airpal på hello edgenode** i ett HDInsight Hadoop-kluster där det redan finns Presto installerad. Detta säkerställer att hello Airpal frågan webbgränssnitt är tillgänglig över hello Internet.

1. Med SSH, Anslut toohello headnode av hello HDInsight-kluster som har installerat Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Kör följande kommando hello när du är ansluten.

        sudo slider registry  --name presto1 --getexp presto 
   
    Du bör se utdata som liknar hello följande:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Observera hello värde för hello från hello utdata **värdet** egenskapen. Du behöver detta när du installerar Airpal på hello klustret edgenode. Från hello utdata över hello-värde som du behöver är **10.0.0.12:9090**.

4. Använd hello mall  **[här](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate ett HDInsight-kluster edgenode och ange hello värden som visas i följande skärmbild hello.

    ![Installera HDInsight Airpal på Presto klustret](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Klicka på **Köp**.

6. När det är tillämpade toohello klusterkonfigurationen hello ändringarna kan du komma åt hello Airpal webbgränssnitt med hjälp av följande hello.

    a. Hello kluster-bladet, klickar du på **program**.

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    b. Från hello **installerade appar** bladet, klickar du på **Portal** mot airpal.

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    c. När du uppmanas ange hello admin-autentiseringsuppgifter som du angav när du skapar hello HDInsight Hadoop-kluster.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Anpassa en Presto installation på HDInsight-kluster

När du har installerat Presto på ett HDInsight Hadoop-kluster, kan du anpassa hello installation toomake ändringar, till exempel minne uppdateringsinställningar, ändra kopplingar, osv. Utför följande steg toodo så hello.

1. Med SSH, Anslut toohello headnode av hello HDInsight-kluster som har installerat Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Ändra konfigurationen av hello filen `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Mer information om Presto konfiguration finns [Presto konfigurationen för YARN-baserade kluster](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Stoppa och avsluta hello aktuell session i Presto.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Starta en ny instans av Presto med hello anpassning.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Vänta tills hello ny instans toobe redo och anteckna presto coordinator adress.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generera benchmark-data för HDInsight-kluster som kör Presto

TPC DS är hello branschstandard för att mäta hello prestanda för många beslut support-system, inklusive system för stordata. Du kan använda Presto på HDInsight-kluster toogenerate data och utvärdera hur jämförs med dina egna HDInsight benchmark-data. Mer information finns i [här](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Se även
* [Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md). Hue är ett webbgränssnitt som gör det enkelt toocreate, köra och spara Pig och Hive-jobb, samt som Bläddra hello standardlagring för HDInsight-kluster.

* [Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md). Använd kluster anpassning tooinstall Giraph på HDInsight Hadoop-kluster. Giraph kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.

* [Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md). Använd kluster anpassning tooinstall Solr på HDInsight Hadoop-kluster. Solr kan tooperform kraftfulla sökningar på lagrade data.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
