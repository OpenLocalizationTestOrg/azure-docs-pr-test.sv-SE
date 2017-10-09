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
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="84608-103">Exempeldata i Azure HDInsight Hive-tabeller</span><span class="sxs-lookup"><span data-stu-id="84608-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="84608-104">I den här artikeln beskriver vi hur toodown sampel data som lagras i Azure HDInsight Hive-tabeller med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="84608-104">In this article, we describe how toodown-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="84608-105">Det omfattar tre provtagningsmetoder för vilket populärt används:</span><span class="sxs-lookup"><span data-stu-id="84608-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="84608-106">Enhetlig slumpmässig provtagning</span><span class="sxs-lookup"><span data-stu-id="84608-106">Uniform random sampling</span></span>
* <span data-ttu-id="84608-107">Slumpmässigt urval av grupper</span><span class="sxs-lookup"><span data-stu-id="84608-107">Random sampling by groups</span></span>
* <span data-ttu-id="84608-108">Stratified provtagning</span><span class="sxs-lookup"><span data-stu-id="84608-108">Stratified sampling</span></span>

<span data-ttu-id="84608-109">hello följande **menyn** länkar tootopics som beskriver hur toosample data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="84608-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="84608-110">**Varför exempel dina data?**</span><span class="sxs-lookup"><span data-stu-id="84608-110">**Why sample your data?**</span></span>
<span data-ttu-id="84608-111">Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="84608-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="84608-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="84608-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="84608-113">Roll i hello Team av vetenskapliga data är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="84608-113">Its role in hello Team Data Science Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="84608-114">Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="84608-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-toosubmit-hive-queries"></a><span data-ttu-id="84608-115">Hur toosubmit Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="84608-115">How toosubmit Hive queries</span></span>
<span data-ttu-id="84608-116">Du kan skicka hive-frågor från hello kommandoraden för Hadoop-konsolen på hello huvudnod hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="84608-116">Hive queries can be submitted from hello Hadoop Command Line console on hello head node of hello Hadoop cluster.</span></span> <span data-ttu-id="84608-117">toodo detta, logga in på hello huvudnod i hello Hadoop-kluster, öppna hello kommandoraden för Hadoop-konsolen och skicka Hive-frågor för hello därifrån.</span><span class="sxs-lookup"><span data-stu-id="84608-117">toodo this, log into hello head node of hello Hadoop cluster, open hello Hadoop Command Line console, and submit hello Hive queries from there.</span></span> <span data-ttu-id="84608-118">Anvisningar för att skicka Hive-frågor i hello kommandoraden för Hadoop-konsolen finns [hur tooSubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="84608-118">For instructions on submitting Hive queries in hello Hadoop Command Line console, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="84608-119"><a name="uniform"></a>Enhetlig slumpmässig provtagning</span><span class="sxs-lookup"><span data-stu-id="84608-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="84608-120">Enhetligt slumpmässiga urval innebär att varje rad i hello datamängden har ett lika risken för att sampla.</span><span class="sxs-lookup"><span data-stu-id="84608-120">Uniform random sampling means that each row in hello data set has an equal chance of being sampled.</span></span> <span data-ttu-id="84608-121">Detta kan implementeras genom att lägga till ett extra fält rand() toohello datauppsättning i hello inre ”” urvalsfråga och hello yttre ”urvalsfråga” villkoret på slumpmässiga fältet.</span><span class="sxs-lookup"><span data-stu-id="84608-121">This can be implemented by adding an extra field rand() toohello data set in hello inner "select" query, and in hello outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="84608-122">Här är en exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="84608-122">Here is an example query:</span></span>

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

<span data-ttu-id="84608-123">Här kan `<sample rate, 0-1>` anger hello andelen poster som hello användare vill toosample.</span><span class="sxs-lookup"><span data-stu-id="84608-123">Here, `<sample rate, 0-1>` specifies hello proportion of records that hello users want toosample.</span></span>

## <span data-ttu-id="84608-124"><a name="group"></a>Slumpmässigt urval av grupper</span><span class="sxs-lookup"><span data-stu-id="84608-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="84608-125">När provtagning kategoriska data, som du kanske vill tooeither inkludera eller exkludera för hello instanser av vissa specifika värdet för en kategoriska variabel.</span><span class="sxs-lookup"><span data-stu-id="84608-125">When sampling categorical data, you may want tooeither include or exclude all of hello instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="84608-126">Detta är begreppet ”sampling av grupp”.</span><span class="sxs-lookup"><span data-stu-id="84608-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="84608-127">Till exempel om du har en kategoriska variabel ”tillstånd”, som har värden NY, MA, CA, NJ, PA osv kan du poster för hello samma tillstånd alltid vara tillsammans, om de samplas eller inte.</span><span class="sxs-lookup"><span data-stu-id="84608-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of hello same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="84608-128">Här är en exempelfråga som exempel av grupp:</span><span class="sxs-lookup"><span data-stu-id="84608-128">Here is an example query that samples by group:</span></span>

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

## <span data-ttu-id="84608-129"><a name="stratified"></a>Stratified provtagning</span><span class="sxs-lookup"><span data-stu-id="84608-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="84608-130">Slumpmässig provtagning stratified med avseende tooa kategoriska variabel när hello tas har värden för det kategoriska som finns i hello samma förhållande som hello överordnade population från vilka hello prover erhölls.</span><span class="sxs-lookup"><span data-stu-id="84608-130">Random sampling is stratified with respect tooa categorical variable when hello samples obtained have values of that categorical that are in hello same ratio as in hello parent population from which hello samples were obtained.</span></span> <span data-ttu-id="84608-131">Med hjälp av hello samma exempel som ovan, anta att dina data har underordnade population av tillstånd Säg NJ har 100 observationer, NY har 60 observationer och WA har 300 observationer.</span><span class="sxs-lookup"><span data-stu-id="84608-131">Using hello same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="84608-132">Om du anger hello frekvensen för stratified provtagning toobe 0,5 sedan hello exemplet fick ska ha ungefär 50, 30 och 150 observationer av Dr, NY och WA respektive.</span><span class="sxs-lookup"><span data-stu-id="84608-132">If you specify hello rate of stratified sampling toobe 0.5, then hello sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="84608-133">Här är en exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="84608-133">Here is an example query:</span></span>

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


<span data-ttu-id="84608-134">Mer information om avancerade provtagningsmetoder som är tillgängliga i Hive finns [LanguageManual provtagning](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="84608-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

