---
title: "aaaAzure Toolkit för IntelliJ - Debug-program på HDInsight Spark | Microsoft Docs"
description: "Lär dig hur använda HDInsight Tools i Azure Toolkit för IntelliJ tooremotely debug program som körs på HDInsight Spark-kluster via vpn."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a>Använd Azure Toolkit för IntelliJ toodebug program på HDInsight Spark via VPN-anslutning

Vi rekommenderar hello sätt att felsöka spark applicaltion via fjärranslutning via ssh. Instruktioner finns i [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

Den här artikeln innehåller stegvisa anvisningar om hur toouse hello HDInsight-verktyg i Azure Toolkit för IntelliJ toosubmit ett Spark-jobb på HDInsight Spark-kluster och felsöka den via fjärranslutning från din dator. toodo så du måste utföra hello följande anvisningar:

1. Skapa en plats-till-plats eller en punkt-till-plats Azure Virtual Network. hello stegen i det här dokumentet förutsätter att du använder ett plats-till-plats-nätverk.
2. Skapa ett Spark-kluster i Azure HDInsight som ingår i hello plats-till-plats Azure Virtual Network.
3. Kontrollera hello anslutningen mellan hello klustret headnode och ditt skrivbord.
4. Skapa en Scala-program i IntelliJ IDEA och konfigurera den för fjärrfelsökning.
5. Kör och felsöka hello program.

## <a name="prerequisites"></a>Krav
* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java Development kit. Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* IntelliJ IDEA. Den här artikeln använder version 2017.1. Du kan installera den från [här](https://www.jetbrains.com/idea/download/).
* HDInsight-verktyg i Azure Toolkit för IntelliJ. HDInsight-verktyg för IntelliJ är tillgängliga som en del av hello Azure Toolkit för IntelliJ. Anvisningar för hur tooinstall hello Azure Toolkit finns [installerar hello Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).
* Logga in på Azure-prenumerationen från IntelliJ IDEA. Följ instruktionerna för hello [här](hdinsight-apache-spark-intellij-tool-plugin.md).
* När du kör Spark Scala program för fjärrfelsökning i en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) som uppstår på grund av tooa saknas WinUtils.exe i Windows. toowork runt det här felet måste du [hämta hello körbara härifrån](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**. Sedan måste du lägga till en miljövariabel **HADOOP_HOME** och ange hello hello variabels värde för**C\WinUtils**.

## <a name="step-1-create-an-azure-virtual-network"></a>Steg 1: Skapa ett virtuellt Azure-nätverk
Följ anvisningarna för hello från hello nedan länkar toocreate ett Azure Virtual Network och kontrollera hello anslutningen mellan hello fjärrskrivbord och Azure Virtual Network.

* [Skapa ett VNet med en plats-till-plats VPN-anslutning med hjälp av Azure Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Skapa ett VNet med en plats-till-plats VPN-anslutning med hjälp av PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Konfigurera en punkt-till-plats-anslutning tooa virtuellt nätverk med PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a>Steg 2: Skapa ett HDInsight Spark-kluster
Du bör också skapa ett Apache Spark-kluster i Azure HDInsight som ingår i hello Azure-nätverk som du skapade. Använd hello information i [skapa Linux-baserade kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Välj hello Azure-nätverk som du skapade i föregående steg i hello som del av valfri konfiguration.

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a>Steg 3: Kontrollera hello anslutningen mellan hello klustret headnode och skrivbordet
1. Hämta hello hello headnode IP-adress. Öppna Ambari UI för hello klustret. Hello kluster-bladet, klickar du på **instrumentpanelen**.

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. Hej Ambari UI från hello övre högra hörnet och klicka på **värdar**.

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. Du bör se en lista över headnodes, arbetarnoder och zookeeper-noder. Hej headnodes har hello **hn*** prefix. Klicka på hello första headnode.

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. Längst ned hello hello-sidan som öppnas från hello **sammanfattning** rutan Kopiera hello IP-adress hello headnode och hello värdnamn.

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. Inkludera hello IP-adress och hello värdnamn för hello headnode toohello **värdar** fil på hello dator från där du vill toorun och felsöka hello Spark jobb. Detta gör att du toocommunicate med hello headnode med hjälp av hello IP-adress samt hello värdnamn.

   1. Öppna en anteckningar med förhöjda behörigheter. Hello Arkiv-menyn klickar du på **öppna** och gå sedan toohello platsen för hello hosts-filen. På en Windows-dator är det `C:\Windows\System32\Drivers\etc\hosts`.
   2. Lägg till följande toohello hello **värdar** fil.

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. Kontrollera att du kan pinga båda hello headnodes med hjälp av hello IP-adress samt hello hostname från hello-dator som du har anslutit toohello virtuella Azure-nätverk som används av hello HDInsight-kluster.
7. SSH i hello klustret headnode använder hello instruktioner på [Anslut tooan HDInsight-kluster med SSH](hdinsight-hadoop-linux-use-ssh-unix.md). Pinga hello IP-adressen för hello stationär dator från hello klustret headnode. Du bör testa anslutning tooboth hello IP-adresser tilldelas toohello dator, en för hello nätverksanslutning och hello andra för hello Azure Virtual Network som hello datorn är ansluten till.
8. Upprepa hello steg för hello samt andra headnode.

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a>Steg 4: Skapa ett Spark Scala-program med Azure Toolkit hello HDInsight-verktyg för IntelliJ och konfigurera den för fjärrfelsökning
1. Öppna IntelliJ IDEA och skapa ett nytt projekt. Kontrollera hello följande alternativ och klickar sedan på i hello dialogrutan Nytt projekt, **nästa**.

    ![Skapa Spark Scala-program](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * Hello till vänster och välj **HDInsight**.
   * Hello högra rutan, Välj **Spark i HDInsight (Scala)**.
   * Klicka på **Nästa**.
2. I nästa hello-fönstret hello följande projektinformation, och klickar sedan på **Slutför**.  
   - Ange ett projektnamn och projektets plats.
   - För **projekt SDK**, Använd Java 1.8 för spark-kluster för 2.x, Java 1.7 för 1.x spark-kluster.
   - För **Spark Version**, Scala guide för att skapa projektet integreras rätt version för Spark SDK och Scala SDK. Om hello spark-kluster av version är lägre 2.0 väljer Väck 1.x. I annat fall bör du välja spark2.x. Det här exemplet använder Spark2.0.2 (Scala 2.11.8).
       ![Skapa Spark Scala-program](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)
  
3. hello Spark projekt skapar automatiskt en artefakt åt dig. toosee hello artefakt, Följ dessa steg.

   1. Från hello **filen** -menyn klickar du på **projektstruktur**.
   2. I hello **projektstruktur** dialogrutan klickar du på **artefakter** toosee hello standard artefakt som har skapats.
   ![Skapa JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)

      Du kan också skapa egna artefakt bly när du klickar på hello  **+**  ikon, markerad i hello bilden ovan.

4. Lägg till bibliotek tooyour projektet. tooadd ett bibliotek Högerklicka hello projektnamn i trädet för hello-projektet och klicka på **öppna Modulinställningar**. I hello **projektstruktur** i dialogrutan hello till vänster och klicka på **bibliotek**, klicka på symbolen hello (+) och klicka sedan på **från Maven**.

    ![Lägga till bibliotek](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    I hello **Download Library från Maven databasen** dialogrutan, söka och ange hello följande bibliotek.

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. Kopiera `yarn-site.xml` och `core-site.xml` från hello kluster headnode och lägga till den toohello projekt. Använd följande kommandon toocopy hello filer hello. Du kan använda [Cygwin](https://cygwin.com/install.html) toorun hello följande `scp` toocopy hello filer från hello klustret headnodes-kommandon.

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    Eftersom vi har redan lagts till hello klustret headnode IP-adress och värddatornamn för hello värdfilen på hello skrivbordet, kan vi använda hello **scp** kommandon i hello följande sätt.

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    Lägga till dessa filer tooyour projekt genom att kopiera dem under hello **/src** mapp i ditt projektträdet, till exempel `<your project directory>\src`.
6. Uppdatera hello `core-site.xml` toomake hello följande ändringar.

   1. `core-site.xml`innehåller hello krypterade nyckeln toohello storage-konto som är associerade med hello-kluster. I hello `core-site.xml` du lagt till toohello projekt, Ersätt hello-krypteringsnyckeln med hello faktiska lagringsnyckeln som associeras med hello standardkontot för lagring. Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-account).

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. Ta bort hello efter poster från hello `core-site.xml`.

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. Spara hello-filen.
7. Lägg till hello Main-klass för ditt program. Från hello **Projektutforskaren**, högerklicka på **src**, peka för**ny**, och klicka sedan på **Scala klassen**.

    ![Lägg till källkod](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. I hello **skapa nya Scala klass** dialogrutan Ange ett namn för **typ** Välj **objekt**, och klicka sedan på **OK**.

    ![Lägg till källkod](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. I hello `MyClusterAppMain.scala` fil, klistra in följande kod hello. Den här koden skapar hello Spark kontext och startar en `executeJob` metod från hello `SparkSample` objekt.

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. Upprepa steg 8 och 9 ovan tooadd kallas för ett nytt objekt i Scala `SparkSample`. toothis klassen Lägg till följande kod hello. Den här koden läser hello data från hello HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som bara har en siffra i hello sjunde kolumnen i hello CSV och skriver hello utdata för**/HVACOut** under hello standard lagringsbehållare som klustret hello.

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. Upprepa steg 8 och 9 ovan tooadd kallas för en ny klass `RemoteClusterDebugging`. Den här klassen implementerar hello Spark test framework som används för att felsöka program. Lägg till följande kod toohello hello `RemoteClusterDebugging` klass.

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     Några viktiga saker toonote här:

   * För `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, kontrollera hello Spark sammansättningen JAR är tillgänglig på hello klusterlagringen på hello angivna sökvägen.
   * För `setJars`, ange hello plats där hello artefakt jar kommer att skapas. Det är vanligtvis `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.
12. I hello `RemoteClusterDebugging` klassen, högerklicka på hello `test` nyckelord och välj **skapa RemoteClusterDebugging Configuration**.

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. Ange ett namn för hello configuration hello i dialogrutan och välj hello **testa typ** som **testnamnet**. Lämnar du alla andra värden som standard, klickar du på **tillämpa**, och klicka sedan på **OK**.

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. Du bör nu se en **Remote kör** configuration listrutan i hello menyraden.

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a>Steg 5: Kör programmet hello i felsökningsläge
1. Öppna i projektet IntelliJ IDEA `SparkSample.scala` och skapa en brytpunkt nästa too'val rdd1'. Välj hello popup-menyn för att skapa en brytpunkt **rad i funktionen executeJob**.

    ![Lägga till en brytpunkt](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. Klicka på hello **Debug kör** knappen Nästa toohello **Remote kör** configuration nedrullningsbara toostart hello program körs.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. När hello programkörningen nått hello brytpunkt bör du se en **felsökare** fliken i hello längst ned i fönstret.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. Klicka på hello (**+**) ikonen tooadd en titta på enligt hello bilden nedan.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    Här, eftersom programmet hello bröt mot innan hello variabeln `rdd1` har skapats med den här titta på kan vi se vad hello 5 första raderna i hello variabeln `rdd`. Tryck på **RETUR**.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    Du ser i hello bilden ovan är att du kan fråga terrabytes av data och felsökning vid körning, hur dina program pågår. Till exempel hello utdata som visas i hello bilden ovan och du kan se att hello första raden i hello utdata är en rubrik. Baserat på det kan du ändra ditt program kod tooskip hello rubrikrad om det behövs.
5. Nu kan du klicka på hello **återuppta programmet** ikonen tooproceed med ditt program körs.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. Du bör se utdata som liknar följande hello om programmet hello är klar.

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Demo
* Skapa Scala projekt (Video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Fjärråtkomst Debug (Video): [Använd Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

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
* [Använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate och skicka Spark Scala-appar](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
