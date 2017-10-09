---
title: aaaSample data i Azure HDInsight Hive-tabeller | Microsoft Docs
description: Ned provtagning data i Azure HDInsight (Hadopop) Hive-tabeller
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 5f86df9b5a18facc875f437abfb004dbe3a06ea4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Exempeldata i Azure HDInsight Hive-tabeller
I den här artikeln beskriver vi hur toodown sampel data som lagras i Azure HDInsight Hive-tabeller med hjälp av Hive-frågor. Det omfattar tre provtagningsmetoder för vilket populärt används:

* Enhetlig slumpmässig provtagning
* Slumpmässigt urval av grupper
* Stratified provtagning

hello följande **menyn** länkar tootopics som beskriver hur toosample data från olika miljöer för lagring.

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

**Varför exempel dina data?**
Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek. Detta underlättar data förstå undersökning och funktionen tekniker. Roll i hello Team av vetenskapliga data är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.

Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-toosubmit-hive-queries"></a>Hur toosubmit Hive-frågor
Du kan skicka hive-frågor från hello kommandoraden för Hadoop-konsolen på hello huvudnod hello Hadoop-kluster. toodo detta, logga in på hello huvudnod i hello Hadoop-kluster, öppna hello kommandoraden för Hadoop-konsolen och skicka Hive-frågor för hello därifrån. Anvisningar för att skicka Hive-frågor i hello kommandoraden för Hadoop-konsolen finns [hur tooSubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Enhetlig slumpmässig provtagning
Enhetligt slumpmässiga urval innebär att varje rad i hello datamängden har ett lika risken för att sampla. Detta kan implementeras genom att lägga till ett extra fält rand() toohello datauppsättning i hello inre ”” urvalsfråga och hello yttre ”urvalsfråga” villkoret på slumpmässiga fältet.

Här är en exempelfråga:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Här kan `<sample rate, 0-1>` anger hello andelen poster som hello användare vill toosample.

## <a name="group"></a>Slumpmässigt urval av grupper
När provtagning kategoriska data, som du kanske vill tooeither inkludera eller exkludera för hello instanser av vissa specifika värdet för en kategoriska variabel. Detta är begreppet ”sampling av grupp”.
Till exempel om du har en kategoriska variabel ”tillstånd”, som har värden NY, MA, CA, NJ, PA osv kan du poster för hello samma tillstånd alltid vara tillsammans, om de samplas eller inte.

Här är en exempelfråga som exempel av grupp:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratified provtagning
Slumpmässig provtagning stratified med avseende tooa kategoriska variabel när hello tas har värden för det kategoriska som finns i hello samma förhållande som hello överordnade population från vilka hello prover erhölls. Med hjälp av hello samma exempel som ovan, anta att dina data har underordnade population av tillstånd Säg NJ har 100 observationer, NY har 60 observationer och WA har 300 observationer. Om du anger hello frekvensen för stratified provtagning toobe 0,5 sedan hello exemplet fick ska ha ungefär 50, 30 och 150 observationer av Dr, NY och WA respektive.

Här är en exempelfråga:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Mer information om avancerade provtagningsmetoder som är tillgängliga i Hive finns [LanguageManual provtagning](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

