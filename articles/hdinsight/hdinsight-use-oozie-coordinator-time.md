---
title: aaaUse tidsbaserade Hadoop Oozie-koordinator i HDInsight | Microsoft Docs
description: "Använd tidsbaserade Hadoop Oozie-koordinator i HDInsight, en stordatatjänst. Lär dig hur toodefine Oozie arbetsflöden och koordinatorer, och skicka jobb."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a><span data-ttu-id="2470a-104">Använda en tidsbaserad Oozie-koordinator med Hadoop i HDInsight toodefine arbetsflöden och samordna jobb</span><span class="sxs-lookup"><span data-stu-id="2470a-104">Use time-based Oozie coordinator with Hadoop in HDInsight toodefine workflows and coordinate jobs</span></span>
<span data-ttu-id="2470a-105">I den här artikeln lär du dig hur toodefine arbetsflöden och koordinatorer och hur tootrigger hello coordinator jobb, baserat på tid.</span><span class="sxs-lookup"><span data-stu-id="2470a-105">In this article, you'll learn how toodefine workflows and coordinators, and how tootrigger hello coordinator jobs, based on time.</span></span> <span data-ttu-id="2470a-106">Det är bra toogo via [Använd Oozie med HDInsight] [ hdinsight-use-oozie] innan den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="2470a-106">It is helpful toogo through [Use Oozie with HDInsight][hdinsight-use-oozie] before you read this article.</span></span> <span data-ttu-id="2470a-107">Dessutom tooOozie, du kan också schemalägga jobb med hjälp av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2470a-107">In addition tooOozie, you can also schedule jobs using Azure Data Factory.</span></span> <span data-ttu-id="2470a-108">toolearn Azure Data Factory finns [Use Pig och Hive med Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="2470a-108">toolearn Azure Data Factory, see [Use Pig and Hive with Data Factory](../data-factory/data-factory-data-transformation-activities.md).</span></span>

> [!NOTE]
> <span data-ttu-id="2470a-109">Den här artikeln kräver ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-109">This article requires a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="2470a-110">Information om hur du använder Oozie, inklusive tidsbaserade jobb på en Linux-baserade kluster finns [Använd Oozie med Hadoop toodefine och kör ett arbetsflöde på Linux-baserat HDInsight](hdinsight-use-oozie-linux-mac.md)</span><span class="sxs-lookup"><span data-stu-id="2470a-110">For information on using Oozie, including time-based jobs, on a Linux-based cluster, see [Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight](hdinsight-use-oozie-linux-mac.md)</span></span>

## <a name="what-is-oozie"></a><span data-ttu-id="2470a-111">Vad är Oozie</span><span class="sxs-lookup"><span data-stu-id="2470a-111">What is Oozie</span></span>
<span data-ttu-id="2470a-112">Apache Oozie är ett arbetsflöde/samordning system som hanterar Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="2470a-112">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="2470a-113">Det är integrerat med hello Hadoop-stacken och stöder Hadoop-jobb för Apache MapReduce, Apache Pig, Apache Hive och Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="2470a-113">It is integrated with hello Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="2470a-114">Det kan också vara används tooschedule jobb som är specifika tooa system, till exempel Java-program eller kommandoskript.</span><span class="sxs-lookup"><span data-stu-id="2470a-114">It can also be used tooschedule jobs that are specific tooa system, such as Java programs or shell scripts.</span></span>

<span data-ttu-id="2470a-115">hello visar följande bild hello arbetsflöde som du kommer att implementera:</span><span class="sxs-lookup"><span data-stu-id="2470a-115">hello following image shows hello workflow you will implement:</span></span>

![Arbetsflödesdiagram][img-workflow-diagram]

<span data-ttu-id="2470a-117">hello arbetsflödet innehåller två åtgärder:</span><span class="sxs-lookup"><span data-stu-id="2470a-117">hello workflow contains two actions:</span></span>

1. <span data-ttu-id="2470a-118">En Hive-åtgärden körs en HiveQL-skript toocount hello förekomster av varje loggningsnivån typ i en loggfil för log4j.</span><span class="sxs-lookup"><span data-stu-id="2470a-118">A Hive action runs a HiveQL script toocount hello occurrences of each log-level type in a log4j log file.</span></span> <span data-ttu-id="2470a-119">Varje logg log4j består av en rad med fält som innehåller en [LOGGNINGSNIVÅ] fältet tooshow hello typ och hello allvarlighetsgrad, till exempel:</span><span class="sxs-lookup"><span data-stu-id="2470a-119">Each log4j log consists of a line of fields that contains a [LOG LEVEL] field tooshow hello type and hello severity, for example:</span></span>

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    <span data-ttu-id="2470a-120">hello utdata för Hive-skriptet är ungefär:</span><span class="sxs-lookup"><span data-stu-id="2470a-120">hello Hive script output is similar to:</span></span>

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    <span data-ttu-id="2470a-121">Mer information om Hive finns i [Använda Hive med HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="2470a-121">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="2470a-122">En åtgärd för Sqoop exporterar hello HiveQL åtgärd tooa utdatatabellen i en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="2470a-122">A Sqoop action exports hello HiveQL action output tooa table in an Azure SQL database.</span></span> <span data-ttu-id="2470a-123">Läs mer om Sqoop [använda Sqoop med HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="2470a-123">For more information about Sqoop, see [Use Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="2470a-124">Versioner som stöds Oozie i HDInsight-kluster, se [vad är nytt i hello-klusterversioner som tillhandahålls av HDInsight?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="2470a-124">For supported Oozie versions on HDInsight clusters, see [What's new in hello cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="2470a-125">Krav</span><span class="sxs-lookup"><span data-stu-id="2470a-125">Prerequisites</span></span>
<span data-ttu-id="2470a-126">Innan du påbörjar den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="2470a-126">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="2470a-127">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="2470a-127">**A workstation with Azure PowerShell**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2470a-128">Stödet för Azure PowerShell för hantering av HDInsight-resurser med hjälp av Azure Service Manager är **föråldrat** och dras tillbaka den 1 januari 2017.</span><span class="sxs-lookup"><span data-stu-id="2470a-128">Azure PowerShell support for managing HDInsight resources using Azure Service Manager is **deprecated**, and will be removed by January 1, 2017.</span></span> <span data-ttu-id="2470a-129">hello stegen i det här dokumentet används hello nya HDInsight-cmdletarna som fungerar med Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="2470a-129">hello steps in this document use hello new HDInsight cmdlets that work with Azure Resource Manager.</span></span>
    >
    > <span data-ttu-id="2470a-130">Följ stegen hello i [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello senaste versionen av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2470a-130">Please follow hello steps in [Install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello latest version of Azure PowerShell.</span></span> <span data-ttu-id="2470a-131">Om du har skript som behöver toobe ändras toouse hello nya cmdletarna som fungerar med Azure Resource Manager kan se [migrera tooAzure Resource Manager-baserade development tools för HDInsight-kluster](hdinsight-hadoop-development-using-azure-resource-manager.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="2470a-131">If you have scripts that need toobe modified toouse hello new cmdlets that work with Azure Resource Manager, see [Migrating tooAzure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) for more information.</span></span>

* <span data-ttu-id="2470a-132">**Ett HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="2470a-132">**An HDInsight cluster**.</span></span> <span data-ttu-id="2470a-133">Information om hur du skapar ett HDInsight-kluster finns i [skapa HDInsight-kluster][hdinsight-provision], eller [komma igång med HDInsight][hdinsight-get-started].</span><span class="sxs-lookup"><span data-stu-id="2470a-133">For information about creating an HDInsight cluster, see [Create HDInsight clusters][hdinsight-provision], or [Get started with HDInsight][hdinsight-get-started].</span></span> <span data-ttu-id="2470a-134">Du behöver följande data toogo igenom kursen hello hello:</span><span class="sxs-lookup"><span data-stu-id="2470a-134">You will need hello following data toogo through hello tutorial:</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="2470a-135">Kluster-egenskap</span><span class="sxs-lookup"><span data-stu-id="2470a-135">Cluster property</span></span></th><th><span data-ttu-id="2470a-136">Variabelnamn för Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="2470a-136">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="2470a-137">Värde</span><span class="sxs-lookup"><span data-stu-id="2470a-137">Value</span></span></th><th><span data-ttu-id="2470a-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-138">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="2470a-139">HDInsight-klustrets namn</span><span class="sxs-lookup"><span data-stu-id="2470a-139">HDInsight cluster name</span></span></td><td><span data-ttu-id="2470a-140">$clusterName</span><span class="sxs-lookup"><span data-stu-id="2470a-140">$clusterName</span></span></td><td></td><td><span data-ttu-id="2470a-141">Hej HDInsight-kluster som du vill köra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-141">hello HDInsight cluster on which you will run this tutorial.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-142">Användarnamn för HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="2470a-142">HDInsight cluster username</span></span></td><td><span data-ttu-id="2470a-143">$clusterUsername</span><span class="sxs-lookup"><span data-stu-id="2470a-143">$clusterUsername</span></span></td><td></td><td><span data-ttu-id="2470a-144">användarnamn för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-144">hello HDInsight cluster user name.</span></span> </td></tr>
    <tr><td><span data-ttu-id="2470a-145">Användarlösenordet för HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="2470a-145">HDInsight cluster user password</span></span> </td><td><span data-ttu-id="2470a-146">$clusterPassword</span><span class="sxs-lookup"><span data-stu-id="2470a-146">$clusterPassword</span></span></td><td></td><td><span data-ttu-id="2470a-147">användarlösenord hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-147">hello HDInsight cluster user password.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-148">Azure storage-kontonamn</span><span class="sxs-lookup"><span data-stu-id="2470a-148">Azure storage account name</span></span></td><td><span data-ttu-id="2470a-149">$storageAccountName</span><span class="sxs-lookup"><span data-stu-id="2470a-149">$storageAccountName</span></span></td><td></td><td><span data-ttu-id="2470a-150">Ett Azure Storage-konto tillgängliga toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-150">An Azure Storage account available toohello HDInsight cluster.</span></span> <span data-ttu-id="2470a-151">Använd hello standardkontot för lagring som du angav under hello klustret etablera processen för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-151">For this tutorial, use hello default storage account that you specified during hello cluster provision process.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-152">Azure Blob-behållarnamn</span><span class="sxs-lookup"><span data-stu-id="2470a-152">Azure Blob container name</span></span></td><td><span data-ttu-id="2470a-153">$containerName</span><span class="sxs-lookup"><span data-stu-id="2470a-153">$containerName</span></span></td><td></td><td><span data-ttu-id="2470a-154">I det här exemplet använder du hello Azure Blob storage-behållare som används för hello HDInsight-kluster standardfilsystem.</span><span class="sxs-lookup"><span data-stu-id="2470a-154">For this example, use hello Azure Blob storage container that is used for hello default HDInsight cluster file system.</span></span> <span data-ttu-id="2470a-155">Som standard har hello samma namn som hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-155">By default, it has hello same name as hello HDInsight cluster.</span></span></td></tr>
    </table><span data-ttu-id="2470a-156">
* **En Azure SQL database**.</span><span class="sxs-lookup"><span data-stu-id="2470a-156">
* **An Azure SQL database**.</span></span> <span data-ttu-id="2470a-157">Du måste konfigurera en brandväggsregel för hello SQL Database server tooallow åtkomst från din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="2470a-157">You must configure a firewall rule for hello SQL Database server tooallow access from your workstation.</span></span> <span data-ttu-id="2470a-158">Instruktioner om hur du skapar en Azure SQL database och konfigurerar hello brandväggen finns [komma igång med Azure SQL database] [sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="2470a-158">For instructions about creating an Azure SQL database and configuring hello firewall, see [Get started using Azure SQL database][sqldatabase-get-started].</span></span> <span data-ttu-id="2470a-159">Den här artikeln innehåller en Windows PowerShell-skript för att skapa hello Azure SQL database-tabellen som ska användas för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-159">This article provides a Windows PowerShell script for creating hello Azure SQL database table that you need for this tutorial.</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="2470a-160">Egenskapen för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="2470a-160">SQL database property</span></span></th><th><span data-ttu-id="2470a-161">Variabelnamn för Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="2470a-161">Windows PowerShell variable name</span></span></th><th><span data-ttu-id="2470a-162">Värde</span><span class="sxs-lookup"><span data-stu-id="2470a-162">Value</span></span></th><th><span data-ttu-id="2470a-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-163">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="2470a-164">SQL server-databasnamn</span><span class="sxs-lookup"><span data-stu-id="2470a-164">SQL database server name</span></span></td><td><span data-ttu-id="2470a-165">$sqlDatabaseServer</span><span class="sxs-lookup"><span data-stu-id="2470a-165">$sqlDatabaseServer</span></span></td><td></td><td><span data-ttu-id="2470a-166">hello SQL database server toowhich Sqoop kommer att exportera data.</span><span class="sxs-lookup"><span data-stu-id="2470a-166">hello SQL database server toowhich Sqoop will export data.</span></span> </td></tr>
    <tr><td><span data-ttu-id="2470a-167">Inloggningsnamn för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="2470a-167">SQL database login name</span></span></td><td><span data-ttu-id="2470a-168">$sqlDatabaseLogin</span><span class="sxs-lookup"><span data-stu-id="2470a-168">$sqlDatabaseLogin</span></span></td><td></td><td><span data-ttu-id="2470a-169">Inloggningsnamn för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2470a-169">SQL Database login name.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-170">Inloggningslösenordet för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="2470a-170">SQL database login password</span></span></td><td><span data-ttu-id="2470a-171">$sqlDatabaseLoginPassword</span><span class="sxs-lookup"><span data-stu-id="2470a-171">$sqlDatabaseLoginPassword</span></span></td><td></td><td><span data-ttu-id="2470a-172">Lösenordet för inloggningen för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2470a-172">SQL Database login password.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-173">SQL-databasnamn</span><span class="sxs-lookup"><span data-stu-id="2470a-173">SQL database name</span></span></td><td><span data-ttu-id="2470a-174">$sqlDatabaseName</span><span class="sxs-lookup"><span data-stu-id="2470a-174">$sqlDatabaseName</span></span></td><td></td><td><span data-ttu-id="2470a-175">hello Azure SQL database toowhich Sqoop kommer att exportera data.</span><span class="sxs-lookup"><span data-stu-id="2470a-175">hello Azure SQL database toowhich Sqoop will export data.</span></span> </td></tr>
    </table>

  > [!NOTE]
  > <span data-ttu-id="2470a-176">Som standard tillåter en Azure SQL database anslutningar från Azure-tjänster, till exempel Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2470a-176">By default an Azure SQL database allows connections from Azure Services, such as Azure HDInsight.</span></span> <span data-ttu-id="2470a-177">Om brandväggsinställningen är inaktiverad måste du aktivera det från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2470a-177">If this firewall setting is disabled, you must enable it from hello Azure Portal.</span></span> <span data-ttu-id="2470a-178">Instruktioner om hur du skapar en SQL-databas och konfigurera brandväggsregler, se [skapa och konfigurera SQL-databas][sqldatabase-get-started].</span><span class="sxs-lookup"><span data-stu-id="2470a-178">For instruction about creating a SQL Database and configuring firewall rules, see [Create and Configure SQL Database][sqldatabase-get-started].</span></span>

> [!NOTE]
> <span data-ttu-id="2470a-179">Fyll hello värden i hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="2470a-179">Fill-in hello values in hello tables.</span></span> <span data-ttu-id="2470a-180">Kommer att vara till hjälp för att gå igenom den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-180">It will be helpful for going through this tutorial.</span></span>

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a><span data-ttu-id="2470a-181">Definiera Oozie arbetsflödet och hello relaterade HiveQL-skript</span><span class="sxs-lookup"><span data-stu-id="2470a-181">Define Oozie workflow and hello related HiveQL script</span></span>
<span data-ttu-id="2470a-182">Oozie arbetsflöden definitioner skrivs i hPDL (en XML-processen definition language).</span><span class="sxs-lookup"><span data-stu-id="2470a-182">Oozie workflows definitions are written in hPDL (an XML process definition language).</span></span> <span data-ttu-id="2470a-183">hello standardfilnamnet för arbetsflödet är *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="2470a-183">hello default workflow file name is *workflow.xml*.</span></span>  <span data-ttu-id="2470a-184">Du spara hello arbetsflödet filen lokalt och sedan distribuera den toohello HDInsight-kluster med hjälp av Azure PowerShell senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-184">You will save hello workflow file locally, and then deploy it toohello HDInsight cluster by using Azure PowerShell later in this tutorial.</span></span>

<span data-ttu-id="2470a-185">hello Hive-åtgärden i hello arbetsflödet anropar en skriptfil för HiveQL.</span><span class="sxs-lookup"><span data-stu-id="2470a-185">hello Hive action in hello workflow calls a HiveQL script file.</span></span> <span data-ttu-id="2470a-186">Den här skriptfilen innehåller tre HiveQL-instruktioner:</span><span class="sxs-lookup"><span data-stu-id="2470a-186">This script file contains three HiveQL statements:</span></span>

1. <span data-ttu-id="2470a-187">**hello DROP TABLE-instruktionen** borttagningar hello log4j Hive-tabell om den finns.</span><span class="sxs-lookup"><span data-stu-id="2470a-187">**hello DROP TABLE statement** deletes hello log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="2470a-188">**hello CREATE TABLE-instruktion** skapar en log4j Hive extern tabell som pekar toohello plats för loggfilen för hello log4j;</span><span class="sxs-lookup"><span data-stu-id="2470a-188">**hello CREATE TABLE statement** creates a log4j Hive external table, which points toohello location of hello log4j log file;</span></span>
3. <span data-ttu-id="2470a-189">**Hej plats för hello log4j loggfilen**.</span><span class="sxs-lookup"><span data-stu-id="2470a-189">**hello location of hello log4j log file**.</span></span> <span data-ttu-id="2470a-190">hello fältavgränsaren är ””,.</span><span class="sxs-lookup"><span data-stu-id="2470a-190">hello field delimiter is ",".</span></span> <span data-ttu-id="2470a-191">hello standard rad avgränsaren är ”\n”.</span><span class="sxs-lookup"><span data-stu-id="2470a-191">hello default line delimiter is "\n".</span></span> <span data-ttu-id="2470a-192">En extern tabell Hive är används tooavoid hello datafil som tas bort från hello ursprungliga platsen, om du vill toorun hello Oozie arbetsflöde flera gånger.</span><span class="sxs-lookup"><span data-stu-id="2470a-192">A Hive external table is used tooavoid hello data file being removed from hello original location, in case you want toorun hello Oozie workflow multiple times.</span></span>
4. <span data-ttu-id="2470a-193">**hello Infoga över instruktionen** räknar hello förekomster av varje loggningsnivån typ från hello log4j Hive-tabell och sparar den hello tooan Azure Blob storage platsen.</span><span class="sxs-lookup"><span data-stu-id="2470a-193">**hello INSERT OVERWRITE statement** counts hello occurrences of each log-level type from hello log4j Hive table, and it saves hello output tooan Azure Blob storage location.</span></span>

> [!NOTE]
> <span data-ttu-id="2470a-194">Det finns ett känt problem i Hive-sökväg.</span><span class="sxs-lookup"><span data-stu-id="2470a-194">There is a known Hive path issue.</span></span> <span data-ttu-id="2470a-195">När du skickar in ett Oozie-jobb körs i det här problemet.</span><span class="sxs-lookup"><span data-stu-id="2470a-195">You will run into this problem when submitting an Oozie job.</span></span> <span data-ttu-id="2470a-196">Hej instruktioner för att åtgärda hello problem kan hittas på hello TechNet Wiki: [HDInsight Hive-fel: Det gick inte toorename][technetwiki-hive-error].</span><span class="sxs-lookup"><span data-stu-id="2470a-196">hello instructions for fixing hello issue can be found on hello TechNet Wiki: [HDInsight Hive error: Unable toorename][technetwiki-hive-error].</span></span>

<span data-ttu-id="2470a-197">**toodefine hello HiveQL-skript filen toobe anropas av hello-arbetsflöde**</span><span class="sxs-lookup"><span data-stu-id="2470a-197">**toodefine hello HiveQL script file toobe called by hello workflow**</span></span>

1. <span data-ttu-id="2470a-198">Skapa en textfil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="2470a-198">Create a text file with hello following content:</span></span>

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    <span data-ttu-id="2470a-199">Det finns tre variabler i hello skript:</span><span class="sxs-lookup"><span data-stu-id="2470a-199">There are three variables used in hello script:</span></span>

   * <span data-ttu-id="2470a-200">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="2470a-200">${hiveTableName}</span></span>
   * <span data-ttu-id="2470a-201">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="2470a-201">${hiveDataFolder}</span></span>
   * <span data-ttu-id="2470a-202">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="2470a-202">${hiveOutputFolder}</span></span>

     <span data-ttu-id="2470a-203">arbetsflöde för hello-definitionsfil (workflow.xml i den här självstudiekursen) skickar dessa värden toothis HiveQL-skript vid körning.</span><span class="sxs-lookup"><span data-stu-id="2470a-203">hello workflow definition file (workflow.xml in this tutorial) will pass these values toothis HiveQL script at run time.</span></span>
2. <span data-ttu-id="2470a-204">Spara hello som **C:\Tutorials\UseOozie\useooziewf.hql** med hjälp av kodningen ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="2470a-204">Save hello file as **C:\Tutorials\UseOozie\useooziewf.hql** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="2470a-205">(Använd anteckningar om din textredigerare inte ger det här alternativet.) Den här skriptfilen blir senare i självstudiekursen hello distribuerade toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-205">(Use Notepad if your text editor doesn't provide this option.) This script file will be deployed toohello HDInsight cluster later in hello tutorial.</span></span>

<span data-ttu-id="2470a-206">**toodefine ett arbetsflöde**</span><span class="sxs-lookup"><span data-stu-id="2470a-206">**toodefine a workflow**</span></span>

1. <span data-ttu-id="2470a-207">Skapa en textfil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="2470a-207">Create a text file with hello following content:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    <span data-ttu-id="2470a-208">Det finns två åtgärder som definierats i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="2470a-208">There are two actions defined in hello workflow.</span></span> <span data-ttu-id="2470a-209">hello start-tooaction är *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="2470a-209">hello start-tooaction is *RunHiveScript*.</span></span> <span data-ttu-id="2470a-210">Om hello-åtgärden körs *OK*, hello nästa åtgärd *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="2470a-210">If hello action runs *OK*, hello next action is *RunSqoopExport*.</span></span>

    <span data-ttu-id="2470a-211">Hej RunHiveScript har flera variabler.</span><span class="sxs-lookup"><span data-stu-id="2470a-211">hello RunHiveScript has several variables.</span></span> <span data-ttu-id="2470a-212">Hello-värden skickas när du skickar hello Oozie jobb från din arbetsstation med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2470a-212">You will pass hello values when you submit hello Oozie job from your workstation by using Azure PowerShell.</span></span>

    <span data-ttu-id="2470a-213">Arbetsflödesvariabler</span><span class="sxs-lookup"><span data-stu-id="2470a-213">Workflow variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="2470a-214">Arbetsflödesvariabler</span><span class="sxs-lookup"><span data-stu-id="2470a-214">Workflow variables</span></span></th><th><span data-ttu-id="2470a-215">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-215">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="2470a-216">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="2470a-216">${jobTracker}</span></span></td><td><span data-ttu-id="2470a-217">Ange hello URL för Spårare för hello Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="2470a-217">Specify hello URL of hello Hadoop job tracker.</span></span> <span data-ttu-id="2470a-218">Använd <strong>jobtrackerhost:9010</strong> kluster i HDInsight version 3.0 och 2.0.</span><span class="sxs-lookup"><span data-stu-id="2470a-218">Use <strong>jobtrackerhost:9010</strong> on HDInsight cluster version 3.0 and 2.0.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-219">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="2470a-219">${nameNode}</span></span></td><td><span data-ttu-id="2470a-220">Ange hello URL för hello Hadoop namn nod.</span><span class="sxs-lookup"><span data-stu-id="2470a-220">Specify hello URL of hello Hadoop name node.</span></span> <span data-ttu-id="2470a-221">Använd hello standard filen system wasb: / / adress, till exempel <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="2470a-221">Use hello default file system wasb:// address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-222">${Könamn}</span><span class="sxs-lookup"><span data-stu-id="2470a-222">${queueName}</span></span></td><td><span data-ttu-id="2470a-223">Anger hello könamnet som hello jobbet ska skickas till.</span><span class="sxs-lookup"><span data-stu-id="2470a-223">Specifies hello queue name that hello job will be submitted to.</span></span> <span data-ttu-id="2470a-224">Använd <strong>standard</strong>.</span><span class="sxs-lookup"><span data-stu-id="2470a-224">Use <strong>default</strong>.</span></span></td></tr>
    </table>

    <span data-ttu-id="2470a-225">Hive åtgärdsvariabler</span><span class="sxs-lookup"><span data-stu-id="2470a-225">Hive action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="2470a-226">Hive variabel</span><span class="sxs-lookup"><span data-stu-id="2470a-226">Hive action variable</span></span></th><th><span data-ttu-id="2470a-227">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-227">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="2470a-228">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="2470a-228">${hiveDataFolder}</span></span></td><td><span data-ttu-id="2470a-229">hello källkatalogen för hello Hive Create Table-kommando.</span><span class="sxs-lookup"><span data-stu-id="2470a-229">hello source directory for hello Hive Create Table command.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-230">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="2470a-230">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="2470a-231">hello utdatamapp för hello Infoga över instruktionen.</span><span class="sxs-lookup"><span data-stu-id="2470a-231">hello output folder for hello INSERT OVERWRITE statement.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-232">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="2470a-232">${hiveTableName}</span></span></td><td><span data-ttu-id="2470a-233">hello namnet på hello Hive-tabell som refererar till hello log4j datafiler.</span><span class="sxs-lookup"><span data-stu-id="2470a-233">hello name of hello Hive table that references hello log4j data files.</span></span></td></tr>
    </table>

    <span data-ttu-id="2470a-234">Sqoop åtgärdsvariabler</span><span class="sxs-lookup"><span data-stu-id="2470a-234">Sqoop action variables</span></span>

    <table border = "1">
    <tr><th><span data-ttu-id="2470a-235">Sqoop variabel</span><span class="sxs-lookup"><span data-stu-id="2470a-235">Sqoop action variable</span></span></th><th><span data-ttu-id="2470a-236">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-236">Description</span></span></th></tr>
    <tr><td><span data-ttu-id="2470a-237">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="2470a-237">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="2470a-238">Anslutningssträngen som SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="2470a-238">SQL Database connection string.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-239">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="2470a-239">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="2470a-240">hello Azure SQL database tabell toowhere hello data kommer att exporteras.</span><span class="sxs-lookup"><span data-stu-id="2470a-240">hello Azure SQL database table toowhere hello data will be exported.</span></span></td></tr>
    <tr><td><span data-ttu-id="2470a-241">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="2470a-241">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="2470a-242">hello utdatamapp för hello Hive Infoga skriva över instruktionen.</span><span class="sxs-lookup"><span data-stu-id="2470a-242">hello output folder for hello Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="2470a-243">Detta är hello samma mapp för hello Sqoop export (export-dir).</span><span class="sxs-lookup"><span data-stu-id="2470a-243">This is hello same folder for hello Sqoop export (export-dir).</span></span></td></tr>
    </table>

    <span data-ttu-id="2470a-244">Läs mer om Oozie arbetsflödet och använder hello arbetsflödesåtgärder [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).</span><span class="sxs-lookup"><span data-stu-id="2470a-244">For more information about Oozie workflow and using hello workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

1. <span data-ttu-id="2470a-245">Spara hello som **C:\Tutorials\UseOozie\workflow.xml** med hjälp av kodningen ANSI (ASCII).</span><span class="sxs-lookup"><span data-stu-id="2470a-245">Save hello file as **C:\Tutorials\UseOozie\workflow.xml** by using ANSI (ASCII) encoding.</span></span> <span data-ttu-id="2470a-246">(Använd anteckningar om din textredigerare inte ger det här alternativet.)</span><span class="sxs-lookup"><span data-stu-id="2470a-246">(Use Notepad if your text editor doesn't provide this option.)</span></span>

<span data-ttu-id="2470a-247">**toodefine coordinator**</span><span class="sxs-lookup"><span data-stu-id="2470a-247">**toodefine coordinator**</span></span>

1. <span data-ttu-id="2470a-248">Skapa en textfil med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="2470a-248">Create a text file with hello following content:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    <span data-ttu-id="2470a-249">Det finns fem variabler som används i definitionsfilen för hello:</span><span class="sxs-lookup"><span data-stu-id="2470a-249">There are five variables used in hello definition file:</span></span>

   | <span data-ttu-id="2470a-250">Variabel</span><span class="sxs-lookup"><span data-stu-id="2470a-250">Variable</span></span> | <span data-ttu-id="2470a-251">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2470a-251">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="2470a-252">${coordFrequency}</span><span class="sxs-lookup"><span data-stu-id="2470a-252">${coordFrequency}</span></span> |<span data-ttu-id="2470a-253">Jobbet paus tid.</span><span class="sxs-lookup"><span data-stu-id="2470a-253">Job pause time.</span></span> <span data-ttu-id="2470a-254">Frekvensen är alltid uttrycks i minuter.</span><span class="sxs-lookup"><span data-stu-id="2470a-254">Frequency is always expressed in minutes.</span></span> |
   | <span data-ttu-id="2470a-255">${coordStart}</span><span class="sxs-lookup"><span data-stu-id="2470a-255">${coordStart}</span></span> |<span data-ttu-id="2470a-256">Jobbets starttid.</span><span class="sxs-lookup"><span data-stu-id="2470a-256">Job start time.</span></span> |
   | <span data-ttu-id="2470a-257">${coordEnd}</span><span class="sxs-lookup"><span data-stu-id="2470a-257">${coordEnd}</span></span> |<span data-ttu-id="2470a-258">Jobbets sluttid.</span><span class="sxs-lookup"><span data-stu-id="2470a-258">Job end time.</span></span> |
   | <span data-ttu-id="2470a-259">${coordTimezone}</span><span class="sxs-lookup"><span data-stu-id="2470a-259">${coordTimezone}</span></span> |<span data-ttu-id="2470a-260">Oozie bearbetar coordinator jobb i en fast tidszon med ingen sommartid (vanligtvis representeras med hjälp av UTC).</span><span class="sxs-lookup"><span data-stu-id="2470a-260">Oozie processes coordinator jobs in a fixed time zone with no daylight saving time (typically represented by using UTC).</span></span> <span data-ttu-id="2470a-261">Den här tidszonen kallas hello ”Oozie bearbetning tidszonen”.</span><span class="sxs-lookup"><span data-stu-id="2470a-261">This time zone is referred as hello "Oozie processing timezone."</span></span> |
   | <span data-ttu-id="2470a-262">${wfPath}</span><span class="sxs-lookup"><span data-stu-id="2470a-262">${wfPath}</span></span> |<span data-ttu-id="2470a-263">hello sökvägen för hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="2470a-263">hello path for hello workflow.xml.</span></span>  <span data-ttu-id="2470a-264">Du måste ange om hello Arbetsflödesnamnet filen inte är hello standardfilnamnet (workflow.xml).</span><span class="sxs-lookup"><span data-stu-id="2470a-264">If hello workflow file name is not hello default file name (workflow.xml), you must specify it.</span></span> |
2. <span data-ttu-id="2470a-265">Spara hello som **C:\Tutorials\UseOozie\coordinator.xml** med hjälp av hello ANSI (ASCII) kodning.</span><span class="sxs-lookup"><span data-stu-id="2470a-265">Save hello file as **C:\Tutorials\UseOozie\coordinator.xml** by using hello ANSI (ASCII) encoding.</span></span> <span data-ttu-id="2470a-266">(Använd anteckningar om din textredigerare inte ger det här alternativet.)</span><span class="sxs-lookup"><span data-stu-id="2470a-266">(Use Notepad if your text editor doesn't provide this option.)</span></span>

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a><span data-ttu-id="2470a-267">Distribuera hello Oozie-projektet och förbereda hello självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="2470a-267">Deploy hello Oozie project and prepare hello tutorial</span></span>
<span data-ttu-id="2470a-268">Du ska köra en Azure PowerShell-skriptet tooperform hello följande:</span><span class="sxs-lookup"><span data-stu-id="2470a-268">You will run an Azure PowerShell script tooperform hello following:</span></span>

* <span data-ttu-id="2470a-269">Kopiera hello HiveQL-skript (useoozie.hql) tooAzure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span><span class="sxs-lookup"><span data-stu-id="2470a-269">Copy hello HiveQL script (useoozie.hql) tooAzure Blob storage, wasb:///tutorials/useoozie/useoozie.hql.</span></span>
* <span data-ttu-id="2470a-270">Kopiera workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="2470a-270">Copy workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span></span>
* <span data-ttu-id="2470a-271">Kopiera coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.</span><span class="sxs-lookup"><span data-stu-id="2470a-271">Copy coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.</span></span>
* <span data-ttu-id="2470a-272">Kopiera hello-datafil (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="2470a-272">Copy hello data file (/example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span></span>
* <span data-ttu-id="2470a-273">Skapa en Azure SQL database-tabellen för att lagra Sqoop export av data.</span><span class="sxs-lookup"><span data-stu-id="2470a-273">Create an Azure SQL database table for storing Sqoop export data.</span></span> <span data-ttu-id="2470a-274">hello tabellnamnet är *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="2470a-274">hello table name is *log4jLogCount*.</span></span>

<span data-ttu-id="2470a-275">**Förstå HDInsight lagring**</span><span class="sxs-lookup"><span data-stu-id="2470a-275">**Understand HDInsight storage**</span></span>

<span data-ttu-id="2470a-276">HDInsight använder Azure Blob storage för lagring av data.</span><span class="sxs-lookup"><span data-stu-id="2470a-276">HDInsight uses Azure Blob storage for data storage.</span></span> <span data-ttu-id="2470a-277">wasb: / / är Microsofts implementering av hello Hadoop distributed file system (HDFS) i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="2470a-277">wasb:// is Microsoft's implementation of hello Hadoop distributed file system (HDFS) in Azure Blob storage.</span></span> <span data-ttu-id="2470a-278">Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="2470a-278">For more information, see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="2470a-279">När du etablerar ett HDInsight-kluster, utses ett Azure Blob storage-konto och en specifik behållare från detta konto hello standardfilsystem, som i HDFS.</span><span class="sxs-lookup"><span data-stu-id="2470a-279">When you provision an HDInsight cluster, an Azure Blob storage account and a specific container from that account is designated as hello default file system, like in HDFS.</span></span> <span data-ttu-id="2470a-280">Dessutom toothis storage-konto, du kan lägga till ytterligare lagringskonton från hello samma Azure-prenumeration eller från olika Azure-prenumerationer under hello etableringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2470a-280">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or from different Azure subscriptions during hello provisioning process.</span></span> <span data-ttu-id="2470a-281">Anvisningar om att lägga till ytterligare lagringskonton finns [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="2470a-281">For instructions about adding additional storage accounts, see [Provision HDInsight clusters][hdinsight-provision].</span></span> <span data-ttu-id="2470a-282">toosimplify hello Azure PowerShell-skriptet som används i den här självstudiekursen, alla hello filer lagras i hello standard filsystemets behållare finns på */självstudier/useoozie*.</span><span class="sxs-lookup"><span data-stu-id="2470a-282">toosimplify hello Azure PowerShell script used in this tutorial, all of hello files are stored in hello default file system container located at */tutorials/useoozie*.</span></span> <span data-ttu-id="2470a-283">Den här behållaren har som standard hello samma namn som hello HDInsight-klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="2470a-283">By default, this container has hello same name as hello HDInsight cluster name.</span></span>
<span data-ttu-id="2470a-284">hello-syntaxen är:</span><span class="sxs-lookup"><span data-stu-id="2470a-284">hello syntax is:</span></span>

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> <span data-ttu-id="2470a-285">Endast hello *wasb: / /* syntax stöds i HDInsight-kluster av version 3.0.</span><span class="sxs-lookup"><span data-stu-id="2470a-285">Only hello *wasb://* syntax is supported in HDInsight cluster version 3.0.</span></span> <span data-ttu-id="2470a-286">hello äldre *asv: / /* syntax stöds i HDInsight 2.1 och 1.6 kluster, men stöds inte i HDInsight 3.0-kluster.</span><span class="sxs-lookup"><span data-stu-id="2470a-286">hello older *asv://* syntax is supported in HDInsight 2.1 and 1.6 clusters, but it is not supported in HDInsight 3.0 clusters.</span></span>
>
> <span data-ttu-id="2470a-287">Hej wasb: / / sökvägen är en virtuell sökväg.</span><span class="sxs-lookup"><span data-stu-id="2470a-287">hello wasb:// path is a virtual path.</span></span> <span data-ttu-id="2470a-288">Mer information finns i [använda Azure Blob storage med HDInsight][hdinsight-storage].</span><span class="sxs-lookup"><span data-stu-id="2470a-288">For more information see [Use Azure Blob storage with HDInsight][hdinsight-storage].</span></span>

<span data-ttu-id="2470a-289">En fil som lagras i hello standard filsystemets behållare kan nås från HDInsight med hjälp av hello följande URI: er (jag använder workflow.xml som exempel):</span><span class="sxs-lookup"><span data-stu-id="2470a-289">A file that is stored in hello default file system container can be accessed from HDInsight by using any of hello following URIs (I am using workflow.xml as an example):</span></span>

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

<span data-ttu-id="2470a-290">Om du vill tooaccess hello filen direkt från hello storage-konto är hello blob-namnet för hello-filen:</span><span class="sxs-lookup"><span data-stu-id="2470a-290">If you want tooaccess hello file directly from hello storage account, hello blob name for hello file is:</span></span>

    tutorials/useoozie/workflow.xml

<span data-ttu-id="2470a-291">**Förstå Hive interna och externa tabeller**</span><span class="sxs-lookup"><span data-stu-id="2470a-291">**Understand Hive internal and external tables**</span></span>

<span data-ttu-id="2470a-292">Det finns några saker du behöver tooknow om Hive interna och externa tabeller:</span><span class="sxs-lookup"><span data-stu-id="2470a-292">There are a few things you need tooknow about Hive internal and external tables:</span></span>

* <span data-ttu-id="2470a-293">hello CREATE TABLE-kommando skapar en intern tabell, även kallat en hanterad tabell.</span><span class="sxs-lookup"><span data-stu-id="2470a-293">hello CREATE TABLE command creates an internal table, also known as a managed table.</span></span> <span data-ttu-id="2470a-294">hello datafilen måste finnas i hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="2470a-294">hello data file must be located in hello default container.</span></span>
* <span data-ttu-id="2470a-295">hello CREATE TABLE-kommando flyttar hello data filen toohello/hive-datalager<TableName> mapp i hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="2470a-295">hello CREATE TABLE command moves hello data file toohello /hive/warehouse/<TableName> folder in hello default container.</span></span>
* <span data-ttu-id="2470a-296">hello Skapa extern tabell kommando skapar en extern tabell.</span><span class="sxs-lookup"><span data-stu-id="2470a-296">hello CREATE EXTERNAL TABLE command creates an external table.</span></span> <span data-ttu-id="2470a-297">hello-datafilen kan finnas utanför hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="2470a-297">hello data file can be located outside hello default container.</span></span>
* <span data-ttu-id="2470a-298">hello Skapa extern tabell kommando flyttar inte hello-datafilen.</span><span class="sxs-lookup"><span data-stu-id="2470a-298">hello CREATE EXTERNAL TABLE command does not move hello data file.</span></span>
* <span data-ttu-id="2470a-299">hello Skapa extern tabell kommandot kan inte eventuella undermappar på hello-mappen som anges i hello plats-satsen.</span><span class="sxs-lookup"><span data-stu-id="2470a-299">hello CREATE EXTERNAL TABLE command doesn't allow any subfolders under hello folder that is specified in hello LOCATION clause.</span></span> <span data-ttu-id="2470a-300">Detta är hello skäl till varför hello självstudiekursen gör en kopia av hello sample.log filen.</span><span class="sxs-lookup"><span data-stu-id="2470a-300">This is hello reason why hello tutorial makes a copy of hello sample.log file.</span></span>

<span data-ttu-id="2470a-301">Mer information finns i [HDInsight: Hive interna och externa tabeller introduktion][cindygross-hive-tables].</span><span class="sxs-lookup"><span data-stu-id="2470a-301">For more information, see [HDInsight: Hive Internal and External Tables Intro][cindygross-hive-tables].</span></span>

<span data-ttu-id="2470a-302">**tooprepare hello självstudiekursen**</span><span class="sxs-lookup"><span data-stu-id="2470a-302">**tooprepare hello tutorial**</span></span>

1. <span data-ttu-id="2470a-303">Öppna hello Windows PowerShell ISE (hello Start i Windows 8 skärm anger **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="2470a-303">Open hello Windows PowerShell ISE (in hello Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="2470a-304">Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="2470a-304">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="2470a-305">Kör följande kommando tooconnect tooyour Azure-prenumeration hello i hello längst ned i fönstret:</span><span class="sxs-lookup"><span data-stu-id="2470a-305">In hello bottom pane, run hello following command tooconnect tooyour Azure subscription:</span></span>

    ```powershell
    Add-AzureAccount
    ```

    <span data-ttu-id="2470a-306">Du kommer att tillfrågas tooenter dina Azure-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2470a-306">You will be prompted tooenter your Azure account credentials.</span></span> <span data-ttu-id="2470a-307">Den här metoden för att lägga till en prenumerationsanslutning timeout och efter 12 timmar du har toorun hello cmdlet igen.</span><span class="sxs-lookup"><span data-stu-id="2470a-307">This method of adding a subscription connection times out, and after 12 hours, you will have toorun hello cmdlet again.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2470a-308">Om du har flera Azure-prenumerationer och hello standard prenumerationen är inte hello en du vill toouse använder hello <strong>Välj AzureSubscription</strong> cmdlet tooselect en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2470a-308">If you have multiple Azure subscriptions and hello default subscription is not hello one you want toouse, use hello <strong>Select-AzureSubscription</strong> cmdlet tooselect a subscription.</span></span>

3. <span data-ttu-id="2470a-309">Kopiera följande skript i hello Skriptfönster hello och ange hello första sex variabler:</span><span class="sxs-lookup"><span data-stu-id="2470a-309">Copy hello following script into hello script pane, and then set hello first six variables:</span></span>

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    <span data-ttu-id="2470a-310">Fler beskrivningar av hello variabler finns i hello [krav](#prerequisites) avsnitt i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-310">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

4. <span data-ttu-id="2470a-311">Lägg till följande toohello skriptet i hello Skriptfönster hello:</span><span class="sxs-lookup"><span data-stu-id="2470a-311">Append hello following toohello script in hello script pane:</span></span>

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create hello log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. <span data-ttu-id="2470a-312">Klicka på **kör skriptet** eller tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-312">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="2470a-313">hello utdata blir liknar:</span><span class="sxs-lookup"><span data-stu-id="2470a-313">hello output will be similar to:</span></span>

    ![Förberedelse av kursen utdata][img-preparation-output]

## <a name="run-hello-oozie-project"></a><span data-ttu-id="2470a-315">Kör hello Oozie-projekt</span><span class="sxs-lookup"><span data-stu-id="2470a-315">Run hello Oozie project</span></span>
<span data-ttu-id="2470a-316">Azure PowerShell tillhandahåller inte för närvarande alla cmdlets för att definiera Oozie jobb.</span><span class="sxs-lookup"><span data-stu-id="2470a-316">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="2470a-317">Du kan använda hello **Invoke-RestMethod** cmdlet tooinvoke Oozie-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="2470a-317">You can use hello **Invoke-RestMethod** cmdlet tooinvoke Oozie web services.</span></span> <span data-ttu-id="2470a-318">hello Oozie web services API är en HTTP-REST-API för JSON.</span><span class="sxs-lookup"><span data-stu-id="2470a-318">hello Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="2470a-319">Mer information om webbtjänster för hello Oozie API finns [Apache Oozie 4.0 dokumentationen] [ apache-oozie-400] (för HDInsight-kluster av version 3.0) eller [Apache Oozie 3.3.2 dokumentationen] [ apache-oozie-332] (för HDInsight-kluster av version 2.1).</span><span class="sxs-lookup"><span data-stu-id="2470a-319">For more information about hello Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight cluster version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight cluster version 2.1).</span></span>

<span data-ttu-id="2470a-320">**toosubmit ett Oozie-jobb**</span><span class="sxs-lookup"><span data-stu-id="2470a-320">**toosubmit an Oozie job**</span></span>

1. <span data-ttu-id="2470a-321">Öppna hello Windows PowerShell ISE (i Windows 8 startskärmen, skriver **PowerShell_ISE**, och klicka sedan på **Windows PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="2470a-321">Open hello Windows PowerShell ISE (in Windows 8 Start screen, type **PowerShell_ISE**, and then click **Windows PowerShell ISE**.</span></span> <span data-ttu-id="2470a-322">Mer information finns i [starta Windows PowerShell i Windows 8 och Windows][powershell-start]).</span><span class="sxs-lookup"><span data-stu-id="2470a-322">For more information, see [Start Windows PowerShell on Windows 8 and Windows][powershell-start]).</span></span>
2. <span data-ttu-id="2470a-323">Kopiera hello följande skript i hello Skriptfönster och sedan ange hello först 14 variabler (dock hoppa över **$storageUri**).</span><span class="sxs-lookup"><span data-stu-id="2470a-323">Copy hello following script into hello script pane, and then set hello first fourteen variables (however, skip **$storageUri**).</span></span>

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    <span data-ttu-id="2470a-324">Fler beskrivningar av hello variabler finns i hello [krav](#prerequisites) avsnitt i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="2470a-324">For more descriptions of hello variables, see hello [Prerequisites](#prerequisites) section in this tutorial.</span></span>

    <span data-ttu-id="2470a-325">$coordstart och $coordend är hello arbetsflödet start- och sluttid.</span><span class="sxs-lookup"><span data-stu-id="2470a-325">$coordstart and $coordend are hello workflow starting and ending time.</span></span> <span data-ttu-id="2470a-326">toofind Sök ”utc-tid” på bing.com hello UTC/GMT tid. Hej $coordFrequency är hur ofta i minuter som du vill toorun hello arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="2470a-326">toofind out hello UTC/GMT time, search "utc time" on bing.com. hello $coordFrequency is how often in minutes you want toorun hello workflow.</span></span>
3. <span data-ttu-id="2470a-327">Lägg till hello följande toohello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-327">Append hello following toohello script.</span></span> <span data-ttu-id="2470a-328">Den här delen definierar hello Oozie nyttolast:</span><span class="sxs-lookup"><span data-stu-id="2470a-328">This part defines hello Oozie payload:</span></span>

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > <span data-ttu-id="2470a-329">hello största skillnaden jämfört med toohello skicka nyttolast-arbetsflödesfil är hello variabel **oozie.coord.application.path**.</span><span class="sxs-lookup"><span data-stu-id="2470a-329">hello major difference compared toohello workflow submission payload file is hello variable **oozie.coord.application.path**.</span></span> <span data-ttu-id="2470a-330">När du skickar ett arbetsflödesjobb, använder du **oozie.wf.application.path** i stället.</span><span class="sxs-lookup"><span data-stu-id="2470a-330">When you submit a workflow job, you use **oozie.wf.application.path** instead.</span></span>

4. <span data-ttu-id="2470a-331">Lägg till hello följande toohello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-331">Append hello following toohello script.</span></span> <span data-ttu-id="2470a-332">Den här delen kontrollerar hello Oozie web Servicestatus:</span><span class="sxs-lookup"><span data-stu-id="2470a-332">This part checks hello Oozie web service status:</span></span>

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. <span data-ttu-id="2470a-333">Lägg till hello följande toohello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-333">Append hello following toohello script.</span></span> <span data-ttu-id="2470a-334">Den här delen skapas ett jobb för Oozie:</span><span class="sxs-lookup"><span data-stu-id="2470a-334">This part creates an Oozie job:</span></span>

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="2470a-335">När du skickar ett arbetsflödesjobb måste du göra en annan web service anropet toostart hello jobbet när hello jobbet har skapats.</span><span class="sxs-lookup"><span data-stu-id="2470a-335">When you submit a workflow job, you must make another web service call toostart hello job after hello job is created.</span></span> <span data-ttu-id="2470a-336">I det här fallet utlöses hello coordinator jobb av tid.</span><span class="sxs-lookup"><span data-stu-id="2470a-336">In this case, hello coordinator job is triggered by time.</span></span> <span data-ttu-id="2470a-337">hello jobbet startar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2470a-337">hello job will start automatically.</span></span>

6. <span data-ttu-id="2470a-338">Lägg till hello följande toohello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-338">Append hello following toohello script.</span></span> <span data-ttu-id="2470a-339">Den här delen kontrollerar hello Oozie jobbstatus:</span><span class="sxs-lookup"><span data-stu-id="2470a-339">This part checks hello Oozie job status:</span></span>

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. <span data-ttu-id="2470a-340">(Valfritt) Lägg till hello följande toohello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-340">(Optional) Append hello following toohello script.</span></span>

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. <span data-ttu-id="2470a-341">Lägg till följande toohello skript hello:</span><span class="sxs-lookup"><span data-stu-id="2470a-341">Append hello following toohello script:</span></span>

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    <span data-ttu-id="2470a-342">Ta bort hello #-tecken om du vill toorun hello ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="2470a-342">Remove hello # signs if you want toorun hello additional functions.</span></span>
9. <span data-ttu-id="2470a-343">Om ditt HDinsight-kluster är version 2.1, ersätter du ”https://$clusterName.azurehdinsight.net:443/oozie/v2/” med ”https://$clusterName.azurehdinsight.net:443/oozie/v1/”.</span><span class="sxs-lookup"><span data-stu-id="2470a-343">If your HDinsight cluster is version 2.1, replace "https://$clusterName.azurehdinsight.net:443/oozie/v2/" with "https://$clusterName.azurehdinsight.net:443/oozie/v1/".</span></span> <span data-ttu-id="2470a-344">HDInsight-kluster av version 2.1 stöder inte har stöd för version 2 av hello-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="2470a-344">HDInsight cluster version 2.1 does not supports version 2 of hello web services.</span></span>
10. <span data-ttu-id="2470a-345">Klicka på **kör skriptet** eller tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-345">Click **Run Script** or press **F5** toorun hello script.</span></span> <span data-ttu-id="2470a-346">hello utdata blir liknar:</span><span class="sxs-lookup"><span data-stu-id="2470a-346">hello output will be similar to:</span></span>

     ![Kursen kör arbetsflöde utdata][img-runworkflow-output]
11. <span data-ttu-id="2470a-348">Ansluta tooyour SQL-databas toosee hello exporterade data.</span><span class="sxs-lookup"><span data-stu-id="2470a-348">Connect tooyour SQL Database toosee hello exported data.</span></span>

<span data-ttu-id="2470a-349">**toocheck hello jobbet felloggen**</span><span class="sxs-lookup"><span data-stu-id="2470a-349">**toocheck hello job error log**</span></span>

<span data-ttu-id="2470a-350">tootroubleshoot ett arbetsflöde hello Oozie loggfilen finns på C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log från hello klustret headnode.</span><span class="sxs-lookup"><span data-stu-id="2470a-350">tootroubleshoot a workflow, hello Oozie log file can be found at C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log from hello cluster headnode.</span></span> <span data-ttu-id="2470a-351">Mer information om RDP finns [administrera HDInsight-kluster med hello Azure-portalen][hdinsight-admin-portal].</span><span class="sxs-lookup"><span data-stu-id="2470a-351">For information on RDP, see [Administering HDInsight clusters using hello Azure portal][hdinsight-admin-portal].</span></span>

<span data-ttu-id="2470a-352">**toorerun hello självstudiekursen**</span><span class="sxs-lookup"><span data-stu-id="2470a-352">**toorerun hello tutorial**</span></span>

<span data-ttu-id="2470a-353">toorerun hello arbetsflödet, måste du utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="2470a-353">toorerun hello workflow, you must perform hello following tasks:</span></span>

* <span data-ttu-id="2470a-354">Ta bort utdatafilen för hello Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="2470a-354">Delete hello Hive script output file.</span></span>
* <span data-ttu-id="2470a-355">Ta bort hello data i hello log4jLogsCount tabell.</span><span class="sxs-lookup"><span data-stu-id="2470a-355">Delete hello data in hello log4jLogsCount table.</span></span>

<span data-ttu-id="2470a-356">Här är en Windows PowerShell-skript som du kan använda:</span><span class="sxs-lookup"><span data-stu-id="2470a-356">Here is a sample Windows PowerShell script that you can use:</span></span>

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a><span data-ttu-id="2470a-357">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2470a-357">Next steps</span></span>
<span data-ttu-id="2470a-358">I kursen får du lära sig hur toodefine ett arbetsflöde för Oozie och Oozie-koordinator och hur toorun Oozie-koordinator jobb med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2470a-358">In this tutorial, you learned how toodefine an Oozie workflow and an Oozie coordinator, and how toorun an Oozie coordinator job by using Azure PowerShell.</span></span> <span data-ttu-id="2470a-359">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="2470a-359">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="2470a-360">[Komma igång med HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="2470a-360">[Get started with HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="2470a-361">[Använda Azure Blob storage med HDInsight][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="2470a-361">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="2470a-362">[Administrera HDInsight med hjälp av Azure PowerShell][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="2470a-362">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="2470a-363">[Ladda upp data tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="2470a-363">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="2470a-364">[Använda Sqoop med HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="2470a-364">[Use Sqoop with HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="2470a-365">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="2470a-365">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="2470a-366">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="2470a-366">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="2470a-367">[Utveckla Java-MapReduce-program för HDInsight][hdinsight-develop-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="2470a-367">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-java-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
