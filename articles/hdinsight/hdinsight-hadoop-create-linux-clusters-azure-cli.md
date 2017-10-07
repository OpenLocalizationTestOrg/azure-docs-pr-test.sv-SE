---
title: "aaaCreate Hadoop-kluster med hjälp av kommandoradsverktyget - hello Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster med hello plattformsoberoende Azure CLI 1.0."
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
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a>Skapa HDInsight-kluster med hello Azure CLI

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

hello stegen i den här genomgången för dokument som skapar ett HDInsight 3.5-kluster med hello Azure CLI 1.0.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Krav

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. hello stegen i det här dokumentet testades senast med Azure CLI version 0.10.14.

    > [!IMPORTANT]
    > hello stegen i det här dokumentet fungerar inte med Azure CLI 2.0. Azure CLI 2.0 stöder inte att skapa ett HDInsight-kluster.

## <a name="log-in-tooyour-azure-subscription"></a>Logga in tooyour Azure-prenumeration

Följ stegen i hello [ansluta tooan Azure-prenumeration från hello Azure-kommandoradsgränssnittet (Azure CLI)](../xplat-cli-connect.md) och ansluta tooyour prenumeration med hjälp av hello **inloggning** metod.

## <a name="create-a-cluster"></a>Skapa ett kluster

hello följande steg ska utföras från en kommandorad, till exempel PowerShell- eller Bash.

1. Använd följande kommando tooauthenticate tooyour Azure-prenumeration hello:

        azure login

    Du är begärd tooprovide ditt namn och lösenord. Om du har flera Azure-prenumerationer, Använd `azure account set <subscriptionname>` tooset hello prenumeration som hello Azure CLI-kommandon används.

2. Växla tooAzure Resource Manager-läge med hello följande kommando:

        azure config mode arm

3. Skapa en resursgrupp. Den här resursgruppen innehåller hello HDInsight-kluster och associerad storage-konto.

        azure group create groupname location

    * Ersätt `groupname` med ett unikt namn för hello grupp.

    * Ersätt `location` med hello geografiska region som du vill toocreate hello grupp i.

       En lista över giltiga platser använder hello `azure location list` kommando och Använd sedan någon av hello platser från hello `Name` kolumn.

4. Skapa ett lagringskonto. Det här lagringskontot används som hello standardlagring för hello HDInsight-kluster.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Ersätt `groupname` med hello-grupp som skapades i föregående steg i hello hello namn.

    * Ersätt `location` med hello samma plats som används i hello föregående steg.

    * Ersätt `storagename` med ett unikt namn för hello storage-konto.

        > [!NOTE]
        > Mer information om hello parametrar som används i det här kommandot använder `azure storage account create -h` tooview hjälp för det här kommandot.

5. Hämta hello nyckel används tooaccess hello storage-konto.

        azure storage account keys list -g groupname storagename

    * Ersätt `groupname` med hello resursgruppens namn.
    * Ersätt `storagename` med hello namnet hello storage-konto.

     Hello data som returneras, spara hello `key` värde för `key1`.

6. Skapa ett HDInsight-kluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Ersätt `groupname` med hello resursgruppens namn.

    * Ersätt `Hadoop` med hello typ av kluster du vill toocreate. Till exempel `Hadoop`, `HBase`, `Kafka`, `Spark`, eller `Storm`.

     > [!IMPORTANT]
     > HDInsight-kluster som har olika typer som toohello arbetsbelastning eller teknik som hello klustret är inställd för. Det finns ingen metod som stöds toocreate ett kluster som kombinerar flera typer, till exempel Storm och HBase på ett kluster.

    * Ersätt `location` med hello samma plats som används i föregående steg.

    * Ersätt `storagename` med hello lagringskontonamn.

    * Ersätt `storagekey` med hello-nyckeln som hämtades i föregående steg i hello.

    * För hello `--defaultStorageContainer` parametern, Använd hello samma namn som du använder för hello klustret.

    * Ersätt `admin` och `httppassword` med hello namn och lösenord du vill toouse när hello klustret via HTTPS.

    * Ersätt `sshuser` och `sshuserpassword` med hello användarnamn och lösenord du vill toouse vid åtkomst till hello kluster med SSH

    > [!IMPORTANT]
    > Det här exemplet skapar ett kluster med två worker anteckningar. Du kan också ändra hello antalet arbetarnoder när klustret har skapats genom att utföra skalning åtgärder. Om du tänker använda mer än 32 arbetarnoder, måste du välja en huvudnod storlek med minst 8 kärnor och 14 GB RAM-minne. Du kan ange hello huvudnod storlek med hello `--headNodeSize` parameter när klustret skapas.
    >
    > Mer information om noden storlekar och relaterade kostnader finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

    Det kan ta flera minuter för hello klustret skapas processen toofinish. Vanligtvis cirka 15.

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett HDInsight-kluster med hjälp av hello Azure CLI, använder du följande toolearn hur hello toowork med ditt kluster:

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
