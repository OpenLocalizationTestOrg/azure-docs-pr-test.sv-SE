---
title: "aaaInstall eller uppdatera Mono på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse en viss version av Mono med HDInsight-kluster. Mono är används toorun .NET-program på Linux-baserade HDInsight-kluster."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: hdinsightactive
ms.openlocfilehash: 1e8a8aaeff231c93a9e232379448517b326da965
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-or-update-mono-on-hdinsight"></a><span data-ttu-id="a7b6d-104">Installera eller uppdatera Mono på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7b6d-104">Install or update Mono on HDInsight</span></span>

<span data-ttu-id="a7b6d-105">Lär dig hur tooinstall en viss version av [Mono](https://www.mono-project.com) på HDInsight 3.4 eller högre.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-105">Learn how tooinstall a specific version of [Mono](https://www.mono-project.com) on HDInsight 3.4 or higher.</span></span>

<span data-ttu-id="a7b6d-106">Mono är installerad på HDInsight 3.4 och högre och har använt toorun .NET-program.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-106">Mono is installed on HDInsight 3.4 and higher, and is used toorun .NET applications.</span></span> <span data-ttu-id="a7b6d-107">Mer information om hello version Mono som ingår i varje version av HDInsight finns [HDInsight component-versioning](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="a7b6d-107">For information on hello version of Mono included with each HDInsight version, see [HDInsight component versioning](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="a7b6d-108">tooinstall en annan version på klustret, Använd hello skriptåtgärd i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-108">tooinstall a different version on your cluster, use hello script action in this document.</span></span> 

## <a name="how-it-works"></a><span data-ttu-id="a7b6d-109">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="a7b6d-109">How it works</span></span>

<span data-ttu-id="a7b6d-110">Det här skriptet accepterar hello följande parameter:</span><span class="sxs-lookup"><span data-stu-id="a7b6d-110">This script accepts hello following parameter:</span></span>

* <span data-ttu-id="a7b6d-111">__Monoljud versionsnumret__: hello version av monoljud tooinstall.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-111">__Mono version number__: hello version of Mono tooinstall.</span></span> <span data-ttu-id="a7b6d-112">hello-version måste vara tillgänglig från [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span><span class="sxs-lookup"><span data-stu-id="a7b6d-112">hello version must be available from [https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/](https://download.mono-project.com/repo/debian/dists/wheezy/snapshots/).</span></span>

<span data-ttu-id="a7b6d-113">hello skriptet installerar hello följande monoljud paket:</span><span class="sxs-lookup"><span data-stu-id="a7b6d-113">hello script installs hello following Mono packages:</span></span>

* <span data-ttu-id="a7b6d-114">__slutföra Mono__</span><span class="sxs-lookup"><span data-stu-id="a7b6d-114">__mono-complete__</span></span>

* <span data-ttu-id="a7b6d-115">__CA-certifikat-mono__</span><span class="sxs-lookup"><span data-stu-id="a7b6d-115">__ca-certificates-mono__</span></span>

## <a name="hello-script"></a><span data-ttu-id="a7b6d-116">hello-skript</span><span class="sxs-lookup"><span data-stu-id="a7b6d-116">hello script</span></span>

<span data-ttu-id="a7b6d-117">__Script plats__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span><span class="sxs-lookup"><span data-stu-id="a7b6d-117">__Script location__: [https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash](https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash)</span></span>

<span data-ttu-id="a7b6d-118">__Krav för__:</span><span class="sxs-lookup"><span data-stu-id="a7b6d-118">__Requirements__:</span></span>

* <span data-ttu-id="a7b6d-119">hello skriptet måste tillämpas på hello __huvudnoder__ och __arbetarnoder__.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-119">hello script must be applied on hello __head nodes__ and __worker nodes__.</span></span>

## <a name="toouse-hello-script"></a><span data-ttu-id="a7b6d-120">toouse hello skript</span><span class="sxs-lookup"><span data-stu-id="a7b6d-120">toouse hello script</span></span>

<span data-ttu-id="a7b6d-121">Mer information om hur toouse skriptet med HDInsight, se hello [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-121">For information on how toouse this script with HDInsight, see hello [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.</span></span> <span data-ttu-id="a7b6d-122">Du kan använda skriptet hello via hello Azure-portalen, Azure PowerShell eller hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-122">You can use hello script through hello Azure portal, Azure PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="a7b6d-123">Följande hello skript åtgärd dokument, använda hello följande URI:</span><span class="sxs-lookup"><span data-stu-id="a7b6d-123">While following hello script action document, use hello following URI:</span></span>

    https://hdiconfigactions.blob.core.windows.net/install-mono/install-mono.bash

> [!NOTE]
> <span data-ttu-id="a7b6d-124">När du konfigurerar HDInsight med det här skriptet, markera hello skriptet som __bevarade__.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-124">When configuring HDInsight with this script, mark hello script as __Persisted__.</span></span> <span data-ttu-id="a7b6d-125">Den här inställningen kan HDInsight tooapply hello skriptet tooworker noder som har lagts till via skalning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-125">This setting allows HDInsight tooapply hello script tooworker nodes added through scaling operations.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a7b6d-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7b6d-126">Next steps</span></span>

<span data-ttu-id="a7b6d-127">Du har lärt dig hur tooupgrade eller installera en viss version av Mono på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a7b6d-127">You have learned how tooupgrade or install a specific version of Mono on HDInsight.</span></span> <span data-ttu-id="a7b6d-128">Mer information om hur du använder .NET-program med Mono på HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="a7b6d-128">For more information on using .NET applications with Mono on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="a7b6d-129">Använda .NET för strömning MapReduce på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7b6d-129">Use .NET for streaming MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)
* [<span data-ttu-id="a7b6d-130">Använda .NET med Hive och Pig i HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7b6d-130">Use .NET with Hive and Pig on HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)
* [<span data-ttu-id="a7b6d-131">Utveckla C#-lösningar med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7b6d-131">Develop C# solutions with Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="a7b6d-132">Migrera .NET lösningar tooLinux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7b6d-132">Migrate .NET solutions tooLinux-based HDInsight</span></span>](hdinsight-hadoop-migrate-dotnet-to-linux.md)

<span data-ttu-id="a7b6d-133">Mer information om hur du använder skriptåtgärder finns [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)</span><span class="sxs-lookup"><span data-stu-id="a7b6d-133">For more information on using script actions, see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md)</span></span>