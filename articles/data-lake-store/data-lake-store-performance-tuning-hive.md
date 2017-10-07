---
title: aaaAzure Data Lake Store Hive prestanda justera riktlinjer | Microsoft Docs
description: Azure Data Lake Store Hive prestandajustering riktlinjer
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e44daeb6ad3b64e893c709df63b56444a330729f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Prestandajustering för Hive i HDInsight och Azure Data Lake Store

hello standardinställningarna har ställts in tooprovide goda prestanda i många olika användningsfall.  För i/o-intensiva frågor vara Hive ögonen öppna tooget bättre prestanda med ADLS.  

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.
* **Kör Hive i HDInsight**.  toolearn om hur du kör Hive-jobb i HDInsight, se [använda Hive i HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)
* **Prestandajustering riktlinjer för ADLS**.  Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametrar

Här följer hello viktigaste inställningar tootune för bättre prestanda för ADLS:

* **hive.tez.container.size** – hello mängden minne som används av varje uppgifter

* **tez.Grouping.min storlek** – minsta storlek varje mapper

* **tez.Grouping.Max storlek** – maximal storlek för varje mapper

* **hive.Exec.Reducer.bytes.per.Reducer** – storlek varje reducer

**hive.tez.container.size** -hello behållarens storlek avgör hur mycket minne som är tillgänglig för varje aktivitet.  Detta är hello huvudsakliga indata för styra hello samtidighet i Hive.  

**tez.Grouping.min storlek** – den här parametern kan tooset hello minimala storleken för varje mappning.  Om hello antalet mappers som Tez väljer är mindre än hello värdet på parametern använder Tez hello-värdet som anges här.  

**tez.Grouping.Max storlek** – hello-parametern kan du tooset hello maximal storlek för varje mappning.  Om hello antalet mappers som Tez väljer är större än hello värdet på parametern använder Tez hello-värdet som anges här.  

**hive.Exec.Reducer.bytes.per.Reducer** – den här parametern anger hello storleken för varje reducer.  Som standard är varje reducer 256MB.  

## <a name="guidance"></a>Riktlinjer

**Ange hive.exec.reducer.bytes.per.reducer** – hello standardvärdet fungerar bra när hello data är okomprimerade.  För data som komprimerats, bör du minska hello reducer hello storlek.  

**Ange hive.tez.container.size** – på varje nod minne har angetts av yarn.nodemanager.resource.memory mb och korrekt ska anges på HDI-klustret som standard.  Mer information om hur hello lämpliga minne i YARN finns [efter](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

I/o-intensiva arbetsbelastningar kan utnyttja mer parallellitet genom att minska hello Tez behållarens storlek. Detta ger hello användaren fler behållare, vilket ökar samtidighet.  Vissa Hive-frågor kräver dock en stor mängd minne (t.ex. MapJoin).  Om hello aktiviteten inte har tillräckligt med minne, får du en out-of minne undantag under körningen.  Om du får slut på minne undantag, bör du öka hello minne.   

hello antalet samtidiga aktiviteter som körs eller parallellitet kommer att begränsas av hello totalt YARN-minne.  hello antal YARN behållare styr hur många samtidiga uppgifter kan köras.  Du kan gå tooAmbari toofind hello YARN minne per nod.  Navigera tooYARN och visa hello konfigurationerna fliken.  Hej YARN minne visas i det här fönstret.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
hello viktiga tooimproving prestanda med hjälp av ADLS är tooincrease hello samtidighet så mycket som möjligt.  Tez beräknar automatiskt hello antalet uppgifter som ska skapas, så du inte behöver tooset den.   

## <a name="example-calculation"></a>Exempel beräkning

Anta att du har en 8 noder D14 kluster.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Begränsningar
**ADLS-begränsning** 

UIf du träffar hello begränsar bandbredd under förutsättning av ADLS, börjar du toosee aktivitet fel. Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.  Du kan minska hello parallellitet genom att öka storleken på Tez-behållare.  Om du behöver mer samtidighet för jobbet, kontaktar du oss.   

toocheck om du har komma begränsats måste tooenable hello felsökningsloggning på hello på klientsidan. Här visas hur du kan göra det:

1. Placera hello efter egenskap i hello log4j egenskaper i Hive-config. Detta kan göras från Ambari vyn: log4j.logger.com.microsoft.azure.datalake.store=DEBUG starta om alla hello noder/för hello config tootake effekt.

2. Om du har komma begränsats visas hello HTTP 429 felkod i loggfilen för hello hive. hello hive-loggfilen finns i /tmp/&lt;användare&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Mer information om Hive-inställning

Här följer några bloggar som hjälper dig att finjustera Hive-frågor:
* [Optimera Hive-frågor för Hadoop i HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Felsökning av Hive frågeprestanda](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Ignite prata på optimera Hive i HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
