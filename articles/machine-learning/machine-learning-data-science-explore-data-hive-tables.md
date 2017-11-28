---
title: "aaaExplore data i Hive-tabeller med Hive-frågor | Microsoft Docs"
description: "Utforska data i Hive-tabeller med hjälp av Hive-frågor."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 2ede3d41682aa08ced19284f7a83ec95e0c2a93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="97c53-103">Utforska data i Hive-tabeller med Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="97c53-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="97c53-104">Det här dokumentet innehåller exempel Hive-skript som används tooexplore data i Hive-tabeller i ett HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="97c53-104">This document provides sample Hive scripts that are used tooexplore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="97c53-105">hello följande **menyn** länkar tootopics som beskriver hur toouse verktyg tooexplore data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="97c53-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="97c53-106">Krav</span><span class="sxs-lookup"><span data-stu-id="97c53-106">Prerequisites</span></span>
<span data-ttu-id="97c53-107">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="97c53-107">This article assumes that you have:</span></span>

* <span data-ttu-id="97c53-108">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="97c53-108">Created an Azure storage account.</span></span> <span data-ttu-id="97c53-109">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="97c53-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="97c53-110">Etablera ett anpassat Hadoop-kluster med hello HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="97c53-110">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span> <span data-ttu-id="97c53-111">Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="97c53-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="97c53-112">hello data har laddats upp tooHive tabeller i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="97c53-112">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="97c53-113">Om den inte har följer du anvisningarna för hello i [skapa och läsa in tooHive datatabeller](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tabeller först.</span><span class="sxs-lookup"><span data-stu-id="97c53-113">If it has not, follow hello instructions in [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="97c53-114">Aktivera fjärråtkomst toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="97c53-114">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="97c53-115">Om du behöver mer information, se [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="97c53-115">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="97c53-116">Om du behöver mer information om hur toosubmit Hive-frågor finns [hur tooSubmit Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="97c53-116">If you need instructions on how toosubmit Hive queries, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="97c53-117">Exempelskript Hive-fråga för datagranskning</span><span class="sxs-lookup"><span data-stu-id="97c53-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="97c53-118">Hämta hello antal observationer per partition`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="97c53-118">Get hello count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="97c53-119">Hämta hello antal observationer per dag`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="97c53-119">Get hello count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="97c53-120">Hämta hello nivåer i en kategoriska kolumn</span><span class="sxs-lookup"><span data-stu-id="97c53-120">Get hello levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="97c53-121">Hämta hello antalet nivåer i kombination med två kategoriska kolumner`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="97c53-121">Get hello number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="97c53-122">Hämta hello distribution för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="97c53-122">Get hello distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="97c53-123">Extrahera poster från två tabeller</span><span class="sxs-lookup"><span data-stu-id="97c53-123">Extract records from joining two tables</span></span>
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="97c53-124">Ytterligare fråga skript för taxi resa datascenarier</span><span class="sxs-lookup"><span data-stu-id="97c53-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="97c53-125">Exempel på frågor som är specifika för[NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="97c53-125">Examples of queries that are specific too[NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="97c53-126">Dessa frågor som redan har angivna dataschemat och är redo toobe som skickats toorun.</span><span class="sxs-lookup"><span data-stu-id="97c53-126">These queries already have data schema specified and are ready toobe submitted toorun.</span></span>

