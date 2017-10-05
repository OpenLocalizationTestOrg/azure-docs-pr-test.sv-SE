---
title: "Utforska data i Hive-tabeller med Hive-frågor | Microsoft Docs"
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
ms.openlocfilehash: 67a33a9abc3d3dcdd2fc7205e11feff97e3582a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="3b982-103">Utforska data i Hive-tabeller med Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="3b982-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="3b982-104">Det här dokumentet innehåller exempel på Hive-skript som används för att utforska data i Hive-tabeller i ett HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b982-104">This document provides sample Hive scripts that are used to explore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="3b982-105">Följande **menyn** länkar till avsnitt som beskriver hur du använder Verktyg för att utforska data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="3b982-105">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="3b982-106">Krav</span><span class="sxs-lookup"><span data-stu-id="3b982-106">Prerequisites</span></span>
<span data-ttu-id="3b982-107">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="3b982-107">This article assumes that you have:</span></span>

* <span data-ttu-id="3b982-108">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3b982-108">Created an Azure storage account.</span></span> <span data-ttu-id="3b982-109">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="3b982-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="3b982-110">Etablera ett anpassat Hadoop-kluster med HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="3b982-110">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span> <span data-ttu-id="3b982-111">Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3b982-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="3b982-112">Data har överförts till Hive-tabeller i Azure HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3b982-112">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="3b982-113">Om det inte har det, följ instruktionerna i [skapa och läsa in data till Hive-tabeller](machine-learning-data-science-move-hive-tables.md) först överföra data till Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="3b982-113">If it has not, follow the instructions in [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="3b982-114">Aktivera fjärråtkomst till klustret.</span><span class="sxs-lookup"><span data-stu-id="3b982-114">Enabled remote access to the cluster.</span></span> <span data-ttu-id="3b982-115">Om du behöver mer information, se [komma åt det Head nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="3b982-115">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="3b982-116">Om du behöver information om hur du skicka Hive-frågor finns [så skicka Hive-frågor](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="3b982-116">If you need instructions on how to submit Hive queries, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="3b982-117">Exempelskript Hive-fråga för datagranskning</span><span class="sxs-lookup"><span data-stu-id="3b982-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="3b982-118">Hämta antal observationer per partition`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="3b982-118">Get the count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="3b982-119">Hämta antal observationer per dag`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="3b982-119">Get the count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="3b982-120">Hämta nivåerna i en kategoriska kolumn</span><span class="sxs-lookup"><span data-stu-id="3b982-120">Get the levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="3b982-121">Hämta antalet nivåer i kombination med två kategoriska kolumner`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="3b982-121">Get the number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="3b982-122">Hämta distribution för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="3b982-122">Get the distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="3b982-123">Extrahera poster från två tabeller</span><span class="sxs-lookup"><span data-stu-id="3b982-123">Extract records from joining two tables</span></span>
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="3b982-124">Ytterligare fråga skript för taxi resa datascenarier</span><span class="sxs-lookup"><span data-stu-id="3b982-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="3b982-125">Exempel på frågor som är specifika för [NYC Taxi resa Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarier finns också i [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="3b982-125">Examples of queries that are specific to [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="3b982-126">De här frågorna har angetts dataschemat redan och är redo att skickas till kör.</span><span class="sxs-lookup"><span data-stu-id="3b982-126">These queries already have data schema specified and are ready to be submitted to run.</span></span>

