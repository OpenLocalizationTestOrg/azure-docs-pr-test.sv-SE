---
title: aaaApache Storm-topologier med Visual Studio och C# - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate Storm-topologier i C#. Skapa en enkel word antal topologi i Visual Studio med hjälp av hello Hadoop-verktyg för Visual Studio."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: larryfr
ms.openlocfilehash: b3fb01a4dda144fd7fb4141e624e31e667f93753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-c-topologies-for-apache-storm-by-using-hello-data-lake-tools-for-visual-studio"></a><span data-ttu-id="79ba1-104">Utveckla C#-topologier för Apache Storm genom att använda hello Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79ba1-104">Develop C# topologies for Apache Storm by using hello Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="79ba1-105">Lär dig hur toocreate en C# Storm-topologi med hjälp av hello Azure Data Lake (Hadoop) tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="79ba1-105">Learn how toocreate a C# Storm topology by using hello Azure Data Lake (Hadoop) tools for Visual Studio.</span></span> <span data-ttu-id="79ba1-106">Det här dokumentet går igenom hello processen att skapa ett Storm-projekt i Visual Studio, lokal testning och distribuera den tooan Apache Storm på Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-106">This document walks through hello process of creating a Storm project in Visual Studio, testing it locally, and deploying it tooan Apache Storm on Azure HDInsight cluster.</span></span>

<span data-ttu-id="79ba1-107">Du också lära dig hur toocreate hybridtopologier som använder C# och Java-komponenter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-107">You also learn how toocreate hybrid topologies that use C# and Java components.</span></span>

> [!NOTE]
> <span data-ttu-id="79ba1-108">Medan hello stegen i det här dokumentet är beroende av en Windows-utvecklingsmiljö med Visual Studio, kan hello kompilerade projekt vara skickade tooeither ett Linux- eller Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-108">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooeither a Linux or Windows-based HDInsight cluster.</span></span> <span data-ttu-id="79ba1-109">Linux-baserade kluster som skapas efter den 28 oktober 2016 stöder endast SCP.NET topologier.</span><span class="sxs-lookup"><span data-stu-id="79ba1-109">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="79ba1-110">toouse C#-topologi med en Linux-baserade kluster, måste du uppdatera hello Microsoft.SCP.Net.SDK NuGet-paketet används av ditt projekt tooversion 0.10.0.6 eller senare.</span><span class="sxs-lookup"><span data-stu-id="79ba1-110">toouse a C# topology with a Linux-based cluster, you must update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or later.</span></span> <span data-ttu-id="79ba1-111">hello version av hello paketet måste även matcha hello huvudversion av Storm installerad på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79ba1-111">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span>

| <span data-ttu-id="79ba1-112">HDInsight-version</span><span class="sxs-lookup"><span data-stu-id="79ba1-112">HDInsight version</span></span> | <span data-ttu-id="79ba1-113">Storm-version</span><span class="sxs-lookup"><span data-stu-id="79ba1-113">Storm version</span></span> | <span data-ttu-id="79ba1-114">SCP.NET version</span><span class="sxs-lookup"><span data-stu-id="79ba1-114">SCP.NET version</span></span> | <span data-ttu-id="79ba1-115">Monoljud standardversion</span><span class="sxs-lookup"><span data-stu-id="79ba1-115">Default Mono version</span></span> |
|:-----------------:|:-------------:|:---------------:|:--------------------:|
| <span data-ttu-id="79ba1-116">3.3</span><span class="sxs-lookup"><span data-stu-id="79ba1-116">3.3</span></span> |<span data-ttu-id="79ba1-117">0.10.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-117">0.10.x</span></span> |<span data-ttu-id="79ba1-118">0.10.x.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-118">0.10.x.x</span></span></br><span data-ttu-id="79ba1-119">(endast på Windows-baserade HDInsight)</span><span class="sxs-lookup"><span data-stu-id="79ba1-119">(only on Windows-based HDInsight)</span></span> | <span data-ttu-id="79ba1-120">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="79ba1-120">NA</span></span> |
| <span data-ttu-id="79ba1-121">3.4</span><span class="sxs-lookup"><span data-stu-id="79ba1-121">3.4</span></span> | <span data-ttu-id="79ba1-122">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-122">0.10.0.x</span></span> | <span data-ttu-id="79ba1-123">0.10.0.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-123">0.10.0.x</span></span> | <span data-ttu-id="79ba1-124">3.2.8</span><span class="sxs-lookup"><span data-stu-id="79ba1-124">3.2.8</span></span> |
| <span data-ttu-id="79ba1-125">3.5</span><span class="sxs-lookup"><span data-stu-id="79ba1-125">3.5</span></span> | <span data-ttu-id="79ba1-126">1.0.2.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-126">1.0.2.x</span></span> | <span data-ttu-id="79ba1-127">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-127">1.0.0.x</span></span> | <span data-ttu-id="79ba1-128">4.2.1</span><span class="sxs-lookup"><span data-stu-id="79ba1-128">4.2.1</span></span> |
| <span data-ttu-id="79ba1-129">3.6</span><span class="sxs-lookup"><span data-stu-id="79ba1-129">3.6</span></span> | <span data-ttu-id="79ba1-130">1.1.0.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-130">1.1.0.x</span></span> | <span data-ttu-id="79ba1-131">1.0.0.x</span><span class="sxs-lookup"><span data-stu-id="79ba1-131">1.0.0.x</span></span> | <span data-ttu-id="79ba1-132">4.2.8</span><span class="sxs-lookup"><span data-stu-id="79ba1-132">4.2.8</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="79ba1-133">C#-topologier på Linux-baserade kluster måste använda .NET 4.5 och Använd monoljud toorun på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-133">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="79ba1-134">Kontrollera [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) för potentiella inkompatibiliteter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-134">Check [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) for potential incompatibilities.</span></span>

## <a name="install-visual-studio"></a><span data-ttu-id="79ba1-135">Installera Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79ba1-135">Install Visual Studio</span></span>

<span data-ttu-id="79ba1-136">Du kan utveckla C#-topologier med SCP.NET med någon av följande versioner av Visual Studio hello:</span><span class="sxs-lookup"><span data-stu-id="79ba1-136">You can develop C# topologies with SCP.NET by using one of hello following versions of Visual Studio:</span></span>

* <span data-ttu-id="79ba1-137">Visual Studio 2012 med [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="79ba1-137">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

* <span data-ttu-id="79ba1-138">Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="79ba1-138">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>

* <span data-ttu-id="79ba1-139">Visual Studio 2015 eller [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span><span class="sxs-lookup"><span data-stu-id="79ba1-139">Visual Studio 2015 or [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)</span></span>

* <span data-ttu-id="79ba1-140">2017 för Visual Studio (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="79ba1-140">Visual Studio 2017 (any edition)</span></span>

## <a name="install-data-lake-tools-for-visual-studio"></a><span data-ttu-id="79ba1-141">Installera Data Lake-verktyg för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79ba1-141">Install Data Lake tools for Visual Studio</span></span>

<span data-ttu-id="79ba1-142">tooinstall Data Lake-verktyg för Visual Studio gör hello i [komma igång med Data Lake-verktyg för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="79ba1-142">tooinstall Data Lake tools for Visual Studio, follow hello steps in [Get started using Data Lake tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

## <a name="install-java"></a><span data-ttu-id="79ba1-143">Installera Java</span><span class="sxs-lookup"><span data-stu-id="79ba1-143">Install Java</span></span>

<span data-ttu-id="79ba1-144">När du skickar en Storm-topologi från Visual Studio genererar SCP.NET en zip-fil som innehåller hello topologi och beroenden.</span><span class="sxs-lookup"><span data-stu-id="79ba1-144">When you submit a Storm topology from Visual Studio, SCP.NET generates a zip file that contains hello topology and dependencies.</span></span> <span data-ttu-id="79ba1-145">Java är används toocreate dessa zip-filer, eftersom den använder ett format som är mer kompatibel med Linux-baserade kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-145">Java is used toocreate these zip files, because it uses a format that is more compatible with Linux-based clusters.</span></span>

1. <span data-ttu-id="79ba1-146">Installera hello Java Developer Kit (JDK) 7 eller senare på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="79ba1-146">Install hello Java Developer Kit (JDK) 7 or later on your development environment.</span></span> <span data-ttu-id="79ba1-147">Du kan hämta hello Oracle JDK från [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="79ba1-147">You can get hello Oracle JDK from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span> <span data-ttu-id="79ba1-148">Du kan också använda [andra Java-distributioner](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="79ba1-148">You can also use [other Java distributions](http://openjdk.java.net/).</span></span>

2. <span data-ttu-id="79ba1-149">Hej `JAVA_HOME` miljö variabeln måste punkt toohello katalog som innehåller Java.</span><span class="sxs-lookup"><span data-stu-id="79ba1-149">hello `JAVA_HOME` environment variable must point toohello directory that contains Java.</span></span>

3. <span data-ttu-id="79ba1-150">Hej `PATH` miljövariabeln måste innehålla hello `%JAVA_HOME%\bin` directory.</span><span class="sxs-lookup"><span data-stu-id="79ba1-150">hello `PATH` environment variable must include hello `%JAVA_HOME%\bin` directory.</span></span>

<span data-ttu-id="79ba1-151">Du kan använda följande C#-konsolen programmet tooverify att Java och hello JDK är korrekt installerade hello:</span><span class="sxs-lookup"><span data-stu-id="79ba1-151">You can use hello following C# console application tooverify that Java and hello JDK are correctly installed:</span></span>

```csharp
using System;
using System.IO;
namespace ConsoleApplication2
{
   class Program
   {
       static void Main(string[] args)
       {
           string javaHome = Environment.GetEnvironmentVariable(“JAVA_HOME”);
           if (!string.IsNullOrEmpty(javaHome))
           {
               string jarExe = Path.Combine(javaHome + @”\bin”, “jar.exe”);
               if (File.Exists(jarExe))
               {
                   Console.WriteLine(“JAVA Is Installed properly”);
                    return;
               }
               else
               {
                   Console.WriteLine(“A valid JAVA JDK is not found. Looks like JRE is installed instead of JDK.”);
               }
           }
           else
           {
             Console.WriteLine(“A valid JAVA JDK is not found. JAVA_HOME environment variable is not set.”);
           }
       }  
   }
}
```

## <a name="storm-templates"></a><span data-ttu-id="79ba1-152">Storm-mallar</span><span class="sxs-lookup"><span data-stu-id="79ba1-152">Storm templates</span></span>

<span data-ttu-id="79ba1-153">hello Data Lake-verktyg för Visual Studio innehåller hello följande mallar:</span><span class="sxs-lookup"><span data-stu-id="79ba1-153">hello Data Lake tools for Visual Studio provide hello following templates:</span></span>

| <span data-ttu-id="79ba1-154">Projekttypen</span><span class="sxs-lookup"><span data-stu-id="79ba1-154">Project type</span></span> | <span data-ttu-id="79ba1-155">Visar</span><span class="sxs-lookup"><span data-stu-id="79ba1-155">Demonstrates</span></span> |
| --- | --- |
| <span data-ttu-id="79ba1-156">Storm-program</span><span class="sxs-lookup"><span data-stu-id="79ba1-156">Storm Application</span></span> |<span data-ttu-id="79ba1-157">Ett tomt Storm-topologi projekt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-157">An empty Storm topology project.</span></span> |
| <span data-ttu-id="79ba1-158">Storm Azure SQL Writer exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-158">Storm Azure SQL Writer Sample</span></span> |<span data-ttu-id="79ba1-159">Hur toowrite tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="79ba1-159">How toowrite tooAzure SQL Database.</span></span> |
| <span data-ttu-id="79ba1-160">Storm Azure Cosmos DB Reader-exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-160">Storm Azure Cosmos DB Reader Sample</span></span> |<span data-ttu-id="79ba1-161">Hur tooread från Azure Cosmos-databasen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-161">How tooread from Azure Cosmos DB.</span></span> |
| <span data-ttu-id="79ba1-162">Storm Azure Cosmos DB Writer exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-162">Storm Azure Cosmos DB Writer Sample</span></span> |<span data-ttu-id="79ba1-163">Hur toowrite tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="79ba1-163">How toowrite tooAzure Cosmos DB.</span></span> |
| <span data-ttu-id="79ba1-164">Storm EventHub Reader-exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-164">Storm EventHub Reader Sample</span></span> |<span data-ttu-id="79ba1-165">Hur tooread från Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="79ba1-165">How tooread from Azure Event Hubs.</span></span> |
| <span data-ttu-id="79ba1-166">Storm EventHub Writer exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-166">Storm EventHub Writer Sample</span></span> |<span data-ttu-id="79ba1-167">Hur toowrite tooAzure Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="79ba1-167">How toowrite tooAzure Event Hubs.</span></span> |
| <span data-ttu-id="79ba1-168">Storm HBase Reader-exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-168">Storm HBase Reader Sample</span></span> |<span data-ttu-id="79ba1-169">Hur tooread från HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-169">How tooread from HBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="79ba1-170">Storm HBase Writer exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-170">Storm HBase Writer Sample</span></span> |<span data-ttu-id="79ba1-171">Hur toowrite tooHBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-171">How toowrite tooHBase on HDInsight clusters.</span></span> |
| <span data-ttu-id="79ba1-172">Storm Hybrid-exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-172">Storm Hybrid Sample</span></span> |<span data-ttu-id="79ba1-173">Hur toouse en Java-komponent.</span><span class="sxs-lookup"><span data-stu-id="79ba1-173">How toouse a Java component.</span></span> |
| <span data-ttu-id="79ba1-174">Storm-exempel</span><span class="sxs-lookup"><span data-stu-id="79ba1-174">Storm Sample</span></span> |<span data-ttu-id="79ba1-175">En grundläggande word count-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-175">A basic word count topology.</span></span> |

> [!WARNING]
> <span data-ttu-id="79ba1-176">Inte alla mallar för att fungera med Linux-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="79ba1-176">Not all templates will work with Linux-based HDInsight.</span></span> <span data-ttu-id="79ba1-177">Nuget-paket som används av hello mallar är kanske inte kompatibelt med Mono.</span><span class="sxs-lookup"><span data-stu-id="79ba1-177">Nuget packages used by hello templates may not be compatible with Mono.</span></span> <span data-ttu-id="79ba1-178">Kontrollera hello [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokumentera och använda hello [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="79ba1-178">Check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document and use hello [.NET Portability Analyzer](hdinsight-hadoop-migrate-dotnet-to-linux.md#automated-portability-analysis) tooidentify potential problems.</span></span>

<span data-ttu-id="79ba1-179">I hello stegen i det här dokumentet använder du hello grundläggande Storm-program projektet typen toocreate en topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-179">In hello steps in this document, you use hello basic Storm Application project type toocreate a topology.</span></span>

### <a name="hbase-templates-notes"></a><span data-ttu-id="79ba1-180">HBase mallar anteckningar</span><span class="sxs-lookup"><span data-stu-id="79ba1-180">HBase templates notes</span></span>

<span data-ttu-id="79ba1-181">Hej HBase läsare och skrivare mallar kan du använda hello HBase REST API inte hello HBase Java API toocommunicate med en HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-181">hello HBase reader and writer templates use hello HBase REST API, not hello HBase Java API, toocommunicate with an HBase on HDInsight cluster.</span></span>

### <a name="eventhub-templates-notes"></a><span data-ttu-id="79ba1-182">Anteckningar för EventHub-mallar</span><span class="sxs-lookup"><span data-stu-id="79ba1-182">EventHub templates notes</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79ba1-183">hello Java-baserad EventHub-kanalen komponent som ingår i hello EventHub Reader mallen inte kanske fungerar med Storm på HDInsight version 3.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="79ba1-183">hello Java-based EventHub spout component included with hello EventHub Reader template may not work with Storm on HDInsight version 3.5 or later.</span></span> <span data-ttu-id="79ba1-184">Det finns en uppdaterad version av den här komponenten på [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span><span class="sxs-lookup"><span data-stu-id="79ba1-184">An updated version of this component is available at [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/HDI3.5/lib).</span></span>

<span data-ttu-id="79ba1-185">En exempeltopologi som använder den här komponenten och fungerar med Storm på HDInsight 3.5 finns [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="79ba1-185">For an example topology that uses this component and works with Storm on HDInsight 3.5, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

## <a name="create-a-c-topology"></a><span data-ttu-id="79ba1-186">Skapa en C#-topologi</span><span class="sxs-lookup"><span data-stu-id="79ba1-186">Create a C# topology</span></span>

1. <span data-ttu-id="79ba1-187">Öppna Visual Studio, markera **filen** > **ny**, och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-187">Open Visual Studio, select **File** > **New**, and then select **Project**.</span></span>

2. <span data-ttu-id="79ba1-188">Från hello **nytt projekt** fönstret expanderar **installerad** > **mallar**, och välj **Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-188">From hello **New Project** window, expand **Installed** > **Templates**, and select **Azure Data Lake**.</span></span> <span data-ttu-id="79ba1-189">Välj hello listan över mallar **Storm-program**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-189">From hello list of templates, select **Storm Application**.</span></span> <span data-ttu-id="79ba1-190">Längst ned hello hello-skärmen, ange **WordCount** som hello hello programmets namn.</span><span class="sxs-lookup"><span data-stu-id="79ba1-190">At hello bottom of hello screen, enter **WordCount** as hello name of hello application.</span></span>

    ![Skärmbild av nytt projekt fönster](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

3. <span data-ttu-id="79ba1-192">När du har skapat hello projekt, ska du ha hello följande filer:</span><span class="sxs-lookup"><span data-stu-id="79ba1-192">After you have created hello project, you should have hello following files:</span></span>

   * <span data-ttu-id="79ba1-193">**Program.CS**: den här filen definierar hello topologi för projektet.</span><span class="sxs-lookup"><span data-stu-id="79ba1-193">**Program.cs**: This file defines hello topology for your project.</span></span> <span data-ttu-id="79ba1-194">En standard-topologi som består av en kanal och en bult skapas som standard.</span><span class="sxs-lookup"><span data-stu-id="79ba1-194">A default topology that consists of one spout and one bolt is created by default.</span></span>

   * <span data-ttu-id="79ba1-195">**Spout.CS**: en exempel-kanal som genererar slumptal.</span><span class="sxs-lookup"><span data-stu-id="79ba1-195">**Spout.cs**: An example spout that emits random numbers.</span></span>

   * <span data-ttu-id="79ba1-196">**Bolt.CS**: ett exempel bult som ett antal siffror som sänds av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="79ba1-196">**Bolt.cs**: An example bolt that keeps a count of numbers emitted by hello spout.</span></span>

     <span data-ttu-id="79ba1-197">När du skapar hello projekt NuGet hämtningar hello senaste [SCP.NET paketet](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span><span class="sxs-lookup"><span data-stu-id="79ba1-197">When you create hello project, NuGet downloads hello latest [SCP.NET package](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/).</span></span>

     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

### <a name="implement-hello-spout"></a><span data-ttu-id="79ba1-198">Implementera hello kanal</span><span class="sxs-lookup"><span data-stu-id="79ba1-198">Implement hello spout</span></span>

1. <span data-ttu-id="79ba1-199">Öppna **Spout.cs**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-199">Open **Spout.cs**.</span></span> <span data-ttu-id="79ba1-200">Kanaler är används tooread data i en topologi från en extern källa.</span><span class="sxs-lookup"><span data-stu-id="79ba1-200">Spouts are used tooread data in a topology from an external source.</span></span> <span data-ttu-id="79ba1-201">hello huvudkomponenterna för en kanal är:</span><span class="sxs-lookup"><span data-stu-id="79ba1-201">hello main components for a spout are:</span></span>

   * <span data-ttu-id="79ba1-202">**NextTuple**: anropas av Storm när hello kanal tillåts tooemit nya tupplar.</span><span class="sxs-lookup"><span data-stu-id="79ba1-202">**NextTuple**: Called by Storm when hello spout is allowed tooemit new tuples.</span></span>

   * <span data-ttu-id="79ba1-203">**Ack** (endast transaktionella topologi): hanterar bekräftelser som initieras av andra komponenter i hello-topologi för tupplar skickas från hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="79ba1-203">**Ack** (transactional topology only): Handles acknowledgements initiated by other components in hello topology for tuples sent from hello spout.</span></span> <span data-ttu-id="79ba1-204">Bekräfta en tuppel kan hello kanal vet att den har bearbetats av underordnade komponenter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-204">Acknowledging a tuple lets hello spout know that it was processed successfully by downstream components.</span></span>

   * <span data-ttu-id="79ba1-205">**Misslyckas** (endast transaktionella topologi): hanterar tupplar som misslyckas bearbetning av andra komponenter i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-205">**Fail** (transactional topology only): Handles tuples that are fail-processing other components in hello topology.</span></span> <span data-ttu-id="79ba1-206">Implementera en metod som misslyckas kan du toore-genererar hello tuppel så att den kan bearbetas igen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-206">Implementing a Fail method allows you toore-emit hello tuple so that it can be processed again.</span></span>

2. <span data-ttu-id="79ba1-207">Ersätt hello innehållet i hello **prata** klassen med följande text hello.</span><span class="sxs-lookup"><span data-stu-id="79ba1-207">Replace hello contents of hello **Spout** class with hello following text.</span></span> <span data-ttu-id="79ba1-208">Den här kanal avger slumpmässigt en mening i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-208">This spout randomly emits a sentence into hello topology.</span></span>

    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "hello cow jumped over hello moon",
        "an apple a day keeps hello doctor away",
        "four score and seven years ago",
        "snow white and hello seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set hello instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // hello schema for hello default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of hello spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // hello sentence toobe emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```

### <a name="implement-hello-bolts"></a><span data-ttu-id="79ba1-209">Implementera hello bultar</span><span class="sxs-lookup"><span data-stu-id="79ba1-209">Implement hello bolts</span></span>

1. <span data-ttu-id="79ba1-210">Ta bort befintliga hello **Bolt.cs** filen från hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-210">Delete hello existing **Bolt.cs** file from hello project.</span></span>

2. <span data-ttu-id="79ba1-211">I **Solution Explorer**, högerklicka på hello-projektet och välj **Lägg till** > **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-211">In **Solution Explorer**, right-click hello project, and select **Add** > **New item**.</span></span> <span data-ttu-id="79ba1-212">Välj hello listan **Storm bult**, och ange **Splitter.cs** som hello namn.</span><span class="sxs-lookup"><span data-stu-id="79ba1-212">From hello list, select **Storm Bolt**, and enter **Splitter.cs** as hello name.</span></span> <span data-ttu-id="79ba1-213">Upprepa den här processen toocreate andra bulten med namnet **Counter.cs**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-213">Repeat this process toocreate a second bolt named **Counter.cs**.</span></span>

   * <span data-ttu-id="79ba1-214">**Splitter.CS**: implementerar en bult som delar upp meningar i enskilda ord och genererar en ny dataström ord.</span><span class="sxs-lookup"><span data-stu-id="79ba1-214">**Splitter.cs**: Implements a bolt that splits sentences into individual words, and emits a new stream of words.</span></span>

   * <span data-ttu-id="79ba1-215">**Counter.CS**: implementerar en bult som räknar varje ord och genererar en ny dataström ord och hello antalet för varje ord.</span><span class="sxs-lookup"><span data-stu-id="79ba1-215">**Counter.cs**: Implements a bolt that counts each word, and emits a new stream of words and hello count for each word.</span></span>

     > [!NOTE]
     > <span data-ttu-id="79ba1-216">Dessa bultar läsa och skriva toostreams, men du kan också använda en bult toocommunicate med källor, till exempel en databas eller tjänst.</span><span class="sxs-lookup"><span data-stu-id="79ba1-216">These bolts read and write toostreams, but you can also use a bolt toocommunicate with sources such as a database or service.</span></span>

3. <span data-ttu-id="79ba1-217">Öppna **Splitter.cs**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-217">Open **Splitter.cs**.</span></span> <span data-ttu-id="79ba1-218">Det har endast en metod som standard: **kör**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-218">It has only one method by default: **Execute**.</span></span> <span data-ttu-id="79ba1-219">hello Execute-metoden anropas när hello bult tar emot en tuppel för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="79ba1-219">hello Execute method is called when hello bolt receives a tuple for processing.</span></span> <span data-ttu-id="79ba1-220">Här kan du läsa och bearbeta inkommande tupplar och generera utgående tupplar.</span><span class="sxs-lookup"><span data-stu-id="79ba1-220">Here, you can read and process incoming tuples, and emit outbound tuples.</span></span>

4. <span data-ttu-id="79ba1-221">Ersätt hello innehållet i hello **delningslisten** klassen med följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="79ba1-221">Replace hello contents of hello **Splitter** class with hello following code:</span></span>

    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (hello sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (hello word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of hello bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello sentence from hello tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```

5. <span data-ttu-id="79ba1-222">Öppna **Counter.cs**, och Ersätt hello klassen innehållet med hello följande:</span><span class="sxs-lookup"><span data-stu-id="79ba1-222">Open **Counter.cs**, and replace hello class contents with hello following:</span></span>

    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - hello word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - hello word and hello word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get hello word from hello tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for hello word in hello dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment hello count
        count++;
        // Update hello count in hello dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit hello word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```

### <a name="define-hello-topology"></a><span data-ttu-id="79ba1-223">Definiera hello-topologi</span><span class="sxs-lookup"><span data-stu-id="79ba1-223">Define hello topology</span></span>

<span data-ttu-id="79ba1-224">Kanaler och bultar ordnas i ett diagram som definierar hur hello data flödar mellan komponenter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-224">Spouts and bolts are arranged in a graph, which defines how hello data flows between components.</span></span> <span data-ttu-id="79ba1-225">För den här topologin är hello diagrammet:</span><span class="sxs-lookup"><span data-stu-id="79ba1-225">For this topology, hello graph is as follows:</span></span>

![Diagram över hur komponenter ordnas](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

<span data-ttu-id="79ba1-227">Meningar orsakat från hello kanal och är distribuerade tooinstances av hello delningslisten bulten.</span><span class="sxs-lookup"><span data-stu-id="79ba1-227">Sentences are emitted from hello spout, and are distributed tooinstances of hello Splitter bolt.</span></span> <span data-ttu-id="79ba1-228">hello delningslisten bult delar hello meningar i ord som är distribuerade toohello räknaren bulten.</span><span class="sxs-lookup"><span data-stu-id="79ba1-228">hello Splitter bolt breaks hello sentences into words, which are distributed toohello Counter bolt.</span></span>

<span data-ttu-id="79ba1-229">Eftersom ordräkning lagras lokalt i hello räknarinstansen, vi vill toomake till att specifika ord flöda toohello samma räknarinstansen bulten.</span><span class="sxs-lookup"><span data-stu-id="79ba1-229">Because word count is held locally in hello Counter instance, we want toomake sure that specific words flow toohello same Counter bolt instance.</span></span> <span data-ttu-id="79ba1-230">Varje instans håller reda på specifika ord.</span><span class="sxs-lookup"><span data-stu-id="79ba1-230">Each instance keeps track of specific words.</span></span> <span data-ttu-id="79ba1-231">Eftersom hello delningslisten bult upprätthåller inget tillstånd, det verkligen spelar ingen roll vilken instans av hello delningslisten tar emot vilka meningen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-231">Since hello Splitter bolt maintains no state, it really doesn't matter which instance of hello splitter receives which sentence.</span></span>

<span data-ttu-id="79ba1-232">Öppna **Program.cs**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-232">Open **Program.cs**.</span></span> <span data-ttu-id="79ba1-233">viktiga hello-metoden är **GetTopologyBuilder**, som har använt toodefine hello-topologi som har skickats tooStorm.</span><span class="sxs-lookup"><span data-stu-id="79ba1-233">hello important method is **GetTopologyBuilder**, which is used toodefine hello topology that is submitted tooStorm.</span></span> <span data-ttu-id="79ba1-234">Ersätt hello innehållet i **GetTopologyBuilder** med hello följande kod tooimplement hello-topologi som beskrivs ovan:</span><span class="sxs-lookup"><span data-stu-id="79ba1-234">Replace hello contents of **GetTopologyBuilder** with hello following code tooimplement hello topology described previously:</span></span>

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add hello spout toohello topology.
// Name hello component 'sentences'
// Name hello field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add hello splitter bolt toohello topology.
// Name hello component 'splitter'
// Name hello field that is emitted 'word'
// Use suffleGrouping toodistribute incoming tuples
//   from hello 'sentences' spout across instances
//   of hello splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add hello counter bolt toohello topology.
// Name hello component 'counter'
// Name hello fields that are emitted 'word' and 'count'
// Use fieldsGrouping tooensure that tuples are routed
//   toocounter instances based on hello contents of field
//   position 0 (hello word). This could also have been
//   List<string>(){"word"}.
//   This ensures that hello word 'jumped', for example, will always
//   go toohello same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

## <a name="submit-hello-topology"></a><span data-ttu-id="79ba1-235">Skicka hello-topologi</span><span class="sxs-lookup"><span data-stu-id="79ba1-235">Submit hello topology</span></span>

1. <span data-ttu-id="79ba1-236">I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-236">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="79ba1-237">Om du uppmanas du ange hello autentiseringsuppgifter för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="79ba1-237">If prompted, enter hello credentials for your Azure subscription.</span></span> <span data-ttu-id="79ba1-238">Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-238">If you have more than one subscription, sign in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="79ba1-239">Välj ditt Storm på HDInsight-kluster från hello **Storm-kluster** listrutan och välj sedan **skicka**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-239">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="79ba1-240">Du kan övervaka om hello ansökan har genomförts med hello **utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-240">You can monitor if hello submission is successful by using hello **Output** window.</span></span>

3. <span data-ttu-id="79ba1-241">När hello-topologi har skickats, hello **Storm-topologier** för hello klustret ska visas.</span><span class="sxs-lookup"><span data-stu-id="79ba1-241">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="79ba1-242">Välj hello **WordCount** topologi från hello listan tooview information om hello kör topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-242">Select hello **WordCount** topology from hello list tooview information about hello running topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="79ba1-243">Du kan också visa **Storm-topologier** från **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-243">You can also view **Storm Topologies** from **Server Explorer**.</span></span> <span data-ttu-id="79ba1-244">Expandera **Azure** > **HDInsight**, högerklicka på ett Storm på HDInsight-kluster och välj sedan **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-244">Expand **Azure** > **HDInsight**, right-click a Storm on HDInsight cluster, and then select **View Storm Topologies**.</span></span>

    <span data-ttu-id="79ba1-245">tooview information om hello komponenter i hello topologi dubbelklicka hello-komponenten i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="79ba1-245">tooview information about hello components in hello topology, double-click hello component in hello diagram.</span></span>

4. <span data-ttu-id="79ba1-246">Från hello **Topology Summary** klickar du på **Kill** toostop hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-246">From hello **Topology Summary** view, click **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="79ba1-247">Storm-topologier fortsätta toorun förrän de inaktiveras eller hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="79ba1-247">Storm topologies continue toorun until they are deactivated, or hello cluster is deleted.</span></span>

## <a name="transactional-topology"></a><span data-ttu-id="79ba1-248">Transaktionell topologi</span><span class="sxs-lookup"><span data-stu-id="79ba1-248">Transactional topology</span></span>

<span data-ttu-id="79ba1-249">hello tidigare topologin är icke-transaktionell.</span><span class="sxs-lookup"><span data-stu-id="79ba1-249">hello previous topology is non-transactional.</span></span> <span data-ttu-id="79ba1-250">hello komponenter i hello topologi implementeras inte funktionen tooreplaying meddelanden.</span><span class="sxs-lookup"><span data-stu-id="79ba1-250">hello components in hello topology do not implement functionality tooreplaying messages.</span></span> <span data-ttu-id="79ba1-251">Ett exempel på en transaktionell topologi, skapa ett projekt och välj **Storm exempel** som hello projekttypen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-251">For an example of a transactional topology, create a project and select **Storm Sample** as hello project type.</span></span>

<span data-ttu-id="79ba1-252">Transaktionell topologier implementera hello följande toosupport omsändning av data:</span><span class="sxs-lookup"><span data-stu-id="79ba1-252">Transactional topologies implement hello following toosupport replay of data:</span></span>

* <span data-ttu-id="79ba1-253">**Cachelagring av metadata**: hello kanal måste lagra metadata om hello data som sänds, så att hello data kan hämtas och orsakat igen om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="79ba1-253">**Metadata caching**: hello spout must store metadata about hello data emitted, so that hello data can be retrieved and emitted again if a failure occurs.</span></span> <span data-ttu-id="79ba1-254">Eftersom hello data som sänds av hello exempel är liten lagras hello rådata för varje tuppel i en ordlista för att svarsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="79ba1-254">Because hello data emitted by hello sample is small, hello raw data for each tuple is stored in a dictionary for replay.</span></span>

* <span data-ttu-id="79ba1-255">**Ack**: varje bult i hello-topologi kan anropa `this.ctx.Ack(tuple)` tooacknowledge att den har bearbetat en tuppel.</span><span class="sxs-lookup"><span data-stu-id="79ba1-255">**Ack**: Each bolt in hello topology can call `this.ctx.Ack(tuple)` tooacknowledge that it has successfully processed a tuple.</span></span> <span data-ttu-id="79ba1-256">När alla bultar har ADE hello tuppel, hello `Ack` hello kanal-metoden har anropats.</span><span class="sxs-lookup"><span data-stu-id="79ba1-256">When all bolts have acked hello tuple, hello `Ack` method of hello spout is invoked.</span></span> <span data-ttu-id="79ba1-257">Hej `Ack` metoden kan hello kanal tooremove data som har lagrats i cacheminnet för att svarsidentifiering.</span><span class="sxs-lookup"><span data-stu-id="79ba1-257">hello `Ack` method allows hello spout tooremove data that was cached for replay.</span></span>

* <span data-ttu-id="79ba1-258">**Misslyckas**: varje bult kan anropa `this.ctx.Fail(tuple)` tooindicate bearbetningen misslyckades för en tuppel.</span><span class="sxs-lookup"><span data-stu-id="79ba1-258">**Fail**: Each bolt can call `this.ctx.Fail(tuple)` tooindicate that processing has failed for a tuple.</span></span> <span data-ttu-id="79ba1-259">hello fel sprider toohello `Fail` metod för hello kanal, där hello tuppel spelas upp med hjälp av cachelagrade metadata.</span><span class="sxs-lookup"><span data-stu-id="79ba1-259">hello failure propagates toohello `Fail` method of hello spout, where hello tuple can be replayed by using cached metadata.</span></span>

* <span data-ttu-id="79ba1-260">**Aktivitetssekvensen ID**: när avger en tuppel, en unik sekvens-ID kan anges.</span><span class="sxs-lookup"><span data-stu-id="79ba1-260">**Sequence ID**: When emitting a tuple, a unique sequence ID can be specified.</span></span> <span data-ttu-id="79ba1-261">Det här värdet identifierar hello tuppel för bearbetning av replay (Ack och misslyckas).</span><span class="sxs-lookup"><span data-stu-id="79ba1-261">This value identifies hello tuple for replay (Ack and Fail) processing.</span></span> <span data-ttu-id="79ba1-262">Till exempel hello kanal i hello **Storm exempel** projektet använder hello följande vid sändning av data:</span><span class="sxs-lookup"><span data-stu-id="79ba1-262">For example, hello spout in hello **Storm Sample** project uses hello following when emitting data:</span></span>

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    <span data-ttu-id="79ba1-263">Den här koden skickar en tuppel som innehåller en mening toohello standard dataström med hello sekvens-ID-värde som finns i **lastSeqId**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-263">This code emits a tuple that contains a sentence toohello default stream, with hello sequence ID value contained in **lastSeqId**.</span></span> <span data-ttu-id="79ba1-264">I det här exemplet **lastSeqId** ökas för varje tuppel orsakat.</span><span class="sxs-lookup"><span data-stu-id="79ba1-264">For this example, **lastSeqId** is incremented for every tuple emitted.</span></span>

<span data-ttu-id="79ba1-265">Som visas i hello **Storm exempel** projekt, om en komponent är transaktionell kan anges vid körning, baserat på konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-265">As demonstrated in hello **Storm Sample** project, whether a component is transactional can be set at runtime, based on configuration.</span></span>

## <a name="hybrid-topology-with-c-and-java"></a><span data-ttu-id="79ba1-266">Hybrid-topologi med C# och Java</span><span class="sxs-lookup"><span data-stu-id="79ba1-266">Hybrid topology with C# and Java</span></span>

<span data-ttu-id="79ba1-267">Du kan också använda Data Lake-verktyg för Visual Studio toocreate hybridtopologier, där vissa komponenter är C# och andra är Java.</span><span class="sxs-lookup"><span data-stu-id="79ba1-267">You can also use Data Lake tools for Visual Studio toocreate hybrid topologies, where some components are C# and others are Java.</span></span>

<span data-ttu-id="79ba1-268">Ett exempel på en hybrid-topologi, skapa ett projekt och välj **Storm Hybrid exempel**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-268">For an example of a hybrid topology, create a project and select **Storm Hybrid Sample**.</span></span> <span data-ttu-id="79ba1-269">Den här typen av exemplet visar hello följande begrepp:</span><span class="sxs-lookup"><span data-stu-id="79ba1-269">This sample type demonstrates hello following concepts:</span></span>

* <span data-ttu-id="79ba1-270">**Java-kanal** och **C# bult**: definierade i **HybridTopology_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-270">**Java spout** and **C# bolt**: Defined in **HybridTopology_javaSpout_csharpBolt**.</span></span>

    * <span data-ttu-id="79ba1-271">En transaktionell version har definierats i **HybridTopologyTx_javaSpout_csharpBolt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-271">A transactional version is defined in **HybridTopologyTx_javaSpout_csharpBolt**.</span></span>

* <span data-ttu-id="79ba1-272">**C#-kanal** och **Java bult**: definierade i **HybridTopology_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-272">**C# spout** and **Java bolt**: Defined in **HybridTopology_csharpSpout_javaBolt**.</span></span>

    * <span data-ttu-id="79ba1-273">En transaktionell version har definierats i **HybridTopologyTx_csharpSpout_javaBolt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-273">A transactional version is defined in **HybridTopologyTx_csharpSpout_javaBolt**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="79ba1-274">Den här versionen visas också hur toouse Clojure kod från en textfil som en Java-komponent.</span><span class="sxs-lookup"><span data-stu-id="79ba1-274">This version also demonstrates how toouse Clojure code from a text file as a Java component.</span></span>


<span data-ttu-id="79ba1-275">tooswitch hello topologin som används när hello projektet skickas bara flytta hello `[Active(true)]` instruktionen toohello topologi du vill toouse, innan den skickas toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="79ba1-275">tooswitch hello topology that is used when hello project is submitted, simply move hello `[Active(true)]` statement toohello topology you want toouse, before submitting it toohello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="79ba1-276">Alla hello Java-filer som krävs har angetts som en del av det här projektet i hello **JavaDependency** mapp.</span><span class="sxs-lookup"><span data-stu-id="79ba1-276">All hello Java files that are required are provided as part of this project in hello **JavaDependency** folder.</span></span>

<span data-ttu-id="79ba1-277">Tänk hello följande när du skapar och skickar en hybrid-topologi:</span><span class="sxs-lookup"><span data-stu-id="79ba1-277">Consider hello following when you are creating and submitting a hybrid topology:</span></span>

* <span data-ttu-id="79ba1-278">Du måste använda **JavaComponentConstructor** toocreate en instans av hello Java-klass för en kanal eller en bult.</span><span class="sxs-lookup"><span data-stu-id="79ba1-278">You must use **JavaComponentConstructor** toocreate an instance of hello Java class for a spout or bolt.</span></span>

* <span data-ttu-id="79ba1-279">Du bör använda **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize till eller från Java-komponenter från Java-dataobjekt tooJSON.</span><span class="sxs-lookup"><span data-stu-id="79ba1-279">You should use **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** tooserialize data into or out of Java components from Java objects tooJSON.</span></span>

* <span data-ttu-id="79ba1-280">När hello topologi toohello server, måste du använda hello **ytterligare konfigurationer** alternativet toospecify hello **Java sökvägar**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-280">When submitting hello topology toohello server, you must use hello **Additional configurations** option toospecify hello **Java File paths**.</span></span> <span data-ttu-id="79ba1-281">hello-sökvägen ska vara hello-katalog som innehåller hello JAR-filer som innehåller Java-klasser.</span><span class="sxs-lookup"><span data-stu-id="79ba1-281">hello path specified should be hello directory that contains hello JAR files that contain your Java classes.</span></span>

### <a name="azure-event-hubs"></a><span data-ttu-id="79ba1-282">Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="79ba1-282">Azure Event Hubs</span></span>

<span data-ttu-id="79ba1-283">SCP.NET version 0.9.4.203 införs en ny klass och metod för att arbeta med hello Event Hub kanal (en Java kanal som läser från Händelsehubbar).</span><span class="sxs-lookup"><span data-stu-id="79ba1-283">SCP.NET version 0.9.4.203 introduces a new class and method specifically for working with hello Event Hub spout (a Java spout that reads from Event Hubs).</span></span> <span data-ttu-id="79ba1-284">När du skapar en topologi som använder en Event Hub-kanal, Använd hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="79ba1-284">When you create a topology that uses an Event Hub spout, use hello following methods:</span></span>

* <span data-ttu-id="79ba1-285">**EventHubSpoutConfig** klass: skapar ett objekt som innehåller hello konfiguration för komponenten för hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="79ba1-285">**EventHubSpoutConfig** class: Creates an object that contains hello configuration for hello spout component.</span></span>

* <span data-ttu-id="79ba1-286">**TopologyBuilder.SetEventHubSpout** metod: lägger till hello Event Hub kanal komponenten toohello topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-286">**TopologyBuilder.SetEventHubSpout** method: Adds hello Event Hub spout component toohello topology.</span></span>

> [!NOTE]
> <span data-ttu-id="79ba1-287">Du måste fortfarande använda hello **CustomizedInteropJSONSerializer** tooserialize data som produceras av hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="79ba1-287">You must still use hello **CustomizedInteropJSONSerializer** tooserialize data produced by hello spout.</span></span>

## <span data-ttu-id="79ba1-288"><a id="configurationmanager"></a>Använd ConfigurationManager</span><span class="sxs-lookup"><span data-stu-id="79ba1-288"><a id="configurationmanager"></a>Use ConfigurationManager</span></span>

<span data-ttu-id="79ba1-289">Använd inte **ConfigurationManager** tooretrieve configuration värden från bultar och prata komponenter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-289">Don't use **ConfigurationManager** tooretrieve configuration values from bolt and spout components.</span></span> <span data-ttu-id="79ba1-290">Detta kan orsaka ett undantag för null-pekare.</span><span class="sxs-lookup"><span data-stu-id="79ba1-290">Doing so can cause a null pointer exception.</span></span> <span data-ttu-id="79ba1-291">I stället skickas hello konfiguration för ditt projekt i hello Storm-topologi som en nyckel och värde-par i kontexten för hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-291">Instead, hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="79ba1-292">Varje komponent som förlitar sig på konfigurationsvärden måste hämta dem från hello kontext under initieringen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-292">Each component that relies on configuration values must retrieve them from hello context during initialization.</span></span>

<span data-ttu-id="79ba1-293">hello följande kod visar hur tooretrieve dessa värden:</span><span class="sxs-lookup"><span data-stu-id="79ba1-293">hello following code demonstrates how tooretrieve these values:</span></span>

```csharp
public class MyComponent : ISCPBolt
{
    // toohold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of hello context for this component instance
        this.ctx = ctx;
        // If it exists, load hello configuration for hello component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve hello value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

<span data-ttu-id="79ba1-294">Om du använder en `Get` metoden tooreturn en instans av komponenten, måste du se till att den skickar båda hello `Context` och `Dictionary<string, Object>` parametrar toohello konstruktor.</span><span class="sxs-lookup"><span data-stu-id="79ba1-294">If you use a `Get` method tooreturn an instance of your component, you must ensure that it passes both hello `Context` and `Dictionary<string, Object>` parameters toohello constructor.</span></span> <span data-ttu-id="79ba1-295">hello följande exempel är en grundläggande `Get` metod som korrekt skickar dessa värden:</span><span class="sxs-lookup"><span data-stu-id="79ba1-295">hello following example is a basic `Get` method that properly passes these values:</span></span>

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-tooupdate-scpnet"></a><span data-ttu-id="79ba1-296">Hur tooupdate SCP.NET</span><span class="sxs-lookup"><span data-stu-id="79ba1-296">How tooupdate SCP.NET</span></span>

<span data-ttu-id="79ba1-297">Nya versioner av SCP.NET stöd för uppgradering av paketet via NuGet.</span><span class="sxs-lookup"><span data-stu-id="79ba1-297">Recent releases of SCP.NET support package upgrade through NuGet.</span></span> <span data-ttu-id="79ba1-298">När en ny uppdatering är tillgänglig kan få du ett meddelande om uppgraderingen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-298">When a new update is available, you receive an upgrade notification.</span></span> <span data-ttu-id="79ba1-299">toomanually Sök efter en uppgradering, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="79ba1-299">toomanually check for an upgrade, follow these steps:</span></span>

1. <span data-ttu-id="79ba1-300">I **Solution Explorer**, högerklicka på hello-projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-300">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>

2. <span data-ttu-id="79ba1-301">Välj hello Pakethanteraren **uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-301">From hello package manager, select **Updates**.</span></span> <span data-ttu-id="79ba1-302">Om en uppdatering är tillgänglig, visas.</span><span class="sxs-lookup"><span data-stu-id="79ba1-302">If an update is available, it is listed.</span></span> <span data-ttu-id="79ba1-303">Klicka på **uppdatering** för hello paketet tooinstall den.</span><span class="sxs-lookup"><span data-stu-id="79ba1-303">Click **Update** for hello package tooinstall it.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="79ba1-304">Om projektet har skapats med en tidigare version av SCP.NET som inte har använt NuGet, måste du utföra hello följande steg tooupdate tooa nyare version:</span><span class="sxs-lookup"><span data-stu-id="79ba1-304">If your project was created with an earlier version of SCP.NET that did not use NuGet, you must perform hello following steps tooupdate tooa newer version:</span></span>
>
> 1. <span data-ttu-id="79ba1-305">I **Solution Explorer**, högerklicka på hello-projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-305">In **Solution Explorer**, right-click hello project, and select **Manage NuGet Packages**.</span></span>
> 2. <span data-ttu-id="79ba1-306">Med hjälp av hello **Sök** fältet, söka efter och Lägg sedan till **Microsoft.SCP.Net.SDK** toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-306">Using hello **Search** field, search for, and then add, **Microsoft.SCP.Net.SDK** toohello project.</span></span>

## <a name="troubleshoot-common-issues-with-topologies"></a><span data-ttu-id="79ba1-307">Felsöka vanliga problem med topologier</span><span class="sxs-lookup"><span data-stu-id="79ba1-307">Troubleshoot common issues with topologies</span></span>

### <a name="null-pointer-exceptions"></a><span data-ttu-id="79ba1-308">Undantag för null-pekare</span><span class="sxs-lookup"><span data-stu-id="79ba1-308">Null pointer exceptions</span></span>

<span data-ttu-id="79ba1-309">När du använder en C#-topologi med en Linux-baserade HDInsight-kluster, bultar och prata komponenter som använder **ConfigurationManager** tooread konfigurationsinställningar vid körning kan returnera null-pekare undantag.</span><span class="sxs-lookup"><span data-stu-id="79ba1-309">When you are using a C# topology with a Linux-based HDInsight cluster, bolt and spout components that use **ConfigurationManager** tooread configuration settings at runtime may return null pointer exceptions.</span></span>

<span data-ttu-id="79ba1-310">hello konfiguration för ditt projekt skickas till hello Storm-topologi som en nyckel och värde-par i kontexten för hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-310">hello configuration for your project is passed into hello Storm topology as a key and value pair in hello topology context.</span></span> <span data-ttu-id="79ba1-311">De kan hämtas från hello Ordlisteobjekt som skickas tooyour komponenter när de har initierats.</span><span class="sxs-lookup"><span data-stu-id="79ba1-311">It can be retrieved from hello dictionary object that is passed tooyour components when they are initialized.</span></span>

<span data-ttu-id="79ba1-312">Mer information finns i hello [ConfigurationManager](#configurationmanager) i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="79ba1-312">For more information, see hello [ConfigurationManager](#configurationmanager) section of this document.</span></span>

### <a name="systemtypeloadexception"></a><span data-ttu-id="79ba1-313">System.TypeLoadException</span><span class="sxs-lookup"><span data-stu-id="79ba1-313">System.TypeLoadException</span></span>

<span data-ttu-id="79ba1-314">Hello följande fel kan uppstå när du använder en C#-topologi med en Linux-baserade HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="79ba1-314">When you are using a C# topology with a Linux-based HDInsight cluster, you may encounter hello following error:</span></span>

    System.TypeLoadException: Failure has occurred while loading a type.

<span data-ttu-id="79ba1-315">Det här felet uppstår när du använder en binärfil som inte är kompatibel med hello version av .NET som har stöd för Mono.</span><span class="sxs-lookup"><span data-stu-id="79ba1-315">This error occurs when you use a binary that is not compatible with hello version of .NET that Mono supports.</span></span>

<span data-ttu-id="79ba1-316">För Linux-baserade HDInsight-kluster, kontrollerar du att projektet använder binärfiler som kompilerats för .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="79ba1-316">For Linux-based HDInsight clusters, make sure that your project uses binaries compiled for .NET 4.5.</span></span>

### <a name="test-a-topology-locally"></a><span data-ttu-id="79ba1-317">Testa en topologi lokalt</span><span class="sxs-lookup"><span data-stu-id="79ba1-317">Test a topology locally</span></span>

<span data-ttu-id="79ba1-318">Även om det är enkelt toodeploy ett topologi tooa kluster, i vissa fall kan behöva du tootest en topologi lokalt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-318">Although it is easy toodeploy a topology tooa cluster, in some cases, you may need tootest a topology locally.</span></span> <span data-ttu-id="79ba1-319">Använd följande steg toorun hello och testa hello exempeltopologi i den här självstudiekursen lokalt i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="79ba1-319">Use hello following steps toorun and test hello example topology in this tutorial locally in your development environment.</span></span>

> [!WARNING]
> <span data-ttu-id="79ba1-320">Lokal testning fungerar bara för basic och C#-topologier endast.</span><span class="sxs-lookup"><span data-stu-id="79ba1-320">Local testing only works for basic, C#-only topologies.</span></span> <span data-ttu-id="79ba1-321">Du kan inte använda lokala testar hybridtopologier eller topologier med flera strömmar.</span><span class="sxs-lookup"><span data-stu-id="79ba1-321">You cannot use local testing for hybrid topologies or topologies that use multiple streams.</span></span>

1. <span data-ttu-id="79ba1-322">I **Solution Explorer**, högerklicka på hello-projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-322">In **Solution Explorer**, right-click hello project, and select **Properties**.</span></span> <span data-ttu-id="79ba1-323">Ändra hello i hello projektegenskaperna **utdata typen** för**konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-323">In hello project properties, change hello **Output type** too**Console Application**.</span></span>

    ![Skärmbild av projektegenskaperna med utdatatypen markerat](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

   > [!NOTE]
   > <span data-ttu-id="79ba1-325">Kom ihåg toochange hello **utdata typen** tillbaka för**klassbiblioteket** innan du distribuerar hello topologi tooa klustret.</span><span class="sxs-lookup"><span data-stu-id="79ba1-325">Remember toochange hello **Output type** back too**Class Library** before you deploy hello topology tooa cluster.</span></span>

2. <span data-ttu-id="79ba1-326">I **Solution Explorer**, högerklicka på hello-projektet och välj sedan **Lägg till** > **nytt objekt**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-326">In **Solution Explorer**, right-click hello project, and then select **Add** > **New Item**.</span></span> <span data-ttu-id="79ba1-327">Välj **klassen**, och ange **LocalTest.cs** som hello klassnamn.</span><span class="sxs-lookup"><span data-stu-id="79ba1-327">Select **Class**, and enter **LocalTest.cs** as hello class name.</span></span> <span data-ttu-id="79ba1-328">Klicka slutligen på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-328">Finally, click **Add**.</span></span>

3. <span data-ttu-id="79ba1-329">Öppna **LocalTest.cs**, och Lägg till följande hello **med** uttryck överst hello:</span><span class="sxs-lookup"><span data-stu-id="79ba1-329">Open **LocalTest.cs**, and add hello following **using** statement at hello top:</span></span>

    ```csharp
    using Microsoft.SCP;
    ```

4. <span data-ttu-id="79ba1-330">Använd hello följande kod som hello hello **LocalTest** klass:</span><span class="sxs-lookup"><span data-stu-id="79ba1-330">Use hello following code as hello contents of hello **LocalTest** class:</span></span>

    ```csharp
    // Drives hello topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test hello spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of hello spout, using hello local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext toopersist hello data stream toofile
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test hello splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set hello data stream toohello data created by hello spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test hello counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used tooinitialize
            // components in hello development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of hello bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set hello data stream toohello data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from hello stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in hello batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext toopersist hello data stream toofile
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    <span data-ttu-id="79ba1-331">Ta en stund tooread via hello kodkommentarer.</span><span class="sxs-lookup"><span data-stu-id="79ba1-331">Take a moment tooread through hello code comments.</span></span> <span data-ttu-id="79ba1-332">Den här koden använder **LocalContext** toorun hello komponenter i hello development environment och den kvarstår hello dataströmmen mellan komponenter tootext filer på lokala hello-enheten.</span><span class="sxs-lookup"><span data-stu-id="79ba1-332">This code uses **LocalContext** toorun hello components in hello development environment, and it persists hello data stream between components tootext files on hello local drive.</span></span>

1. <span data-ttu-id="79ba1-333">Öppna **Program.cs**, och Lägg till följande toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="79ba1-333">Open **Program.cs**, and add hello following toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize hello runtime
    SCPRuntime.Initialize();

    //If we are not running under hello local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. <span data-ttu-id="79ba1-334">Spara hello ändringar och klicka sedan på **F5** eller välj **felsöka** > **Start Debugging** toostart hello projektet.</span><span class="sxs-lookup"><span data-stu-id="79ba1-334">Save hello changes, and then click **F5** or select **Debug** > **Start Debugging** toostart hello project.</span></span> <span data-ttu-id="79ba1-335">Ett konsolfönster ska visas och logga status som hello testerna pågår.</span><span class="sxs-lookup"><span data-stu-id="79ba1-335">A console window should appear, and log status as hello tests progress.</span></span> <span data-ttu-id="79ba1-336">När **testerna slutförda** visas trycker du på alla viktiga tooclose hello fönster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-336">When **Tests finished** appears, press any key tooclose hello window.</span></span>

3. <span data-ttu-id="79ba1-337">Använd **Windows Explorer** toolocate hello katalog som innehåller ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-337">Use **Windows Explorer** toolocate hello directory that contains your project.</span></span> <span data-ttu-id="79ba1-338">Till exempel: **C:\Users\<användarnamn > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-338">For example: **C:\Users\<your_user_name>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**.</span></span> <span data-ttu-id="79ba1-339">Öppna i den här katalogen, **Bin**, och klicka sedan på **felsöka**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-339">In this directory, open **Bin**, and then click **Debug**.</span></span> <span data-ttu-id="79ba1-340">Du bör se hello textfiler som skapas när hello tester kördes: sentences.txt, counter.txt och splitter.txt.</span><span class="sxs-lookup"><span data-stu-id="79ba1-340">You should see hello text files that were produced when hello tests ran: sentences.txt, counter.txt, and splitter.txt.</span></span> <span data-ttu-id="79ba1-341">Öppna varje textfil och inspektera hello data.</span><span class="sxs-lookup"><span data-stu-id="79ba1-341">Open each text file and inspect hello data.</span></span>

   > [!NOTE]
   > <span data-ttu-id="79ba1-342">Strängdata kvarstår som en matris med decimalvärden i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="79ba1-342">String data persists as an array of decimal values in these files.</span></span> <span data-ttu-id="79ba1-343">Till exempel \[[97,103,111]] i hello **splitter.txt** filen är hello word *och*.</span><span class="sxs-lookup"><span data-stu-id="79ba1-343">For example, \[[97,103,111]] in hello **splitter.txt** file is hello word *and*.</span></span>

> [!NOTE]
> <span data-ttu-id="79ba1-344">Vara säker på att tooset hello **projekttyp** tillbaka för**klassbiblioteket** innan du distribuerar tooa Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-344">Be sure tooset hello **Project type** back too**Class Library** before deploying tooa Storm on HDInsight cluster.</span></span>

### <a name="log-information"></a><span data-ttu-id="79ba1-345">Informationen i felloggen</span><span class="sxs-lookup"><span data-stu-id="79ba1-345">Log information</span></span>

<span data-ttu-id="79ba1-346">Du kan enkelt logga information från din topologi-komponenter med hjälp av `Context.Logger`.</span><span class="sxs-lookup"><span data-stu-id="79ba1-346">You can easily log information from your topology components by using `Context.Logger`.</span></span> <span data-ttu-id="79ba1-347">Hello följande skapas ett informationsmeddelande loggpost:</span><span class="sxs-lookup"><span data-stu-id="79ba1-347">For example, hello following creates an informational log entry:</span></span>

```csharp
Context.Logger.Info("Component started");
```

<span data-ttu-id="79ba1-348">Loggade informationen kan visas från hello **loggen för Hadoop-tjänsten**, som finns i **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-348">Logged information can be viewed from hello **Hadoop Service Log**, which is found in **Server Explorer**.</span></span> <span data-ttu-id="79ba1-349">Expandera hello post för ditt Storm på HDInsight-klustret och expandera sedan **loggen för Hadoop-tjänsten**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-349">Expand hello entry for your Storm on HDInsight cluster, and then expand **Hadoop Service Log**.</span></span> <span data-ttu-id="79ba1-350">Välj slutligen hello log file tooview.</span><span class="sxs-lookup"><span data-stu-id="79ba1-350">Finally, select hello log file tooview.</span></span>

> [!NOTE]
> <span data-ttu-id="79ba1-351">hello loggfilerna lagras i hello Azure storage-konto som används av klustret.</span><span class="sxs-lookup"><span data-stu-id="79ba1-351">hello logs are stored in hello Azure storage account that is used by your cluster.</span></span> <span data-ttu-id="79ba1-352">tooview hello loggar i Visual Studio, måste du logga in toohello Azure-prenumeration som äger hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="79ba1-352">tooview hello logs in Visual Studio, you must sign in toohello Azure subscription that owns hello storage account.</span></span>

### <a name="view-error-information"></a><span data-ttu-id="79ba1-353">Visa information om fel</span><span class="sxs-lookup"><span data-stu-id="79ba1-353">View error information</span></span>

<span data-ttu-id="79ba1-354">tooview fel som uppstått i en topologi som körs med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="79ba1-354">tooview errors that have occurred in a running topology, use hello following steps:</span></span>

1. <span data-ttu-id="79ba1-355">Från **Server Explorer**, högerklicka på hello Storm på HDInsight-kluster och välj **visa Storm-topologier**.</span><span class="sxs-lookup"><span data-stu-id="79ba1-355">From **Server Explorer**, right-click hello Storm on HDInsight cluster, and select **View Storm topologies**.</span></span>

2. <span data-ttu-id="79ba1-356">För hello **prata** och **Bolts**, hello **senaste fel** kolumnen innehåller information om hello senaste fel.</span><span class="sxs-lookup"><span data-stu-id="79ba1-356">For hello **Spout** and **Bolts**, hello **Last Error** column contains information on hello last error.</span></span>

3. <span data-ttu-id="79ba1-357">Välj hello **prata Id** eller **bulten Id** för hello-komponent som har ett fel visas.</span><span class="sxs-lookup"><span data-stu-id="79ba1-357">Select hello **Spout Id** or **Bolt Id** for hello component that has an error listed.</span></span> <span data-ttu-id="79ba1-358">På sidan hello som visas, ytterligare fel information visas i hello **fel** avsnitt på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="79ba1-358">On hello details page that is displayed, additional error information is listed in hello **Errors** section at hello bottom of hello page.</span></span>

4. <span data-ttu-id="79ba1-359">tooobtain mer information, Välj en **Port** från hello **Executors** på hello-sidan, toosee hello Storm worker logg för hello senaste några minuter.</span><span class="sxs-lookup"><span data-stu-id="79ba1-359">tooobtain more information, select a **Port** from hello **Executors** section of hello page, toosee hello Storm worker log for hello last few minutes.</span></span>

### <a name="errors-submitting-topologies"></a><span data-ttu-id="79ba1-360">Fel skickar topologier</span><span class="sxs-lookup"><span data-stu-id="79ba1-360">Errors submitting topologies</span></span>

<span data-ttu-id="79ba1-361">Om det uppstår fel som skickar en topologi tooHDInsight hittar loggar för hello server-komponenter som hanterar topologi skickas på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-361">If you encounter errors submitting a topology tooHDInsight, you can find logs for hello server-side components that handle topology submission on your HDInsight cluster.</span></span> <span data-ttu-id="79ba1-362">tooretrieve dessa loggar, Använd hello följande kommando från en kommandorad:</span><span class="sxs-lookup"><span data-stu-id="79ba1-362">tooretrieve these logs, use hello following command from a command line:</span></span>

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

<span data-ttu-id="79ba1-363">Ersätt __sshuser__ med hello SSH-användarkontot för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-363">Replace __sshuser__ with hello SSH user account for hello cluster.</span></span> <span data-ttu-id="79ba1-364">Ersätt __klusternamn__ med hello namnet hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="79ba1-364">Replace __clustername__ with hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="79ba1-365">Mer information om hur du använder `scp` och `ssh` med HDInsight, se [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="79ba1-365">For more information on using `scp` and `ssh` with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="79ba1-366">Bidrag kan misslyckas av flera skäl:</span><span class="sxs-lookup"><span data-stu-id="79ba1-366">Submissions can fail for multiple reasons:</span></span>

* <span data-ttu-id="79ba1-367">JDK är inte installerat eller är inte i hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="79ba1-367">JDK is not installed or is not in hello path.</span></span>
* <span data-ttu-id="79ba1-368">Nödvändiga beroendena för Java ingår inte i hello bidrag.</span><span class="sxs-lookup"><span data-stu-id="79ba1-368">Required Java dependencies are not included in hello submission.</span></span>
* <span data-ttu-id="79ba1-369">Inkompatibel beroenden.</span><span class="sxs-lookup"><span data-stu-id="79ba1-369">Incompatible dependencies.</span></span>
* <span data-ttu-id="79ba1-370">Dubblettnamn topologi.</span><span class="sxs-lookup"><span data-stu-id="79ba1-370">Duplicate topology names.</span></span>

<span data-ttu-id="79ba1-371">Om hello `hdinsight-scpwebapi.out` loggen innehåller en `FileNotFoundException`, detta kan bero på hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="79ba1-371">If hello `hdinsight-scpwebapi.out` log contains a `FileNotFoundException`, this might be caused by hello following conditions:</span></span>

* <span data-ttu-id="79ba1-372">Hej JDK finns inte i hello sökväg på hello-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="79ba1-372">hello JDK is not in hello path on hello development environment.</span></span> <span data-ttu-id="79ba1-373">Kontrollera att hello JDK installeras i hello development environment och som `%JAVA_HOME%/bin` i hello sökvägen.</span><span class="sxs-lookup"><span data-stu-id="79ba1-373">Verify that hello JDK is installed in hello development environment, and that `%JAVA_HOME%/bin` is in hello path.</span></span>
* <span data-ttu-id="79ba1-374">Du saknar ett Java-beroende.</span><span class="sxs-lookup"><span data-stu-id="79ba1-374">You are missing a Java dependency.</span></span> <span data-ttu-id="79ba1-375">Kontrollera att du inkluderar alla nödvändiga .jar-filer som en del av hello ansökan.</span><span class="sxs-lookup"><span data-stu-id="79ba1-375">Make sure you are including any required .jar files as part of hello submission.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79ba1-376">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79ba1-376">Next steps</span></span>

<span data-ttu-id="79ba1-377">Ett exempel på databearbetning från Händelsehubbar finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="79ba1-377">For an example of processing data from Event Hubs, see [Process events from Azure Event Hubs with Storm on HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span>

<span data-ttu-id="79ba1-378">Ett exempel på en C#-topologi som delar upp dataströmmen data i flera strömmar finns [C# Storm exempel](https://github.com/Blackmist/csharp-storm-example).</span><span class="sxs-lookup"><span data-stu-id="79ba1-378">For an example of a C# topology that splits stream data into multiple streams, see [C# Storm example](https://github.com/Blackmist/csharp-storm-example).</span></span>

<span data-ttu-id="79ba1-379">Mer information om hur du skapar C#-topologier, finns i toodiscover [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span><span class="sxs-lookup"><span data-stu-id="79ba1-379">toodiscover more information about creating C# topologies, see [GitHub](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).</span></span>

<span data-ttu-id="79ba1-380">Flera sätt toowork med HDInsight och mer Storm på HDInsight-exempel finns hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="79ba1-380">For more ways toowork with HDInsight and more Storm on HDInsight samples, see hello following documents:</span></span>

<span data-ttu-id="79ba1-381">**Microsoft SCP.NET**</span><span class="sxs-lookup"><span data-stu-id="79ba1-381">**Microsoft SCP.NET**</span></span>

* [<span data-ttu-id="79ba1-382">Programmeringsguide för SCP</span><span class="sxs-lookup"><span data-stu-id="79ba1-382">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)

<span data-ttu-id="79ba1-383">**Apache Storm på HDInsight**</span><span class="sxs-lookup"><span data-stu-id="79ba1-383">**Apache Storm on HDInsight**</span></span>

* [<span data-ttu-id="79ba1-384">Distribuera och övervaka topologier med Apache Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-384">Deploy and monitor topologies with Apache Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)
* [<span data-ttu-id="79ba1-385">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-385">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

<span data-ttu-id="79ba1-386">**Apache Hadoop i HDInsight**</span><span class="sxs-lookup"><span data-stu-id="79ba1-386">**Apache Hadoop on HDInsight**</span></span>

* [<span data-ttu-id="79ba1-387">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-387">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="79ba1-388">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-388">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="79ba1-389">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-389">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="79ba1-390">**Apache HBase på HDInsight**</span><span class="sxs-lookup"><span data-stu-id="79ba1-390">**Apache HBase on HDInsight**</span></span>

* [<span data-ttu-id="79ba1-391">Komma igång med HBase på HDInsight</span><span class="sxs-lookup"><span data-stu-id="79ba1-391">Getting started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started.md)
