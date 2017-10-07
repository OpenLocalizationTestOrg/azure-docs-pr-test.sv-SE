---
title: "aaaRun ett Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight | Microsoft Docs"
description: "Lär dig hur toorun enkel Hive, Pig och MapReduce-jobb med Azure Cosmos DB och Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="ee5c7-103"><a name="Azure Cosmos DB-HDInsight"></a>Kör ett Apache Hive, Pig eller Hadoop-jobb med hjälp av Azure Cosmos DB och HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee5c7-103"><a name="Azure Cosmos DB-HDInsight"></a>Run an Apache Hive, Pig, or Hadoop job using Azure Cosmos DB and HDInsight</span></span>
<span data-ttu-id="ee5c7-104">De här självstudierna visar hur toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], och [Apache Hadoop] [ apache-hadoop] MapReduce-jobb på Azure HDInsight med Cosmos DB Hadoop-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-104">This tutorial shows you how toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], and [Apache Hadoop][apache-hadoop] MapReduce jobs on Azure HDInsight with Cosmos DB's Hadoop connector.</span></span> <span data-ttu-id="ee5c7-105">Cosmos DB'S Hadoop-anslutningen kan Cosmos DB tooact som både källa och mottagare för Hive, Pig och MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-105">Cosmos DB's Hadoop connector allows Cosmos DB tooact as both a source and sink for Hive, Pig, and MapReduce jobs.</span></span> <span data-ttu-id="ee5c7-106">Den här kursen använder Cosmos DB som både hello källa och mål för Hadoop-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-106">This tutorial will use Cosmos DB as both hello data source and destination for Hadoop jobs.</span></span>

<span data-ttu-id="ee5c7-107">Den här kursen att du kunna tooanswer hello följande frågor:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-107">After completing this tutorial, you'll be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="ee5c7-108">Hur jag för att läsa in data från Cosmos-databasen med ett Hive, Pig eller MapReduce-jobb?</span><span class="sxs-lookup"><span data-stu-id="ee5c7-108">How do I load data from Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>
* <span data-ttu-id="ee5c7-109">Hur lagrar data i Cosmos-databasen med ett Hive, Pig eller MapReduce-jobb?</span><span class="sxs-lookup"><span data-stu-id="ee5c7-109">How do I store data in Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>

<span data-ttu-id="ee5c7-110">Vi rekommenderar att komma igång genom att titta på hello följande video, där vi kör via ett Hive-jobb med hjälp av Cosmos-DB och HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-110">We recommend getting started by watching hello following video, where we run through a Hive job using Cosmos DB and HDInsight.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

<span data-ttu-id="ee5c7-111">Gå sedan tillbaka toothis artikel, där du får hello fullständig information om hur du kan köra analytics-jobb på Cosmos-DB-data.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-111">Then, return toothis article, where you'll receive hello full details on how you can run analytics jobs on your Cosmos DB data.</span></span>

> [!TIP]
> <span data-ttu-id="ee5c7-112">Den här kursen förutsätter att du har tidigare erfarenhet av Apache Hadoop, Hive och Pig.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-112">This tutorial assumes that you have prior experience using Apache Hadoop, Hive, and/or Pig.</span></span> <span data-ttu-id="ee5c7-113">Om du är ny tooApache Hadoop Hive och Pig, rekommenderar vi att besöka hello [Apache Hadoop-dokumentation][apache-hadoop-doc].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-113">If you are new tooApache Hadoop, Hive, and Pig, we recommend visiting hello [Apache Hadoop documentation][apache-hadoop-doc].</span></span> <span data-ttu-id="ee5c7-114">Den här kursen förutsätter också att du har tidigare erfarenhet av Cosmos-DB och har en Cosmos-DB-konto.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-114">This tutorial also assumes that you have prior experience with Cosmos DB and have a Cosmos DB account.</span></span> <span data-ttu-id="ee5c7-115">Om du är ny tooCosmos DB eller du har inte ett Cosmos-DB-konto, Läs vår [komma igång] [ getting-started] sidan.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-115">If you are new tooCosmos DB or you do not have a Cosmos DB account, please check out our [Getting Started][getting-started] page.</span></span>
>
>

<span data-ttu-id="ee5c7-116">Har du inte tid toocomplete hello självstudier och bara vill tooget hello fullständigt exempelprogram PowerShell-skript för Hive, Pig och MapReduce?</span><span class="sxs-lookup"><span data-stu-id="ee5c7-116">Don't have time toocomplete hello tutorial and just want tooget hello full sample PowerShell scripts for Hive, Pig, and MapReduce?</span></span> <span data-ttu-id="ee5c7-117">Inga problem, hämta dem [här][hdinsight-samples].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-117">Not a problem, get them [here][hdinsight-samples].</span></span> <span data-ttu-id="ee5c7-118">hello download innehåller också hello hql, pig och java-filer för exemplen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-118">hello download also contains hello hql, pig, and java files for these samples.</span></span>

## <span data-ttu-id="ee5c7-119"><a name="NewestVersion"></a>Senaste versionen</span><span class="sxs-lookup"><span data-stu-id="ee5c7-119"><a name="NewestVersion"></a>Newest Version</span></span>
<table border='1'>
    <tr><th><span data-ttu-id="ee5c7-120">Hadoop Connector-Version</span><span class="sxs-lookup"><span data-stu-id="ee5c7-120">Hadoop Connector Version</span></span></th>
        <td><span data-ttu-id="ee5c7-121">1.2.0</span><span class="sxs-lookup"><span data-stu-id="ee5c7-121">1.2.0</span></span></td></tr>
    <tr><th><span data-ttu-id="ee5c7-122">Skript-Uri</span><span class="sxs-lookup"><span data-stu-id="ee5c7-122">Script Uri</span></span></th>
        <td><span data-ttu-id="ee5c7-123">https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</span><span class="sxs-lookup"><span data-stu-id="ee5c7-123">https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</span></span></td></tr>
    <tr><th><span data-ttu-id="ee5c7-124">Ändrad den</span><span class="sxs-lookup"><span data-stu-id="ee5c7-124">Date Modified</span></span></th>
        <td><span data-ttu-id="ee5c7-125">04/26/2016</span><span class="sxs-lookup"><span data-stu-id="ee5c7-125">04/26/2016</span></span></td></tr>
    <tr><th><span data-ttu-id="ee5c7-126">Stöds HDInsight-versioner</span><span class="sxs-lookup"><span data-stu-id="ee5c7-126">Supported HDInsight Versions</span></span></th>
        <td><span data-ttu-id="ee5c7-127">3.1, 3.2</span><span class="sxs-lookup"><span data-stu-id="ee5c7-127">3.1, 3.2</span></span></td></tr>
    <tr><th><span data-ttu-id="ee5c7-128">Ändringslogg</span><span class="sxs-lookup"><span data-stu-id="ee5c7-128">Change Log</span></span></th>
        <td><span data-ttu-id="ee5c7-129">Uppdatera Azure Cosmos DB Java SDK too1.6.0</span><span class="sxs-lookup"><span data-stu-id="ee5c7-129">Updated Azure Cosmos DB Java SDK too1.6.0</span></span></br>
            <span data-ttu-id="ee5c7-130">Tillagt stöd för partitionerade samlingar som både källa och mottagare</span><span class="sxs-lookup"><span data-stu-id="ee5c7-130">Added support for partitioned collections as both a source and sink</span></span></br>
        </td></tr>
</table>

## <span data-ttu-id="ee5c7-131"><a name="Prerequisites"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="ee5c7-131"><a name="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="ee5c7-132">Se till att du har hello följande innan du följer hello instruktioner i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-132">Before following hello instructions in this tutorial, ensure that you have hello following:</span></span>

* <span data-ttu-id="ee5c7-133">Ett Cosmos-DB-konto, en databas och en samling med dokument i.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-133">A Cosmos DB account, a database, and a collection with documents inside.</span></span> <span data-ttu-id="ee5c7-134">Mer information finns i [komma igång med Cosmos DB][getting-started].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-134">For more information, see [Getting Started with Cosmos DB][getting-started].</span></span> <span data-ttu-id="ee5c7-135">Importera exempeldata Cosmos-DB-konto med hello [Cosmos-DB-Importverktyget][import-data].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-135">Import sample data into your Cosmos DB account with hello [Cosmos DB import tool][import-data].</span></span>
* <span data-ttu-id="ee5c7-136">Genomflöde.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-136">Throughput.</span></span> <span data-ttu-id="ee5c7-137">Läser och skriver från HDInsight räknas mot dina angivna frågeenheter för samlingar.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-137">Reads and writes from HDInsight will be counted towards your allotted request units for your collections.</span></span>
* <span data-ttu-id="ee5c7-138">Kapacitet för ytterligare en lagrad procedur inom varje utdata samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-138">Capacity for an additional stored procedure within each output collection.</span></span> <span data-ttu-id="ee5c7-139">hello lagrade procedurer används för att överföra resulterande dokument.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-139">hello stored procedures are used for transferring resulting documents.</span></span>
* <span data-ttu-id="ee5c7-140">Kapacitet för hello resulterande dokument från hello Hive, Pig eller MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-140">Capacity for hello resulting documents from hello Hive, Pig, or MapReduce jobs.</span></span>
* <span data-ttu-id="ee5c7-141">[*Valfritt*] kapacitet för en ytterligare samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-141">[*Optional*] Capacity for an additional collection.</span></span>

> [!WARNING]
> <span data-ttu-id="ee5c7-142">Ordning tooavoid hello att skapa en ny samling under någon av hello jobb, du antingen skriva ut hello resultat toostdout, spara hello utdata tooyour WASB behållare eller ange en redan befintlig samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-142">In order tooavoid hello creation of a new collection during any of hello jobs, you can either print hello results toostdout, save hello output tooyour WASB container, or specify an already existing collection.</span></span> <span data-ttu-id="ee5c7-143">Hello skiftläget för att ange en befintlig samling, nya dokument kommer att skapas i hello samlingen och redan befintliga dokument påverkas endast om det finns en konflikt i *ID*.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-143">In hello case of specifying an existing collection, new documents will be created inside hello collection and already existing documents will only be affected if there is a conflict in *ids*.</span></span> <span data-ttu-id="ee5c7-144">**hello-anslutningen automatiskt skriver över befintliga dokument med id-konflikter**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-144">**hello connector will automatically overwrite existing documents with id conflicts**.</span></span> <span data-ttu-id="ee5c7-145">Du kan stänga av den här funktionen genom att ange hello upsert alternativet toofalse.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-145">You can turn off this feature by setting hello upsert option toofalse.</span></span> <span data-ttu-id="ee5c7-146">Om upsert är false och en konflikt uppstår, hello Hadoop-jobb misslyckas; ett id konflikt fel rapporteras.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-146">If upsert is false and a conflict occurs, hello Hadoop job will fail; reporting an id conflict error.</span></span>
>
>

## <span data-ttu-id="ee5c7-147"><a name="ProvisionHDInsight"></a>Steg 1: Skapa ett nytt HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="ee5c7-147"><a name="ProvisionHDInsight"></a>Step 1: Create a new HDInsight cluster</span></span>
<span data-ttu-id="ee5c7-148">Den här kursen använder skriptåtgärd från hello Azure Portal toocustomize ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-148">This tutorial uses Script Action from hello Azure Portal toocustomize your HDInsight cluster.</span></span> <span data-ttu-id="ee5c7-149">I den här kursen ska vi använda hello Azure Portal toocreate ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-149">In this tutorial, we will use hello Azure Portal toocreate your HDInsight cluster.</span></span> <span data-ttu-id="ee5c7-150">Anvisningar för hur toouse PowerShell-cmdletar eller hello HDInsight .NET SDK checka ut den [anpassa HDInsight-kluster med skriptåtgärder] [ hdinsight-custom-provision] artikel.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-150">For instructions on how toouse PowerShell cmdlets or hello HDInsight .NET SDK, check out the [Customize HDInsight clusters using Script Action][hdinsight-custom-provision] article.</span></span>

1. <span data-ttu-id="ee5c7-151">Logga in toohello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-151">Sign in toohello [Azure Portal][azure-portal].</span></span>
2. <span data-ttu-id="ee5c7-152">Klicka på **+ ny** på hello överkant hello vänster navigeringsfält, söka efter **HDInsight** i hello övre sökfältet på hello nytt blad.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-152">Click **+ New** on hello top of hello left navigation, search for **HDInsight** in hello top search bar on hello New blade.</span></span>
3. <span data-ttu-id="ee5c7-153">**HDInsight** publiceras av **Microsoft** visas överst hello hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-153">**HDInsight** published by **Microsoft** will appear at hello top of hello Results.</span></span> <span data-ttu-id="ee5c7-154">Klicka på den och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-154">Click on it and then click **Create**.</span></span>
4. <span data-ttu-id="ee5c7-155">Skapa bladet på hello nya HDInsight-kluster, ange din **klusternamnet** och välj hello **prenumeration** önskade tooprovision resursen under.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-155">On hello New HDInsight Cluster create blade, enter your **Cluster Name** and select hello **Subscription** you want tooprovision this resource under.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="ee5c7-156">Klusternamn</span><span class="sxs-lookup"><span data-stu-id="ee5c7-156">Cluster name</span></span></td><td><span data-ttu-id="ee5c7-157">Namnet hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-157">Name hello cluster.</span></span><br/>
<span data-ttu-id="ee5c7-158">DNS-namn måste börja och sluta med ett alfanumeriska tecken och får innehålla tankstreck.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-158">DNS name must start and end with an alpha numeric character, and may contain dashes.</span></span><br/>
<span data-ttu-id="ee5c7-159">hello-fältet måste vara en sträng mellan 3 och 63 tecken.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-159">hello field must be a string between 3 and 63 characters.</span></span></td></tr>
        <tr><td><span data-ttu-id="ee5c7-160">Prenumerationsnamn</span><span class="sxs-lookup"><span data-stu-id="ee5c7-160">Subscription Name</span></span></td>
            <td><span data-ttu-id="ee5c7-161">Om du har mer än en Azure-prenumeration, Välj hello-prenumeration som är värd för ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-161">If you have more than one Azure Subscription, select hello subscription that will host your HDInsight cluster.</span></span> </td></tr>
    </table><span data-ttu-id="ee5c7-162">
5.Klicka på **Välj typ av kluster** och ange hello följande egenskaper toohello angivna värden.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-162">
5. Click **Select Cluster Type** and set hello following properties toohello specified values.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="ee5c7-163">Klustertyp</span><span class="sxs-lookup"><span data-stu-id="ee5c7-163">Cluster type</span></span></td><td><span data-ttu-id="ee5c7-164"><strong>Hadoop</strong></span><span class="sxs-lookup"><span data-stu-id="ee5c7-164"><strong>Hadoop</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="ee5c7-165">Kluster-nivå</span><span class="sxs-lookup"><span data-stu-id="ee5c7-165">Cluster tier</span></span></td><td><span data-ttu-id="ee5c7-166"><strong>Standard</strong></span><span class="sxs-lookup"><span data-stu-id="ee5c7-166"><strong>Standard</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="ee5c7-167">Operativsystem</span><span class="sxs-lookup"><span data-stu-id="ee5c7-167">Operating System</span></span></td><td><span data-ttu-id="ee5c7-168"><strong>Windows</strong></span><span class="sxs-lookup"><span data-stu-id="ee5c7-168"><strong>Windows</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="ee5c7-169">Version</span><span class="sxs-lookup"><span data-stu-id="ee5c7-169">Version</span></span></td><td><span data-ttu-id="ee5c7-170">senaste versionen</span><span class="sxs-lookup"><span data-stu-id="ee5c7-170">latest version</span></span></td></tr>
    </table>

    <span data-ttu-id="ee5c7-171">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-171">Now, click **SELECT**.</span></span>

    ![Ange Hadoop HDInsight inledande klusterinformation][image-customprovision-page1]
6. <span data-ttu-id="ee5c7-173">Klicka på **autentiseringsuppgifter** tooset dina autentiseringsuppgifter för inloggning och fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-173">Click on **Credentials** tooset your login and remote access credentials.</span></span> <span data-ttu-id="ee5c7-174">Välj din **kluster inloggning användarnamn** och **kluster inloggningslösenordet**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-174">Choose your **Cluster Login Username** and **Cluster Login Password**.</span></span>

    <span data-ttu-id="ee5c7-175">Om du vill tooremote i klustret, väljer *Ja* längst hello hello bladet och ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-175">If you want tooremote into your cluster, select *yes* at hello bottom of hello blade and provide a username and password.</span></span>
7. <span data-ttu-id="ee5c7-176">Klicka på **datakällan** tooset åtkomst till den primära platsen för data.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-176">Click on **Data Source** tooset your primary location for data access.</span></span> <span data-ttu-id="ee5c7-177">Välj hello **urvalsmetod** och ange ett redan befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-177">Choose hello **Selection Method** and specify an already existing storage account or create a new one.</span></span>
8. <span data-ttu-id="ee5c7-178">På Hej samma bladet, ange en **standard behållaren** och en **plats**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-178">On hello same blade, specify a **Default Container** and a **Location**.</span></span> <span data-ttu-id="ee5c7-179">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-179">And, click **SELECT**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-180">Välj en plats Stäng tooyour Cosmos DB konto region för bättre prestanda</span><span class="sxs-lookup"><span data-stu-id="ee5c7-180">Select a location close tooyour Cosmos DB account region for better performance</span></span>
   >
   >
9. <span data-ttu-id="ee5c7-181">Klicka på **priser** tooselect hello antal och typer av noder.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-181">Click on **Pricing** tooselect hello number and type of nodes.</span></span> <span data-ttu-id="ee5c7-182">Du kan behålla hello standardkonfigurationen och skala hello antalet arbetarnoder vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-182">You can keep hello default configuration and scale hello number of Worker nodes later on.</span></span>
10. <span data-ttu-id="ee5c7-183">Klicka på **valfri konfiguration**, sedan **skriptåtgärder** i hello valfri konfiguration bladet.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-183">Click **Optional Configuration**, then **Script Actions** in hello Optional Configuration Blade.</span></span>

     <span data-ttu-id="ee5c7-184">I skriptåtgärder, anger du följande information toocustomize hello ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-184">In Script Actions, enter hello following information toocustomize your HDInsight cluster.</span></span>

     <table border='1'>
         <tr><th><span data-ttu-id="ee5c7-185">Egenskap</span><span class="sxs-lookup"><span data-stu-id="ee5c7-185">Property</span></span></th><th><span data-ttu-id="ee5c7-186">Värde</span><span class="sxs-lookup"><span data-stu-id="ee5c7-186">Value</span></span></th></tr>
         <tr><td><span data-ttu-id="ee5c7-187">Namn</span><span class="sxs-lookup"><span data-stu-id="ee5c7-187">Name</span></span></td>
             <td><span data-ttu-id="ee5c7-188">Ange ett namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-188">Specify a name for hello script action.</span></span></td></tr>
         <tr><td><span data-ttu-id="ee5c7-189">Skript-URI</span><span class="sxs-lookup"><span data-stu-id="ee5c7-189">Script URI</span></span></td>
             <td><span data-ttu-id="ee5c7-190">Ange hello URI toohello skript som är anropade toocustomize hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-190">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span></br></br>
<span data-ttu-id="ee5c7-191">Ange:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-191">Please enter:</span></span> </br> <span data-ttu-id="ee5c7-192"><strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-192"><strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="ee5c7-193">Huvud</span><span class="sxs-lookup"><span data-stu-id="ee5c7-193">Head</span></span></td>
             <td><span data-ttu-id="ee5c7-194">Klicka på hello kryssrutan toorun hello PowerShell-skript till hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-194">Click hello checkbox toorun hello PowerShell script onto hello Head node.</span></span></br></br><span data-ttu-id="ee5c7-195">
             <strong>Den här kryssrutan</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-195">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="ee5c7-196">Underordnad</span><span class="sxs-lookup"><span data-stu-id="ee5c7-196">Worker</span></span></td>
             <td><span data-ttu-id="ee5c7-197">Klicka på hello kryssrutan toorun hello PowerShell-skript till hello arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-197">Click hello checkbox toorun hello PowerShell script onto hello Worker node.</span></span></br></br><span data-ttu-id="ee5c7-198">
             <strong>Den här kryssrutan</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-198">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="ee5c7-199">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="ee5c7-199">Zookeeper</span></span></td>
             <td><span data-ttu-id="ee5c7-200">Klicka på hello kryssrutan toorun hello PowerShell-skript till hello Zookeeper.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-200">Click hello checkbox toorun hello PowerShell script onto hello Zookeeper.</span></span></br></br><span data-ttu-id="ee5c7-201">
             <strong>Behövs inte</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-201">
             <strong>Not needed</strong>.</span></span>
             </td></tr>
         <tr><td><span data-ttu-id="ee5c7-202">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ee5c7-202">Parameters</span></span></td>
             <td><span data-ttu-id="ee5c7-203">Ange hello parametrar, om det krävs av hello skript.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-203">Specify hello parameters, if required by hello script.</span></span></br></br><span data-ttu-id="ee5c7-204">
             <strong>Inga parametrar som behövs för</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-204">
             <strong>No Parameters needed</strong>.</span></span></td></tr>
     </table><span data-ttu-id="ee5c7-205">
11.Skapa en ny **resursgruppen** eller Använd en befintlig resursgrupp under Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-205">
11. Create either a new **Resource Group** or use an existing Resource Group under your Azure Subscription.</span></span>
<span data-ttu-id="ee5c7-206">12.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-206">12.</span></span> <span data-ttu-id="ee5c7-207">Kontrollera nu **PIN-kod toodashboard** tootrack dess distribution och klickar på **skapa**!</span><span class="sxs-lookup"><span data-stu-id="ee5c7-207">Now, check **Pin toodashboard** tootrack its deployment and click **Create**!</span></span>

## <span data-ttu-id="ee5c7-208"><a name="InstallCmdlets"></a>Steg 2: Installera och konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ee5c7-208"><a name="InstallCmdlets"></a>Step 2: Install and configure Azure PowerShell</span></span>
1. <span data-ttu-id="ee5c7-209">Installera Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-209">Install Azure PowerShell.</span></span> <span data-ttu-id="ee5c7-210">Anvisningar finns [här][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-210">Instructions can be found [here][powershell-install-configure].</span></span>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-211">Du kan också använda Hdinsight's online Hive-Redigeraren för Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-211">Alternatively, just for Hive queries, you can use HDInsight's online Hive Editor.</span></span> <span data-ttu-id="ee5c7-212">toodo så logga in toohello [Azure Portal][azure-portal], klickar du på **HDInsight** på hello vänster tooview en lista över dina HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-212">toodo so, sign in toohello [Azure Portal][azure-portal], click **HDInsight** on hello left pane tooview a list of your HDInsight clusters.</span></span> <span data-ttu-id="ee5c7-213">Klicka på hello kluster du vill använda toorun Hive-frågor på och klicka sedan på **frågan konsolen**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-213">Click hello cluster you want toorun Hive queries on, and then click **Query Console**.</span></span>
   >
   >
2. <span data-ttu-id="ee5c7-214">Öppna hello Azure PowerShell Integrated Scripting Environment:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-214">Open hello Azure PowerShell Integrated Scripting Environment:</span></span>

   * <span data-ttu-id="ee5c7-215">På en dator som kör Windows 8 eller Windows Server 2012 eller senare, kan du använda hello inbyggda sökning.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-215">On a computer running Windows 8 or Windows Server 2012 or higher, you can use hello built-in Search.</span></span> <span data-ttu-id="ee5c7-216">Skriv från startskärmen hello **powershell ise** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-216">From hello Start screen, type **powershell ise** and click **Enter**.</span></span>
   * <span data-ttu-id="ee5c7-217">Använd hello Start-menyn på en dator som kör en version tidigare än Windows 8 eller Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-217">On a computer running a version earlier than Windows 8 or Windows Server 2012, use hello Start menu.</span></span> <span data-ttu-id="ee5c7-218">Hello Start-menyn, Skriv **kommandotolk** hello sökrutan och sedan i resultatlista hello, klickar du på **kommandotolk**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-218">From hello Start menu, type **Command Prompt** in hello search box, then in hello list of results, click **Command Prompt**.</span></span> <span data-ttu-id="ee5c7-219">I hello kommandotolk, skriver **powershell_ise** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-219">In hello Command Prompt, type **powershell_ise** and click **Enter**.</span></span>
3. <span data-ttu-id="ee5c7-220">Lägg till ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-220">Add your Azure Account.</span></span>

   1. <span data-ttu-id="ee5c7-221">Skriv i hello konsolfönstret, **Add-AzureAccount** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-221">In hello Console Pane, type **Add-AzureAccount** and click **Enter**.</span></span>
   2. <span data-ttu-id="ee5c7-222">Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-222">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
   3. <span data-ttu-id="ee5c7-223">Ange hello lösenord för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-223">Type in hello password for your Azure subscription.</span></span>
   4. <span data-ttu-id="ee5c7-224">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-224">Click **Sign in**.</span></span>
4. <span data-ttu-id="ee5c7-225">följande diagram hello identifierar hello viktiga delar i datormiljön Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-225">hello following diagram identifies hello important parts of your Azure PowerShell Scripting Environment.</span></span>

    ![Diagram för Azure PowerShell][azure-powershell-diagram]

## <span data-ttu-id="ee5c7-227"><a name="RunHive"></a>Steg 3: Kör en Hive-jobb med hjälp av Cosmos-databas och HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee5c7-227"><a name="RunHive"></a>Step 3: Run a Hive job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ee5c7-228">Alla variabler som anges av < > måste fyllas i med hjälp av inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-228">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="ee5c7-229">Ange hello följande variabler i PowerShell-skript-fönstret.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-229">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p><span data-ttu-id="ee5c7-230">Vi börjar konstruera frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-230">Let's begin constructing your query string.</span></span> <span data-ttu-id="ee5c7-231">Vi ska skriva en Hive-fråga som tar alla dokument systemgenererade tidsstämplar (_ts) och unika ID: n (_rid) från en samling Azure Cosmos DB, räknar alla dokument med hello minut och sedan lagrar hello resultaten tillbaka till en ny Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-231">We'll write a Hive query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>

    <p><span data-ttu-id="ee5c7-232">Först ska vi skapa en Hive-tabell från våra Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-232">First, let's create a Hive table from our Azure Cosmos DB collection.</span></span> <span data-ttu-id="ee5c7-233">Lägg till hello följande kodfragment toohello PowerShell-skript fönstret <strong>när</strong> hello kodstycke från #1.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-233">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="ee5c7-234">Kontrollera att du inkluderar hello valfria DocumentDB.query parametern t trim våra dokument toojust _ts och _rid.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-234">Make sure you include hello optional DocumentDB.query parameter t trim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-235">**Namnge DocumentDB.inputCollections var inte fel.**</span><span class="sxs-lookup"><span data-stu-id="ee5c7-235">**Naming DocumentDB.inputCollections was not a mistake.**</span></span> <span data-ttu-id="ee5c7-236">Ja, tillåter vi lägger till flera samlingar som indata:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-236">Yes, we allow adding multiple collections as an input:</span></span> </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. <span data-ttu-id="ee5c7-237">Nu ska vi skapa en Hive-tabell för hello utdata samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-237">Next, let's create a Hive table for hello output collection.</span></span> <span data-ttu-id="ee5c7-238">hello utdata dokumentegenskaper blir hello månad, dag, timme, minut och hello totala antalet förekomster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-238">hello output document properties will be hello month, day, hour, minute, and hello total number of occurrences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-239">**Namnge DocumentDB.outputCollections var ännu igen inte fel.**</span><span class="sxs-lookup"><span data-stu-id="ee5c7-239">**Yet again, naming DocumentDB.outputCollections was not a mistake.**</span></span> <span data-ttu-id="ee5c7-240">Ja, tillåter vi lägger till flera samlingar som utdata:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-240">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="ee5c7-241">'*DocumentDB.outputCollections*'='*\<utdata för DocumentDB-samlingsnamn 1\>*,*\<utdata för DocumentDB-samlingsnamn 2\>*'</span><span class="sxs-lookup"><span data-stu-id="ee5c7-241">'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*'</span></span> </br> <span data-ttu-id="ee5c7-242">hello samling namnen avgränsas utan blanksteg, med bara ett komma.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-242">hello collection names are separated without spaces, using only a single comma.</span></span> </br></br>
   > <span data-ttu-id="ee5c7-243">Dokument kommer att distribuerade resursallokering över flera samlingar.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-243">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="ee5c7-244">En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-244">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. <span data-ttu-id="ee5c7-245">Slutligen utdata ska vi tally hello dokument efter månad, dag, timme och minut och infoga hello resultaten tillbaka till hello Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-245">Finally, let's tally hello documents by month, day, hour, and minute and insert hello results back into hello output Hive table.</span></span>

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. <span data-ttu-id="ee5c7-246">Lägg till följande skript fragment toocreate jobbdefinitionen Hive från föregående frågan om hello hello.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-246">Add hello following script snippet toocreate a Hive job definition from hello previous query.</span></span>

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    <span data-ttu-id="ee5c7-247">Du kan också använda hello - filen växla toospecify en skriptfil för HiveQL på HDFS.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-247">You can also use hello -File switch toospecify a HiveQL script file on HDFS.</span></span>
4. <span data-ttu-id="ee5c7-248">Lägg till följande fragment toosave hello starttiden hello och skicka hello Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-248">Add hello following snippet toosave hello start time and submit hello Hive job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. <span data-ttu-id="ee5c7-249">Lägg till följande toowait för hello Hive-jobbet toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-249">Add hello following toowait for hello Hive job toocomplete.</span></span>

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. <span data-ttu-id="ee5c7-250">Lägg till hello följande tooprint hello standard utdata och hello start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-250">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. <span data-ttu-id="ee5c7-251">**Kör** nya skriptet!</span><span class="sxs-lookup"><span data-stu-id="ee5c7-251">**Run** your new script!</span></span> <span data-ttu-id="ee5c7-252">**Klicka på** hello grönt köra knappen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-252">**Click** hello green execute button.</span></span>
8. <span data-ttu-id="ee5c7-253">Kontrollera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-253">Check hello results.</span></span> <span data-ttu-id="ee5c7-254">Logga in på hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-254">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="ee5c7-255">Klicka på <strong>Bläddra</strong> på hello vänster panel.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-255">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
   2. <span data-ttu-id="ee5c7-256">Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-256">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
   3. <span data-ttu-id="ee5c7-257">Hitta och klickar på <strong>Azure Cosmos DB konton</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-257">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
   4. <span data-ttu-id="ee5c7-258">Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i Hive-frågan.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-258">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Hive query.</span></span></br>
   5. <span data-ttu-id="ee5c7-259">Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-259">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

   <span data-ttu-id="ee5c7-260">Du ser hello resultaten av Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-260">You will see hello results of your Hive query.</span></span>

   ![Hive frågeresultat][image-hive-query-results]

## <span data-ttu-id="ee5c7-262"><a name="RunPig"></a>Steg 4: Kör ett Pig-jobb med hjälp av Cosmos-DB och HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee5c7-262"><a name="RunPig"></a>Step 4: Run a Pig job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ee5c7-263">Alla variabler som anges av < > måste fyllas i med hjälp av inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-263">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="ee5c7-264">Ange hello följande variabler i PowerShell-skript-fönstret.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-264">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p><span data-ttu-id="ee5c7-265">Vi börjar konstruera frågesträngen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-265">Let's begin constructing your query string.</span></span> <span data-ttu-id="ee5c7-266">Vi ska skriva en Pig-fråga som tar alla dokument systemgenererade tidsstämplar (_ts) och unika ID: n (_rid) från en samling Azure Cosmos DB, räknar alla dokument med hello minut och sedan lagrar hello resultaten tillbaka till en ny Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-266">We'll write a Pig query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>
    <p><span data-ttu-id="ee5c7-267">Ladda först dokument från Cosmos-DB i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-267">First, load documents from Cosmos DB into HDInsight.</span></span> <span data-ttu-id="ee5c7-268">Lägg till hello följande kodfragment toohello PowerShell-skript fönstret <strong>när</strong> hello kodstycke från #1.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-268">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="ee5c7-269">Kontrollera att tooadd en DocumentDB fråga toohello valfria DocumentDB-fråga parametern tootrim våra dokument toojust _ts och _rid.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-269">Make sure tooadd a DocumentDB query toohello optional DocumentDB query parameter tootrim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-270">Ja, tillåter vi lägger till flera samlingar som indata:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-270">Yes, we allow adding multiple collections as an input:</span></span> </br>
   > <span data-ttu-id="ee5c7-271">'*\<Indata för DocumentDB-samlingsnamn 1\>*,*\<indata för DocumentDB-samlingsnamn 2\>*'</span><span class="sxs-lookup"><span data-stu-id="ee5c7-271">'*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</span></span></br> <span data-ttu-id="ee5c7-272">hello samling namnen avgränsas utan blanksteg, med bara ett komma.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-272">hello collection names are separated without spaces, using only a single comma.</span></span> </b>
   >
   >

    <span data-ttu-id="ee5c7-273">Dokument kommer att distribuerade resursallokering över flera samlingar.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-273">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="ee5c7-274">En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-274">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. <span data-ttu-id="ee5c7-275">Nu ska vi tally hello dokument av hello månad, dag, timme, minut och hello totala antalet förekomster.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-275">Next, let's tally hello documents by hello month, day, hour, minute, and hello total number of occurrences.</span></span>

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. <span data-ttu-id="ee5c7-276">Slutligen ska vi lagra hello resultat i vår nya utdata-samlingen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-276">Finally, let's store hello results into our new output collection.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ee5c7-277">Ja, tillåter vi lägger till flera samlingar som utdata:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-277">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="ee5c7-278">'\<Utdata för DocumentDB-samlingsnamn 1\>,\<utdata för DocumentDB-samlingsnamn 2\>'</span><span class="sxs-lookup"><span data-stu-id="ee5c7-278">'\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</span></span></br> <span data-ttu-id="ee5c7-279">hello samling namnen avgränsas utan blanksteg, med bara ett komma.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-279">hello collection names are separated without spaces, using only a single comma.</span></span></br>
   > <span data-ttu-id="ee5c7-280">Dokument kan vara distribuerade resursallokering för hello flera samlingar.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-280">Documents will be distributed round-robin across hello multiple collections.</span></span> <span data-ttu-id="ee5c7-281">En batch med dokument kommer att lagras i en samling och sedan en andra grupp dokument kommer att lagras i hello nästa insamling och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-281">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. <span data-ttu-id="ee5c7-282">Lägg till följande skript fragment toocreate jobbdefinitionen Pig från föregående frågan om hello hello.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-282">Add hello following script snippet toocreate a Pig job definition from hello previous query.</span></span>

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    <span data-ttu-id="ee5c7-283">Du kan också använda hello - filen växla toospecify en skriptfil för Pig på HDFS.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-283">You can also use hello -File switch toospecify a Pig script file on HDFS.</span></span>
6. <span data-ttu-id="ee5c7-284">Lägg till följande fragment toosave hello starttiden hello och skicka hello Pig-jobbet.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-284">Add hello following snippet toosave hello start time and submit hello Pig job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. <span data-ttu-id="ee5c7-285">Lägg till följande toowait för hello Pig-jobbet toocomplete hello.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-285">Add hello following toowait for hello Pig job toocomplete.</span></span>

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. <span data-ttu-id="ee5c7-286">Lägg till hello följande tooprint hello standard utdata och hello start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-286">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. <span data-ttu-id="ee5c7-287">**Kör** nya skriptet!</span><span class="sxs-lookup"><span data-stu-id="ee5c7-287">**Run** your new script!</span></span> <span data-ttu-id="ee5c7-288">**Klicka på** hello grönt köra knappen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-288">**Click** hello green execute button.</span></span>
10. <span data-ttu-id="ee5c7-289">Kontrollera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-289">Check hello results.</span></span> <span data-ttu-id="ee5c7-290">Logga in på hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-290">Sign into hello [Azure Portal][azure-portal].</span></span>

    1. <span data-ttu-id="ee5c7-291">Klicka på <strong>Bläddra</strong> på hello vänster panel.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-291">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
    2. <span data-ttu-id="ee5c7-292">Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-292">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
    3. <span data-ttu-id="ee5c7-293">Hitta och klickar på <strong>Azure Cosmos DB konton</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-293">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
    4. <span data-ttu-id="ee5c7-294">Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i Pig frågan.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-294">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Pig query.</span></span></br>
    5. <span data-ttu-id="ee5c7-295">Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-295">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

    <span data-ttu-id="ee5c7-296">Hello resultatet av frågan Pig visas.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-296">You will see hello results of your Pig query.</span></span>

    ![Pig frågeresultat][image-pig-query-results]

## <span data-ttu-id="ee5c7-298"><a name="RunMapReduce"></a>Steg 5: Kör ett MapReduce-jobb med hjälp av Azure Cosmos DB och HDInsight</span><span class="sxs-lookup"><span data-stu-id="ee5c7-298"><a name="RunMapReduce"></a>Step 5: Run a MapReduce job using Azure Cosmos DB and HDInsight</span></span>
1. <span data-ttu-id="ee5c7-299">Ange hello följande variabler i PowerShell-skript-fönstret.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-299">Set hello following variables in your PowerShell Script pane.</span></span>

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. <span data-ttu-id="ee5c7-300">Vi ska köra ett MapReduce-jobb som räknar hello antalet förekomster för varje dokumentegenskap från Azure DB som Cosmos-samling.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-300">We'll execute a MapReduce job that tallies hello number of occurrences for each Document property from your Azure Cosmos DB collection.</span></span> <span data-ttu-id="ee5c7-301">Lägg till det här skriptet fragment **när** hello fragment ovan.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-301">Add this script snippet **after** hello snippet above.</span></span>

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > <span data-ttu-id="ee5c7-302">TallyProperties v01.jar medföljer hello anpassad installation av hello Cosmos DB Hadoop-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-302">TallyProperties-v01.jar comes with hello custom installation of hello Cosmos DB Hadoop Connector.</span></span>
   >
   >
3. <span data-ttu-id="ee5c7-303">Lägg till hello efter kommandot toosubmit hello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-303">Add hello following command toosubmit hello MapReduce job.</span></span>

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    <span data-ttu-id="ee5c7-304">Dessutom toohello MapReduce jobbdefinitionen du också ange klusternamnet hello HDInsight där du vill toorun hello MapReduce-jobb och hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-304">In addition toohello MapReduce job definition, you also provide hello HDInsight cluster name where you want toorun hello MapReduce job, and hello credentials.</span></span> <span data-ttu-id="ee5c7-305">Hej AzureHDInsightJob Start är en asynkron anrop.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-305">hello Start-AzureHDInsightJob is an asynchronized call.</span></span> <span data-ttu-id="ee5c7-306">toocheck hello slutförande av hello jobb, Använd hello *vänta AzureHDInsightJob* cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-306">toocheck hello completion of hello job, use hello *Wait-AzureHDInsightJob* cmdlet.</span></span>
4. <span data-ttu-id="ee5c7-307">Lägg till följande kommando toocheck hello eventuella fel med körs hello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-307">Add hello following command toocheck any errors with running hello MapReduce job.</span></span>

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. <span data-ttu-id="ee5c7-308">**Kör** nya skriptet!</span><span class="sxs-lookup"><span data-stu-id="ee5c7-308">**Run** your new script!</span></span> <span data-ttu-id="ee5c7-309">**Klicka på** hello grönt köra knappen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-309">**Click** hello green execute button.</span></span>
6. <span data-ttu-id="ee5c7-310">Kontrollera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-310">Check hello results.</span></span> <span data-ttu-id="ee5c7-311">Logga in på hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-311">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="ee5c7-312">Klicka på <strong>Bläddra</strong> på hello vänster panel.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-312">Click <strong>Browse</strong> on hello left-side panel.</span></span>
   2. <span data-ttu-id="ee5c7-313">Klicka på <strong>allt</strong> hello längst upp till höger i hello Bläddra panelen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-313">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span>
   3. <span data-ttu-id="ee5c7-314">Hitta och klickar på <strong>Azure Cosmos DB konton</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-314">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span>
   4. <span data-ttu-id="ee5c7-315">Leta sedan reda din <strong>Azure Cosmos-DB konto</strong>, sedan <strong>Azure Cosmos DB Database</strong> och <strong>Azure Cosmos DB samling</strong> som är associerade med hello utdata samling som anges i MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-315">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your MapReduce job.</span></span>
   5. <span data-ttu-id="ee5c7-316">Klicka slutligen på <strong>Dokumentutforskaren</strong> under <strong>Developer Tools</strong>.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-316">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span>

      <span data-ttu-id="ee5c7-317">Du ser hello resultaten av MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-317">You will see hello results of your MapReduce job.</span></span>

      ![MapReduce frågeresultat][image-mapreduce-query-results]

## <span data-ttu-id="ee5c7-319"><a name="NextSteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ee5c7-319"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="ee5c7-320">Grattis!</span><span class="sxs-lookup"><span data-stu-id="ee5c7-320">Congratulations!</span></span> <span data-ttu-id="ee5c7-321">Du körde din första Hive, Pig och MapReduce-jobb med hjälp av Azure Cosmos DB och HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-321">You just ran your first Hive, Pig, and MapReduce jobs using Azure Cosmos DB and HDInsight.</span></span>

<span data-ttu-id="ee5c7-322">Vi har öppen källkod våra Hadoop-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="ee5c7-322">We have open sourced our Hadoop Connector.</span></span> <span data-ttu-id="ee5c7-323">Om du är intresserad av du kan bidra på [GitHub][github].</span><span class="sxs-lookup"><span data-stu-id="ee5c7-323">If you're interested, you can contribute on [GitHub][github].</span></span>

<span data-ttu-id="ee5c7-324">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="ee5c7-324">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="ee5c7-325">[Utveckla ett Java-program med Documentdb][documentdb-java-application]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-325">[Develop a Java application with Documentdb][documentdb-java-application]</span></span>
* <span data-ttu-id="ee5c7-326">[Utveckla Java-MapReduce-program för Hadoop i HDInsight][hdinsight-develop-deploy-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-326">[Develop Java MapReduce programs for Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]</span></span>
* <span data-ttu-id="ee5c7-327">[Komma igång med Hadoop med Hive i HDInsight tooanalyze mobila luren användning][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-327">[Get started using Hadoop with Hive in HDInsight tooanalyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="ee5c7-328">[Använda MapReduce med HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span></span>
* <span data-ttu-id="ee5c7-329">[Använda Hive med HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-329">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="ee5c7-330">[Använda Pig med HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-330">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="ee5c7-331">[Anpassa HDInsight-kluster med skriptåtgärder][hdinsight-hadoop-customize-cluster]</span><span class="sxs-lookup"><span data-stu-id="ee5c7-331">[Customize HDInsight clusters using Script Action][hdinsight-hadoop-customize-cluster]</span></span>

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
