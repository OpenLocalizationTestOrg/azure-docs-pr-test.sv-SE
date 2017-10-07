---
title: "Hadoop-aaaCreate kluster med hjälp av PowerShell - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate Hadoop, HBase, Storm eller Spark kluster på Linux för HDInsight med hjälp av Azure PowerShell."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Skapa Linux-baserade kluster i HDInsight med hjälp av Azure PowerShell

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell är en kraftfull skriptmiljö som du kan använda toocontrol och automatisera hello distribution och hantering av dina arbetsbelastningar i Microsoft Azure. Det här dokumentet innehåller information om hur toocreate en Linux-baserat HDInsight-kluster med hjälp av Azure PowerShell. Den innehåller också ett exempelskript.

> [!NOTE]
> Azure PowerShell är bara tillgänglig på Windows-klienter. Om du använder en Linux-, Unix- eller Mac OS X-klient, se [skapa ett Linux-baserat HDInsight-kluster med Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) information om hur du använder hello Azure CLI toocreate ett kluster.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du påbörjar den här proceduren:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Azure PowerShell-stöd för hantering av HDInsight-resurser med hjälp av Azure Service Manager **är föråldrat** och togs bort den 1 januari 2017. hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.
    >
    > Följ stegen hello i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello senaste versionen av Azure PowerShell. Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.

## <a name="create-cluster"></a>Skapa kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

toocreate ett HDInsight-kluster med hjälp av Azure PowerShell, måste du utföra följande procedurer hello:

* Skapa en Azure-resursgrupp
* Skapa ett Azure Storage-konto
* Skapa en Azure Blob-behållare
* Skapa ett HDInsight-kluster

hello följande skript visar hur toocreate ett nytt kluster:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

hello värdena du anger för hello klusterinloggning används toocreate hello Hadoop användarkontot för hello klustret. Använd det här kontot tooconnect tooservices hello kluster, till exempel web användargränssnitt eller REST API: er som värd.

hello värden du anger för hello SSH-användare är används toocreate hello SSH-användare för hello-kluster. Använd det här kontot toostart en fjärransluten SSH-session på hello klustret och köra jobb. Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

> [!IMPORTANT]
> Om du planerar toouse mer än 32 arbetarnoder (när klustret skapas eller genom att skala hello klustret när den har skapats), måste du också ange en storlek för huvudnod med minst 8 kärnor och 14 GB RAM-minne.
>
> Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

Det kan ta upp too20 minuter toocreate ett kluster.

## <a name="create-cluster-configuration-object"></a>Skapa kluster: konfigurationsobjekt

Du kan också skapa ett HDInsight configuration objekt med hjälp av `New-AzureRmHDInsightClusterConfig` cmdlet. Du kan sedan ändra den här konfigurationen objektet tooenable ytterligare konfigurationsalternativ för klustret. Använd slutligen hello `-Config` parametern för hello `New-AzureRmHDInsightCluster` cmdlet toouse hello konfiguration.

hello skapar följande skript ett configuration-objekt tooconfigure R-Server på typ av HDInsight-kluster. hello konfiguration kan en kantnod och RStudio ett ytterligare storage-konto.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> Med hjälp av ett lagringskonto i en annan plats än hello HDInsight-kluster stöds inte. När du använder det här exemplet kan du skapa hello ytterligare lagringskonto i hello samma plats som hello-server.

## <a name="customize-clusters"></a>Anpassa kluster

* Se [anpassa HDInsight-kluster med Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett HDInsight-kluster, Använd följande resurser toolearn hur hello toowork med ditt kluster.

### <a name="hadoop-clusters"></a>Hadoop-kluster

* [Använda Hive med HDInsight](hdinsight-use-hive.md)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-kluster

* [Kom igång med HBase på HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Utveckla Java-program för HBase i HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-kluster

* [Utveckla Java-topologier för Storm på HDInsight](hdinsight-storm-develop-java-topology.md)
* [Använda Python komponenter i Storm på HDInsight](hdinsight-storm-develop-python-topology.md)
* [Distribuera och övervaka topologier med Storm på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-kluster

* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)

