---
title: "HDInsight Hadoop datavetenskap genomgång med Hive i Azure | Microsoft Docs"
description: "Exempel på Team av vetenskapliga data som beskriver genom att använda Hive på Azure HDInsight Hadoop att göra förutsägelseanalyser."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev
ms.openlocfilehash: fb06c3c1b1ae30d970a2e4d45a49e22e9d78b876
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="hdinsight-hadoop-data-science-walkthroughs-using-hive-on-azure"></a>HDInsight Hadoop datavetenskap genomgång med Hive på Azure 

Dessa genomgång använda Hive med HDInsight Hadoop-kluster för att göra förutsägelseanalyser. De följer stegen som beskrivs i Team av vetenskapliga data. En översikt över Team av vetenskapliga data, se [datavetenskap Process](data-science-process-overview.md). En introduktion till Azure HDInsight, se [introduktion till Azure HDInsight, Hadoop-teknikstacken och Hadoop-kluster](../hdinsight/hdinsight-hadoop-introduction.md).

Ytterligare datavetenskap genomgångar som kör Team datavetenskap Process grupperas efter den **plattform** som de använder: 

[!INCLUDE [tdsp-walkthroughs-by-platform](../../includes/tdsp-walkthroughs-by-platform.md)]


## <a name="predict-taxi-tips-using-hive-with-hdinsight-hadoop"></a>Förutsäga taxi tips med Hive med HDInsight Hadoop

Den [Använd HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md) genomgången använder data från New York taxibilar för att förutsäga: 

- Om ett tips är betald 
- Distribution av tips belopp

Scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/). Du lär dig hur du lagrar, utforska och funktionen tekniker data från en offentligt tillgängliga NYC Taxitransport resa och avgiften dataset. Du kan också använda Azure Machine Learning för att skapa och distribuera modeller.

## <a name="predict-advertisement-clicks-using-hive-with-hdinsight-hadoop"></a>Förutsäga annons klick med Hive med HDInsight Hadoop

Den [Använd Azure HDInsight Hadoop-kluster på en datamängd för 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) genomgången använder offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) klickar du på dataset för att förutsäga om ett tips är betald och intervallet för belopp som förväntat. Scenariot implementeras med hjälp av Hive med en [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) för att lagra, utforska funktion tekniker och ned exempeldata. Azure Machine Learning används för att skapa, träna och betygsätta en binär klassificering modell förutsäga om en användare klickar på en annons. Avslutar den här genomgången visar hur du publicerar en av dessa modeller som en webbtjänst.


## <a name="next-steps"></a>Nästa steg

En beskrivning av de viktiga komponenter som utgör teamet datavetenskap Process finns [Team datavetenskap processöversikt](data-science-process-overview.md).

En beskrivning av livscykeln Team av vetenskapliga data som du kan använda för att strukturera datavetenskap projekt finns [Team datavetenskap Process livscykel](data-science-process-lifecycle.md). Livscykeln beskrivs stegen från början till slut att projekt vanligtvis följer när de utförs. 

