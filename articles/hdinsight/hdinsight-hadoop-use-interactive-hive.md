---
title: aaaUse interaktiva Hive i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse interaktiva Hive (Hive på LLAP) i HDInsight."
keywords: 
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 0957643c-4936-48a3-84a3-5dc83db4ab1a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 9e751a08091d18bc1b3d070468feef87f6828c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-interactive-hive-in-hdinsight-preview"></a>Använda interaktiva Hive i HDInsight (förhandsgranskning)
Interaktiva Hive (kallas även [Live långa och processen](https://cwiki.apache.org/confluence/display/Hive/LLAP)) är en ny HDInsight [kluster typen](hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktiva Hive tillåter i cacheminnet som gör Hive-frågor mycket mer interaktiva och snabbare. Denna nya funktion gör HDInsight en hello världens de flesta performant, flexibla och öppna stordatalösningar på hello moln med InMemory-cacheminnen (med Hive och Spark) och avancerade analyser genom djupgående integrering med R-tjänster. 

hello interaktiva Hive klustret skiljer sig från hello Hadoop-kluster. Den bara innehåller hello Hive-tjänsten. 

> [!NOTE]
> MapReduce, Pig, Sqoop, Oozie och andra tjänster ska tas bort från den här typen av klustret snart.
> hello Hive-tjänsten i hello interaktiva Hive kluster är endast tillgänglig via hello Ambari Hive-vyn, Beeline och Hive ODBC. Det går inte att komma åt via Hive-konsolen, Templeton, Azure CLI och Azure PowerShell. 
> 
> 

## <a name="create-an-interactive-hive-cluster"></a>Skapa en interaktiv Hive-kluster
Interaktiva Hive-kluster stöds bara på Linux-baserade kluster. Information om hur du skapar HDInsight-kluster finns i [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="execute-hive-from-interactive-hive"></a>Köra Hive från interaktiva Hive
Det finns olika alternativ för hur du kan köra Hive-frågor:

* Kör Hive med hello Ambari Hive-vyn
  
    Hello information om hur du använder hello Hive-vy finns i [Använd hello Hive-vy med Hadoop i HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).
* Kör Hive med Beeline
  
    Hello information om hur du använder Beeline på HDInsight finns i [använda Hive med Hadoop i HDInsight med Beeline](hdinsight-hadoop-use-hive-beeline.md).
  
    Du kan använda Beeline från hello headnode eller en tom kantnod.  Du bör använda Beeline från en tom kantnod.  Information om hur du skapar ett HDInsight-kluster med en tom edgenode finns [använda tom edge-noder i HDInsight](hdinsight-apps-use-edge-node.md).
* Kör Hive med hjälp av Hive ODBC
  
    Hello information om hur du använder Hive ODBC finns [ansluta Excel tooHadoop med hello Microsoft Hive ODBC-drivrutin](hdinsight-connect-excel-hive-odbc-driver.md).

**toofind hello JDBC-anslutningssträngen:**

1. Logga in tooAmbari med hello följande URL: https://<ClusterName>. AzureHDInsight.net.
2. Klicka på **Hive** hello vänstra menyn.
3. Klicka på hello markerat ikon toocopy hello-URL:
   
   ![HDInsight Hadoop Hive interaktiva LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Se även
* [Skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Lär dig hur toocreate interaktiva Hive-kluster i HDInsight.
* [Använda Hive med Hadoop i HDInsight med Beeline](hdinsight-hadoop-use-hive-beeline.md): Lär dig hur toouse Beeline toosubmit Hive-frågor.
* [Ansluta Excel tooHadoop med hello Microsoft Hive ODBC-drivrutin](hdinsight-connect-excel-hive-odbc-driver.md): Lär dig hur tooconnect Excel tooHive.

