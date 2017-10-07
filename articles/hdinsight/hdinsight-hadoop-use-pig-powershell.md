---
title: aaaUse Hadoop Pig med PowerShell i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toosubmit Pig jobb tooa Hadoop-kluster i HDInsight med hjälp av Azure PowerShell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a>Använda Azure PowerShell toorun Pig-jobb med HDInsight

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Det här dokumentet innehåller ett exempel på hur Azure PowerShell toosubmit Pig jobb tooa Hadoop på HDInsight-kluster. Pig kan du toowrite MapReduce-jobb med hjälp av språk (Pig Latin) som modeller Datatransformationer, snarare än mappa och minska funktioner.

> [!NOTE]
> Det här dokumentet ger inte en detaljerad beskrivning av vad hello Pig Latin-uttryck som används i hello exempel göra. Information om hello Pig Latin används i det här exemplet finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Förhandskrav

* **Ett Azure HDInsight-kluster**

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **En arbetsstation med Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>Köra Pig-jobb med hjälp av PowerShell

Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely köra Pig-jobb i HDInsight. Internt, PowerShell använder REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) körs på hello HDInsight-kluster.

hello används följande cmdletar när Pig-jobb som körs på en fjärransluten HDInsight-kluster:

* **Login-AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration
* **Nya AzureRmHDInsightPigJobDefinition**: skapar ett *jobbet definition* anges med hjälp av hello Pig Latin-instruktioner
* **Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb
* **Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb. Den väntar tills hello jobbet har slutförts eller hello väntetiden har överskridits.
* **Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb

hello följande steg visar hur toouse dessa cmdlets toorun ett jobb på ditt HDInsight-kluster.

1. Med hjälp av en redigerare, spara hello efter koden som **pigjob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Öppna en ny Windows PowerShell-kommandotolk. Ändra kataloger toohello placeringen hello **pigjob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:

        .\pigjob.ps1

    Du kan ange toolog i tooyour Azure-prenumeration. Du uppmanas sedan hello HTTPs/Admin kontonamn och lösenord för hello HDInsight-kluster.

2. När hello jobbet har slutförts, måste den returnera information liknande toohello följande text:

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Felsökning

Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning. tooview felinformation för det här jobbet kan lägga till hello efter kommandot toohello hello **pigjob.ps1** filen, spara den och kör det igen.

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Hello information som har skrivits tooSTDERR på hello server när du körde hello jobbet returneras och det kan hjälpa att avgöra varför hello jobb som misslyckas.

## <a id="summary"></a>Sammanfattning
Som du kan se tillhandahåller Azure PowerShell ett enkelt sätt toorun Pig-jobb på ett HDInsight-kluster och jobbstatus för övervakaren hello hämta hello utdata.

## <a id="nextsteps"></a>Nästa steg
Allmän information om Pig i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
