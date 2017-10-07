---
title: "aaaCreate funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor | Microsoft Docs"
description: "Exempel på Hive-frågor som genererar funktioner i data som lagras i ett Azure HDInsight Hadoop-kluster."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8a94c71-979b-4707-b8fd-85b47d309a30
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 686282bf0fb84ea82758d3c5b7de2bd90f0cf159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="2e51d-103">Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="2e51d-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="2e51d-104">Det här dokumentet beskrivs hur toocreate funktioner för data som lagras i ett Azure HDInsight Hadoop-kluster med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="2e51d-104">This document shows how toocreate features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="2e51d-105">Dessa Hive-frågor använder inbäddade Hive användardefinierade funktioner (UDF), hello skript som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="2e51d-105">These Hive queries use embedded Hive User Defined Functions (UDFs), hello scripts for which are provided.</span></span>

<span data-ttu-id="2e51d-106">hello åtgärder som krävs för toocreate funktioner kan vara minnesintensiva.</span><span class="sxs-lookup"><span data-stu-id="2e51d-106">hello operations needed toocreate features can be memory intensive.</span></span> <span data-ttu-id="2e51d-107">hello prestanda för Hive-frågor blir mer kritiska i sådana fall och kan förbättras genom att justera vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="2e51d-107">hello performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="2e51d-108">hello finjustering av dessa parametrar beskrivs i hello sista delen.</span><span class="sxs-lookup"><span data-stu-id="2e51d-108">hello tuning of these parameters is discussed in hello final section.</span></span>

<span data-ttu-id="2e51d-109">Exempel på hello frågor som visas är specifik toohello [NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="2e51d-109">Examples of hello queries that are presented are specific toohello [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="2e51d-110">Dessa frågor som redan har angivna dataschemat och är redo toobe som skickats toorun.</span><span class="sxs-lookup"><span data-stu-id="2e51d-110">These queries already have data schema specified and are ready toobe submitted toorun.</span></span> <span data-ttu-id="2e51d-111">Under hello slutliga beskrivs också parametrar som användare kan justera så att hello prestanda för Hive-frågor kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="2e51d-111">In hello final section, parameters that users can tune so that hello performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="2e51d-112">Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="2e51d-112">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="2e51d-113">Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="2e51d-113">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e51d-114">Krav</span><span class="sxs-lookup"><span data-stu-id="2e51d-114">Prerequisites</span></span>
<span data-ttu-id="2e51d-115">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="2e51d-115">This article assumes that you have:</span></span>

* <span data-ttu-id="2e51d-116">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2e51d-116">Created an Azure storage account.</span></span> <span data-ttu-id="2e51d-117">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="2e51d-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="2e51d-118">Etablera ett anpassat Hadoop-kluster med hello HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="2e51d-118">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="2e51d-119">Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="2e51d-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="2e51d-120">hello data har laddats upp tooHive tabeller i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="2e51d-120">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="2e51d-121">Om den inte har följer [skapa och läsa in tooHive datatabeller](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tabeller först.</span><span class="sxs-lookup"><span data-stu-id="2e51d-121">If it has not, please follow [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="2e51d-122">Aktivera fjärråtkomst toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="2e51d-122">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="2e51d-123">Om du behöver mer information, se [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="2e51d-123">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="2e51d-124"><a name="hive-featureengineering"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="2e51d-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="2e51d-125">I det här avsnittet beskrivs flera exempel på hello sätt där funktioner kan genereras med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="2e51d-125">In this section, several examples of hello ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="2e51d-126">När du har genererat ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckeln, som sedan kan förenas med hello ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="2e51d-126">Once you have generated additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, which can then be joined with hello original table.</span></span> <span data-ttu-id="2e51d-127">Här följer hello exempel visas:</span><span class="sxs-lookup"><span data-stu-id="2e51d-127">Here are hello examples presented:</span></span>

1. [<span data-ttu-id="2e51d-128">Frekvens baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="2e51d-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="2e51d-129">Riskerna med Kategoriska variabler i binär klassificering</span><span class="sxs-lookup"><span data-stu-id="2e51d-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="2e51d-130">Extrahera funktioner från Datetime Field</span><span class="sxs-lookup"><span data-stu-id="2e51d-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="2e51d-131">Extrahera funktioner från textfält</span><span class="sxs-lookup"><span data-stu-id="2e51d-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="2e51d-132">Beräkna avståndet mellan GPS koordinater</span><span class="sxs-lookup"><span data-stu-id="2e51d-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="2e51d-133"><a name="hive-frequencyfeature"></a>Frekvens baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="2e51d-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="2e51d-134">Det är ofta användbara toocalculate hello frekvenser hello nivåer av en kategoriska variabel eller hello frekvenser för vissa kombinationer av nivåerna från flera kategoriska variabler.</span><span class="sxs-lookup"><span data-stu-id="2e51d-134">It is often useful toocalculate hello frequencies of hello levels of a categorical variable, or hello frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="2e51d-135">Användare kan använda följande skript toocalculate hello dessa frekvenser:</span><span class="sxs-lookup"><span data-stu-id="2e51d-135">Users can use hello following script toocalculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="2e51d-136"><a name="hive-riskfeature"></a>Riskerna med Kategoriska variabler i binär klassificering</span><span class="sxs-lookup"><span data-stu-id="2e51d-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="2e51d-137">I binär klassificering behöver vi tooconvert icke-numeriska kategoriska variabler i numeriska funktioner när hello modeller används bara ta numeriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="2e51d-137">In binary classification, we need tooconvert non-numeric categorical variables into numeric features when hello models being used only take numeric features.</span></span> <span data-ttu-id="2e51d-138">Detta görs genom att ersätta alla icke-numeriska nivå med numeriska risk.</span><span class="sxs-lookup"><span data-stu-id="2e51d-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="2e51d-139">I det här avsnittet visar vi några allmänna Hive-frågor som beräknar hello riskvärden (loggen oddsen) för en kategoriska variabel.</span><span class="sxs-lookup"><span data-stu-id="2e51d-139">In this section, we show some generic Hive queries that calculate hello risk values (log odds) of a categorical variable.</span></span>

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

<span data-ttu-id="2e51d-140">I det här exemplet variabler `smooth_param1` och `smooth_param2` beräknas toosmooth hello riskvärden från hello data.</span><span class="sxs-lookup"><span data-stu-id="2e51d-140">In this example, variables `smooth_param1` and `smooth_param2` are set toosmooth hello risk values calculated from hello data.</span></span> <span data-ttu-id="2e51d-141">Intervallet mellan -Inf och Inf är risker.</span><span class="sxs-lookup"><span data-stu-id="2e51d-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="2e51d-142">Risker > 0 anger att hello sannolikheten att hello mål är lika too1 är större än 0,5.</span><span class="sxs-lookup"><span data-stu-id="2e51d-142">A risks > 0 indicates that hello probability that hello target is equal too1 is greater than 0.5.</span></span>

<span data-ttu-id="2e51d-143">När hello risk tabell beräknas tilldela användare risk värden tooa tabell genom att anslutas med hello risk tabell.</span><span class="sxs-lookup"><span data-stu-id="2e51d-143">After hello risk table is calculated, users can assign risk values tooa table by joining it with hello risk table.</span></span> <span data-ttu-id="2e51d-144">anslutande hello Hive-frågan har angetts i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2e51d-144">hello Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="2e51d-145"><a name="hive-datefeatures"></a>Extrahera funktioner från Datetime-Fields</span><span class="sxs-lookup"><span data-stu-id="2e51d-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="2e51d-146">Hive levereras med en uppsättning UDF: er för bearbetning av datetime-fält.</span><span class="sxs-lookup"><span data-stu-id="2e51d-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="2e51d-147">I Hive, hello standardformatet för datum/tid är ”åååå-MM-dd 00:00:00 ' ('1970-01-01 12:21:32, till exempel).</span><span class="sxs-lookup"><span data-stu-id="2e51d-147">In Hive, hello default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="2e51d-148">I det här avsnittet visar vi exempel som extraheras hello dagen i en månad, hello månad från ett datetime-fält och andra exempel som konverteras en datetime-sträng i formatet än hello standard format tooa datetime-sträng i formatet.</span><span class="sxs-lookup"><span data-stu-id="2e51d-148">In this section, we show examples that extract hello day of a month, hello month from a datetime field, and other examples that convert a datetime string in a format other than hello default format tooa datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="2e51d-149">Den här Hive-fråga förutsätter att hello *&#60; datetime-fält >* i hello standard datetime-format.</span><span class="sxs-lookup"><span data-stu-id="2e51d-149">This Hive query assumes that hello *&#60;datetime field>* is in hello default datetime format.</span></span>

<span data-ttu-id="2e51d-150">Om ett datetime-fält är inte i hello standardformat, du behöver tooconvert hello datetime-fält i Unix tidsstämpel först och sedan konvertera hello Unix tid stämpel tooa datetime-sträng som är i hello standard format.</span><span class="sxs-lookup"><span data-stu-id="2e51d-150">If a datetime field is not in hello default format, you need tooconvert hello datetime field into Unix time stamp first, and then convert hello Unix time stamp tooa datetime string that is in hello default format.</span></span> <span data-ttu-id="2e51d-151">När hello datetime är i formatet, kan användare använda hello inbäddade datetime UDF: er tooextract funktioner.</span><span class="sxs-lookup"><span data-stu-id="2e51d-151">When hello datetime is in default format, users can apply hello embedded datetime UDFs tooextract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of hello datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="2e51d-152">I den här frågan om hello *&#60; datetime-fält >* har hello mönster som *03/26/2015 12:04:39*, hello *' &#60; mönstret för hello datetime-fält >'* ska vara `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="2e51d-152">In this query, if hello *&#60;datetime field>* has hello pattern like *03/26/2015 12:04:39*, hello *'&#60;pattern of hello datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="2e51d-153">tootest, användare kan köra</span><span class="sxs-lookup"><span data-stu-id="2e51d-153">tootest it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="2e51d-154">Hej *hivesampletable* i den här frågan är förinstallerat på alla Azure HDInsight Hadoop-kluster som standard när hello kluster etableras.</span><span class="sxs-lookup"><span data-stu-id="2e51d-154">hello *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when hello clusters are provisioned.</span></span>

### <span data-ttu-id="2e51d-155"><a name="hive-textfeatures"></a>Extrahera funktioner från textfält</span><span class="sxs-lookup"><span data-stu-id="2e51d-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="2e51d-156">När hello Hive-tabellen har ett textfält som innehåller en sträng med ord som är avgränsade med blanksteg, extraheras hello följande fråga hello längd hello sträng och hello antalet ord i hello-sträng.</span><span class="sxs-lookup"><span data-stu-id="2e51d-156">When hello Hive table has a text field that contains a string of words that are delimited by spaces, hello following query extracts hello length of hello string, and hello number of words in hello string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="2e51d-157"><a name="hive-gpsdistance"></a>Beräkna avstånd mellan GPS-koordinatuppsättningar</span><span class="sxs-lookup"><span data-stu-id="2e51d-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="2e51d-158">hello-fråga som anges i det här avsnittet kan vara direkt toohello NYC Taxi resa Data.</span><span class="sxs-lookup"><span data-stu-id="2e51d-158">hello query given in this section can be directly applied toohello NYC Taxi Trip Data.</span></span> <span data-ttu-id="2e51d-159">hello syftet med den här frågan är tooshow hur tooapply inbäddad matematiska funktioner i Hive toogenerate funktioner.</span><span class="sxs-lookup"><span data-stu-id="2e51d-159">hello purpose of this query is tooshow how tooapply an embedded mathematical functions in Hive toogenerate features.</span></span>

<span data-ttu-id="2e51d-160">hello fält som används i den här frågan är hello GPS-koordinater för hämtning och dropoff platser, med namnet *hämtning\_longitud*, *hämtning\_latitud*,  *dropoff\_longitud*, och *dropoff\_latitud*.</span><span class="sxs-lookup"><span data-stu-id="2e51d-160">hello fields that are used in this query are hello GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="2e51d-161">hello-frågor som beräknar hello direkt avståndet mellan hello hämtning och dropoff koordinater är:</span><span class="sxs-lookup"><span data-stu-id="2e51d-161">hello queries that calculate hello direct distance between hello pickup and dropoff coordinates are:</span></span>

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

<span data-ttu-id="2e51d-162">hello matematiska formler som beräknar hello avståndet mellan två GPS-koordinater kan hittas på hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">flyttbar typen skript</a> plats som skapats av Peter Lapisu.</span><span class="sxs-lookup"><span data-stu-id="2e51d-162">hello mathematical equations that calculate hello distance between two GPS coordinates can be found on hello <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="2e51d-163">I sin Javascript hello funktionen `toRad()` är bara *lat_or_lon*pi/180 *, som konverterar tooradians grader.</span><span class="sxs-lookup"><span data-stu-id="2e51d-163">In his Javascript, hello function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees tooradians.</span></span> <span data-ttu-id="2e51d-164">Här, *lat_or_lon* är hello latitud och longitud.</span><span class="sxs-lookup"><span data-stu-id="2e51d-164">Here, *lat_or_lon* is hello latitude or longitude.</span></span> <span data-ttu-id="2e51d-165">Eftersom Hive inte ger hello funktionen `atan2`, men ger hello funktionen `atan`, hello `atan2` funktionen implementeras av `atan` funktion i hello ovan Hive-fråga med hello definitionen i <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank"> Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="2e51d-165">Since Hive does not provide hello function `atan2`, but provides hello function `atan`, hello `atan2` function is implemented by `atan` function in hello above Hive query using hello definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="2e51d-167">En fullständig lista över Hive inbäddade UDF: er finns i hello **inbyggda funktioner** avsnittet hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="2e51d-167">A full list of Hive embedded UDFs can be found in hello **Built-in Functions** section on hello <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="2e51d-168"><a name="tuning"></a>Avancerade alternativ: finjustera Hive parametrar tooImprove frågan hastighet</span><span class="sxs-lookup"><span data-stu-id="2e51d-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters tooImprove Query Speed</span></span>
<span data-ttu-id="2e51d-169">Hej standardparametern inställningarna för Hive-kluster inte kan vara lämplig för hello Hive-frågor och hello data som bearbetar hello frågor.</span><span class="sxs-lookup"><span data-stu-id="2e51d-169">hello default parameter settings of Hive cluster might not be suitable for hello Hive queries and hello data that hello queries are processing.</span></span> <span data-ttu-id="2e51d-170">I det här avsnittet diskuterar vi vissa parametrar som användare kan justera som förbättrar prestanda hello Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="2e51d-170">In this section, we discuss some parameters that users can tune that improve hello performance of Hive queries.</span></span> <span data-ttu-id="2e51d-171">Användarna måste tooadd hello parametrar frågor innan hello frågor för bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="2e51d-171">Users need tooadd hello parameter tuning queries before hello queries of processing data.</span></span>

1. <span data-ttu-id="2e51d-172">**Java heap utrymme**: för frågor som rör koppla stora datauppsättningar eller bearbetning långa poster **heap utrymmet börjar ta slut** är en av hello vanliga fel.</span><span class="sxs-lookup"><span data-stu-id="2e51d-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of hello common error.</span></span> <span data-ttu-id="2e51d-173">Detta kan anpassas genom att ange parametrar *mapreduce.map.java.opts* och *mapreduce.task.io.sort.mb* toodesired värden.</span><span class="sxs-lookup"><span data-stu-id="2e51d-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* toodesired values.</span></span> <span data-ttu-id="2e51d-174">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="2e51d-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="2e51d-175">Den här parametern allokerar 4GB minne tooJava heap utrymme så att det blir sortering effektivare genom att allokera mer minne för den.</span><span class="sxs-lookup"><span data-stu-id="2e51d-175">This parameter allocates 4GB memory tooJava heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="2e51d-176">Om det inte finns något jobb fel fel relaterade tooheap utrymme är det en bra idé tooplay med dessa allokeringar.</span><span class="sxs-lookup"><span data-stu-id="2e51d-176">It is a good idea tooplay with these allocations if there are any job failure errors related tooheap space.</span></span>

1. <span data-ttu-id="2e51d-177">**DFS-blockstorlek** : den här parametern anger hello minsta enheten som hello filen system lagrar.</span><span class="sxs-lookup"><span data-stu-id="2e51d-177">**DFS block size** : This parameter sets hello smallest unit of data that hello file system stores.</span></span> <span data-ttu-id="2e51d-178">Exempelvis om hello DFS blockstorleken är 128MB, lagras sedan några data av storleken och mindre än in too128MB i ett enda block när data som är större än 128MB tilldelas extra block.</span><span class="sxs-lookup"><span data-stu-id="2e51d-178">As an example, if hello DFS block size is 128MB, then any data of size less than and up too128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="2e51d-179">Om du väljer en liten blockstorlek medför stora kostnaderna i Hadoop eftersom hello namn har tooprocess många fler begäranden toofind hello relevanta block som rör toohello filen.</span><span class="sxs-lookup"><span data-stu-id="2e51d-179">Choosing a very small block size causes large overheads in Hadoop since hello name node has tooprocess many more requests toofind hello relevant block pertaining toohello file.</span></span> <span data-ttu-id="2e51d-180">En rekommenderad inställning när behandlar gigabyte (eller större) data är:</span><span class="sxs-lookup"><span data-stu-id="2e51d-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="2e51d-181">**Optimera join-uttrycket i Hive** : Anslut till åtgärder i hello kartan/minska framework vanligtvis sker i hello minska fasen ibland, enorma vinster kan uppnås genom att schemalägga kopplingar i hello kartan fasen (kallas även ”mapjoins”).</span><span class="sxs-lookup"><span data-stu-id="2e51d-181">**Optimizing join operation in Hive** : While join operations in hello map/reduce framework typically take place in hello reduce phase, sometimes, enormous gains can be achieved by scheduling joins in hello map phase (also called "mapjoins").</span></span> <span data-ttu-id="2e51d-182">toodirect Hive toodo detta när det är möjligt, vi kan ange:</span><span class="sxs-lookup"><span data-stu-id="2e51d-182">toodirect Hive toodo this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="2e51d-183">**Anger hello antal mappers tooHive** : medan Hadoop tillåter hello användaren tooset hello antalet förminskningsapparater, hello antal mappers är vanligtvis inte anges av hello användare.</span><span class="sxs-lookup"><span data-stu-id="2e51d-183">**Specifying hello number of mappers tooHive** : While Hadoop allows hello user tooset hello number of reducers, hello number of mappers is typically not be set by hello user.</span></span> <span data-ttu-id="2e51d-184">Ett tips som gör att viss mån av kontrollen i det här antalet är toochoose hello Hadoop variabler *mapred.min.split.size* och *mapred.max.split.size* som hello storleken på varje mappning uppgiften avgörs av:</span><span class="sxs-lookup"><span data-stu-id="2e51d-184">A trick that allows some degree of control on this number is toochoose hello Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as hello size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="2e51d-185">Normalt hello standardvärdet *mapred.min.split.size* är 0, som *mapred.max.split.size* är **Long.MAX** och *dfs.block.size* är 64MB.</span><span class="sxs-lookup"><span data-stu-id="2e51d-185">Typically, hello default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="2e51d-186">Som vi ser angivna hello datastorleken justera parametrarna av ”inställningen” dem kan vi tootune hello antalet mappers används.</span><span class="sxs-lookup"><span data-stu-id="2e51d-186">As we can see, given hello data size, tuning these parameters by "setting" them allows us tootune hello number of mappers used.</span></span>
4. <span data-ttu-id="2e51d-187">Några fler **avancerade alternativ** för att optimera Hive prestanda anges nedan.</span><span class="sxs-lookup"><span data-stu-id="2e51d-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="2e51d-188">Dessa kan du tooset hello minne som allokerats toomap minska uppgifter och kan vara användbar vid modifiera prestanda.</span><span class="sxs-lookup"><span data-stu-id="2e51d-188">These allow you tooset hello memory allocated toomap and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="2e51d-189">Kom ihåg att hello *mapreduce.reduce.memory.mb* får inte vara större än hello storlek för fysiskt minne för varje arbetsnod i hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="2e51d-189">Please keep in mind that hello *mapreduce.reduce.memory.mb* cannot be greater than hello physical memory size of each worker node in hello Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

