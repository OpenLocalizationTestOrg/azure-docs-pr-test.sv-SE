---
title: "Använda Zeppelin för att köra Hive-frågor i Azure HDInsight | Microsoft Docs"
description: "Lär dig använda Zeppelin för att köra Hive-frågor."
keywords: "hdinsight hadoop, hive, interaktiva fråga, LLAP"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jgao
ms.openlocfilehash: 39f99bef252e93db55e0493ee284ef78b7d087a1
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/04/2018
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Använda Zeppelin för att köra Hive-frågor i Azure HDInsight 

HDInsight interaktiva frågan innehåller Zeppelin-anteckningsböcker som du kan använda för att köra interaktiva Hive-frågor. I den här artikeln lär du dig för att köra Hive-frågor i Azure HDInsight med Zeppelin. 

## <a name="prerequisites"></a>Förutsättningar
Innan du fortsätter med den här artikeln, måste du ha följande:

* **Interaktiva frågan för HDInsight-kluster**. Se [Skapa kluster](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) att skapa ett HDInsight-kluster.  Se till att använda interaktiva frågetypen. 

## <a name="create-a-zeppelin-note"></a>Skapa en Zeppelin-kommentar

1. Bläddra till följande URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Ersätt **CLUSTERNAME** med namnet på klustret.

2. Ange ditt Hadoop-användarnamn och lösenord. Från sidan Zeppelin kan du skapa en ny kommentar eller öppna befintliga anteckningar. HiveSample innehåller vissa Hive-exempelfrågor.  

    ![HDInsight interaktiva fråga zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Klicka på **Skapa ny anteckning**.
4. Ange eller välj följande värden:

    - Namn: Ange ett namn för anteckningen.
    - Standard-tolken: Välj **JDBC**.

5. Klicka på **skapa anteckning**.
6. Kör följande Hive-fråga:

        %jdbc(hive)
        show tables

    ![HDInsight interaktiva zeppelin Kör fråga](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    Den **%jdbc(hive)** instruktion i den första raden visar den bärbara datorn att använda Hive JDBC-tolken.

    Frågan kan returnera en Hive-tabell som kallas *hivesampletable*.

    Följande är två mer Hive-frågor som du kan köra mot hivesampletable. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Jämföra den traditionella Hive, kommer resultatet av frågan tillbaka måste snabbare.


## <a name="next-steps"></a>Nästa steg
I den här artikeln beskrivs hur du visualisera data från HDInsight med hjälp av Power BI.  Mer information finns i följande artiklar:

* [Visualisera Hive-data med Microsoft Power BI i Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Visualisera interaktiva frågan Hive-data med Power BI i Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Anslut Excel till HDInsight med Microsoft Hive ODBC-drivrutinen](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Ansluta Excel till Hadoop med Power Query](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Ansluta till Azure HDInsight och köra Hive-frågor med Data Lake-verktyg för Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Använda Azure HDInsight-verktyg för Visual Studio Code](hdinsight-for-vscode.md).
* [Överföra Data till HDInsight](./hdinsight-upload-data.md).
