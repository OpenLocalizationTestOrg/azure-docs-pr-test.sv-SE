---
title: aaaUse Apache Storm med Power BI - Azure HDInsight | Microsoft Docs
description: "Skapa en Power BI-rapport med hjälp av data från en C#-topologi som körs på en Apache Storm-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a><span data-ttu-id="e0bcf-103">Använd Power BI toovisualize data från en Apache Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="e0bcf-103">Use Power BI toovisualize data from an Apache Storm topology</span></span>

<span data-ttu-id="e0bcf-104">Powerbi kan du toovisually visa data som rapporter.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-104">Power BI allows you toovisually display data as reports.</span></span> <span data-ttu-id="e0bcf-105">Det här dokumentet innehåller ett exempel på hur toouse Apache Storm på HDInsight toogenerate data för Power BI.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-105">This document provides an example of how toouse Apache Storm on HDInsight toogenerate data for Power BI.</span></span>

> [!NOTE]
> <span data-ttu-id="e0bcf-106">hello stegen i det här dokumentet förlitar sig på en Windows-utvecklingsmiljö med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-106">hello steps in this document rely on a Windows development environment with Visual Studio.</span></span> <span data-ttu-id="e0bcf-107">hello kompilerade projekt kan vara skickade tooa Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-107">hello compiled project can be submitted tooa Linux-based HDInsight cluster.</span></span> <span data-ttu-id="e0bcf-108">Linux-baserade kluster skapas bara efter 10/28/2016 stöd SCP.NET topologier.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-108">Only Linux-based clusters created after 10/28/2016 support SCP.NET topologies.</span></span>
>
> <span data-ttu-id="e0bcf-109">toouse en C#-topologi med en Linux-baserade kluster update hello Microsoft.SCP.Net.SDK NuGet-paketet används av ditt projekt tooversion 0.10.0.6 eller högre.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-109">toouse a C# topology with a Linux-based cluster, update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or higher.</span></span> <span data-ttu-id="e0bcf-110">hello version av hello paketet måste även matcha hello huvudversion av Storm installerad på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-110">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="e0bcf-111">Storm på HDInsight versionerna 3.3 och 3.4 använder till exempel Storm version 0.10.x, medan HDInsight 3.5 använder Storm 1.0.x.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-111">For example, Storm on HDInsight versions 3.3 and 3.4 use Storm version 0.10.x, while HDInsight 3.5 uses Storm 1.0.x.</span></span>
>
> <span data-ttu-id="e0bcf-112">C#-topologier på Linux-baserade kluster måste använda .NET 4.5 och Använd monoljud toorun på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-112">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="e0bcf-113">De flesta saker fungerar.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-113">Most things work.</span></span> <span data-ttu-id="e0bcf-114">Men du bör kontrollera hello [Mono kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokument för potentiella inkompatibiliteter.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-114">However you should check hello [Mono Compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>
>
> <span data-ttu-id="e0bcf-115">En Java-version av det här projektet som fungerar med Linux- eller Windows-baserade HDInsight finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="e0bcf-115">For a Java version of this project, which works with Linux-based or Windows-based HDInsight, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0bcf-116">Krav</span><span class="sxs-lookup"><span data-stu-id="e0bcf-116">Prerequisites</span></span>

* <span data-ttu-id="e0bcf-117">En Azure Active Directory-användare med [Power BI](https://powerbi.com) åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-117">An Azure Active Directory user with [Power BI](https://powerbi.com) access.</span></span>
* <span data-ttu-id="e0bcf-118">Ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-118">An HDInsight cluster.</span></span> <span data-ttu-id="e0bcf-119">Mer information finns i [Kom igång med Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="e0bcf-119">For more information, see [Get started with Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e0bcf-120">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e0bcf-121">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e0bcf-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e0bcf-122">Visual Studio (ett av följande versioner hello)</span><span class="sxs-lookup"><span data-stu-id="e0bcf-122">Visual Studio (one of hello following versions)</span></span>

  * <span data-ttu-id="e0bcf-123">Visual Studio 2012 med [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="e0bcf-123">Visual Studio 2012 with [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>
  * <span data-ttu-id="e0bcf-124">Visual Studio 2013 med [uppdatering 4](http://www.microsoft.com/download/details.aspx?id=44921) eller [Visual Studio Community 2013](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="e0bcf-124">Visual Studio 2013 with [update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span></span>
  * [<span data-ttu-id="e0bcf-125">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="e0bcf-125">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * <span data-ttu-id="e0bcf-126">2017 för Visual Studio (någon utgåva)</span><span class="sxs-lookup"><span data-stu-id="e0bcf-126">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="e0bcf-127">Hej HDInsight Tools för Visual Studio: se [komma igång med hello HDInsight Tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) information om installationsinformation.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-127">hello HDInsight Tools for Visual Studio: See [Get started using hello HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installation information.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="e0bcf-128">Hur det fungerar</span><span class="sxs-lookup"><span data-stu-id="e0bcf-128">How it works</span></span>

<span data-ttu-id="e0bcf-129">Det här exemplet innehåller en C# Storm-topologi som genererar slumpmässigt loggdata för Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="e0bcf-129">This example contains a C# Storm topology that randomly generates Internet Information Services (IIS) log data.</span></span> <span data-ttu-id="e0bcf-130">Dessa data skrivs sedan tooa SQL-databas, och där det är används toogenerate rapporter i Power BI.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-130">This data is then written tooa SQL Database, and from there it is used toogenerate reports in Power BI.</span></span>

<span data-ttu-id="e0bcf-131">hello följande filer implementera hello huvudsakliga funktionerna i det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-131">hello following files implement hello main functionality of this example:</span></span>

* <span data-ttu-id="e0bcf-132">**SqlAzureBolt.cs**: skriver information i hello Storm-topologi tooSQL databas.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-132">**SqlAzureBolt.cs**: Writes information produced in hello Storm topology tooSQL Database.</span></span>
* <span data-ttu-id="e0bcf-133">**IISLogsTable.sql**: hello Transact-SQL-instruktioner används toogenerate hello databas som hello data lagras i.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-133">**IISLogsTable.sql**: hello Transact-SQL statements used toogenerate hello database that hello data is stored in.</span></span>

> [!WARNING]
> <span data-ttu-id="e0bcf-134">Skapa hello tabell i SQL-databasen innan du startar hello-topologi på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-134">Create hello table in SQL Database before starting hello topology on your HDInsight cluster.</span></span>

## <a name="download-hello-example"></a><span data-ttu-id="e0bcf-135">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e0bcf-135">Download hello example</span></span>

<span data-ttu-id="e0bcf-136">Hämta hello [HDInsight C# Storm Power BI exempel](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span><span class="sxs-lookup"><span data-stu-id="e0bcf-136">Download hello [HDInsight C# Storm Power BI example](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span></span> <span data-ttu-id="e0bcf-137">toodownload, antingen förgrening/klona den med hjälp av [git](http://git-scm.com/), eller Använd hello **hämta** länk toodownload en .zip hello-arkivet.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-137">toodownload it, either fork/clone it using [git](http://git-scm.com/), or use hello **Download** link toodownload a .zip of hello archive.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="e0bcf-138">Skapa en databas</span><span class="sxs-lookup"><span data-stu-id="e0bcf-138">Create a database</span></span>

1. <span data-ttu-id="e0bcf-139">toocreate en databas, Använd hello steg i hello [SQL Database-självstudier](../sql-database/sql-database-get-started.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-139">toocreate a database, use hello steps in hello [SQL Database tutorial](../sql-database/sql-database-get-started.md) document.</span></span>

2. <span data-ttu-id="e0bcf-140">Anslut toohello databasen med följande hello stegen i hello [ansluta tooa SQL Database med Visual Studio](../sql-database/sql-database-connect-query.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-140">Connect toohello database by following hello steps in hello [Connect tooa SQL Database with Visual Studio](../sql-database/sql-database-connect-query.md) document.</span></span>

3. <span data-ttu-id="e0bcf-141">I Object Explorer, högerklicka på hello-databasen och välj **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-141">In Object Explorer, right-click hello database and select  **New Query**.</span></span> <span data-ttu-id="e0bcf-142">Klistra in hello innehållet i hello **IISLogsTable.sql** -fil som ingår i hello hämtade projektet till hello frågefönstret och sedan använda Ctrl + Skift + E tooexecute hello frågan.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-142">Paste hello contents of hello **IISLogsTable.sql** file included in hello downloaded project into hello query window, and then use Ctrl + Shift + E tooexecute hello query.</span></span> <span data-ttu-id="e0bcf-143">Du bör få ett meddelande som hello kommandon har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-143">You should receive a message that hello commands completed successfully.</span></span>

## <a name="configure-hello-sample"></a><span data-ttu-id="e0bcf-144">Konfigurera hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e0bcf-144">Configure hello sample</span></span>

1. <span data-ttu-id="e0bcf-145">Från hello [Azure-portalen](https://portal.azure.com), Välj din SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-145">From hello [Azure portal](https://portal.azure.com), select your SQL database.</span></span> <span data-ttu-id="e0bcf-146">Från hello **Essentials** avsnitt i hello SQL-databasbladet väljer **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-146">From hello **Essentials** section of hello SQL database blade, select **Show database connection strings**.</span></span> <span data-ttu-id="e0bcf-147">Kopiera hello hello listan som visas, **ADO.NET (SQL-autentisering)** information.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-147">From hello list that appears, copy hello **ADO.NET (SQL authentication)** information.</span></span>

2. <span data-ttu-id="e0bcf-148">Öppna hello exempel i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-148">Open hello sample in Visual Studio.</span></span> <span data-ttu-id="e0bcf-149">Från **Solution Explorer**öppnar hello **App.config** filen och leta upp följande post hello:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-149">From **Solution Explorer**, open hello **App.config** file, and then find hello following entry:</span></span>

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    <span data-ttu-id="e0bcf-150">Ersätt hello **# TOBEFILLED ##** värdet med hello databasanslutningssträng kopierade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-150">Replace hello **##TOBEFILLED##** value with hello database connection string copied in hello previous step.</span></span> <span data-ttu-id="e0bcf-151">Ersätt **{din\_username}** och **{din\_lösenord}** med hello användarnamn och lösenord för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-151">Replace **{your\_username}** and **{your\_password}** with hello username and password for hello database.</span></span>

3. <span data-ttu-id="e0bcf-152">Spara och Stäng hello-filer.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-152">Save and close hello files.</span></span>

## <a name="deploy-hello-sample"></a><span data-ttu-id="e0bcf-153">Distribuera hello-exempel</span><span class="sxs-lookup"><span data-stu-id="e0bcf-153">Deploy hello sample</span></span>

1. <span data-ttu-id="e0bcf-154">Från **Solution Explorer**, högerklicka på hello **StormToSQL** projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-154">From **Solution Explorer**, right-click hello **StormToSQL** project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="e0bcf-155">Välj hello HDInsight-kluster från hello **Storm-kluster** listrutan dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-155">Select hello HDInsight cluster from hello **Storm Cluster** dropdown dialog.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e0bcf-156">Det kan ta några sekunder efter Hej **Storm-kluster** listrutan toopopulate med servernamn.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-156">It may take a few seconds for hello **Storm Cluster** dropdown toopopulate with server names.</span></span>
   >
   > <span data-ttu-id="e0bcf-157">Om du uppmanas ange hello inloggningsuppgifterna för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-157">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="e0bcf-158">Om du har mer än en prenumeration kan du logga in toohello en som innehåller ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-158">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="e0bcf-159">När hello-topologi har skickats, hello __topologi Viewer__ visas.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-159">When hello topology has been submitted, hello __Topology Viewer__ appears.</span></span> <span data-ttu-id="e0bcf-160">tooview den här topologin, Välj hello SqlAzureWriterTopology tas hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-160">tooview this topology, select hello SqlAzureWriterTopology entry from hello list.</span></span>

    ![hello topologier med hello-topologi som valts](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    <span data-ttu-id="e0bcf-162">Du kan använda den här vyn toosee informationen hello-topologi eller dubbelklicka på en post (till exempel hello SqlAzureBolt) toosee specifika tooa komponenten i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-162">You can use this view toosee information on hello topology, or double-click an entry (such as hello SqlAzureBolt) toosee information specific tooa component in hello topology.</span></span>

3. <span data-ttu-id="e0bcf-163">Efter hello har topologi för några minuter kördes returnerade toohello SQL frågefönstret som du använde toocreate hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-163">After hello topology has ran for a few minutes, return toohello SQL query window you used toocreate hello database.</span></span> <span data-ttu-id="e0bcf-164">Ersätt befintliga hello-instruktioner med hello följande fråga:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-164">Replace hello existing statements with hello following query:</span></span>

        select * from iislogs;

    <span data-ttu-id="e0bcf-165">Använda Ctrl + Skift + E tooexecute hello fråga och du får resultat liknande toohello följande data:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-165">Use Ctrl + Shift + E tooexecute hello query, and you should receive results similar toohello following data:</span></span>

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    <span data-ttu-id="e0bcf-166">Dessa data har skrivits från hello Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-166">This data has been written from hello Storm topology.</span></span>

## <a name="create-a-report"></a><span data-ttu-id="e0bcf-167">Skapa en rapport</span><span class="sxs-lookup"><span data-stu-id="e0bcf-167">Create a report</span></span>

1. <span data-ttu-id="e0bcf-168">Ansluta toohello [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) för Power BI.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-168">Connect toohello [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) for Power BI.</span></span> 

2. <span data-ttu-id="e0bcf-169">Inom **databaser**väljer **hämta**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-169">Within **Databases**, select **Get**.</span></span>

3. <span data-ttu-id="e0bcf-170">Välj **Azure SQL Database**, och välj sedan **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-170">Select **Azure SQL Database**, and then select **Connect**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e0bcf-171">Du kan bli ombedd toodownload hello Power BI Desktop toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-171">You may be asked toodownload hello Power BI Desktop toocontinue.</span></span> <span data-ttu-id="e0bcf-172">I så fall använder du följande steg tooconnect hello:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-172">If so, use hello following steps tooconnect:</span></span>
    >
    > 1. <span data-ttu-id="e0bcf-173">Öppna Power BI Desktop och välj __hämta Data__.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-173">Open Power BI Desktop and select __Get Data__.</span></span>
    > <span data-ttu-id="e0bcf-174">2 Markera __Azure__, och sedan __Azure SQL database__.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-174">2  Select __Azure__, and then __Azure SQL database__.</span></span>

4. <span data-ttu-id="e0bcf-175">Ange hello information tooconnect tooyour Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-175">Enter hello information tooconnect tooyour Azure SQL Database.</span></span> <span data-ttu-id="e0bcf-176">Du hittar den här informationen genom att besöka hello [Azure-portalen](https://portal.azure.com) och välja din SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-176">You can find this information by visiting hello [Azure portal](https://portal.azure.com) and selecting your SQL database.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e0bcf-177">Du kan också ange hello uppdateringsintervall och anpassade filter med hjälp av **aktivera avancerade alternativ** hello ansluta dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-177">You can also set hello refresh interval and custom filters by using **Enable Advanced Options** from hello connect dialog.</span></span>

5. <span data-ttu-id="e0bcf-178">När du har anslutit visas en ny datamängd med samma namn som hello databas du är ansluten till hello.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-178">After you've connected, you will see a new dataset with hello same name as hello database you connected to.</span></span> <span data-ttu-id="e0bcf-179">Välj hello dataset toobegin skapar rapporter.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-179">Select hello dataset toobegin designing a report.</span></span>

6. <span data-ttu-id="e0bcf-180">Från **fält**, expandera hello **IISLOGS** transaktionen.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-180">From **Fields**, expand hello **IISLOGS** entry.</span></span> <span data-ttu-id="e0bcf-181">toocreate en rapport som visar hello URI beror, markera kryssrutan för hello för **URISTEM**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-181">toocreate a report that lists hello URI stems, select hello checkbox for **URISTEM**.</span></span>

    ![Skapa en rapport](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. <span data-ttu-id="e0bcf-183">Dra därefter **metoden** toohello rapporten.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-183">Next, drag **METHOD** toohello report.</span></span> <span data-ttu-id="e0bcf-184">hello rapporten uppdateringar toolist hello beror och hello motsvarande HTTP-metoden för hello HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-184">hello report updates toolist hello stems and hello corresponding HTTP method used for hello HTTP request.</span></span>

    ![lägga till hello Metoddata](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. <span data-ttu-id="e0bcf-186">Från hello **visualiseringar** kolumn, Välj hello **fält** ikon och väljer sedan hello nedpilen bredvid för**metoden** i hello **värden**avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-186">From hello **Visualizations** column, select hello **Fields** icon, and then select hello down arrow next too**METHOD** in hello **Values** section.</span></span> <span data-ttu-id="e0bcf-187">toodisplay antalet hur många gånger en URI har använts, Välj **antal**.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-187">toodisplay a count of how many times a URI has been accessed, select **Count**.</span></span>

    ![Ändra tooa antal metoder](./media/hdinsight-storm-power-bi-topology/count.png)

9. <span data-ttu-id="e0bcf-189">Välj därefter hello **staplat liggande diagram** toochange hur hello information visas.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-189">Next, select hello **Stacked column chart** toochange how hello information is displayed.</span></span>

    ![Ändra tooa staplade diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. <span data-ttu-id="e0bcf-191">toosave hello rapport, Välj **spara** och ange ett namn för hello rapporten.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-191">toosave hello report, select **Save** and enter a name for hello report.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="e0bcf-192">Stoppa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="e0bcf-192">Stop hello topology</span></span>

<span data-ttu-id="e0bcf-193">hello topologi fortsätter toorun tills du stoppa den eller ta bort hello Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-193">hello topology continues toorun until you stop it or delete hello Storm on HDInsight cluster.</span></span> <span data-ttu-id="e0bcf-194">toostop Hej topologi, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-194">toostop hello topology, perform hello following steps:</span></span>

1. <span data-ttu-id="e0bcf-195">I Visual Studio tillbaka toohello topologi viewer och väljer hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-195">In Visual Studio, return toohello topology viewer and select hello topology.</span></span>

2. <span data-ttu-id="e0bcf-196">Välj hello **Kill** knappen toostop hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-196">Select hello **Kill** button toostop hello topology.</span></span>

    ![Avsluta hello topologi sammanfattning-knappen](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="e0bcf-198">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="e0bcf-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="e0bcf-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0bcf-199">Next steps</span></span>

<span data-ttu-id="e0bcf-200">I det här dokumentet du lärt dig hur toosend data från en-databas med Storm-topologi tooSQL visualisera hello data med Power BI.</span><span class="sxs-lookup"><span data-stu-id="e0bcf-200">In this document, you learned how toosend data from a Storm topology tooSQL Database, then visualize hello data using Power BI.</span></span> <span data-ttu-id="e0bcf-201">Mer information om hur toowork med andra Azure tekniker med Storm på HDInsight, se hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="e0bcf-201">For information on how toowork with other Azure technologies using Storm on HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="e0bcf-202">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="e0bcf-202">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
