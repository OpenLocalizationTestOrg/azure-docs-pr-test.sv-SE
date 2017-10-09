---
title: "aaaUse Hadoop Pig med fjärrskrivbord i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse hello Pig kommandot toorun Pig Latin rapporter från ett fjärrskrivbord anslutning tooa Windows-baserade Hadoop-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Köra Pig-jobb från en fjärrskrivbordsanslutning
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Det här dokumentet innehåller en genomgång för hello Pig kommandot toorun Pig Latin rapporter från ett fjärrskrivbord anslutning tooa Windows-baserade HDInsight-kluster. Pig Latin kan du toocreate MapReduce program genom att beskriva Datatransformationer, snarare än mappa och minska funktioner.

> [!IMPORTANT]
> Fjärrskrivbord är bara tillgängligt på HDInsight-kluster som använder Windows som hello-operativsystem. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> För HDInsight 3.4 eller större finns [använda Pig med HDInsight och SSH](hdinsight-hadoop-use-pig-ssh.md) information om hur du kör interaktivt Pig-jobb direkt på hello kluster från en kommandorad.

## <a id="prereq"></a>Förhandskrav
toocomplete hello stegen i den här artikeln, behöver du hello följande.

* Ett kluster med Windows-baserade HDInsight (Hadoop på HDInsight)
* En klientdator som kör Windows 10, Windows 8 eller Windows 7

## <a id="connect"></a>Ansluta med fjärrskrivbord
Aktivera Fjärrskrivbord för hello HDInsight-kluster och sedan ansluta tooit genom att följa anvisningarna hello på [ansluta tooHDInsight kluster med RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Använd hello Pig kommando
1. När du har en fjärrskrivbordsanslutning starta hello **Hadoop kommandoraden** med hjälp av hello-ikon på hello skrivbordet.
2. Använd följande kommando för toostart hello Pig hello:

        %pig_home%\bin\pig

    Du kan välja en `grunt>` prompt.
3. Ange hello följande instruktion:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Det här kommandot laddar hello innehållet i hello sample.log fil till hello loggar filen. Du kan visa hello innehållet i hello-fil med hjälp av hello följande kommando:

        DUMP LOGS;
4. Transformera hello data genom att använda ett reguljärt uttryck tooextract endast hello loggningsnivån för varje post:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Du kan använda **DUMP** tooview hello data efter hello omvandling. I det här fallet `DUMP LEVELS;`.
5. Fortsätta att tillämpa transformationer med hjälp av hello följande instruktioner. Använd `DUMP` tooview hello resultatet av hello omvandling efter varje steg.

    <table>
    <tr>
    <th>Instruktionen</th><th>Vad verktyget gör</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER nivåer av LOGLEVEL inte är null.</td><td>Tar bort rader som innehåller ett null-värde för hello loggningsnivån och lagrar hello resultat i FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = grupp FILTEREDLEVELS av LOGLEVEL;</td><td>Grupper hello rader genom att loggningsnivån och lagrar hello resultat i GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>FREKVENSERNA = foreach GROUPEDLEVELS generera grupp som LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) som antal;</td><td>Skapar en ny uppsättning som innehåller varje unik logg värde och hur många gånger den sker. Detta är lagrad i frekvenser</td>
    </tr>
    <tr>
    <td>RESULTATET = order frekvenser av antal desc;</td><td>Sorterar hello loggningsnivåer efter antal (fallande) och lagras i resultatet</td>
    </tr>
    </table>
6.Du kan också spara hello resultatet av en omvandling med hjälp av hello `STORE` instruktionen. Till exempel hello följande kommando sparar hello `RESULT` toohello **/example/data/pigout** katalog i hello standardbehållare för lagring för klustret:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > hello data lagras i hello angiven katalog i filer med namnet **del nnnnn**. Om hello katalogen redan finns visas ett felmeddelande.
   >
   >
7. tooexit hello grunt kommandoprompt, ange hello följande instruktion.

        QUIT;

### <a name="pig-latin-batch-files"></a>Pig Latin batch-filer
Du kan också använda hello Pig kommandot toorun Pig Latin som ingår i en fil.

1. När du avslutar hello grunt prompt öppna **anteckningar** och skapa en ny fil med namnet **pigbatch.pig** i hello **PIG_HOME %** directory.
2. Skriv eller klistra in hello följande rader i hello **pigbatch.pig** filen och spara den:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. Använd hello följande toorun hello **pigbatch.pig** filen hello pig kommandot.

        pig %PIG_HOME%\pigbatch.pig

    När hello batch-jobbet är klart du bör se hello följande utdata som ska vara hello densamma som när du använde `DUMP RESULT;` i hello föregående steg:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Sammanfattning
Som du ser kan hello Pig-kommandot toointeractively kör MapReduce åtgärder eller köra Pig Latin-jobb som lagras i en batchfil.

## <a id="nextsteps"></a>Nästa steg
Allmän information om Pig i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
