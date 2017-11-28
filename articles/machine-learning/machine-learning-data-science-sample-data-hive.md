---
title: "Exempel på data i Azure HDInsight Hive-tabeller | Microsoft Docs"
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
ms.openlocfilehash: d46297dfaf85976114fbf610803e5f1a997041e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a><span data-ttu-id="d8810-103">Exempeldata i Azure HDInsight Hive-tabeller</span><span class="sxs-lookup"><span data-stu-id="d8810-103">Sample data in Azure HDInsight Hive tables</span></span>
<span data-ttu-id="d8810-104">I den här artikeln beskrivs vi hur du ned-sample data som lagras i Azure HDInsight Hive-tabeller med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="d8810-104">In this article, we describe how to down-sample data stored in Azure HDInsight Hive tables using Hive queries.</span></span> <span data-ttu-id="d8810-105">Det omfattar tre provtagningsmetoder för vilket populärt används:</span><span class="sxs-lookup"><span data-stu-id="d8810-105">We cover three popularly used sampling methods:</span></span>

* <span data-ttu-id="d8810-106">Enhetlig slumpmässig provtagning</span><span class="sxs-lookup"><span data-stu-id="d8810-106">Uniform random sampling</span></span>
* <span data-ttu-id="d8810-107">Slumpmässigt urval av grupper</span><span class="sxs-lookup"><span data-stu-id="d8810-107">Random sampling by groups</span></span>
* <span data-ttu-id="d8810-108">Stratified provtagning</span><span class="sxs-lookup"><span data-stu-id="d8810-108">Stratified sampling</span></span>

<span data-ttu-id="d8810-109">Följande **menyn** länkar till avsnitt som beskriver hur du exempeldata från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="d8810-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span>

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="d8810-110">**Varför exempel dina data?**</span><span class="sxs-lookup"><span data-stu-id="d8810-110">**Why sample your data?**</span></span>
<span data-ttu-id="d8810-111">Om datamängden som du planerar att analysera är stort, men det är vanligtvis en bra idé att ned-sample data för att minska det till en mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="d8810-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="d8810-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="d8810-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="d8810-113">Roll i teamet datavetenskap processen är att aktivera snabb prototyper för databearbetning funktions- och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="d8810-113">Its role in the Team Data Science Process is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="d8810-114">Sampling uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="d8810-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="how-to-submit-hive-queries"></a><span data-ttu-id="d8810-115">Hur du skickar in Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="d8810-115">How to submit Hive queries</span></span>
<span data-ttu-id="d8810-116">Du kan skicka hive-frågor från kommandoraden för Hadoop-konsolen på huvudnod Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="d8810-116">Hive queries can be submitted from the Hadoop Command Line console on the head node of the Hadoop cluster.</span></span> <span data-ttu-id="d8810-117">Gör detta genom att logga in huvudnod Hadoop-kluster, öppna konsolen Hadoop kommandoraden och skicka Hive-frågor därifrån.</span><span class="sxs-lookup"><span data-stu-id="d8810-117">To do this, log into the head node of the Hadoop cluster, open the Hadoop Command Line console, and submit the Hive queries from there.</span></span> <span data-ttu-id="d8810-118">Anvisningar för att skicka Hive-frågor i konsolen Hadoop kommandoraden finns [så skicka Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="d8810-118">For instructions on submitting Hive queries in the Hadoop Command Line console, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="d8810-119"><a name="uniform"></a>Enhetlig slumpmässig provtagning</span><span class="sxs-lookup"><span data-stu-id="d8810-119"><a name="uniform"></a> Uniform random sampling</span></span>
<span data-ttu-id="d8810-120">Enhetligt slumpmässiga urval innebär att varje rad i datamängden som har ett lika risken för att sampla.</span><span class="sxs-lookup"><span data-stu-id="d8810-120">Uniform random sampling means that each row in the data set has an equal chance of being sampled.</span></span> <span data-ttu-id="d8810-121">Detta kan implementeras genom att lägga till ett extra fält rand() datauppsättningen i frågan ”Välj” inre och yttre ”Välj” frågan villkoret på slumpmässiga fältet.</span><span class="sxs-lookup"><span data-stu-id="d8810-121">This can be implemented by adding an extra field rand() to the data set in the inner "select" query, and in the outer "select" query that condition on that random field.</span></span>

<span data-ttu-id="d8810-122">Här är en exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="d8810-122">Here is an example query:</span></span>

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

<span data-ttu-id="d8810-123">Här kan `<sample rate, 0-1>` anger andelen poster som användare vill använda som exempel.</span><span class="sxs-lookup"><span data-stu-id="d8810-123">Here, `<sample rate, 0-1>` specifies the proportion of records that the users want to sample.</span></span>

## <span data-ttu-id="d8810-124"><a name="group"></a>Slumpmässigt urval av grupper</span><span class="sxs-lookup"><span data-stu-id="d8810-124"><a name="group"></a> Random sampling by groups</span></span>
<span data-ttu-id="d8810-125">När provtagning kategoriska data, du kanske vill inkludera eller exkludera alla instanser av vissa specifika värdet för en kategoriska variabel.</span><span class="sxs-lookup"><span data-stu-id="d8810-125">When sampling categorical data, you may want to either include or exclude all of the instances of some particular value of a categorical variable.</span></span> <span data-ttu-id="d8810-126">Detta är begreppet ”sampling av grupp”.</span><span class="sxs-lookup"><span data-stu-id="d8810-126">This is what is meant by "sampling by group".</span></span>
<span data-ttu-id="d8810-127">Om du har en kategoriska variabel ”tillstånd” som har värden NY, MA, CA, NJ, PA osv, önskade poster för samma tillstånd alltid vara tillsammans, om de samplas eller inte.</span><span class="sxs-lookup"><span data-stu-id="d8810-127">For example, if you have a categorical variable "State", which has values NY, MA, CA, NJ, PA, etc, you want records of the same state be always together, whether they are sampled or not.</span></span>

<span data-ttu-id="d8810-128">Här är en exempelfråga som exempel av grupp:</span><span class="sxs-lookup"><span data-stu-id="d8810-128">Here is an example query that samples by group:</span></span>

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

## <span data-ttu-id="d8810-129"><a name="stratified"></a>Stratified provtagning</span><span class="sxs-lookup"><span data-stu-id="d8810-129"><a name="stratified"></a>Stratified sampling</span></span>
<span data-ttu-id="d8810-130">Slumpmässig provtagning är stratified med avseende på en kategoriska variabel när de prov som erhålls har värden för att kategoriska som finns i samma förhållandet som överordnade populationen fick exemplen.</span><span class="sxs-lookup"><span data-stu-id="d8810-130">Random sampling is stratified with respect to a categorical variable when the samples obtained have values of that categorical that are in the same ratio as in the parent population from which the samples were obtained.</span></span> <span data-ttu-id="d8810-131">I det här exemplet som ovan, anta att dina data har underordnade population av tillstånd, säg NJ har 100 observationer, NY har 60 observationer och WA har 300 observationer.</span><span class="sxs-lookup"><span data-stu-id="d8810-131">Using the same example as above, suppose your data has sub-populations by states, say NJ has 100 observations, NY has 60 observations, and WA has 300 observations.</span></span> <span data-ttu-id="d8810-132">Om du anger mängden stratified provtagning vara 0,5 bör sedan exemplet fick ha ungefär 50, 30 och 150 observationer av Dr, NY och WA respektive.</span><span class="sxs-lookup"><span data-stu-id="d8810-132">If you specify the rate of stratified sampling to be 0.5, then the sample obtained should have approximately 50, 30, and 150 observations of NJ, NY, and WA respectively.</span></span>

<span data-ttu-id="d8810-133">Här är en exempelfråga:</span><span class="sxs-lookup"><span data-stu-id="d8810-133">Here is an example query:</span></span>

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


<span data-ttu-id="d8810-134">Mer information om avancerade provtagningsmetoder som är tillgängliga i Hive finns [LanguageManual provtagning](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span><span class="sxs-lookup"><span data-stu-id="d8810-134">For information on more advanced sampling methods that are available in Hive, see [LanguageManual Sampling](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).</span></span>

