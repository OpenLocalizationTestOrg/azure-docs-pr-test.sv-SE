---
title: "aaaCreate Hadoop-kluster med säker överföring storage-konton i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toocreate HDInsight-kluster med säker överföring aktiverat Azure storage-konton."
keywords: hadoop getting started,hadoop linux,hadoop quickstart,secure transfer,azure storage account
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Skapa ett Hadoop-kluster med lagringskonton som använder säker överföring i Azure HDInsight

Hej [säker överföring krävs](../storage/common/storage-require-secure-transfer.md) funktionen förbättrar hello säkerheten för din Azure Storage-konto genom att tillämpa alla begäranden tooyour kontot via en säker anslutning. Den här funktionen och hello wasbs schemat stöds endast av HDInsight-kluster av version 3,6 eller senare. 

## <a name="prerequisites"></a>Krav
Innan du börjar den här vägledningen måste du ha:

* **Azure-prenumeration**: toocreate ett kostnadsfritt en månad konto, bläddra för[azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Ett Azure Storage-konto med säker överföring aktiverat**. Hello instruktioner finns i [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) och [kräver säker överföring](../storage/common/storage-require-secure-transfer.md).
* **En Blob-behållare på hello lagringskontot**. 
## <a name="create-cluster"></a>Skapa kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


I det här avsnittet skapar du ett Hadoop-kluster i HDInsight med en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). hello-mallen finns i [Gibhub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Du behöver inte ha någon erfarenhet av Resource Manager-mallar för att kunna följa de här självstudierna. För andra metoder för att skapa kluster och förstå hello egenskaper som används i den här självstudiekursen, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicka på hello efter bilden toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Följ hello instruktioner toocreate hello kluster med hello följande specifikationer: 

    - Ange HDInsight version 3.6.  hello standardversionen är 3.5. Version 3.6 eller senare krävs.
    - Ange ett lagringskonto som använder säker överföring.
    - Använd ett kort namn för hello storage-konto.
    - Både hello storage-konto och hello blob-behållaren måste skapas i förväg. 

    Hello instruktioner finns i [Skapa kluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 

Om du använder skriptet åtgärd tooprovide egna konfigurationsfiler, måste du använda wasbs i hello följande inställningar:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Lägga till fler lagringskonton

Det finns flera alternativ tooadd ytterligare säker överföring aktiverat storage-konton:

- Ändra hello Azure Resource Manager-mall i hello sista avsnittet.
- Skapa ett kluster med hello [Azure-portalen](https://portal.azure.com) och ange länkade storage-konto.
- Använd skriptet åtgärd tooadd ytterligare säker överföring aktiverat storage-konton tooan befintligt HDInsight-kluster.  Mer information finns i [lägga till ytterligare lagringsutrymme konton tooHDInsight](hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Nästa steg
I den här kursen har du lärt dig hur toocreate ett HDInsight-kluster och aktivera säker överföring av toohello storage-konton.

toolearn mer information om hur du analyserar data med HDInsight, se hello följande artiklar:

* toolearn mer information om hur du använder Hive med HDInsight, inklusive hur tooperform Hive-frågor från Visual Studio finns [använda Hive med HDInsight][hdinsight-use-hive].
* toolearn om Pig, ett språk används tootransform data, se [använda Pig med HDInsight][hdinsight-use-pig].
* toolearn om MapReduce, ett hur toowrite program som bearbetar data i Hadoop, se [använda MapReduce med HDInsight][hdinsight-use-mapreduce].
* toolearn om hur du använder hello HDInsight Tools för Visual Studio tooanalyze data i HDInsight, se [komma igång med Visual Studio Hadoop-verktygen för HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Mer information om hur HDInsight lagrar data toolearn eller hur tooget data till HDInsight, se hello följande artiklar:

* Mer information om hur HDInsight använder Azure Storage finns i [Använda Azure Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Mer information om hur tooupload data tooHDInsight finns [överför data tooHDInsight][hdinsight-upload-data].

toolearn mer om att skapa eller hantera HDInsight-kluster, se hello följande artiklar:

* toolearn om hur du hanterar ditt Linux-baserade HDInsight-kluster finns [hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).
* toolearn mer om hello-alternativ som du kan välja när du skapar ett HDInsight-kluster finns [skapa HDInsight i Linux med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md).
* Om du är bekant med Linux och Hadoop, men vill tooknow närmare information om Hadoop på hello HDInsight, se [arbeta med HDInsight på Linux](hdinsight-hadoop-linux-information.md). Detta ger information som:
  
  * URL: er för tjänster som ligger på hello klustret, till exempel Ambari och WebHCat
  * hello platsen för Hadoop-filer och exempel på hello lokalt filsystem
  * hello användning av Azure Storage (WASB) i stället för HDFS som hello standard datalager

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


