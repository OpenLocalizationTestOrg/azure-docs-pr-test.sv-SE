---
title: aaaUse Hadoop Hive med PowerShell i HDInsight - Azure | Microsoft Docs
description: "Använd PowerShell toorun Hive-frågor i Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a>Köra Hive-frågor med hjälp av PowerShell
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Det här dokumentet innehåller ett exempel på hur Azure PowerShell i hello Azure-resursgrupp läge toorun Hive-frågor i en Hadoop på HDInsight-kluster.

> [!NOTE]
> Det här dokumentet ger inte en detaljerad beskrivning av vad hello HiveQL-instruktioner som används i hello exempel göra. Mer information om hello HiveQL som används i det här exemplet finns [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md).

**Förutsättningar**

* **Ett Azure HDInsight-kluster**: Det spelar ingen roll om hello klustret är Windows eller Linux-baserade.

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* **En arbetsstation med Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Köra Hive-frågor med hjälp av Azure PowerShell

Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely kör Hive-frågor i HDInsight. Internt hello cmdlets gör REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) på hello HDInsight-kluster.

hello används följande cmdletar när du kör Hive-frågor i en fjärransluten HDInsight-kluster:

* **Lägg till AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration
* **Nya AzureRmHDInsightHiveJobDefinition**: skapar ett *jobbet definition* anges med hjälp av hello HiveQL-instruktioner
* **Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb
* **Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb. Den väntar tills hello jobbet slutförs eller hello väntetiden har överskridits.
* **Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb
* **Anropa AzureRmHDInsightHiveJob**: används toorun HiveQL-instruktioner. Den här cmdlet block hello frågan har slutförts, och returnerar sedan hello resultat
* **Använd AzureRmHDInsightCluster**: Anger hello aktuella klustret toouse för hello **Invoke-AzureRmHDInsightHiveJob** kommando

hello följande steg visar hur toouse dessa cmdlets toorun ett jobb i HDInsight-kluster:

1. Med hjälp av en redigerare, spara hello efter koden som **hivejob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Öppna ett nytt **Azure PowerShell** kommandotolk. Ändra kataloger toohello placeringen hello **hivejob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:

        .\hivejob.ps1

    När hello skriptet körs, är du tillfrågas tooenter hello klustrets namn och hello HTTPS/Admin autentiseringsuppgifter för hello klustret. Du kanske också ange toolog i tooyour Azure-prenumeration.

3. När hello jobbet är slutfört, returnerar information liknande toohello följande thext:

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Som tidigare nämnts **Invoke-Hive** kan använda toorun en fråga och vänta på hello svar. Använd följande skript toosee hur Invoke-Hive fungerar hello:

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    hello utdata ser ut så hello följande text:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Du kan använda hello Azure PowerShell för längre HiveQL frågor **här strängar** cmdlet eller HiveQL skriptfiler. hello följande fragment visas hur toouse hello **Invoke-Hive** cmdlet toorun en HiveQL-skriptfil. Hej HiveQL skriptfilen måste vara överförs toowasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Mer information om **här strängar**, se <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">med hjälp av Windows PowerShell här-strängar</a>.

## <a name="troubleshooting"></a>Felsökning

Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning. tooview felinformation för det här jobbet kan lägga till hello efter toohello hello **hivejob.ps1** filen, spara den och kör det igen.

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Denna cmdlet returnerar hello information som skrivs tooSTDERR på hello server när du körde hello jobbet.

## <a name="summary"></a>Sammanfattning

Som du kan se Azure PowerShell tillhandahåller ett enkelt sätt toorun Hive-frågor i ett HDInsight-kluster, övervaka hello jobbstatusen och hämta hello utdata.

## <a name="next-steps"></a>Nästa steg

Allmän information om Hive i HDInsight:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)

Information om andra sätt kan du arbeta med Hadoop i HDInsight:

* [Använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)
