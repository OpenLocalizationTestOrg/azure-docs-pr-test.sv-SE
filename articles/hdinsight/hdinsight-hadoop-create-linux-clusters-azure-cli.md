---
title: "Skapa Hadoop-kluster med hjälp av kommandoraden-Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du skapar HDInsight-kluster med flera plattformar Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 8f2fcb46789d000cd66164508f1159338dcae5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Skapa HDInsight-kluster med Azure CLI

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Stegen i den här genomgången för dokument som skapar ett HDInsight 3.5-kluster med Azure CLI 1.0.

> [!IMPORTANT]
> Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Krav

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. Stegen i det här dokumentet testades senast med Azure CLI version 0.10.14.

    > [!IMPORTANT]
    > Stegen i det här dokumentet fungerar inte med Azure CLI 2.0. Azure CLI 2.0 stöder inte att skapa ett HDInsight-kluster.

## <a name="log-in-to-your-azure-subscription"></a>Logga in till din Azure-prenumeration

Följ stegen i [Anslut till en Azure-prenumeration från Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md) och anslut till din prenumeration med hjälp av metoden **inloggning**.

## <a name="create-a-cluster"></a>Skapa ett kluster

Följande steg ska utföras från en kommandorad, till exempel PowerShell- eller Bash.

1. Använd följande kommando för att autentisera till din Azure-prenumeration:

        azure login

    Du uppmanas att ange ditt namn och lösenord. Om du har flera Azure-prenumerationer, Använd `azure account set <subscriptionname>` att ange den prenumeration som använder Azure CLI-kommandona.

2. Växla till läget Azure Resource Manager, med följande kommando:

        azure config mode arm

3. Skapa en resursgrupp. Den här resursgruppen innehåller HDInsight-klustret och associerad storage-konto.

        azure group create groupname location

    * Ersätt `groupname` med ett unikt namn för gruppen.

    * Ersätt `location` med den geografiska region som du vill skapa gruppen i.

       En lista över giltiga platser, använder den `azure location list` kommando och Använd sedan någon av platserna från den `Name` kolumn.

4. Skapa ett lagringskonto. Det här lagringskontot används som standardlagring för HDInsight-klustret.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Ersätt `groupname` med namnet på gruppen som skapades i föregående steg.

    * Ersätt `location` med samma plats som används i föregående steg.

    * Ersätt `storagename` med ett unikt namn för lagringskontot.

        > [!NOTE]
        > Mer information om de parametrar som används i det här kommandot använder `azure storage account create -h` vill visa hjälp för det här kommandot.

5. Hämta den nyckel som används för att komma åt lagringskontot.

        azure storage account keys list -g groupname storagename

    * Ersätt `groupname` med resursgruppens namn.
    * Ersätt `storagename` med namnet på lagringskontot.

     Spara i de data som returneras av `key` värde för `key1`.

6. Skapa ett HDInsight-kluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Ersätt `groupname` med resursgruppens namn.

    * Ersätt `Hadoop` med den typ av kluster som du vill skapa. Till exempel `Hadoop`, `HBase`, `Kafka`, `Spark`, eller `Storm`.

     > [!IMPORTANT]
     > HDInsight-kluster som har olika typer som motsvarar arbetsbelastning eller teknik som klustret är inställd för. Det finns ingen stöds metod för att skapa ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster.

    * Ersätt `location` med samma plats som används i föregående steg.

    * Ersätt `storagename` med namnet på lagringskontot.

    * Ersätt `storagekey` med den nyckel som hämtades i föregående steg.

    * För den `--defaultStorageContainer` parametern, använder samma namn som du använder för klustret.

    * Ersätt `admin` och `httppassword` med namnet och lösenordet som du vill använda när klustret via HTTPS.

    * Ersätt `sshuser` och `sshuserpassword` med användarnamn och lösenord som du vill använda när du ansluter till klustret via SSH

    > [!IMPORTANT]
    > Det här exemplet skapar ett kluster med två worker anteckningar. Du kan också ändra antalet arbetarnoder när klustret har skapats genom att utföra skalning åtgärder. Om du tänker använda mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne. Du kan ange huvudnod storlek med den `--headNodeSize` parameter när klustret skapas.
    >
    > Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

    Det kan ta flera minuter för klusterskapandeprocessen ska slutföras. Vanligtvis cirka 15.

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett HDInsight-kluster med hjälp av Azure CLI, använder du följande information om hur du arbetar med ditt kluster:

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
