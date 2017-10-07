---
title: aaaAnalyze sensordata med Hive och Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur hello tooanalyze sensordata med Hive-fråga konsol med HDInsight (Hadoop) och visualisera hello data i Microsoft Excel med PowerView."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 70e595705c33d9835dc9809161f79c3ac5ece870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-using-hello-hive-query-console-on-hadoop-in-hdinsight"></a>Analysera sensordata med hello Hive-fråga konsolen på Hadoop i HDInsight

Lär dig hur hello tooanalyze sensordata med Hive-fråga konsol med HDInsight (Hadoop) och visualisera hello data i Microsoft Excel med hjälp av Power View.

> [!IMPORTANT]
> hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).


I det här exemplet använder Hive tooprocess historiska data och identifiera problem med uppvärmning och luftkonditionering system. Mer specifikt kan du identifiera datorer är inte tooreliably bibehålla en inställd temperatur genom att utföra följande uppgifter hello:

* Skapa HIVE-tabeller tooquery data som lagras i en fil med kommaavgränsade värden (CSV) filer.
* Skapa HIVE-frågor tooanalyze hello data.
* tooretrieve hello analyserade data använda Microsoft Excel tooconnect tooHDInsight.
* toovisualize hello data använda Power View.

![Ett diagram över hello lösningsarkitektur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Krav

* Ett kluster i HDInsight (Hadoop): se [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md) information om hur du skapar ett kluster.
* Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel används för datavisualisering med [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC-drivrutinen](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="toorun-hello-sample"></a>toorun hello-exempel

1. Från din webbläsare, navigerar du toohello följande URL: 

         https://<clustername>.azurehdinsight.net

    Ersätt `<clustername>` med hello namnet på ditt HDInsight-kluster.

    När du uppmanas, autentisera med Hej administratörsanvändarnamn och lösenord som du använde vid etablering av det här klustret.

2. Från hello webbsida som öppnas, klickar du på hello **komma igång-galleriet** fliken och sedan under hello **lösningar med exempeldata** kategori, klicka på hello **Sensor dataanalys** exempel.

    ![Komma igång galleriet bild](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Följ instruktionerna på hello webbsida toofinish hello sampel hello.
