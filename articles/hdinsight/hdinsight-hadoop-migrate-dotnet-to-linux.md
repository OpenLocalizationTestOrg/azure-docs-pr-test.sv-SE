---
title: "aaaUse .NET med Hadoop MapReduce på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse .NET-program för strömning MapReduce på Linux-baserade HDInsight."
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 5a4e6dc1b4dafa8cc40780e3371fa4b8ba3e3d48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-toolinux-based-hdinsight"></a><span data-ttu-id="f3675-103">Migrera .NET lösningar för Windows-baserade HDInsight tooLinux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="f3675-103">Migrate .NET solutions for Windows-based HDInsight tooLinux-based HDInsight</span></span>

<span data-ttu-id="f3675-104">Linux-baserade HDInsight-kluster Använd [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET-program.</span><span class="sxs-lookup"><span data-stu-id="f3675-104">Linux-based HDInsight clusters use [Mono (https://mono-project.com)](https://mono-project.com) toorun .NET applications.</span></span> <span data-ttu-id="f3675-105">Mono kan toouse .NET-komponenter, till exempel MapReduce program med Linux-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f3675-105">Mono allows you toouse .NET components such as MapReduce applications with Linux-based HDInsight.</span></span> <span data-ttu-id="f3675-106">Lär dig hur toomigrate .NET lösningar som skapats för Windows-baserade HDInsight-kluster toowork med Mono på Linux-baserade HDInsight i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f3675-106">In this document, learn how toomigrate .NET solutions created for Windows-based HDInsight clusters toowork with Mono on Linux-based HDInsight.</span></span>

## <a name="mono-compatibility-with-net"></a><span data-ttu-id="f3675-107">Monoljud kompatibilitet med .NET</span><span class="sxs-lookup"><span data-stu-id="f3675-107">Mono compatibility with .NET</span></span>

<span data-ttu-id="f3675-108">Monoljud version 4.2.1 ingår i HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="f3675-108">Mono version 4.2.1 is included with HDInsight version 3.5.</span></span> <span data-ttu-id="f3675-109">Mer information om hello version Mono som ingår i HDInsight finns [HDInsight komponenten versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="f3675-109">For more information on hello version of Mono included with HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md).</span></span> <span data-ttu-id="f3675-110">tooinstall en viss version av Mono, se hello [installera eller uppdatera Mono](hdinsight-hadoop-install-mono.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f3675-110">tooinstall a specific version of Mono, see hello [Install or update Mono](hdinsight-hadoop-install-mono.md) document.</span></span>

<span data-ttu-id="f3675-111">Detaljerad information om kompatibilitet mellan Mono och .NET finns hello [monoljud kompatibilitet (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f3675-111">For detailed information on compatibility between Mono and .NET, see hello [Mono compatibility (http://www.mono-project.com/docs/about-mono/compatibility/)](http://www.mono-project.com/docs/about-mono/compatibility/) document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3675-112">Hej SCP.NET framework är kompatibel med Mono.</span><span class="sxs-lookup"><span data-stu-id="f3675-112">hello SCP.NET framework is compatible with Mono.</span></span> <span data-ttu-id="f3675-113">Mer information om hur du använder SCP.NET med Mono finns [Använd Visual Studio toodevelop C#-topologier för Apache Storm på HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="f3675-113">For more information on using SCP.NET with Mono, see [Use Visual Studio toodevelop C# topologies for Apache Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

## <a name="automated-portability-analysis"></a><span data-ttu-id="f3675-114">Automatisk överföring analys</span><span class="sxs-lookup"><span data-stu-id="f3675-114">Automated portability analysis</span></span>

<span data-ttu-id="f3675-115">Hej [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) kan vara toogenerate används en rapport över inkompatibilitet mellan programmet och Mono.</span><span class="sxs-lookup"><span data-stu-id="f3675-115">hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) can be used toogenerate a report of incompatibilities between your application and Mono.</span></span> <span data-ttu-id="f3675-116">Använd följande steg tooconfigure hello analyzer toocheck hello ditt program för monoljud portability:</span><span class="sxs-lookup"><span data-stu-id="f3675-116">Use hello following steps tooconfigure hello analyzer toocheck your application for Mono portability:</span></span>

1. <span data-ttu-id="f3675-117">Installera hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span><span class="sxs-lookup"><span data-stu-id="f3675-117">Install hello [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer).</span></span> <span data-ttu-id="f3675-118">Välj hello version av Visual Studio toouse under installationen.</span><span class="sxs-lookup"><span data-stu-id="f3675-118">During installation, select hello version of Visual Studio toouse.</span></span>

2. <span data-ttu-id="f3675-119">Visual Studio 2015, Välj __analysera__ > __Portability Analyzer inställningar__, och se till att __4.5__ checkas in hello __Mono__  avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f3675-119">From Visual Studio 2015, select __Analyze__ > __Portability Analyzer Settings__, and make sure that __4.5__ is checked in hello __Mono__ section.</span></span>

    ![4.5 incheckad monoljud avsnittet hello analyzer inställningar](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    <span data-ttu-id="f3675-121">Välj __OK__ toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f3675-121">Select __OK__ toosave hello configuration.</span></span>

3. <span data-ttu-id="f3675-122">Välj __analysera__ > __analysera sammansättningen Portability__.</span><span class="sxs-lookup"><span data-stu-id="f3675-122">Select __Analyze__ > __Analyze Assembly Portability__.</span></span> <span data-ttu-id="f3675-123">Välj hello sammansättningen som innehåller din lösning och välj sedan __öppna__ toobegin analys.</span><span class="sxs-lookup"><span data-stu-id="f3675-123">Select hello assembly that contains your solution, and then select __Open__ toobegin analysis.</span></span>

4. <span data-ttu-id="f3675-124">När analysen är klar, välja __analysera__ > __visa analysrapporter__.</span><span class="sxs-lookup"><span data-stu-id="f3675-124">Once analysis is complete, select __Analyze__ > __View analysis reports__.</span></span> <span data-ttu-id="f3675-125">I __Portability analysresultat__väljer __öppna rapporten__ tooopen en rapport.</span><span class="sxs-lookup"><span data-stu-id="f3675-125">In __Portability Analysis Results__, select __Open report__ tooopen a report.</span></span>

    ![Dialogrutan för portabilitet analyzer resultat](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]
> <span data-ttu-id="f3675-127">hello analyzer kan inte fånga alla problem med din lösning.</span><span class="sxs-lookup"><span data-stu-id="f3675-127">hello analyzer cannot catch every problem with your solution.</span></span> <span data-ttu-id="f3675-128">Till exempel en sökväg till filen för `c:\temp\file.txt` anses vara OK eftersom Mono körs på Windows och hello sökvägen är giltig i denna kontext.</span><span class="sxs-lookup"><span data-stu-id="f3675-128">For example, a file path of `c:\temp\file.txt` is considered OK because Mono runs on Windows and hello path is valid in that context.</span></span> <span data-ttu-id="f3675-129">Hello sökväg är inte giltig på en Linux-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f3675-129">However, hello path is not valid on a Linux platform.</span></span>

## <a name="manual-portability-analysis"></a><span data-ttu-id="f3675-130">Manuell överföring analys</span><span class="sxs-lookup"><span data-stu-id="f3675-130">Manual portability analysis</span></span>

<span data-ttu-id="f3675-131">Utföra en manuell granskning av din kod med hello information i hello [programmet Portability (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f3675-131">Perform a manual audit of your code using hello information in hello [Application Portability (http://www.mono-project.com/docs/getting-started/application-portability/)](http://www.mono-project.com/docs/getting-started/application-portability/) document.</span></span>

## <a name="modify-and-build"></a><span data-ttu-id="f3675-132">Ändra och skapa</span><span class="sxs-lookup"><span data-stu-id="f3675-132">Modify and build</span></span>

<span data-ttu-id="f3675-133">Du kan fortsätta toouse toobuild för Visual Studio .NET-lösningar för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f3675-133">You can continue toouse Visual Studio toobuild your .NET solutions for HDInsight.</span></span> <span data-ttu-id="f3675-134">Dock måste du kontrollera att hello-projektet är konfigurerade toouse .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="f3675-134">However, you must ensure that hello project is configured toouse .NET Framework 4.5.</span></span>

## <a name="deploy-and-test"></a><span data-ttu-id="f3675-135">Distribuera och testa</span><span class="sxs-lookup"><span data-stu-id="f3675-135">Deploy and test</span></span>

<span data-ttu-id="f3675-136">När du har ändrat din lösning med hjälp av hello rekommendationer från hello .NET Portability Analyzer eller en manuell analys, måste du testa den med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f3675-136">Once you have modified your solution using hello recommendations from hello .NET Portability Analyzer or from a manual analysis, you must test it with HDInsight.</span></span> <span data-ttu-id="f3675-137">Testa hello lösning på ett Linux-baserat HDInsight-kluster kan du få problem som behöver toobe korrigeras.</span><span class="sxs-lookup"><span data-stu-id="f3675-137">Testing hello solution on a Linux-based HDInsight cluster may reveal subtle problems that need toobe corrected.</span></span> <span data-ttu-id="f3675-138">Vi rekommenderar att du aktiverar ytterligare loggning i ditt program testar det.</span><span class="sxs-lookup"><span data-stu-id="f3675-138">We recommend that you enable additional logging in your application while testing it.</span></span>

<span data-ttu-id="f3675-139">Mer information om hur du använder loggar finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="f3675-139">For more information on accessing logs, see hello following documents:</span></span>

* [<span data-ttu-id="f3675-140">Få åtkomst till YARN-programloggar i Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="f3675-140">Access YARN application logs on Linux-based HDInsight</span></span>](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a><span data-ttu-id="f3675-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f3675-141">Next steps</span></span>

* [<span data-ttu-id="f3675-142">Använda C# med MapReduce på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f3675-142">Use C# with MapReduce on HDInsight</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="f3675-143">Använda C# användardefinierade funktioner med Hive och Pig</span><span class="sxs-lookup"><span data-stu-id="f3675-143">Use C# user defined functions with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="f3675-144">Utveckla C#-topologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f3675-144">Develop C# topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)