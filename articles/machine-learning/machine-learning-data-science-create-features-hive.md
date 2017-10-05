---
title: "Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor | Microsoft Docs"
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
ms.openlocfilehash: e027a6ffcb63868be13432870e484c5cbf2eef4b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a><span data-ttu-id="e342d-103">Skapa funktioner för data i ett Hadoop-kluster med hjälp av Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="e342d-103">Create features for data in an Hadoop cluster using Hive queries</span></span>
<span data-ttu-id="e342d-104">Det här dokumentet beskrivs hur du skapar funktioner för data som lagras i ett Azure HDInsight Hadoop-kluster med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="e342d-104">This document shows how to create features for data stored in an Azure HDInsight Hadoop cluster using Hive queries.</span></span> <span data-ttu-id="e342d-105">Dessa Hive-frågor använda inbäddade Hive användardefinierade funktioner (UDF), skript som tillhandahålls.</span><span class="sxs-lookup"><span data-stu-id="e342d-105">These Hive queries use embedded Hive User Defined Functions (UDFs), the scripts for which are provided.</span></span>

<span data-ttu-id="e342d-106">De åtgärder som behövs för att skapa funktioner kan vara minnesintensiva.</span><span class="sxs-lookup"><span data-stu-id="e342d-106">The operations needed to create features can be memory intensive.</span></span> <span data-ttu-id="e342d-107">Prestanda för Hive-frågor blir mer kritiska i sådana fall och kan förbättras genom att justera vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="e342d-107">The performance of Hive queries becomes more critical in such cases and can be improved by tuning certain parameters.</span></span> <span data-ttu-id="e342d-108">Justering av dessa parametrar beskrivs i den sista delen.</span><span class="sxs-lookup"><span data-stu-id="e342d-108">The tuning of these parameters is discussed in the final section.</span></span>

<span data-ttu-id="e342d-109">Exempel på de frågor som presenteras som är specifika för den [NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="e342d-109">Examples of the queries that are presented are specific to the [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="e342d-110">De här frågorna har angetts dataschemat redan och är redo att skickas till kör.</span><span class="sxs-lookup"><span data-stu-id="e342d-110">These queries already have data schema specified and are ready to be submitted to run.</span></span> <span data-ttu-id="e342d-111">I den sista delen beskrivs också parametrar som användare kan justera så att prestanda för Hive-frågor kan förbättras.</span><span class="sxs-lookup"><span data-stu-id="e342d-111">In the final section, parameters that users can tune so that the performance of Hive queries can be improved are  also discussed.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="e342d-112">Detta **menyn** länkar till avsnitt som beskriver hur du skapar funktioner för data i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="e342d-112">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="e342d-113">Den här uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="e342d-113">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e342d-114">Krav</span><span class="sxs-lookup"><span data-stu-id="e342d-114">Prerequisites</span></span>
<span data-ttu-id="e342d-115">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="e342d-115">This article assumes that you have:</span></span>

* <span data-ttu-id="e342d-116">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="e342d-116">Created an Azure storage account.</span></span> <span data-ttu-id="e342d-117">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="e342d-117">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="e342d-118">Etablera ett anpassat Hadoop-kluster med HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e342d-118">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span>  <span data-ttu-id="e342d-119">Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="e342d-119">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="e342d-120">Data har överförts till Hive-tabeller i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="e342d-120">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="e342d-121">Om den inte har följer [skapa och läsa in data till Hive-tabeller](machine-learning-data-science-move-hive-tables.md) först överföra data till Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="e342d-121">If it has not, please follow [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="e342d-122">Aktivera fjärråtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="e342d-122">Enabled remote access to the cluster.</span></span> <span data-ttu-id="e342d-123">Om du behöver mer information, se [komma åt det Head nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="e342d-123">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <span data-ttu-id="e342d-124"><a name="hive-featureengineering"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e342d-124"><a name="hive-featureengineering"></a>Feature Generation</span></span>
<span data-ttu-id="e342d-125">I det här avsnittet beskrivs flera exempel på sätt som funktioner kan genereras med hjälp av Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="e342d-125">In this section, several examples of the ways in which features can be generating using Hive queries are described.</span></span> <span data-ttu-id="e342d-126">När du har genererat ytterligare funktioner, kan du lägga till dem som kolumner i den befintliga tabellen eller skapa en ny tabell med ytterligare funktioner och primär nyckel, vilket kan sedan kopplas till den ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="e342d-126">Once you have generated additional features, you can either add them as columns to the existing table or create a new table with the additional features and primary key, which can then be joined with the original table.</span></span> <span data-ttu-id="e342d-127">Här följer exempel visas:</span><span class="sxs-lookup"><span data-stu-id="e342d-127">Here are the examples presented:</span></span>

1. [<span data-ttu-id="e342d-128">Frekvens baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e342d-128">Frequency based Feature Generation</span></span>](#hive-frequencyfeature)
2. [<span data-ttu-id="e342d-129">Riskerna med Kategoriska variabler i binär klassificering</span><span class="sxs-lookup"><span data-stu-id="e342d-129">Risks of Categorical Variables in Binary Classification</span></span>](#hive-riskfeature)
3. [<span data-ttu-id="e342d-130">Extrahera funktioner från Datetime Field</span><span class="sxs-lookup"><span data-stu-id="e342d-130">Extract features from Datetime Field</span></span>](#hive-datefeatures)
4. [<span data-ttu-id="e342d-131">Extrahera funktioner från textfält</span><span class="sxs-lookup"><span data-stu-id="e342d-131">Extract features from Text Field</span></span>](#hive-textfeatures)
5. [<span data-ttu-id="e342d-132">Beräkna avståndet mellan GPS koordinater</span><span class="sxs-lookup"><span data-stu-id="e342d-132">Calculate distance between GPS coordinates</span></span>](#hive-gpsdistance)

### <span data-ttu-id="e342d-133"><a name="hive-frequencyfeature"></a>Frekvens baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e342d-133"><a name="hive-frequencyfeature"></a>Frequency based Feature Generation</span></span>
<span data-ttu-id="e342d-134">Det är ofta användbar för att beräkna frekvenserna av en kategoriska variabel eller frekvenser för vissa kombinationer av nivåerna från flera kategoriska variabler.</span><span class="sxs-lookup"><span data-stu-id="e342d-134">It is often useful to calculate the frequencies of the levels of a categorical variable, or the frequencies of certain combinations of levels from multiple categorical variables.</span></span> <span data-ttu-id="e342d-135">Användare kan använda följande skript för att beräkna dessa frekvenser:</span><span class="sxs-lookup"><span data-stu-id="e342d-135">Users can use the following script to calculate these frequencies:</span></span>

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <span data-ttu-id="e342d-136"><a name="hive-riskfeature"></a>Riskerna med Kategoriska variabler i binär klassificering</span><span class="sxs-lookup"><span data-stu-id="e342d-136"><a name="hive-riskfeature"></a>Risks of Categorical Variables in Binary Classification</span></span>
<span data-ttu-id="e342d-137">Vi behöver konvertera icke-numeriska kategoriska variabler till numeriska funktioner när modeller används bara ta numeriska funktioner i binär klassificering.</span><span class="sxs-lookup"><span data-stu-id="e342d-137">In binary classification, we need to convert non-numeric categorical variables into numeric features when the models being used only take numeric features.</span></span> <span data-ttu-id="e342d-138">Detta görs genom att ersätta alla icke-numeriska nivå med numeriska risk.</span><span class="sxs-lookup"><span data-stu-id="e342d-138">This is done by replacing each non-numeric level with a numeric risk.</span></span> <span data-ttu-id="e342d-139">I det här avsnittet visar vi några allmänna Hive-frågor som beräknar riskvärden (loggen oddsen) för en kategoriska variabel.</span><span class="sxs-lookup"><span data-stu-id="e342d-139">In this section, we show some generic Hive queries that calculate the risk values (log odds) of a categorical variable.</span></span>

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

<span data-ttu-id="e342d-140">I det här exemplet variabler `smooth_param1` och `smooth_param2` är inställda på att utjämna riskvärden som beräknats med data.</span><span class="sxs-lookup"><span data-stu-id="e342d-140">In this example, variables `smooth_param1` and `smooth_param2` are set to smooth the risk values calculated from the data.</span></span> <span data-ttu-id="e342d-141">Intervallet mellan -Inf och Inf är risker.</span><span class="sxs-lookup"><span data-stu-id="e342d-141">Risks have a range between -Inf and Inf.</span></span> <span data-ttu-id="e342d-142">Risker > 0 anger att sannolikheten att målet är lika med 1 är större än 0,5.</span><span class="sxs-lookup"><span data-stu-id="e342d-142">A risks > 0 indicates that the probability that the target is equal to 1 is greater than 0.5.</span></span>

<span data-ttu-id="e342d-143">Efter risken beräknade tabell, användare kan tilldela riskvärden till en tabell genom att anslutas med tabellen risk.</span><span class="sxs-lookup"><span data-stu-id="e342d-143">After the risk table is calculated, users can assign risk values to a table by joining it with the risk table.</span></span> <span data-ttu-id="e342d-144">Anslutande Hive-frågan har angetts i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e342d-144">The Hive joining query was provided in previous section.</span></span>

### <span data-ttu-id="e342d-145"><a name="hive-datefeatures"></a>Extrahera funktioner från Datetime-Fields</span><span class="sxs-lookup"><span data-stu-id="e342d-145"><a name="hive-datefeatures"></a>Extract features from Datetime Fields</span></span>
<span data-ttu-id="e342d-146">Hive levereras med en uppsättning UDF: er för bearbetning av datetime-fält.</span><span class="sxs-lookup"><span data-stu-id="e342d-146">Hive comes with a set of UDFs for processing datetime fields.</span></span> <span data-ttu-id="e342d-147">I Hive, standardformatet för datum/tid är ”åååå-MM-dd 00:00:00 ' ('1970-01-01 12:21:32, till exempel).</span><span class="sxs-lookup"><span data-stu-id="e342d-147">In Hive, the default datetime format is 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' for example).</span></span> <span data-ttu-id="e342d-148">I det här avsnittet visar vi exempel som extraherar dagen i månaden, månaden från ett datetime-fält och andra exempel som konverteras en datetime-sträng i formatet än standardformatet till ett datetime-sträng i formatet.</span><span class="sxs-lookup"><span data-stu-id="e342d-148">In this section, we show examples that extract the day of a month, the month from a datetime field, and other examples that convert a datetime string in a format other than the default format to a datetime string in default format.</span></span>

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

<span data-ttu-id="e342d-149">Den här Hive-fråga förutsätter att den *&#60; datetime-fält >* i standard datetime-format.</span><span class="sxs-lookup"><span data-stu-id="e342d-149">This Hive query assumes that the *&#60;datetime field>* is in the default datetime format.</span></span>

<span data-ttu-id="e342d-150">Om ett datetime-fält inte är i standardformatet, måste du konvertera datetime-fält till Unix-tidsstämpel först och sedan konvertera tidsstämpeln Unix till en datetime-sträng som är i standardformatet.</span><span class="sxs-lookup"><span data-stu-id="e342d-150">If a datetime field is not in the default format, you need to convert the datetime field into Unix time stamp first, and then convert the Unix time stamp to a datetime string that is in the default format.</span></span> <span data-ttu-id="e342d-151">När datetime är i formatet kan använda användare inbäddade datetime UDF: er att extrahera funktioner.</span><span class="sxs-lookup"><span data-stu-id="e342d-151">When the datetime is in default format, users can apply the embedded datetime UDFs to extract features.</span></span>

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

<span data-ttu-id="e342d-152">I den här frågan, om den *&#60; datetime-fält >* har mönster som *03/26/2015 12:04:39*, *' &#60; mönstret för datetime-fält >'* ska vara `'MM/dd/yyyy HH:mm:ss'`.</span><span class="sxs-lookup"><span data-stu-id="e342d-152">In this query, if the *&#60;datetime field>* has the pattern like *03/26/2015 12:04:39*, the *'&#60;pattern of the datetime field>'* should be `'MM/dd/yyyy HH:mm:ss'`.</span></span> <span data-ttu-id="e342d-153">Om du vill testa den kan användare som köra</span><span class="sxs-lookup"><span data-stu-id="e342d-153">To test it, users can run</span></span>

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

<span data-ttu-id="e342d-154">Den *hivesampletable* i den här frågan är förinstallerat på alla Azure HDInsight Hadoop-kluster som standard när klustren etableras.</span><span class="sxs-lookup"><span data-stu-id="e342d-154">The *hivesampletable* in this query comes preinstalled on all Azure HDInsight Hadoop clusters by default when the clusters are provisioned.</span></span>

### <span data-ttu-id="e342d-155"><a name="hive-textfeatures"></a>Extrahera funktioner från textfält</span><span class="sxs-lookup"><span data-stu-id="e342d-155"><a name="hive-textfeatures"></a>Extract features from Text Fields</span></span>
<span data-ttu-id="e342d-156">När Hive-tabellen har ett textfält som innehåller en sträng med ord som är avgränsade med blanksteg, extraheras följande fråga längden på strängen och antalet ord i strängen.</span><span class="sxs-lookup"><span data-stu-id="e342d-156">When the Hive table has a text field that contains a string of words that are delimited by spaces, the following query extracts the length of the string, and the number of words in the string.</span></span>

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <span data-ttu-id="e342d-157"><a name="hive-gpsdistance"></a>Beräkna avstånd mellan GPS-koordinatuppsättningar</span><span class="sxs-lookup"><span data-stu-id="e342d-157"><a name="hive-gpsdistance"></a>Calculate distances between sets of GPS coordinates</span></span>
<span data-ttu-id="e342d-158">Frågan i det här avsnittet kan tillämpas direkt på NYC Taxi resa Data.</span><span class="sxs-lookup"><span data-stu-id="e342d-158">The query given in this section can be directly applied to the NYC Taxi Trip Data.</span></span> <span data-ttu-id="e342d-159">Syftet med den här frågan är att visa hur du använder ett inbäddat matematiska funktioner i Hive för att generera funktioner.</span><span class="sxs-lookup"><span data-stu-id="e342d-159">The purpose of this query is to show how to apply an embedded mathematical functions in Hive to generate features.</span></span>

<span data-ttu-id="e342d-160">De fält som används i den här frågan är GPS-koordinaterna för hämtning och dropoff platser, med namnet *hämtning\_longitud*, *hämtning\_latitud*,  *dropoff\_longitud*, och *dropoff\_latitud*.</span><span class="sxs-lookup"><span data-stu-id="e342d-160">The fields that are used in this query are the GPS coordinates of pickup and dropoff locations, named *pickup\_longitude*, *pickup\_latitude*, *dropoff\_longitude*, and *dropoff\_latitude*.</span></span> <span data-ttu-id="e342d-161">Frågorna som beräknar direkt avståndet mellan koordinaterna hämtning och dropoff är:</span><span class="sxs-lookup"><span data-stu-id="e342d-161">The queries that calculate the direct distance between the pickup and dropoff coordinates are:</span></span>

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

<span data-ttu-id="e342d-162">Matematiska formler som beräknar avståndet mellan två GPS-koordinater kan hittas på den <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">flyttbar typen skript</a> plats som skapats av Peter Lapisu.</span><span class="sxs-lookup"><span data-stu-id="e342d-162">The mathematical equations that calculate the distance between two GPS coordinates can be found on the <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Movable Type Scripts</a> site, authored by Peter Lapisu.</span></span> <span data-ttu-id="e342d-163">I sin Javascript funktionen `toRad()` är bara *lat_or_lon*pi/180 *, som konverterar grader till radianer.</span><span class="sxs-lookup"><span data-stu-id="e342d-163">In his Javascript, the function `toRad()` is just *lat_or_lon*pi/180*, which converts degrees to radians.</span></span> <span data-ttu-id="e342d-164">Här, *lat_or_lon* är latitud och longitud.</span><span class="sxs-lookup"><span data-stu-id="e342d-164">Here, *lat_or_lon* is the latitude or longitude.</span></span> <span data-ttu-id="e342d-165">Eftersom Hive inte ger funktionen `atan2`, men ger funktionen `atan`, `atan2` funktionen implementeras av `atan` funktion i Hive frågan ovan med definitionen i <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span><span class="sxs-lookup"><span data-stu-id="e342d-165">Since Hive does not provide the function `atan2`, but provides the function `atan`, the `atan2` function is implemented by `atan` function in the above Hive query using the definition provided in <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-create-features-hive/atan2new.png)

<span data-ttu-id="e342d-167">En fullständig lista över Hive inbäddade UDF: er finns i den **inbyggda funktioner** avsnitt på den <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span><span class="sxs-lookup"><span data-stu-id="e342d-167">A full list of Hive embedded UDFs can be found in the **Built-in Functions** section on the <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>).</span></span>  

## <span data-ttu-id="e342d-168"><a name="tuning"></a>Avancerade alternativ: finjustera Hive parametrar för att förbättra hastighet för frågan</span><span class="sxs-lookup"><span data-stu-id="e342d-168"><a name="tuning"></a> Advanced topics: Tune Hive Parameters to Improve Query Speed</span></span>
<span data-ttu-id="e342d-169">Parametern standardinställningarna för Hive-kluster är kanske inte lämplig för Hive-frågor och de data som bearbetar frågorna.</span><span class="sxs-lookup"><span data-stu-id="e342d-169">The default parameter settings of Hive cluster might not be suitable for the Hive queries and the data that the queries are processing.</span></span> <span data-ttu-id="e342d-170">I det här avsnittet diskuterar vi vissa parametrar som användare kan justera som förbättrar prestandan för Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="e342d-170">In this section, we discuss some parameters that users can tune that improve the performance of Hive queries.</span></span> <span data-ttu-id="e342d-171">Användare måste lägga till parametern justera frågor innan frågor för bearbetning av data.</span><span class="sxs-lookup"><span data-stu-id="e342d-171">Users need to add the parameter tuning queries before the queries of processing data.</span></span>

1. <span data-ttu-id="e342d-172">**Java heap utrymme**: för frågor som rör koppla stora datauppsättningar eller bearbetning långa poster **heap utrymmet börjar ta slut** är ett vanligt fel.</span><span class="sxs-lookup"><span data-stu-id="e342d-172">**Java heap space**: For queries involving joining large datasets, or processing long records, **running out of heap space** is one of the common error.</span></span> <span data-ttu-id="e342d-173">Detta kan anpassas genom att ange parametrar *mapreduce.map.java.opts* och *mapreduce.task.io.sort.mb* till önskade värden.</span><span class="sxs-lookup"><span data-stu-id="e342d-173">This can be tuned by setting parameters *mapreduce.map.java.opts* and *mapreduce.task.io.sort.mb* to desired values.</span></span> <span data-ttu-id="e342d-174">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="e342d-174">Here is an example:</span></span>
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    <span data-ttu-id="e342d-175">Den här parametern allokerar 4GB minne till Java heap utrymmet och även gör sortering effektivare genom att allokera mer minne för den.</span><span class="sxs-lookup"><span data-stu-id="e342d-175">This parameter allocates 4GB memory to Java heap space and also makes sorting more efficient by allocating more memory for it.</span></span> <span data-ttu-id="e342d-176">Det är en bra idé att spela med dessa allokeringar om det finns jobb felen som rör heap utrymme.</span><span class="sxs-lookup"><span data-stu-id="e342d-176">It is a good idea to play with these allocations if there are any job failure errors related to heap space.</span></span>

1. <span data-ttu-id="e342d-177">**DFS-blockstorlek** : den här parametern anger den minsta enheten av data som lagras i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="e342d-177">**DFS block size** : This parameter sets the smallest unit of data that the file system stores.</span></span> <span data-ttu-id="e342d-178">Exempelvis om DFS-blockstorlek är 128MB, sedan några data av storlek mindre än och upp till lagras 128MB i ett enda block när data som är större än 128MB tilldelas extra block.</span><span class="sxs-lookup"><span data-stu-id="e342d-178">As an example, if the DFS block size is 128MB, then any data of size less than and up to 128MB is stored in a single block, while data that is larger than 128MB is allotted extra blocks.</span></span> <span data-ttu-id="e342d-179">Om du väljer en liten blockstorlek medför stora kostnaderna i Hadoop eftersom noden namn har att bearbeta många fler begäranden för att hitta relevanta block som hör till filen.</span><span class="sxs-lookup"><span data-stu-id="e342d-179">Choosing a very small block size causes large overheads in Hadoop since the name node has to process many more requests to find the relevant block pertaining to the file.</span></span> <span data-ttu-id="e342d-180">En rekommenderad inställning när behandlar gigabyte (eller större) data är:</span><span class="sxs-lookup"><span data-stu-id="e342d-180">A recommended setting when dealing with gigabytes (or larger) data is :</span></span>
   
        set dfs.block.size=128m;
2. <span data-ttu-id="e342d-181">**Optimera join-uttrycket i Hive** : vid anslutning till åtgärder i kartan/minska framework vanligtvis sker minska fas, ibland enorma vinster kan uppnås genom att schemalägga kopplingar i fasen mappning (även kallat ”mapjoins”).</span><span class="sxs-lookup"><span data-stu-id="e342d-181">**Optimizing join operation in Hive** : While join operations in the map/reduce framework typically take place in the reduce phase, sometimes, enormous gains can be achieved by scheduling joins in the map phase (also called "mapjoins").</span></span> <span data-ttu-id="e342d-182">Om du vill dirigera Hive för att göra det när det är möjligt, kan vi ange:</span><span class="sxs-lookup"><span data-stu-id="e342d-182">To direct Hive to do this whenever possible, we can set :</span></span>
   
        set hive.auto.convert.join=true;
3. <span data-ttu-id="e342d-183">**Anger antalet mappers till Hive** : medan Hadoop tillåter användaren att ange antalet förminskningsapparater, antal mappers är vanligtvis inte anges av användaren.</span><span class="sxs-lookup"><span data-stu-id="e342d-183">**Specifying the number of mappers to Hive** : While Hadoop allows the user to set the number of reducers, the number of mappers is typically not be set by the user.</span></span> <span data-ttu-id="e342d-184">Ett tips som gör att viss mån av kontrollen i det här antalet är att välja variablerna Hadoop *mapred.min.split.size* och *mapred.max.split.size* som storleken på varje mappning uppgiften avgörs av:</span><span class="sxs-lookup"><span data-stu-id="e342d-184">A trick that allows some degree of control on this number is to choose the Hadoop variables, *mapred.min.split.size* and *mapred.max.split.size* as the size of each map task is determined by :</span></span>
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    <span data-ttu-id="e342d-185">Normalt standardvärdet *mapred.min.split.size* är 0, som *mapred.max.split.size* är **Long.MAX** och *dfs.block.size* är 64MB.</span><span class="sxs-lookup"><span data-stu-id="e342d-185">Typically, the default value of *mapred.min.split.size* is 0, that of *mapred.max.split.size* is **Long.MAX** and that of *dfs.block.size* is 64MB.</span></span> <span data-ttu-id="e342d-186">Som vi ser anges storleken på data justera parametrarna av ”inställningen” dem gör att vi kan justera antalet mappers används.</span><span class="sxs-lookup"><span data-stu-id="e342d-186">As we can see, given the data size, tuning these parameters by "setting" them allows us to tune the number of mappers used.</span></span>
4. <span data-ttu-id="e342d-187">Några fler **avancerade alternativ** för att optimera Hive prestanda anges nedan.</span><span class="sxs-lookup"><span data-stu-id="e342d-187">A few other more **advanced options** for optimizing Hive performance are mentioned below.</span></span> <span data-ttu-id="e342d-188">Dessa kan du ange det minne som allokerats för att mappa och minska uppgifter och kan vara användbar vid modifiera prestanda.</span><span class="sxs-lookup"><span data-stu-id="e342d-188">These allow you to set the memory allocated to map and reduce tasks, and can be useful in tweaking performance.</span></span> <span data-ttu-id="e342d-189">Kontrollera Tänk på att den *mapreduce.reduce.memory.mb* får inte vara större än storlek för fysiskt minne för varje arbetsnod i Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="e342d-189">Please keep in mind that the *mapreduce.reduce.memory.mb* cannot be greater than the physical memory size of each worker node in the Hadoop cluster.</span></span>
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

