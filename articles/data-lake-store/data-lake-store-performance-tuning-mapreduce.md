---
title: aaaAzure Data Lake Store MapReduce prestanda justera riktlinjer | Microsoft Docs
description: Azure Data Lake Store MapReduce prestandajustering riktlinjer
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
ms.openlocfilehash: e21414a23530e65613c85156af0209c88ec54d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Prestandajustering för MapReduce på HDInsight och Azure Data Lake Store


## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.
* **Använda MapReduce på HDInsight**.  Mer information finns i [använda MapReduce i Hadoop i HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)  
* **Prestandajustering riktlinjer för ADLS**.  Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="parameters"></a>Parametrar

När du kör MapReduce-jobb är här hello viktigaste parametrarna som du kan konfigurera tooincrease prestanda på ADLS:

* **Mapreduce.Map.Memory.MB** – hello mängden minne tooallocate tooeach mapper
* **Mapreduce.Job.Maps** – hello antal kartan uppgifter per jobb
* **Mapreduce.Reduce.Memory.MB** – hello mängden minne tooallocate tooeach reducer
* **Mapreduce.Job.reduces** – hello antal minska uppgifter per jobb

**Mapreduce.Map.Memory / Mapreduce.reduce.memory** siffran ska justeras baserat på hur mycket minne som krävs för hello kartan och/eller minska aktivitet.  hello standardvärdena för mapreduce.map.memory och mapreduce.reduce.memory kan visas i Ambari via hello Yarn konfiguration.  Navigera tooYARN i Ambari, och visa hello konfigurationerna fliken.  Hej YARN minne visas.  

**Mapreduce.Job.Maps / Mapreduce.job.reduces** det avgör hello maximalt antal mappers eller förminskningsapparater toobe skapas.  hello antal delningar avgör hur många mappers kommer att skapas för hello MapReduce-jobb.  Därför kan du få mindre mappers du begärt om det finns mindre delningar än hello antalet mappers som begärdes.       

## <a name="guidance"></a>Riktlinjer

**Steg 1: Bestäm antalet jobb som körs** -standard MapReduce använder hello hela klustret på jobbet.  Du kan använda mindre hello kluster med mindre mappers än det finns tillgängliga behållare.  hello riktlinjerna i det här dokumentet förutsätter att programmet hello endast program som körs på klustret.      

**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – hello storlek hello minne för kartan och minska uppgifter är beroende av ditt specifika jobb.  Du kan minska hello minnesstorleken om du vill tooincrease samtidighet.  hello antalet aktiviteter som körs samtidigt beror på hello antal behållare.  Genom att minska hello mängden minne per mapper eller reducer kan fler behållare skapas, som gör det möjligt mer mappers eller förminskningsapparater toorun samtidigt.  Minskar hello mängden minne kan för mycket orsaka vissa processer toorun slut på minne.  Om du får en heap-fel när du kör jobbet, bör du öka hello minne per mapper eller reducer.  Du bör överväga att lägga till fler behållare lägger till extra administration för varje ytterligare behållare som potentiellt kan försämra prestanda.  Ett annat alternativ är tooget mer minne genom att använda ett kluster som har högre mängder minne eller öka hello antalet noder i klustret.  Mer minne kan flera behållare toobe används, vilket innebär att flera samtidiga.  

**Steg 3: Bestäm minne totalt YARN** -tootune mapreduce.job.maps/mapreduce.job.reduces bör du hello minnesmängden totala YARN tillgängliga för användning.  Den här informationen är tillgänglig i Ambari.  Navigera tooYARN och visa hello konfigurationerna fliken.  Hej YARN minne visas i det här fönstret.  Du bör multiplicera hello YARN minne med hello antalet noder i klustret tooget hello totala YARN minnet.

    Total YARN memory = nodes * YARN memory per node
Om du använder ett tomt kluster kan minne vara hello totalt YARN minne för klustret.  Om andra program använder minne, kan du välja tooonly används en del av ditt kluster minne genom att minska hello antalet mappers eller förminskningsapparater toohello behållare vill du toouse.  

**Steg 4: Beräkna antalet YARN behållare** – YARN-behållare kräver hello mängden samtidighet för hello jobb.  Ta totalt YARN-minne och delar av mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces** ange mapreduce.job.maps/mapreduce.job.reduces tooat minst hello antalet tillgängliga behållare.  Du kan experimentera ytterligare genom att öka hello antalet mappers och förminskningsapparater toosee om du får bättre prestanda.  Tänk på att flera mappers har ytterligare kostnader så har för många mappers kan prestanda försämras.  

CPU-schemaläggning och CPU-isolering är inaktiverade som standard så hello antal YARN-behållare är begränsad av minne.

## <a name="example-calculation"></a>Exempel beräkning

Anta att du har ett kluster som består av 8 D14 noder och du vill toorun ett i/o-intensiva jobb.  Här följer hello beräkningar som du bör göra:

**Steg 1: Bestäm antalet jobb som körs** -i vårt exempel antar vi att våra jobbet är hello endast en körs.  

**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – i vårt exempel du kör ett i/o-intensiva jobb och bestämmer 3 GB minne för kartan uppgifter är tillräckligt.

    mapreduce.map.memory = 3GB
**Steg 3: Bestäm totala YARN-minne**

    total memory from hello cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Steg 4: Beräkna antal YARN-behållare**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Begränsningar

**ADLS-begränsning**

Som en tjänst med flera innehavare anger ADLS gränser för nivån bandbredd.  Om du träffar dessa gränser, startar toosee aktivitet fel. Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.  Om du behöver mer bandbredd för jobbet, kontaktar du oss.   

toocheck om du har komma begränsats måste tooenable hello felsökningsloggning på hello på klientsidan. Här visas hur du kan göra det:

1. Placera hello följande egenskap i hello log4j egenskaper i Ambari > YARN > Config > avancerade yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Starta om alla hello noder/för hello config tootake effekt.

3. Om du har komma begränsats visas hello HTTP 429 felkod i hello YARN-loggfilen. Hej YARN loggfilen finns i /tmp/&lt;användare&gt;/yarn.log

## <a name="examples-toorun"></a>Exempel tooRun

toodemonstrate visas hur MapReduce körs på Azure Data Lake Store nedan några exempelkod som körs på ett kluster med hello följande inställningar:

* 16 noden D14v2
* Hadoop-kluster som kör HDI 3,6

För en startpunkt är här några exempel kommandon toorun MapReduce Teragen Terasort och Teravalidate.  Du kan justera de här kommandona baserat på dina resurser.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
