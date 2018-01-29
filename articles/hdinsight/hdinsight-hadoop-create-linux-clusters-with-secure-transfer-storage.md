---
title: "Skapa ett Hadoop-kluster med lagringskonton som använder säker överföring i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du skapar HDInsight-kluster med Azure-lagringskonton som använder säker överföring."
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
ms.date: 11/06/2017
ms.author: jgao
ms.openlocfilehash: 9b537595fd8224536f67989d7529f6030347bfab
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Skapa ett Hadoop-kluster med lagringskonton som använder säker överföring i Azure HDInsight

Funktionen [Säker överföring krävs](../storage/common/storage-require-secure-transfer.md) förbättrar säkerheten för ditt Azure Storage-konto genom att kräva att alla förfrågningar till ditt konto görs via en säker anslutning. Den här funktionen och wasbs-schemat stöds endast av HDInsight-kluster med version 3.6 eller senare. 

## <a name="prerequisites"></a>Krav
Innan du börjar den här vägledningen måste du ha:

* **Azure-prenumeration**: Gå till  [azure.microsoft.com/free](https://azure.microsoft.com/free) för att skapa ett kostnadsfritt provkonto för en månad.
* **Ett Azure Storage-konto med säker överföring aktiverat**. Anvisningar finns i [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) (Skapa ett lagringskonto) och [Require secure transfer](../storage/common/storage-require-secure-transfer.md) (Kräva säker överföring).
* **En blob-behållare för lagringskontot**. 
## <a name="create-cluster"></a>Skapa kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


I det här avsnittet skapar du ett Hadoop-kluster i HDInsight med en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). Mallen finns i [Github](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Du behöver inte ha någon erfarenhet av Resource Manager-mallar för att kunna följa de här självstudierna. Mer information om andra metoder för att skapa kluster och förstå de egenskaper som tillämpas i de här självstudierna finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicka på följande bild för att logga in på Azure och öppna Resource Manager-mallen i Azure Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Följ anvisningarna för att skapa klustret med följande specifikationer: 

    - Ange HDInsight version 3.6.  Standardversionen är 3.5. Version 3.6 eller senare krävs.
    - Ange ett lagringskonto som använder säker överföring.
    - Använd lagringskontots kortnamn.
    - Både lagringskontot och blob-behållaren måste skapas i förväg. 

    Anvisningar finns i avsnittet [Skapa kluster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). 

Om du använder skriptåtgärder för att skapa egna konfigurationsfiler måste du använda wasbs i följande inställningar:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Lägga till fler lagringskonton

Det finns flera alternativ för att lägga till ytterligare lagringskonton som använder säker överföring:

- Ändra Azure Resource Manager-mallen i det sista avsnittet.
- Skapa ett kluster med hjälp av [Azure-portalen](https://portal.azure.com) och ange ett länkat lagringskonto.
- Använd skriptåtgärder om du vill lägga till fler lagringskonton som använder säker överföring i ett befintligt HDInsight-kluster.  Mer information finns i [Add additional storage accounts to HDInsight](hdinsight-hadoop-add-storage.md) (Lägga till fler lagringskonton till HDInsight).

## <a name="next-steps"></a>Nästa steg
I den här kursen har du lärt dig hur du skapar ett HDInsight-kluster och hur du aktiverar säker överföring för lagringskontona.

Mer information om att analysera data med HDInsight finns i följande artiklar:

* Mer information om att använda Hive med HDInsight, inklusive hur du gör Hive-frågor från Visual Studio, finns i [Använda Hive med HDInsight][hdinsight-use-hive].
* Du kan läsa mer om Pig, ett språk som används för att omvandla data, i [Använda Pig med HDInsight][hdinsight-use-pig].
* Du kan läsa mer om MapReduce, ett sätt att skriva appar som bearbetar data i Hadoop, i [Använda MapReduce med HDInsight][hdinsight-use-mapreduce].
* Du kan läsa mer om hur du använder HDInsight Tools för Visual Studio för att analysera data i HDInsight i [Komma igång med Visual Studio Hadoop-verktyg för HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Mer information om hur HDInsight lagrar data eller hur du hämtar data till HDInsight finns i följande artiklar:

* Mer information om hur HDInsight använder Azure Storage finns i [Använda Azure Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).
* Mer information om hur du överför data till HDInsight finns i [Överföra data till HDInsight][hdinsight-upload-data].

Mer information om hur du skapar eller hanterar ett HDInsight-kluster finns i följande artiklar:

* Mer information om att hantera ditt Linux-baserade HDInsight-kluster finns i [Hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md).
* Mer information om de alternativ som du kan välja när du skapar ett HDInsight-kluster finns i [Skapa HDInsight i Linux med anpassade alternativ](hdinsight-hadoop-provision-linux-clusters.md).
* Om du är bekant med Linux och Hadoop, men vill få närmare information om Hadoop i HDInsight, hittar du mer information i [Arbeta med HDInsight i Linux](hdinsight-hadoop-linux-information.md). Detta ger information som:
  
  * URL:er för de tjänster som finns i klustret, till exempel Ambari och WebHCat
  * Platsen för Hadoop-filer och -exempel i det lokala filsystemet
  * Användning av Azure Storage (WASB) i stället för HDFS som datalagringsutrymme av standardtyp

[1]: ../HDInsight/hadoop/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md


