---
title: "aaaInstall och använda Giraph på Hadoop-kluster i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toocustomize HDInsight-kluster med Giraph och hur toouse Giraph."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd473faca9d3c87c29d7566a18fc94211c50f059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Installera och använda Giraph på Windows-baserade HDInsight-kluster

Lär dig hur toocustomize Windows-baserade HDInsight-kluster med Giraph med skriptåtgärder och hur toouse Giraph tooprocess storskaliga diagram. Information om hur du använder Giraph med ett Linux-baserade kluster finns i [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Mer information om hur tooinstall Giraph på en Linux-baserade HDInsight-kluster finns [installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md).


Du kan installera Giraph på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*. Ett exempel skriptet tooinstall Giraph på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). hello exempelskript fungerar bara med HDInsight-kluster av version 3.1. Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).

**Relaterade artiklar**

* [Installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Vad är Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight. Diagram modellen relationer mellan objekt, till exempel hello anslutningar mellan routrar i ett större nätverk som hello Internet, eller relationer mellan personer på sociala nätverk (ibland hänvisade tooas sociala diagram). Diagrammet bearbetning kan du tooreason om hello relationer mellan objekt i ett diagram som:

* Identifiera potentiella vänner baserat på din aktuella relationer.
* Identifiera hello kortaste vägen mellan två datorer i ett nätverk.
* Beräkning av hello sidan rangordningen för webbsidor.

## <a name="install-giraph-using-portal"></a>Installera Giraph med hjälp av portalen
1. Skapa ett kluster med hjälp av hello **skapa anpassade** alternativ, enligt beskrivningen i [skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md).
2. På hello **skriptåtgärder** sidan hello guiden klickar du på **lägga till skriptåtgärd** tooprovide information om hello skriptåtgärd enligt nedan:

    ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Använd skriptåtgärder toocustomize ett kluster")

    <table border='1'>
        <tr><th>Egenskap</th><th>Värde</th></tr>
        <tr><td>Namn</td>
            <td>Ange ett namn för hello skriptåtgärder. Till exempel <b>installera Giraph</b>.</td></tr>
        <tr><td>Skript-URI</td>
            <td>Ange hello identifierare URI (Uniform Resource) toohello skript som är anropade toocustomize hello-kluster. Till exempel <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Nodtyp</td>
            <td>Ange hello noder som hello anpassning skript körs. Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder endast</b>.
        <tr><td>Parametrar</td>
            <td>Ange hello parametrar, om det krävs av hello skript. hello skriptet tooinstall Giraph kräver inte parametrar, så du kan lämna den tom.</td></tr>
    </table>

    Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret. När du har lagt till hello skript klickar du på hello markering toostart skapar hello kluster.

## <a name="use-giraph"></a>Använd Giraph
Vi använder hello SimpleShortestPathsComputation exempel toodemonstrate hello basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementering för att hitta hello shortest path mellan objekt i ett diagram. Använd följande steg tooupload hello exempel data och hello exempel jar, köra ett jobb med hjälp av hello SimpleShortestPathsComputation exempel och visa hello resultat hello.

1. Överför en exempel data filen tooAzure Blob storage. Skapa en ny fil med namnet på din lokala arbetsstation **tiny_graph.txt**. Den ska innehålla hello följande rader:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Överför hello tiny_graph.txt toohello primära fillagring för HDInsight-kluster. Anvisningar för hur tooupload data finns i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).

    Informationen beskriver en relation mellan objekt i en riktat diagram hello formatet [källa\_id, källa\_värde, [[målet\_id], [edge\_värde],...]]. Varje rad representerar en relation mellan en **källa\_id** objekt och en eller flera **dest\_id** objekt. Hej **kant\_värdet** (eller vikt) kan betraktas som hello styrkan eller avståndet mellan hello anslutning mellan **source_id** och **dest\_id**.

    Dras ut, och använder hello värde (eller vikt) som hello avståndet mellan objekt, hello ovanför data kan se ut så här:

    ![tiny_graph.txt ritas som cirklar med rader med olika avståndet mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Kör hello SimpleShortestPathsComputation exempel. Använd hello följande Azure PowerShell-cmdlets toorun hello exempel med hjälp av hello tiny_graph.txt fil som indata.

    > [!IMPORTANT]
    > Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017. hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell. Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create hello definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run hello job, write output toohello Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    I ovanstående exempel hello, ersätter **klusternamn** med hello namnet på ditt HDInsight-kluster som har Giraph installerad.
3. Visa hello resultat. När hello jobbet har slutförts, hello resultat lagras i två utdatafilerna i hello **wasb: / / / exempel/out/shotestpaths** mapp. hello filerna kallas **del-m-00001** och **del-m-00002**. Utför hello följande steg toodownload och visa hello utdata:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select hello current subscription
    Select-AzureSubscription $subscriptionName

    # Create hello Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download hello job output toohello workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Detta skapar hello **shortestpaths-exempel/utdata** katalogstruktur i hello aktuell katalog på din arbetsstation och hämta hello två filer toothat platsen.

    Använd hello **Cat** cmdlet toodisplay hello innehållet i hello filer:

        Cat example/output/shortestpaths/part*

    hello utdata ska se ut ungefär toohello följande:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Hej SimpleShortestPathComputation exempel är hårdkodad toostart med objekt-ID 1 och hitta hello shortest path tooother objekt. Så hello utdata ska läsas som `destination_id distance`, där avstånd är hello värde (eller vikt) av hello kanter obligatoriska mellan objekt-ID 1 och hello mål-ID.

    Du kan visualisera detta för att verifiera hello resultaten av reser hello kortaste sökvägar mellan ID 1 och alla andra objekt. Observera att hello shortest path mellan ID 1 och 4-ID är 5. Detta är hello totala avståndet mellan <span style="color:orange">ID 1 och 3</span>, och sedan <span style="color:red">ID 3 och 4</span>.

    ![Ritning av objekt som cirklar med kortast sökvägar mellan](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Installera Giraph med Aure PowerShell
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Installera Giraph med .NET SDK
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello exemplet visar hur tooinstall Spark med hello .NET SDK. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Se även
* [Installera Giraph på HDInsight Hadoop-kluster (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-provision-clusters.md): allmän information om hur du skapar HDInsight-kluster.
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder.
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md).
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark.
* [Installera R på HDInsight-kluster][hdinsight-install-r]: skriptåtgärder exempel om hur du installerar R.
* [Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install.md): skriptåtgärder exempel om hur du installerar Solr.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
