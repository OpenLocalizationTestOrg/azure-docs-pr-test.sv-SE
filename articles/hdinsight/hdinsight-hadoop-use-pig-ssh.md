---
title: "aaaUse Hadoop Pig med SSH på ett HDInsight-kluster - Azure | Microsoft Docs"
description: "Lär dig hur ansluter tooa Linux-baserade Hadoop-kluster med SSH och sedan använda hello Pig kommandot toorun Pig Latin instruktioner interaktivt eller som en batch jobbet."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a>Köra Pig-jobb på en Linux-baserade kluster med hello Pig-kommandot (SSH)

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Lär dig hur toointeractively köra Pig-jobb från en SSH-anslutning tooyour HDInsight-kluster. Hej Pig Latin programmeringsspråk kan toodescribe transformationer som är kopplade toohello inkommande data tooproduce hello önskade resultatet.

> [!IMPORTANT]
> hello kräver stegen i det här dokumentet ett Linux-baserade HDInsight-kluster. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Ansluta med SSH

Använda SSH tooconnect tooyour HDInsight-kluster. hello följande exempel ansluter tooa kluster med namnet **myhdinsight** som hello-kontot med namnet **sshuser**:

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

**Om du har angett en certifikat-nyckel för SSH-autentisering** när du har skapat hello HDInsight-kluster kan du kanske måste toospecify hello platsen för hello privata nyckel på klientsystemet.

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Om du har angett ett lösenord för SSH-autentisering** när du skapade hello HDInsight-kluster, ange hello lösenord när du uppmanas.

Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Använd hello Pig kommando

1. När du är ansluten, startar du hello Pig-kommandoradsgränssnittet (CLI) med hjälp av hello följande kommando:

        pig

    Efter en stund bör du se en `grunt>` prompt.

2. Ange hello följande instruktion:

        LOGS = LOAD '/example/data/sample.log';

    Det här kommandot läser in hello innehållet i hello sample.log fil i LOGGARNA. Du kan visa hello innehållet i hello-fil med hjälp av följande instruktion hello:

        DUMP LOGS;

3. Därefter transformera hello data genom att använda ett reguljärt uttryck tooextract endast hello loggningsnivån för varje post med hjälp av följande instruktion hello:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Du kan använda **DUMP** tooview hello data efter hello omvandling. I det här fallet använder `DUMP LEVELS;`.

4. Fortsätta att tillämpa transformationer med hjälp av hello instruktioner i den följande tabellen hello:

    | Pig Latin-instruktion | Vilka hello-instruktionen har |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Tar bort rader som innehåller ett null-värde för hello loggningsnivån och lagrar hello resultat i `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Grupper hello rader genom att loggningsnivån och lagrar hello resultat i `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Skapar en mängd som innehåller varje unik logg värde och hur många gånger den sker. hello data lagras i `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Sorterar hello loggningsnivåer efter antal (fallande) och lagras i `RESULT`. |

    > [!TIP]
    > Använd `DUMP` tooview hello resultatet av hello omvandling efter varje steg.

5. Du kan också spara hello resultatet av en omvandling med hjälp av hello `STORE` instruktionen. Till exempel följande instruktion hello sparar hello `RESULT` toohello `/example/data/pigout` på hello standardlagring för klustret:

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > hello data lagras i hello angiven katalog i filer med namnet `part-nnnnn`. Om hello directory redan finns felmeddelande ett.

6. tooexit hello grunt prompten anger du följande instruktion hello:

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin batch-filer

Du kan också använda hello Pig kommandot toorun Pig Latin i en fil.

1. När du avslutar hello grunt kommandotolk, Använd hello följande kommando toopipe STDIN till en fil med namnet `pigbatch.pig`. Den här filen har skapats i hello arbetskatalog för hello SSH-användarkontot.

        cat > ~/pigbatch.pig

2. Skriv eller klistra in hello följande rader och sedan använda Ctrl + D när du är klar.

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. Använd hello följande kommando toorun hello `pigbatch.pig` filen med hello Pig kommando.

        pig ~/pigbatch.pig

    När hello batch-jobbet har slutförts kan du se hello följande utdata:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Nästa steg

Allmän information om Pig i HDInsight finns i hello följande dokument:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

Mer information om andra sätt toowork med Hadoop i HDInsight finns i hello följande dokument:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
