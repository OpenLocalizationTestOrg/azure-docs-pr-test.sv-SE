---
title: "aaaUse R i HDInsight-kluster för toocustomize - Azure | Microsoft Docs"
description: "Lär dig hur tooinstall R med hjälp av skript och Använd R i HDInsight-kluster."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bf5adf2e18dc43a743b29fd1567fad731b9c3ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installera och använda R på HDInsight Hadoop-kluster

Lär dig hur toocustomize Windows baserade HDInsight-kluster med hjälp av skriptåtgärder R och hur toouse R på HDInsight-kluster. Hej [HDInsight erbjudande](https://azure.microsoft.com/pricing/details/hdinsight/) innehåller R-Server som en del av ditt HDInsight-kluster. Detta kan R toouse MapReduce och Spark toorun distribuerade beräkningar. Mer information finns i [Komma igång med R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md). Information om hur du använder R med ett Linux-baserade kluster finns i [installerar och använder R på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-r-scripts-linux.md).

Du kan installera R på någon typ av kluster (Hadoop, Storm, HBase, Spark) på Azure HDInsight med hjälp av *skriptåtgärd*. Ett exempel skriptet tooinstall R på ett HDInsight-kluster är tillgänglig från en skrivskyddad Azure storage blob [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**Relaterade artiklar**

* [Installera och använda R på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): allmän information om hur du skapar HDInsight-kluster
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Vad är R?
Hej <a href="http://www.r-project.org/" target="_blank">R-projekt för statistisk databehandling</a> är en öppen källa och -miljö för statistisk databehandling. R innehåller hundratals inbyggda statistiska funktioner och sin egen programmeringsspråk som kombinerar aspekter av funktions- och objektorienterad programmering. Det ger också omfattande grafiskt funktioner. R är hello prioriterade programmeringsmiljö mest professional statistiker och forskare i en mängd olika fält.

R är kompatibelt med Azure Blob Storage (WASB) så att data som lagras det bearbetas med R på HDInsight.  

## <a name="install-r"></a>Installera R
En [exempel på skript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) tooinstall R på ett HDInsight-kluster är tillgänglig från en skrivskyddad blobb i Azure Storage. Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar hello-kluster med hjälp av hello Azure-portalen.

> [!NOTE]
> hello exempelskript introducerades med HDInsight-kluster av version 3.1. Mer information om HDInsight-kluster-versioner finns [HDInsight-kluster-versioner](hdinsight-component-versioning.md).
>
>

1. När du skapar ett HDInsight-kluster från hello Portal, klickar du på **valfri konfiguration**, och klicka sedan på **skriptåtgärder**.
2. På hello **skriptåtgärder** anger hello följande värden:

    ![Använd skriptåtgärder toocustomize ett kluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Använd skriptåtgärder toocustomize ett kluster")

    <table border='1'>
        <tr><th>Egenskap</th><th>Värde</th></tr>
        <tr><td>Namn</td>
            <td>Ange ett namn för hello skriptåtgärd, till exempel <b>installera R</b>.</td></tr>
        <tr><td>Skript-URI</td>
            <td>Ange hello URI toohello skript som är anropade toocustomize hello klustret, till exempel <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Nodtyp</td>
            <td>Ange hello noder som hello anpassning skript körs. Du kan välja <b>alla noder</b>, <b>huvudnoder endast</b>, eller <b>arbetarnoder</b> endast.
        <tr><td>Parametrar</td>
            <td>Ange hello parametrar, om det krävs av hello skript. Hello skriptet tooinstall R kräver dock inte parametrar, så du kan lämna den tom.</td></tr>
    </table>

    Du kan lägga till fler än en skript åtgärd tooinstall flera komponenter på hello klustret. När du har lagt till hello skript klickar du på hello markerat toostart crating hello klustret.

Du kan också använda hello skriptet tooinstall R i HDInsight med hjälp av Azure PowerShell eller hello HDInsight .NET SDK. Anvisningar för de här procedurerna finns senare i den här artikeln.

## <a name="run-r-scripts"></a>R-skript körs
Det här avsnittet beskrivs hur toorun ett R-skriptet på hello Hadoop-kluster med HDInsight.

1. **Upprätta ett fjärrskrivbord anslutning toohello kluster**: aktivera Fjärrskrivbord för hello-klustret som du skapade med R installerat från hello Portal och ansluter sedan toohello klustret. Instruktioner finns i [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Öppna hello R konsolen**: hello R installationen placerar en länk toohello R-konsol på hello skrivbord hello huvudnod. Klicka på den tooopen hello R-konsolen.
3. **Kör hello R-skriptet**: hello R-skript kan köras direkt från hello R-konsolen genom att klistra in, markera den och trycka på RETUR. Här är ett enkelt exempelskript som genererar hello talen 1 too100 och multiplicerar dem med 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

hello två första raderna anropet hello RHadoop bibliotek som installeras med R. hello sista raden utskrifter hello resultat toohello konsolen. hello utdata ska se ut så här:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Installera R med Aure PowerShell
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  hello exemplet visar hur tooinstall Spark med hjälp av Azure PowerShell. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Installera R med .NET SDK
Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). hello exemplet visar hur tooinstall Spark med hello .NET SDK. Du behöver toocustomize hello skriptet toouse [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).

## <a name="see-also"></a>Se även
* [Installera och använda R på HDinsight Hadoop-kluster (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): allmän information om hur du skapar HDInsight-kluster
* [Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-cluster-customize]: allmän information om hur du anpassar HDInsight-kluster med skriptåtgärder
* [Utveckla skriptåtgärd skript för HDInsight](hdinsight-hadoop-script-actions.md)
* [Installera och använda Spark på HDInsight-kluster][hdinsight-install-spark]: skriptåtgärder exempel om hur du installerar Spark
* [Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install.md): skriptåtgärder exempel om hur du installerar Giraph
* [Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md): skriptåtgärder exempel om hur du installerar Solr.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md
