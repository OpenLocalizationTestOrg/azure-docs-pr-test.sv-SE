---
title: "aaaIntroduction tooR Server på Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse R Server på HDInsight toocreate program för stordataanalys."
services: hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: daf7b70a15748d66510a04da370f39c5f26eb4ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="introduction-toor-server-and-open-source-r-capabilities-on-hdinsight"></a>Introduktion tooR Server och öppen källkod R-funktioner på HDInsight

Microsoft R Server är tillgänglig som ett distributionsalternativ när du skapar HDInsight-kluster i Azure. Den här nya funktionen ger dataanalytiker, statistiker och R programmerare med åtkomst på begäran tooscalable distribuerade metoder för analyser på HDInsight.

Kluster kan vara korrekt storlek toohello projekt och uppgifter till hands och datakanalen när de inte längre behövs. Eftersom de är en del av Azure HDInsight levereras med stöd för på företagsnivå 24/7, ett SLA för 99,9% drifttid klustren och hello möjlighet toointegrate med andra komponenter i hello Azure-ekosystemet.

R Server på HDInsight ger hello senaste funktionerna för analys av R-baserade på datauppsättningar av valfri storlek inlästa tooeither Azure Blob eller Data Lake lagringsutrymme. Eftersom R Server bygger på öppen källkod R kan hello R-baserade program som du skapar utnyttja hello 8000 + öppen källkod R-paket. Det finns också hello-rutiner i ScaleR, Microsofts big analytics datapaketet medföljer R Server.

hello edge nod i ett kluster ger en behändig plats tooconnect toohello klustret och toorun R-skript. Med en kantnod har hello möjlighet att köra hello paralleliserad distribuerade funktioner ScaleR över hello kärnor av hello gränsservern nod. Du kan också köra dem över hello klusternoder hello med hjälp av Scaler's Hadoop kartan minska eller Spark beräkning sammanhang.

använda lokala hello modeller eller förutsägelser som kan hämta resultat från analyser för. De kan också vara operationalized någon annanstans i Azure, i synnerhet via [Azure Machine Learning Studio](http://studio.azureml.net) [webbtjänsten](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-on-hdinsight"></a>Kom igång med R i HDInsight
tooinclude R Server i ett HDInsight-kluster måste du välja hello R Server typ av kluster när du skapar ett HDInsight-kluster med hjälp av hello Azure-portalen. hello typ av R Server-kluster innehåller R Server på hello data klusternoder hello och en kantnod som fungerar som en stannplan för R Server-baserade analytics. Se [komma igång med R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md) en genomgång av hur toocreate hello klustret.

## <a name="learn-about-data-storage-options"></a>Lär dig mer om alternativen för datalagring
Standardlagring för hello HDFS filsystemet på HDInsight-kluster kan associeras med ett Azure Storage-konto eller ett Azure Data Lake store. Den här associationen säkerställer att data överförs toohello klusterlagringen under analysen görs permanent. Det finns flera olika verktyg för hantering av hello data transfer toohello lagringsalternativ du väljer, inklusive hello portal-baserade överför användandet av hello storage-konto och hello [AzCopy](../storage/common/storage-use-azcopy.md) verktyget.

Du har hello möjlighet att lägga till access tooadditional Blob och Data lake lagras under hello klustret etableringsprocessen oavsett hello primära lagringsalternativ används. Se [komma igång med R Server på HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) information om att lägga till konton för tooadditional. Se hello kompletterande [Azure lagringsalternativ för R Server på HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artikel toolearn mer om hur du använder flera lagringskonton.

Du kan också använda [Azure Files](../storage/files/storage-how-to-use-files-linux.md) som ett lagringsalternativ för användning på hello kantnod. Azure Files kan toomount en filresurs som skapats i Azure Storage toohello Linux-filsystem. Mer information om de här alternativen för datalagring för R Server på HDInsight-kluster finns [Azure Storage-alternativen för R Server på HDInsight-kluster](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-hello-cluster"></a>Åtkomst R Server på hello klustret
Du kan ansluta tooR Server på hello kantnod med en webbläsare som du har valt tooinclude RStudio Server under hello etableringsprocessen. Om du inte har installerat den vid etablering av hello kluster, kan du lägga till den senare. Information om hur du installerar RStudio Server när du har skapat ett kluster finns i [RStudio installera Server på HDInsight-kluster](hdinsight-hadoop-r-server-install-r-studio.md). Du kan också ansluta toohello R Server med hjälp av SSH/PuTTY tooaccess hello R-konsolen. 

## <a name="develop-and-run-r-scripts"></a>Utveckla och köra R-skript
du skapar och kör hello R-skript kan använda någon av hello 8000 + öppen källkod R-paket i tillägg toohello paralleliserad och distribuerad rutiner som är tillgängliga i hello ScaleR-biblioteket. I allmänhet körs ett skript som körs med R Server på hello kantnod inom hello R-tolken på noden. hello undantag är de steg som måste toocall en ScaleR funktion med en kontext för beräkning som anges tooHadoop kartan minska (RxHadoopMR) eller Spark (RxSpark). I det här fallet körs Hej funktionen på ett sätt som är distribuerade över dessa data (aktivitet) klusternoder hello som är associerade med hello data refererar till. Mer information om alternativ för hello olika beräkning kontexten finns [Compute-kontexten alternativ för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operationalisera en modell
När dina datamodellering är klar, kan du operationalisera hello modellen toomake förutsägelser för nya data från Azure och lokalt. Den här processen kallas bedömningen. Bedömningen kan göras i HDInsight, Azure Machine Learning eller lokalt.

### <a name="score-in-hdinsight"></a>Poäng i HDInsight
tooscore i HDInsight, skriva ett R-funktion som anropar din modell toomake förutsägelser för en ny datafil som du har läst in tooyour storage-konto. Spara hello förutsägelser tillbaka toohello storage-konto. Du kan köra hello rutinunderhåll på begäran på hello kantnod på klustret eller med hjälp av ett schemalagt jobb.  

### <a name="score-in-azure-machine-learning-aml"></a>Poäng i Azure Maskininlärning (AML)
tooscore med hjälp av en AML webbtjänst, Använd hello Öppna källa Azure Machine Learning R-paket kallas [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) toopublish din modell som Azure-webbtjänst. Det här paketet har förinstallerats hello kantnod för enkelhetens skull. Sedan använder hello i Machine Learning toocreate ett användargränssnitt för hello-webbtjänsten och sedan anropa hello-webbtjänst som krävs för resultatfunktioner.

Om du väljer det här alternativet måste tooconvert alla ScaleR modellen tooequivalent öppen källkod modellobjekt för använder hello-webbtjänsten. Använda ScaleR tvång funktioner, till exempel `as.randomForest()` för ensemble-baserade modeller för den här konverteringen.

### <a name="score-on-premises"></a>Poäng på lokalt
tooscore lokalt när du har skapat din modell, du serialisera hello modell i R, hämta, deserialisera den och sedan använda datorn för resultatfunktioner nya data. Du kan samla in nya data med hjälp av hello-metod som beskrivs tidigare i [Resultatfunktioner i HDInsight](#scoring-in-hdinsight) eller genom att använda [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-hello-cluster"></a>Underhåll hello kluster
### <a name="install-and-maintain-r-packages"></a>Installera och underhålla R-paket
De flesta av hello R-paket som du använder krävs på hello kantnod eftersom de flesta stegen för din R-skript körs det. tooinstall ytterligare R paket på hello kantnod som du kan använda hello vanliga `install.packages()` metod i R.

Om du bara använder rutiner från hello ScaleR bibliotek över hello kluster kan behöver du vanligtvis inte tooinstall ytterligare R-paket på hello datanoder. Du kan dock behöva ytterligare paket toosupport hello användning av **rxExec** eller **RxDataStep** körning på hello datanoder.

I dessa fall kan ytterligare hello-paket installeras med en skriptåtgärd när du har skapat hello klustret. Mer information finns i [skapar ett HDInsight-kluster med R Server](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Ändra inställningarna för Hadoop kartan minska minne
Ett kluster kan vara ändrade toochange hello mängden minne som är tillgängliga tooR Server när den körs en karta minska jobb. toomodify ett kluster, använda hello Apache Ambari-användargränssnitt som är tillgänglig via hello Azure portal, blad för klustret. Instruktioner om hur tooaccess hello Ambari UI för klustret, se [hantera HDInsight-kluster med Ambari-Webbgränssnittet för hello](hdinsight-hadoop-manage-ambari.md).

Det är också möjligt toochange hello mängden minne som är tillgängliga tooR Server med hjälp av Hadoop-växlar i hello anrop för**RxHadoopMR** på följande sätt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Skala ditt kluster
Ett befintligt kluster kan skalas upp eller ned hello-portalen. Genom att skala, får du ytterligare hello-kapacitet som kan behövas för större bearbetningsuppgifter eller du kan skala tillbaka ett kluster när den är inaktiv. Anvisningar om hur tooscale ett kluster, se [hantera HDInsight-kluster](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-hello-system"></a>Underhåll hello system
Underhåll tooapply operativsystemskorrigeringar och andra uppdateringar utförs på hello underliggande virtuella Linux-datorer i ett HDInsight-kluster kontorstid. Normalt görs Underhåll på 3:30 AM (baserat på hello lokal tid för hello VM) varje måndag och torsdag. Uppdateringar utförs så att de inte påverkar mer än en kvartalet hello kluster i taget.  

Eftersom hello huvudnoderna är redundant och påverkas inte alla datanoder, kan alla jobb som körs under den här tiden långsammare. De bör fortfarande köra toocompletion, men. Anpassade programvara eller lokala data som du har bevaras över händelserna underhåll om ett oåterkalleligt fel inträffar som kräver att klustret.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Lär dig mer om IDE-alternativ för R Server på ett HDInsight-kluster
hello Linux kantnod för ett HDInsight-kluster är hello hamnar zon för R-baserade analys. Nya versioner av HDInsight tillhandahåller en standardalternativet för installation hello community-versionen av [RStudio Server](https://www.rstudio.com/products/rstudio-server/) på hello kantnod som en webbläsarbaserad IDE. Användning av RStudio Server som IDE-miljö för utveckling av hello och körning av R-skript kan vara betydligt mer produktiv än att bara använda hello R-konsolen. Om du valde inte tooadd RStudio Server skapar hello klustret men vill tooadd den senare sedan se [installera R Studio Server på HDInsight-kluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-install-r-studio). +

Ett annat alternativ för fullständig IDE är tooinstall skrivbord IDE och använda det tooaccess hello kluster med hjälp av en remote kartan minska eller Spark beräknings-kontext. Alternativen är Microsofts [R Tools för Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS) RStudio och Walware datorns Eclipse-baserade [StatET](http://www.walware.de/goto/statet).

Slutligen kan du öppna hello R Server konsolen på hello kantnod genom att skriva **R** Kommandotolken hello Linux efter anslutning via SSH eller PuTY. När gränssnittet hello-konsolen är det praktiskt toorun en textredigerare för utveckling av R-skript i ett annat fönster och klipp ut och klistra in avsnitt av skriptet i hello R-konsolen efter behov.

## <a name="learn-about-pricing"></a>Läs mer om prissättningen
hello strukturerade avgifter som är associerade med ett HDInsight-kluster med R Server är på samma sätt toohello avgifter för hello standard HDInsight-kluster. De är baserade på hello storleksändring av hello underliggande virtuella datorer över hello namn, data och edge noder med hello lägga till en timme core-höjning. Mer information om priser för HDInsight och hello tillgängligheten för en kostnadsfri 30-dagars utvärderingsversion finns [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Nästa steg
toolearn mer information om hur toouse R Server med HDInsight-kluster, se hello följande avsnitt:

* [Komma igång med R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md)
* [Lägg till RStudio Server tooHDInsight (om inte installerad när klustret skapas)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Alternativ för beräkningskontexter för R Server på HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Alternativ för Azure Storage för R Server på HDInsight](hdinsight-hadoop-r-server-storage.md)
