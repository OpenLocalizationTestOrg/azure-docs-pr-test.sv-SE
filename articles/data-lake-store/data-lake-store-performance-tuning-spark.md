---
title: aaaAzure Data Lake Store Spark prestanda justera riktlinjer | Microsoft Docs
description: Azure Data Lake Store Spark prestandajustering riktlinjer
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
ms.openlocfilehash: da1d172e9cb1199ad95605ea1718e78559f79650
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-spark-on-hdinsight-and-azure-data-lake-store"></a>Prestandajustering för Spark i HDInsight och Azure Data Lake Store

När du justera prestanda på Spark måste tooconsider hello antal appar som körs på klustret.  Som standard kan du köra 4 appar samtidigt på HDI-klustret (Obs: hello standardinställningen är ämne toochange).  Du kan bestämma toouse färre appar så att du kan åsidosätta hello standardinställningar och använder flera hello kluster för dessa appar.  

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto. Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.
* **Köra Spark-kluster på Azure Data Lake Store**.  Mer information finns i [använda HDInsight Spark-kluster tooanalyze data i Data Lake Store](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-use-with-data-lake-store)
* **Prestandajustering riktlinjer för ADLS**.  Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance) 

## <a name="parameters"></a>Parametrar

När du kör Spark jobb, är här de viktigaste hello-inställningar som kan vara ögonen öppna tooincrease prestanda på ADLS:

* **NUM executors** -hello antalet samtidiga uppgifter som kan utföras.

* **Utföraren minne** -hello mängden minne som allokerats tooeach utförare.

* **Utföraren kärnor** -hello antal kärnor allokerade tooeach utförare.                     

**NUM executors** Num executors anger hello högsta antalet uppgifter som kan köras parallellt.  hello faktiska antalet uppgifter som kan köras parallellt är bundet hello minne och processorresurser som är tillgängliga i klustret.

**Utföraren minne** det här är hello mängden minne som allokerats tooeach utförare.  hello-minne som behövs för varje utföraren är beroende av hello jobb.  För komplexa åtgärder måste hello minne toobe högre.  För enkla åtgärder som att läsa och skriva vara minneskrav lägre.  Hej mängden minne för varje utförare kan visas i Ambari.  Navigera tooSpark i Ambari, och visa hello konfigurationerna fliken.  

**Utföraren kärnor** anger hello mängden kärnor används per utföraren som bestämmer hello antalet parallella trådar som kan köra per utförare.  Till exempel om utföraren kärnor = 2, sedan varje utförare kan köra 2 parallella aktiviteter i hello utförare.  hello utföraren-kärnor behövs är beroende av hello jobb.  I/o tunga jobb kräver inte en stor mängd minne per aktivitet så att varje utförare kan hantera flera parallella aktiviteter.

Som standard definieras två virtuella YARN kärnor för varje fysiska kärnor när du kör Spark på HDInsight.  Det här antalet ger en bra balans mellan concurrecy och mängden kontexten växlar från flera trådar.  

## <a name="guidance"></a>Riktlinjer

När du kör Spark analytiska arbetsbelastningar toowork med data i Data Lake Store, rekommenderar vi att du använder hello senaste HDInsight version tooget hello bästa prestanda med Data Lake Store. När jobbet är flera i/o-intensiva, kan vissa parametrar vara konfigurerade tooimprove prestanda.  Azure Data Lake Store är en mycket skalbar lagring-plattform som kan hantera högt genomflöde.  Om hello jobbet i huvudsak av läsning eller skrivning, kan öka samtidighet för i/o-tooand från Azure Data Lake Store öka prestanda.

Det finns några allmänna metoder för tooincrease samtidighet för i/o-intensiva jobb.

**Steg 1: Ta reda på hur många appar använder om klustret** – du bör känna till hur många appar körs på hello klustret, inklusive hello aktuella.  hello standardvärden för varje Spark inställningen förutsätter att det finns 4 appar som körs samtidigt.  Därför ska du bara ha 25% av hello-kluster som är tillgängliga för varje app.  tooget bättre prestanda, kan du åsidosätta hello standardvärdena genom att ändra hello antalet executors.  

**Steg 2: Ange utföraren minne** – hello först öppna tooset är hello utföraren-minne.  hello minne är beroende av hello jobbet att du är pågående toorun.  Du kan öka samtidighet genom att allokera mindre minne per utförare.  Om du ser slut på minne undantag när du kör jobbet, bör du öka hello värde för denna parameter.  Ett alternativ är tooget mer minne genom att använda ett kluster som har högre mängder minne eller öka hello storleken på ditt kluster.  Mer minne kan flera executors toobe används, vilket innebär att flera samtidiga.

**Steg 3: Ange utföraren kärnor** – för i/o-intensiv arbetsbelastning som inte har komplex, är det bra toostart med ett stort antal utföraren kärnor tooincrease hello antalet parallella aktiviteter per utförare.  Ange utförare kärnor too4 är en bra start.   

    executor-cores = 4
Öka hello antal kärnor utföraren får du mer parallellitet så att du kan experimentera med olika utföraren-kärnor.  För jobb som har mer komplicerade åtgärder, bör du minska hello antalet kärnor per utförare.  Om utföraren kärnor är kan högre än 4, sedan skräpinsamling vara ineffektivt och försämra prestanda.

**Steg 4: Fastställ minnesmängden YARN i klustret** – den här informationen är tillgänglig i Ambari.  Navigera tooYARN och visa hello konfigurationerna fliken.  Hej YARN minne visas i det här fönstret.  
Obs: när du arbetar med hello fönstret kan du också se hello standardstorlek YARN behållare.  Hej YARN behållarens storlek är hello samma som minne per utföraren parameter.

    Total YARN memory = nodes * YARN memory per node
**Steg 5: Beräkna num executors**

**Beräkna minne begränsningen** -hello num executors parameter är begränsad genom minne eller CPU.  hello minne begränsningen bestäms av hello mängden tillgängligt minne i YARN för ditt program.  Du bör ta totalt YARN-minne och delar av utföraren minne.  hello begränsningen måste toobe Frigör skalas för hello antal appar så att vi delar med hello antalet appar.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
**Beräkna CPU-begränsningen** -hello CPU-begränsningen beräknas som hello totala virtuella kärnor dividerat med hello antalet kärnor per utförare.  Det finns 2 virtuella kärnor för varje fysiska kärnor.  Liknande toohello minne begränsning, har vi division av hello antal appar.

    virtual cores = (nodes in cluster * # of physical cores in node * 2)
    CPU constraint = (total virtual cores / # of cores per executor) / # of apps
**Ange num executors** – hello num executors parametern bestäms genom att göra hello minst hello minne och hello CPU-begränsningen. 

    num-executors = Min (total virtual Cores / # of cores per executor, available YARN memory / executor-memory)   
Anger ett högre antal num executors öka inte nödvändigtvis prestanda.  Du bör överväga att lägga till flera executors lägger till extra administration för varje ytterligare utföraren som potentiellt kan försämra prestanda.  NUM executors är bundet hello klusterresurser.    

## <a name="example-calculation"></a>Exempel beräkning

Anta att du har ett kluster som består av 8 D4v2 noder som är igång 2 appar inklusive hello som du ska toorun.  

**Steg 1: Ta reda på hur många appar använder om klustret** – du vet att du har 2 appar på klustret, inklusive hello något du ska toorun.  

**Steg 2: Ange utföraren minne** – i det här exemplet bestämma att 6 GB minne utföraren räcka för i/o-intensiva jobbet.  

    executor-memory = 6GB
**Steg 3: Ange utföraren kärnor** – eftersom det är ett i/o-intensiva jobb vi kan ange hello antalet kärnor för varje utföraren too4.  Ange kärnor per utföraren toolarger än 4 kan orsaka skräp samling problem.  

    executor-cores = 4
**Steg 4: Fastställ minnesmängden YARN i klustret** – vi gå tooAmbari toofind ut att varje D4v2 har 25 GB YARN minne.  Eftersom det finns 8 noder, multipliceras hello YARN-minne med 8.

    Total YARN memory = nodes * YARN memory* per node
    Total YARN memory = 8 nodes * 25GB = 200GB
**Steg 5: Beräkna num executors** – hello num executors parametern bestäms genom att göra hello minst hello minne och CPU-begränsningen för hello dividerat med hello antal appar som körs på Spark.    

**Beräkna minne begränsningen** – hello minne begränsningen beräknas som hello totalt YARN-minne dividerat med hello minne per utförare.

    Memory constraint = (total YARN memory / executor memory) / # of apps   
    Memory constraint = (200GB / 6GB) / 2   
    Memory constraint = 16 (rounded)
**Beräkna CPU-begränsningen** -hello CPU-begränsningen beräknas som hello totala yarn kärnor dividerat med hello antalet kärnor per utförare.
    
    YARN cores = nodes in cluster * # of cores per node * 2   
    YARN cores = 8 nodes * 8 cores per D14 * 2 = 128
    CPU constraint = (total YARN cores / # of cores per executor) / # of apps
    CPU constraint = (128 / 4) / 2
    CPU constraint = 16
**Ange num-executors**

    num-executors = Min (memory constraint, CPU constraint)
    num-executors = Min (16, 16)
    num-executors = 16    

