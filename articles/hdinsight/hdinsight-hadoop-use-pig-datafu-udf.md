---
title: "aaaUse DataFu med Pig på HDInsight - Azure | Microsoft Docs"
description: "DataFu är en samling av bibliotek för användning med Hadoop. Lär dig hur du kan använda DataFu med Pig på ditt HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 0016721a-82be-4773-88ad-91e6b2c21cbb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 357ad8f9694cc590115289877e752bdd242bdadc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-datafu-with-pig-on-hdinsight"></a><span data-ttu-id="e2a50-104">Använda DataFu med pig på HDInsight</span><span class="sxs-lookup"><span data-stu-id="e2a50-104">Use DataFu with pig on HDInsight</span></span>

<span data-ttu-id="e2a50-105">Lär dig hur toouse DataFu med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e2a50-105">Learn how toouse DataFu with HDInsight.</span></span> <span data-ttu-id="e2a50-106">DataFu är en samling av bibliotek med öppen källkod för användning med Pig på Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e2a50-106">DataFu is a collection of Open Source libraries for use with Pig on Hadoop.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2a50-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e2a50-107">Prerequisites</span></span>

* <span data-ttu-id="e2a50-108">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e2a50-108">An Azure subscription.</span></span>

* <span data-ttu-id="e2a50-109">Ett Azure HDInsight-kluster (Linux eller Windows-baserade)</span><span class="sxs-lookup"><span data-stu-id="e2a50-109">An Azure HDInsight cluster (Linux or Windows based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e2a50-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e2a50-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e2a50-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e2a50-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e2a50-112">En del om [med Pig på HDInsight](hdinsight-use-pig.md)</span><span class="sxs-lookup"><span data-stu-id="e2a50-112">A basic familiarity with [using Pig on HDInsight](hdinsight-use-pig.md)</span></span>

## <a name="install-datafu-on-linux-based-hdinsight"></a><span data-ttu-id="e2a50-113">Installera DataFu på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="e2a50-113">Install DataFu on Linux-based HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2a50-114">DataFu är installerad på Linux-baserade kluster version 3.3 och högre och på Windows-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="e2a50-114">DataFu is installed on Linux-based clusters version 3.3 and higher, and on Windows-based clusters.</span></span> <span data-ttu-id="e2a50-115">Det är inte installerat på Linux-baserade kluster tidigare än 3.3.</span><span class="sxs-lookup"><span data-stu-id="e2a50-115">It is not installed on Linux-based clusters earlier than 3.3.</span></span>
>
> <span data-ttu-id="e2a50-116">Om du använder ett Windows-baserade kluster eller ett Linux-baserade kluster som är högre än version 3.3 kan du hoppa över det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e2a50-116">If you are using a Windows-based cluster, or a Linux-based cluster higher than version 3.3, skip this section.</span></span>

<span data-ttu-id="e2a50-117">DataFu kan hämtas och installeras från hello Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="e2a50-117">DataFu can be downloaded and installed from hello Maven repository.</span></span> <span data-ttu-id="e2a50-118">Använd hello följande steg tooadd DataFu tooyour HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="e2a50-118">Use hello following steps tooadd DataFu tooyour HDInsight cluster:</span></span>

1. <span data-ttu-id="e2a50-119">Ansluta tooyour Linux-baserade HDInsight-kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="e2a50-119">Connect tooyour Linux-based HDInsight cluster using SSH.</span></span> <span data-ttu-id="e2a50-120">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="e2a50-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="e2a50-121">Använda hello efter kommandot toodownload hello DataFu jar-filen med hello wget-verktyget, eller kopiera och klistra in hello länk i webbläsaren toobegin hello hämtningen.</span><span class="sxs-lookup"><span data-stu-id="e2a50-121">Use hello following command toodownload hello DataFu jar file using hello wget utility, or copy and paste hello link into your browser toobegin hello download.</span></span>

    ```
    wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar
    ```

3. <span data-ttu-id="e2a50-122">Därefter överföra hello toodefault fillagring för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e2a50-122">Next, upload hello file toodefault storage for your HDInsight cluster.</span></span> <span data-ttu-id="e2a50-123">Placera hello-filen i gör lagring det tillgängliga tooall noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="e2a50-123">Placing hello file in default storage makes it available tooall nodes in hello cluster.</span></span>

    ```
    hdfs dfs -put datafu-1.2.0.jar /example/jars
    ```

    > [!NOTE]
    > <span data-ttu-id="e2a50-124">hello föregående kommando lagrar hello jar i `/example/jars` eftersom den här katalogen finns redan på hello klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="e2a50-124">hello previous command stores hello jar in `/example/jars` since this directory already exists on hello cluster storage.</span></span> <span data-ttu-id="e2a50-125">Du kan använda valfri plats som du vill på HDInsight klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="e2a50-125">You can use any location you wish on HDInsight cluster storage.</span></span>

## <a name="use-datafu-with-pig"></a><span data-ttu-id="e2a50-126">Använda DataFu med Pig</span><span class="sxs-lookup"><span data-stu-id="e2a50-126">Use DataFu With Pig</span></span>

<span data-ttu-id="e2a50-127">hello stegen i det här avsnittet förutsätter att du är bekant med Pig på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e2a50-127">hello steps in this section assume that you are familiar with using Pig on HDInsight.</span></span> <span data-ttu-id="e2a50-128">Mer information om hur du använder Pig med HDInsight finns [använda Pig med HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="e2a50-128">For more information on using Pig with HDInsight, see [Use Pig with HDInsight](hdinsight-use-pig.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e2a50-129">Om du har installerat DataFu hello anvisningarna i föregående avsnitt i hello manuellt måste du registrera den innan du använder den.</span><span class="sxs-lookup"><span data-stu-id="e2a50-129">If you manually installed DataFu using hello steps in hello previous section, you must register it before using it.</span></span>
>
> * <span data-ttu-id="e2a50-130">Om klustret använder Azure-lagring kan du använda en `wasb://` sökväg.</span><span class="sxs-lookup"><span data-stu-id="e2a50-130">If your cluster uses Azure Storage, use a `wasb://` path.</span></span> <span data-ttu-id="e2a50-131">Till exempel `register wasb:///example/jars/datafu-1.2.0.jar`.</span><span class="sxs-lookup"><span data-stu-id="e2a50-131">For example, `register wasb:///example/jars/datafu-1.2.0.jar`.</span></span>
>
> * <span data-ttu-id="e2a50-132">Om klustret använder Azure Data Lake Store, använda en `adl://` sökväg.</span><span class="sxs-lookup"><span data-stu-id="e2a50-132">If your cluster uses Azure Data Lake Store, use an `adl://` path.</span></span> <span data-ttu-id="e2a50-133">Till exempel `register adl://home/example/jars/datafu-1.2.0.jar`.</span><span class="sxs-lookup"><span data-stu-id="e2a50-133">For example, `register adl://home/example/jars/datafu-1.2.0.jar`.</span></span>

<span data-ttu-id="e2a50-134">Du kan ofta definiera ett alias för DataFu funktioner.</span><span class="sxs-lookup"><span data-stu-id="e2a50-134">You often define an alias for DataFu functions.</span></span> <span data-ttu-id="e2a50-135">hello följande exempel definierar en alias för `SHA`:</span><span class="sxs-lookup"><span data-stu-id="e2a50-135">hello following example defines an alias of `SHA`:</span></span>

```piglatin
DEFINE SHA datafu.pig.hash.SHA();
```

<span data-ttu-id="e2a50-136">Du kan sedan använda detta alias i ett Pig Latin skriptet toogenerate en hash för hello indata.</span><span class="sxs-lookup"><span data-stu-id="e2a50-136">You can then use this alias in a Pig Latin script toogenerate a hash for hello input data.</span></span> <span data-ttu-id="e2a50-137">Till exempel ersätter hello följande kod hello plats i hello indata med ett hash-värde:</span><span class="sxs-lookup"><span data-stu-id="e2a50-137">For example, hello following code replaces hello location in hello input data with a hash value:</span></span>

```piglatin
raw = LOAD '/HdiSamples/HdiSamples/SensorSampleData/building/building.csv' USING
    org.apache.pig.piggybank.storage.CSVExcelStorage(',', 'NO_MULTILINE', 'UNIX', 'SKIP_INPUT_HEADER') AS
    (int1:int,
     id1:chararray,
     int2:int,
     id2:chararray,
     location:chararray);
mask = FOREACH raw GENERATE int1, id1, int2, id2, SHA(location);
DUMP mask;
```

<span data-ttu-id="e2a50-138">Den genererar hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="e2a50-138">It generates hello following output:</span></span>

    (1,M1,25,AC1000,aa5ab35a9174c2062b7f7697b33fafe5ce404cf5fecf6bfbbf0dc96ba0d90046)
    (2,M2,27,FN39TG,7a1ca4ef7515f7276bae7230545829c27810c9d9e98ab2c06066bee6270d5153)
    (3,M3,28,JDNS77,07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)
    (4,M4,17,GG1919,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (5,M5,3,ACMAX22,1ed8c7e56c947bebc0cfcf88c4ea0f02c944568f71df893a99970e4f0c78cddc)
    (6,M6,9,AC1000,c96dd81db196cca5f57bd4270bbb9d9e9d1b242d67f9364005ee1dfdc2632523)
    (7,M7,13,FN39TG,3e049d78d958038ac6bd5dcf038075cc73362b4956aaf7308c3a69c8eca76297)
    (8,M8,25,JDNS77,c1ef40ce0484c698eb4bd27fe56c1e7b68d74f9780ed674210d0e5013dae45e9)
    (9,M9,11,GG1919,a7d355b26bda6bf1196ccffead0b2cf2b81f0a9de5b4876b44407f1dc07e51e6)
    (10,M10,23,ACMAX22,10436829032f361a3de50048de41755140e581467bc1895e6c1a17f423e42d10)
    (11,M11,14,AC1000,348841ef53dbd5a64008a86be432973db790774fb28b59b0d99702a3188b3705)
    (12,M12,26,FN39TG,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (13,M13,25,JDNS77,ed9ad13611d7164839dd3feaf9a7f6a75a4138f389e0d45f8e07fa38da1116a2)
    (14,M14,17,GG1919,80db4ccdca106d37b920206331fcfe3e9e50a9e763d89b54ce3ad5ac8cf30f03)
    (15,M15,19,ACMAX22,3552ac0c032467fbf592cb4d10c5c9267800c01e343ad8ca557256d882ae9327)
    (16,M16,23,AC1000,07c67d76ef92ac9853588e098983fefba4ba5965f8fec95f42ab0d04c27865ba)
    (17,M17,11,FN39TG,557c1599d9a04895d3817d293e0806a4419a14de31958386182798d0d2ed3a56)
    (18,M18,25,JDNS77,dbc74a36d8e7439c45c64d856388506cc9b1218619cef979c3d605115a7a4546)
    (19,M19,14,GG1919,be55ef3f4c4e6c2d9c2afe2a33ac90ad0f50d4de7f9163999877e2a9ca5a54f8)
    (20,M20,19,ACMAX22,ea0b937ea317101ee2c26b03a4843a19ceced8a2b9673c3cf409a726ca2b0fd8)

## <a name="next-steps"></a><span data-ttu-id="e2a50-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2a50-139">Next steps</span></span>

<span data-ttu-id="e2a50-140">Mer information om DataFu eller Pig finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="e2a50-140">For more information on DataFu or Pig, see hello following documents:</span></span>

* <span data-ttu-id="e2a50-141">[Apache Pig DataFu guiden](http://datafu.incubator.apache.org/docs/datafu/guide.html).</span><span class="sxs-lookup"><span data-stu-id="e2a50-141">[Apache DataFu Pig Guide](http://datafu.incubator.apache.org/docs/datafu/guide.html).</span></span>
* [<span data-ttu-id="e2a50-142">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="e2a50-142">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
