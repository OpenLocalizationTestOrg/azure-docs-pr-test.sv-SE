---
title: aaaUse MapReduce och PowerShell med Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse PowerShell tooremotely kör MapReduce-jobb med Hadoop på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av PowerShell

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Det här dokumentet innehåller ett exempel på hur Azure PowerShell toorun ett MapReduce-jobb i en Hadoop på HDInsight-kluster.

## <a id="prereq"></a>Förhandskrav

* **Ett kluster i Azure HDInsight (Hadoop på HDInsight)**

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **En arbetsstation med Azure PowerShell**.

## <a id="powershell"></a>Kör ett MapReduce-jobb med hjälp av Azure PowerShell

Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely kör MapReduce-jobb i HDInsight. Internt, detta kan åstadkommas med hjälp av REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (kallades Templeton) körs på hello HDInsight-kluster.

hello används följande cmdletar när du kör MapReduce-jobb i en fjärransluten HDInsight-kluster.

* **Login-AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration.

* **Nya AzureRmHDInsightMapReduceJobDefinition**: skapar en ny *jobbet definition* anges med hjälp av hello MapReduce information.

* **Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb.

* **Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb. Den väntar tills hello jobbet slutförs eller hello väntetiden har överskridits.

* **Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb.

hello följande steg visar hur toouse dessa cmdlets toorun ett jobb i HDInsight-kluster.

1. Med hjälp av en redigerare, spara hello efter koden som **mapreducejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Öppna ett nytt **Azure PowerShell** kommandotolk. Ändra kataloger toohello placeringen hello **mapreducejob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:

        .\mapreducejob.ps1

    När du kör skriptet hello efterfrågas hello namnet på hello HDInsight-kluster och hello HTTPS/Admin kontonamn och lösenord för hello-kluster. Du kanske också ange tooauthenticate tooyour Azure-prenumeration.

3. När hello jobbet är slutfört, visas utdata liknande toohello följande text:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Den här utdata visar hello jobbet har slutförts.

    > [!NOTE]
    > Om hello **ExitCode** är ett värde än 0, se [felsökning](#troubleshooting).

    Det här exemplet lagras också hello hämtade filer tooan **output.txt** fil i hello kör du hello skript från.

### <a name="view-output"></a>Visa utdata

Öppna hello **output.txt** fil i en text editor toosee hello ord och räknar producerade av hello jobb.

> [!NOTE]
> hello utdatafilerna till ett MapReduce-jobb är oföränderliga. Om du kör det här exemplet måste så toochange hello namnet på utdatafilen hello.

## <a id="troubleshooting"></a>Felsökning

Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning. tooview felinformation för det här jobbet kan lägga till hello efter kommandot toohello hello **mapreducejob.ps1** filen, spara den och kör det igen.

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Denna cmdlet returnerar hello information som har skrivits tooSTDERR på hello server när du körde hello jobbet och det kan hjälpa att avgöra varför hello jobb som misslyckas.

## <a id="summary"></a>Sammanfattning

Som du kan se tillhandahåller Azure PowerShell ett enkelt sätt toorun MapReduce-jobb på ett HDInsight-kluster och jobbstatus för övervakaren hello hämta hello utdata.

## <a id="nextsteps"></a>Nästa steg

Allmän information om MapReduce-jobb i HDInsight:

* [Använda MapReduce på Hadoop och HDInsight](hdinsight-use-mapreduce.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
