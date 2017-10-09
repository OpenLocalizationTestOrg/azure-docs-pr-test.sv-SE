---
title: aaaHue med Hadoop i HDInsight Linux-baserade kluster - Azure | Microsoft Docs
description: "Lär dig hur tooinstall Hue på HDInsight-kluster och använder tunneltrafik tooroute hello begäranden tooHue. Använd Hue toobrowse lagring och köra Hive och Pig."
keywords: Hue hadoop
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 9e57fcca-e26c-479d-a745-7b80a9290447
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: f086cbad2a90cc6903fbfccbf4a6be44f8999d07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Installera och använda Hue på HDInsight Hadoop-kluster

Lär dig hur tooinstall Hue på HDInsight-kluster och använder tunneltrafik tooroute hello begäranden tooHue.

> [!IMPORTANT]
> hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="what-is-hue"></a>Vad är Hue?
Hue är en uppsättning webbprogram används toointeract med ett Hadoop-kluster. Du kan använda Hue toobrowse hello lagring som associerats med Hadoop-kluster (WASB hello gäller HDInsight-kluster), kör Hive-jobb och Pig-skript och så vidare. hello följande komponenter är tillgängliga med Hue installationer på ett HDInsight Hadoop-kluster.

* Bivax Hive-redigeraren
* Pig
* Metastore manager
* Oozie
* FileBrowser (som nämns tooWASB standardbehållaren)
* Jobbet webbläsare

> [!WARNING]
> Komponenter som ingår i hello HDInsight-kluster stöds fullt ut och Microsoft Support kommer att tooisolate och lösa problem relaterade toothese komponenter.
>
> Anpassade komponenter får stöd för kommersiellt rimliga toohelp du toofurther hello felsökning. Detta kan resultera i att lösa problemet hello eller be tooengage tillgängliga kanaler för hello öppnas teknikerna där djup expertis för att teknik finns. Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).
>
>

## <a name="install-hue-using-script-actions"></a>Installera Hue med hjälp av skriptåtgärder

hello skriptet tooinstall Hue på en Linux-baserade HDInsight-kluster är tillgänglig på https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh. Du kan använda det här skriptet tooinstall Hue i kluster med Azure Storage BLOB (WASB) eller Azure Data Lake Store som standardlagring.

Det här avsnittet innehåller instruktioner om hur toouse hello skript vid etablering av hello-kluster med hjälp av hello Azure-portalen.

> [!NOTE]
> Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK eller Azure Resource Manager-mallar kan också vara används tooapply skriptåtgärder. Du kan också använda skriptet åtgärder tooalready kluster som körs. Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).
>
>

1. Börja etablera ett kluster med hjälp av hello stegen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md), men inte slutföra etablering.

   > [!NOTE]
   > tooinstall Hue på HDInsight-kluster, hello rekommenderade headnode storleken är minst A4 (8 kärnor, 14 GB minne).
   >
   >
2. På hello **valfri konfiguration** bladet väljer **skriptåtgärder**, och ge hello information som visas nedan:

    ![Ange skriptet åtgärdsparametrar för för Hue](./media/hdinsight-hadoop-hue-linux/hue-script-action.png "ange skript åtgärdsparametrar för för Hue")

   * **NAMNET**: Ange ett eget namn för hello skriptåtgärder.
   * **SKRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
   * **HEAD**: Markera det här alternativet
   * **WORKER**: lämnar fältet tomt.
   * **ZOOKEEPER**: lämnar fältet tomt.
   * **PARAMETRARNA**: lämnar fältet tomt.
3. Längst ned hello hello **skriptåtgärder**, använda hello **Välj** toosave hello konfiguration. Använd slutligen hello **Välj** knappen längst ned hello hello **valfri konfiguration** bladet toosave hello ytterligare konfigurationsinformation.
4. Fortsätta etablering hello klustret enligt beskrivningen i [etablera HDInsight-kluster på Linux](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="use-hue-with-hdinsight-clusters"></a>Använda Hue med HDInsight-kluster

SSH-tunnlar är hello endast sätt tooaccess Hue på hello klustret när den körs. Tunneltrafik via SSH kan hello trafik toogo direkt toohello headnode av hello kluster där Hue körs. När etableringen är klar i hello klustret Använd hello följande steg toouse Hue i ett kluster i HDInsight Linux.

> [!NOTE]
> Du rekommenderas att använda Firefox web webbläsare toofollow hello anvisningarna nedan.
>
>

1. Använd hello information i [använda SSH-tunnlar tooaccess Ambari web UI, resurshanteraren, jobbhistorik, NameNode, Oozie och andra Webbgränssnittet](hdinsight-linux-ambari-ssh-tunnel.md) toocreate en SSH tunnel från din klient system toohello HDInsight-kluster och sedan konfigurera din Web webbläsare toouse hello SSH-tunnel som en proxy.

2. När du har skapat en SSH-tunnel och konfigurerat din webbläsare tooproxy trafik via den, måste du söka efter hello värdnamnet för hello primära huvudnod. Du kan göra detta genom att ansluta toohello kluster med SSH på port 22. Till exempel `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` där **användarnamn** är SSH-användarnamn och **KLUSTERNAMN** är hello namnet på klustret.

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

3. När du är ansluten, Använd följande kommando tooobtain hello fullständigt kvalificerade domännamnet för hello primära headnode hello:

        hostname -f

    Detta returnerar en namnet liknande toohello följande:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

    Detta är hello värdnamnet för hello primära headnode där hello Hue webbplatsen finns.
4. Använd hello webbläsare tooopen hello Hue-portalen på http://HOSTNAME:8888. Ersätt VÄRDNAMN med hello-namn som du hämtade i hello föregående steg.

   > [!NOTE]
   > När du loggar in för hello första gången, kommer du att ange toocreate ett konto toolog toohello Hue-portalen. hello-autentiseringsuppgifter som du anger här kommer att vara begränsade toohello portal och är inte relaterade toohello administratören eller SSH-autentiseringsuppgifter som du angav vid etablera hello klustret.
   >
   >

    ![Hue-portalen för inloggningen toohello](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-login.png "ange autentiseringsuppgifter för Hue-portalen")

### <a name="run-a-hive-query"></a>Köra en Hive-fråga
1. Hello Hue-portalen klickar du på **frågan redigerare**, och klicka sedan på **Hive** tooopen hello Hive-redigeraren.

    ![Använda Hive](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-use-hive.png "använda Hive")
2. På hello **hjälpa** fliken, under **databasen**, bör du se **hivesampletable**. Det här är en exempeltabell som levereras med Hadoop-kluster i HDInsight. Ange en exempelfråga i hello högra fönstret och se hello utdata på hello **resultat** fliken hello i fönstret nedan, som visas i hello skärmdump.

    ![Köra Hive-fråga](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-hive-query.png "kör Hive-fråga")

    Du kan också använda hello **diagram** fliken toosee en bild av hello resultat.

### <a name="browse-hello-cluster-storage"></a>Bläddra hello klusterlagringen
1. Hello Hue-portalen klickar du på **Filhanteraren** i hello övre högra hörnet av hello menyraden.
2. Som standard hello webbläsaren öppnas på hello **/användare/myuser** directory. Klicka på hello snedstreck före hello användarkatalog i hello sökvägen toogo toohello rot hello Azure storage-behållare som associerats med hello kluster.

    ![Använd Filhanteraren](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-file-browser.png "Använd filläsare")
3. Högerklicka på en fil eller mapp toosee hello tillgängliga åtgärder. Använd hello **överför** knappen i hello höger tooupload filer toohello aktuella katalogen. Använd hello **ny** knappen toocreate nya filer eller kataloger.

> [!NOTE]
> hello Hue webbläsaren kan endast visa hello innehållet i hello standardbehållaren som är associerade med hello HDInsight-kluster. Eventuella ytterligare konton/behållare för lagring som du har associerat med hello kluster är inte tillgänglig via hello Filhanteraren. Hello ytterligare behållare som är associerade med hello klustret kommer dock alltid vara tillgänglig för hello Hive-jobb. Om du anger kommandot hello exempelvis `dfs -ls wasb://newcontainer@mystore.blob.core.windows.net` i hello Hive-redigeraren, kan du se hello innehållet i ytterligare behållare också. I det här kommandot **newcontainer** är inte hello standardbehållaren som är kopplade till ett kluster.
>
>

## <a name="important-considerations"></a>Viktiga överväganden
1. hello skriptet som används för tooinstall Hue installerar den endast på hello primära headnode hello-klustret.

2. Under installationen av flera Hadoop-tjänsterna (HDFS, YARN, MR2, Oozie) startas om för att uppdatera hello konfiguration. När hello skriptet har slutförts installera Hue kan det ta lite tid innan andra Hadoop services toostart. Detta kan påverka Hues prestanda från början. När alla tjänster startas Hue fungerar som den ska.
3. Hue förstår inte Tez jobb, vilket är hello aktuell standard för Hive. Om du vill toouse MapReduce som hello motorn för körning av Hive, uppdatera hello skriptet toouse hello följande kommando i skriptet:

        set hive.execution.engine=mr;

4. Du kan ha ett scenario där dina tjänster som körs på hello primära headnode medan hello Resource Manager kan köras på hello sekundär med Linux-kluster. Ett sådant scenario kan resultera i fel (se nedan) när du använder Hue tooview information om jobb som KÖRS på hello klustret. Du kan visa hello jobbinformation när hello jobbet har slutförts.

   ![Hue-portalen fel](./media/hdinsight-hadoop-hue-linux/hdinsight-hue-portal-error.png "Hue portal fel")

   Detta förfaller tooa kända problem. Som en tillfällig lösning kan du ändra Ambari så att hello active Resource Manager körs även på hello primära headnode.
5. Hue förstår WebHDFS medan HDInsight-kluster använder Azure Storage med `wasb://`. Så installerar hello anpassat skript används med skriptåtgärder WebWasb, vilket är en WebHDFS-kompatibelt tjänst för pratar tooWASB. Så även om hello Hue-portalen står HDFS platser (t.ex. när du flyttar musen över hello **Filhanteraren**), ska tolkas som WASB.

## <a name="next-steps"></a>Nästa steg
* [Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md). Använd kluster anpassning tooinstall Giraph på HDInsight Hadoop-kluster. Giraph kan du tooperform diagrammet bearbetning med Hadoop och den kan användas med Azure HDInsight.
* [Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md). Använd kluster anpassning tooinstall Solr på HDInsight Hadoop-kluster. Solr kan tooperform kraftfulla sökningar på lagrade data.
* [Installera R på HDInsight-kluster](hdinsight-hadoop-r-scripts-linux.md). Använd kluster anpassning tooinstall R på HDInsight Hadoop-kluster. R är ett språk med öppen källkod och miljö för statistisk databehandling. Det ger hundratals inbyggda statistiska funktioner och sin egen programmeringsspråk som kombinerar aspekter av funktions- och objektorienterad programmering. Det ger också omfattande grafiskt funktioner.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
