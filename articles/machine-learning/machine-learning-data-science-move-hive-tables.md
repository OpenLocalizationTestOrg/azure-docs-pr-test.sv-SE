---
title: "aaaCreate Hive tabeller och Läs in data från Azure Blob Storage | Microsoft Docs"
description: "Skapa Hive-tabeller och läsa in data i blob toohive tabeller"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: cff9280d-18ce-4b66-a54f-19f358d1ad90
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 09622972bcac31c2971858393a8340f24e4b7390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-hive-tables-and-load-data-from-azure-blob-storage"></a><span data-ttu-id="06021-103">Skapa Hive-tabeller och Läs in data från Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="06021-103">Create Hive tables and load data from Azure Blob Storage</span></span>
<span data-ttu-id="06021-104">Det här avsnittet innehåller allmänna Hive-frågor som skapar Hive-tabeller och läsa in data från Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="06021-104">This topic presents generic Hive queries that create Hive tables and load data from Azure blob storage.</span></span> <span data-ttu-id="06021-105">Vägledning finns även på partitionering Hive-tabeller och om hur du använder hello optimerade raden kolumner (ORC) formatering tooimprove frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="06021-105">Some guidance is also provided on partitioning Hive tables and on using hello Optimized Row Columnar (ORC) formatting tooimprove query performance.</span></span>

<span data-ttu-id="06021-106">Detta **menyn** länkar tootopics som beskriver hur tooingest data till mål-miljöer där hello data kan lagras och behandlas under hello Team Data vetenskap processen (TDSP).</span><span class="sxs-lookup"><span data-stu-id="06021-106">This **menu** links tootopics that describe how tooingest data into target environments where hello data can be stored and processed during hello Team Data Science Process (TDSP).</span></span>

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="06021-107">Krav</span><span class="sxs-lookup"><span data-stu-id="06021-107">Prerequisites</span></span>
<span data-ttu-id="06021-108">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="06021-108">This article assumes that you have:</span></span>

* <span data-ttu-id="06021-109">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="06021-109">Created an Azure storage account.</span></span> <span data-ttu-id="06021-110">Om du behöver mer information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="06021-110">If you need instructions, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>
* <span data-ttu-id="06021-111">Etablera ett anpassat Hadoop-kluster med hello HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="06021-111">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span>  <span data-ttu-id="06021-112">Om du behöver mer information, se [anpassa Azure HDInsight Hadoop-kluster för avancerade analyser](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="06021-112">If you need instructions, see [Customize Azure HDInsight Hadoop clusters for advanced analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="06021-113">Aktiverade fjärråtkomst toohello klustret inloggad och öppnas hello Hadoop kommandoradskonsol.</span><span class="sxs-lookup"><span data-stu-id="06021-113">Enabled remote access toohello cluster, logged in, and opened hello Hadoop Command-Line console.</span></span> <span data-ttu-id="06021-114">Om du behöver mer information, se [åtkomst hello Head-nod för Hadoop-kluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="06021-114">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

## <a name="upload-data-tooazure-blob-storage"></a><span data-ttu-id="06021-115">Ladda upp data tooAzure blob-lagring</span><span class="sxs-lookup"><span data-stu-id="06021-115">Upload data tooAzure blob storage</span></span>
<span data-ttu-id="06021-116">Om du har skapat en virtuell Azure-dator genom att följa instruktionerna i hello [ställa in en virtuell Azure-dator för avancerade analyser](machine-learning-data-science-setup-virtual-machine.md), den här skriptfilen skulle ha varit hämtade toohello *C:\\ Användare\\\<användarnamn\>\\dokument\\datavetenskap skript* på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="06021-116">If you created an Azure virtual machine by following hello instructions provided in [Set up an Azure virtual machine for advanced analytics](machine-learning-data-science-setup-virtual-machine.md), this script file should have been downloaded toohello *C:\\Users\\\<user name\>\\Documents\\Data Science Scripts* directory on hello virtual machine.</span></span> <span data-ttu-id="06021-117">Dessa Hive-frågor kräver bara att du ansluter dina egna dataschemat och Azure blob storage-konfiguration i hello relevanta fält toobe klar för överföring.</span><span class="sxs-lookup"><span data-stu-id="06021-117">These Hive queries only require that you plug in your own data schema and Azure blob storage configuration in hello appropriate fields toobe ready for submission.</span></span>

<span data-ttu-id="06021-118">Vi förutsätter att hello data för Hive-tabeller är i ett **okomprimerad** tabellformat och att hello data har överförts toohello standard (eller tooan ytterligare) behållare för hello storage-konto som används av hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-118">We assume that hello data for Hive tables is in an **uncompressed** tabular format, and that hello data has been uploaded toohello default (or tooan additional) container of hello storage account used by hello Hadoop cluster.</span></span>

<span data-ttu-id="06021-119">Om du vill toopractice på hello **NYC Taxi resa Data**, måste du:</span><span class="sxs-lookup"><span data-stu-id="06021-119">If you want toopractice on hello **NYC Taxi Trip Data**, you need to:</span></span>

* <span data-ttu-id="06021-120">**Hämta** hello 24 [NYC Taxi resa Data](http://www.andresmh.com/nyctaxitrips) filer (12 resa filer och 12 avgiften-filer)</span><span class="sxs-lookup"><span data-stu-id="06021-120">**download** hello 24 [NYC Taxi Trip Data](http://www.andresmh.com/nyctaxitrips) files (12 Trip files and 12 Fare files),</span></span>
* <span data-ttu-id="06021-121">**Packa upp** alla filer i CSV-filer, och sedan</span><span class="sxs-lookup"><span data-stu-id="06021-121">**unzip** all files into .csv files, and then</span></span>
* <span data-ttu-id="06021-122">**Överför** dem toohello standard (eller lämplig behållare) av hello Azure storage-konto som har skapats av hello proceduren som beskrivs i hello [anpassa Azure HDInsight Hadoop-kluster för Advanced Analytics processen och teknik ](machine-learning-data-science-customize-hadoop-cluster.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="06021-122">**upload** them toohello default (or appropriate container) of hello Azure storage account that was created by hello procedure outlined in hello [Customize Azure HDInsight Hadoop clusters for Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md) topic.</span></span> <span data-ttu-id="06021-123">hello processen tooupload hello CSV-filer toohello standardbehållaren för hello storage-konto kan hittas på den här [sidan](machine-learning-data-science-process-hive-walkthrough.md#upload).</span><span class="sxs-lookup"><span data-stu-id="06021-123">hello process tooupload hello .csv files toohello default container on hello storage account can be found on this [page](machine-learning-data-science-process-hive-walkthrough.md#upload).</span></span>

## <span data-ttu-id="06021-124"><a name="submit"></a>Hur toosubmit Hive-frågor</span><span class="sxs-lookup"><span data-stu-id="06021-124"><a name="submit"></a>How toosubmit Hive queries</span></span>
<span data-ttu-id="06021-125">Du kan skicka hive-frågor med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="06021-125">Hive queries can be submitted by using:</span></span>

1. [<span data-ttu-id="06021-126">Skicka Hive-frågor via Hadoop kommandoraden i headnode av Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="06021-126">Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>](#headnode)
2. [<span data-ttu-id="06021-127">Skicka Hive-frågor med hello Hive-redigeraren</span><span class="sxs-lookup"><span data-stu-id="06021-127">Submit Hive queries with hello Hive Editor</span></span>](#hive-editor)
3. [<span data-ttu-id="06021-128">Skicka Hive-frågor med Azure PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="06021-128">Submit Hive queries with Azure PowerShell Commands</span></span>](#ps)

<span data-ttu-id="06021-129">Hive-frågor är SQL-liknande.</span><span class="sxs-lookup"><span data-stu-id="06021-129">Hive queries are SQL-like.</span></span> <span data-ttu-id="06021-130">Om du är bekant med SQL hello kan vara [Hive för SQL användare Cheat blad](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) användbart.</span><span class="sxs-lookup"><span data-stu-id="06021-130">If you are familiar with SQL, you may find hello [Hive for SQL Users Cheat Sheet](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) useful.</span></span>

<span data-ttu-id="06021-131">När en Hive-fråga, kan du också styra hello målet hello utdata från Hive-frågor om den finnas på hello skärmen eller tooa lokal fil på hello huvudnod eller tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="06021-131">When submitting a Hive query, you can also control hello destination of hello output from Hive queries, whether it be on hello screen or tooa local file on hello head node or tooan Azure blob.</span></span>

### <span data-ttu-id="06021-132"><a name="headnode"></a> 1. Skicka Hive-frågor via Hadoop kommandoraden i headnode av Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="06021-132"><a name="headnode"></a> 1. Submit Hive queries through Hadoop Command Line in headnode of Hadoop cluster</span></span>
<span data-ttu-id="06021-133">Om hello Hive är-frågan komplex eller skicka den direkt i hello huvudnod hello Hadoop-kluster normalt leder toofaster Stäng runt än att skicka den med en Hive-redigerare eller Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="06021-133">If hello Hive query is complex, submitting it directly in hello head node of hello Hadoop cluster typically leads toofaster turn around than submitting it with a Hive Editor or Azure PowerShell scripts.</span></span>

<span data-ttu-id="06021-134">Logga in toohello huvudnod hello Hadoop-kluster, öppna hello Hadoop kommandoraden på hello skrivbord hello huvudnod och anger kommandot `cd %hive_home%\bin`.</span><span class="sxs-lookup"><span data-stu-id="06021-134">Log in toohello head node of hello Hadoop cluster, open hello Hadoop Command Line on hello desktop of hello head node, and enter command `cd %hive_home%\bin`.</span></span>

<span data-ttu-id="06021-135">Det finns tre sätt toosubmit Hive-frågor i hello Hadoop-kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="06021-135">You have three ways toosubmit Hive queries in hello Hadoop Command Line:</span></span>

* <span data-ttu-id="06021-136">direkt</span><span class="sxs-lookup"><span data-stu-id="06021-136">directly</span></span>
* <span data-ttu-id="06021-137">med hjälp av .hql filer</span><span class="sxs-lookup"><span data-stu-id="06021-137">using .hql files</span></span>
* <span data-ttu-id="06021-138">med hello Hive kommandokonsolen</span><span class="sxs-lookup"><span data-stu-id="06021-138">with hello Hive command console</span></span>

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a><span data-ttu-id="06021-139">Skicka Hive-frågor direkt i Hadoop kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="06021-139">Submit Hive queries directly in Hadoop Command Line.</span></span>
<span data-ttu-id="06021-140">Du kan köra kommandot som `hive -e "<your hive query>;` toosubmit enkel Hive-frågor direkt i Hadoop kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="06021-140">You can run command like `hive -e "<your hive query>;` toosubmit simple Hive queries directly in Hadoop Command Line.</span></span> <span data-ttu-id="06021-141">Här är ett exempel där hello röda rutan beskrivs hello kommando som skickar hello Hive-fråga och hello gröna rutan beskrivs hello utdata från hello Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="06021-141">Here is an example, where hello red box outlines hello command that submits hello Hive query, and hello green box outlines hello output from hello Hive query.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a><span data-ttu-id="06021-143">Skicka Hive-frågor i .hql filer</span><span class="sxs-lookup"><span data-stu-id="06021-143">Submit Hive queries in .hql files</span></span>
<span data-ttu-id="06021-144">När hello Hive-frågan är mer komplicerad och har flera rader, är redigerar frågor i kommandoraden eller Hive kommandokonsolen inte praktiskt.</span><span class="sxs-lookup"><span data-stu-id="06021-144">When hello Hive query is more complicated and has multiple lines, editing queries in command line or Hive command console is not practical.</span></span> <span data-ttu-id="06021-145">Ett alternativ är toouse en textredigerare i hello huvudnod hello Hadoop-kluster toosave hello Hive-frågor i en .hql-fil i en lokal katalog på hello huvudnod.</span><span class="sxs-lookup"><span data-stu-id="06021-145">An alternative is toouse a text editor in hello head node of hello Hadoop cluster toosave hello Hive queries in a .hql file in a local directory of hello head node.</span></span> <span data-ttu-id="06021-146">Sedan kan skicka hello Hive-fråga i hello .hql filen med hjälp av hello `-f` argumentet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06021-146">Then hello Hive query in hello .hql file can be submitted by using hello `-f` argument as follows:</span></span>

    hive -f "<path toohello .hql file>"

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)

<span data-ttu-id="06021-148">**Ignorera förlopp status skärmen utskrift av Hive-frågor**</span><span class="sxs-lookup"><span data-stu-id="06021-148">**Suppress progress status screen print of Hive queries**</span></span>

<span data-ttu-id="06021-149">Som standard när Hive-frågan har skickats i Hadoop kommandoraden skrivs hello fortskrider hello kartan/minska jobb ut på skärmen.</span><span class="sxs-lookup"><span data-stu-id="06021-149">By default, after Hive query is submitted in Hadoop Command Line, hello progress of hello Map/Reduce job is printed out on screen.</span></span> <span data-ttu-id="06021-150">toosuppress hello skärmen utskrift av hello kartan/minska jobb pågår, kan du använda ett argument `-S` (”S” i versaler) i hello kommandoraden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06021-150">toosuppress hello screen print of hello Map/Reduce job progress, you can use an argument `-S` ("S" in upper case) in hello command line as follows:</span></span>

    hive -S -f "<path toohello .hql file>"
<span data-ttu-id="06021-151">.</span><span class="sxs-lookup"><span data-stu-id="06021-151">.</span></span>    <span data-ttu-id="06021-152">hive -S -e ”<Hive queries>”</span><span class="sxs-lookup"><span data-stu-id="06021-152">hive -S -e "<Hive queries>"</span></span>

#### <a name="submit-hive-queries-in-hive-command-console"></a><span data-ttu-id="06021-153">Skicka Hive-frågor i Hive-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="06021-153">Submit Hive queries in Hive command console.</span></span>
<span data-ttu-id="06021-154">Du kan också ange hello Hive kommandokonsolen genom att köra kommandot `hive` i Hadoop kommandoraden och skicka Hive-frågor i Hive-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="06021-154">You can also first enter hello Hive command console by running command `hive` in Hadoop Command Line, and then submit Hive queries in Hive command console.</span></span> <span data-ttu-id="06021-155">Här är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="06021-155">Here is an example.</span></span> <span data-ttu-id="06021-156">I det här exemplet hello två röda rutor markeringen hello kommandon som används för tooenter hello Hive kommandokonsolen och hello skickade i Hive kommandokonsolen respektive Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="06021-156">In this example, hello two red boxes highlight hello commands used tooenter hello Hive command console, and hello Hive query submitted in Hive command console, respectively.</span></span> <span data-ttu-id="06021-157">hello gröna rutan visar hello utdata från hello Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="06021-157">hello green box highlights hello output from hello Hive query.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

<span data-ttu-id="06021-159">hello föregående exempel utdata direkt hello Hive frågeresultat på skärmen.</span><span class="sxs-lookup"><span data-stu-id="06021-159">hello previous examples directly output hello Hive query results on screen.</span></span> <span data-ttu-id="06021-160">Du kan också skriva hello tooa lokala utdatafilen på hello huvudnod eller tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="06021-160">You can also write hello output tooa local file on hello head node, or tooan Azure blob.</span></span> <span data-ttu-id="06021-161">Du kan sedan använda andra verktyg toofurther analysera hello utdata för Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="06021-161">Then, you can use other tools toofurther analyze hello output of Hive queries.</span></span>

<span data-ttu-id="06021-162">**Hive-fråga resultat tooa lokala utdatafilen.**</span><span class="sxs-lookup"><span data-stu-id="06021-162">**Output Hive query results tooa local file.**</span></span>
<span data-ttu-id="06021-163">toooutput Hive-fråga resultat tooa lokal katalog på hello huvudnod, har du toosubmit hello Hive-fråga i hello Hadoop kommandoraden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06021-163">toooutput Hive query results tooa local directory on hello head node, you have toosubmit hello Hive query in hello Hadoop Command Line as follows:</span></span>

    hive -e "<hive query>" > <local path in hello head node>

<span data-ttu-id="06021-164">I följande exempel hello, hello utdata för Hive-fråga skrivs till en fil `hivequeryoutput.txt` i katalogen `C:\apps\temp`.</span><span class="sxs-lookup"><span data-stu-id="06021-164">In hello following example, hello output of Hive query is written into a file `hivequeryoutput.txt` in directory `C:\apps\temp`.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

<span data-ttu-id="06021-166">**Utdata Hive-fråga resultat tooan Azure blob**</span><span class="sxs-lookup"><span data-stu-id="06021-166">**Output Hive query results tooan Azure blob**</span></span>

<span data-ttu-id="06021-167">Du kan också spara hello Hive-fråga resultat tooan Azure blob inom hello standardbehållaren för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-167">You can also output hello Hive query results tooan Azure blob, within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="06021-168">hello Hive-fråga för det här är följande:</span><span class="sxs-lookup"><span data-stu-id="06021-168">hello Hive query for this is as follows:</span></span>

    insert overwrite directory wasb:///<directory within hello default container> <select clause from ...>

<span data-ttu-id="06021-169">I följande exempel hello, hello utdata för Hive-fråga skrivs tooa blob directory `queryoutputdir` inom hello standardbehållaren för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-169">In hello following example, hello output of Hive query is written tooa blob directory `queryoutputdir` within hello default container of hello Hadoop cluster.</span></span> <span data-ttu-id="06021-170">Här, behöver du bara tooprovide hello katalognamn utan hello blob namn.</span><span class="sxs-lookup"><span data-stu-id="06021-170">Here, you only need tooprovide hello directory name, without hello blob name.</span></span> <span data-ttu-id="06021-171">Ett fel returneras om du anger både katalog och blob namn, exempelvis `wasb:///queryoutputdir/queryoutput.txt`.</span><span class="sxs-lookup"><span data-stu-id="06021-171">An error is thrown if you provide both directory and blob names, such as `wasb:///queryoutputdir/queryoutput.txt`.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

<span data-ttu-id="06021-173">Om du öppnar hello standardbehållaren för hello Hadoop-kluster med Azure Lagringsutforskaren ser hello utdata från hello Hive-fråga som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="06021-173">If you open hello default container of hello Hadoop cluster using Azure Storage Explorer, you can see hello output of hello Hive query as shown in hello following figure.</span></span> <span data-ttu-id="06021-174">Du kan använda hello filter (visas med röd ruta) tooonly hämta hello blob med angivna bokstäverna i namn.</span><span class="sxs-lookup"><span data-stu-id="06021-174">You can apply hello filter (highlighted by red box) tooonly retrieve hello blob with specified letters in names.</span></span>

![Skapa arbetsyta](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

### <span data-ttu-id="06021-176"><a name="hive-editor"></a> 2. Skicka Hive-frågor med hello Hive-redigeraren</span><span class="sxs-lookup"><span data-stu-id="06021-176"><a name="hive-editor"></a> 2. Submit Hive queries with hello Hive Editor</span></span>
<span data-ttu-id="06021-177">Du kan också använda hello frågan konsolen (Hive-redigeraren) genom att ange en URL i formatet hello *https://&#60; Hadoop-klusternamn >.azurehdinsight.net/Home/HiveEditor* i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="06021-177">You can also use hello Query Console (Hive Editor) by entering a URL of hello form *https://&#60;Hadoop cluster name>.azurehdinsight.net/Home/HiveEditor* into a web browser.</span></span> <span data-ttu-id="06021-178">Du måste vara inloggad hello finns den här konsolen och du måste ha autentiseringsuppgifter här din Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-178">You must be logged in hello see this console and so you need your Hadoop cluster credentials here.</span></span>

### <span data-ttu-id="06021-179"><a name="ps"></a> 3. Skicka Hive-frågor med Azure PowerShell-kommandon</span><span class="sxs-lookup"><span data-stu-id="06021-179"><a name="ps"></a> 3. Submit Hive queries with Azure PowerShell Commands</span></span>
<span data-ttu-id="06021-180">Du kan också använda PowerShell toosubmit Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="06021-180">You can also use PowerShell toosubmit Hive queries.</span></span> <span data-ttu-id="06021-181">Instruktioner finns i [skicka Hive-jobb med hjälp av PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="06021-181">For instructions, see [Submit Hive jobs using PowerShell](../hdinsight/hdinsight-hadoop-use-hive-powershell.md).</span></span>

## <span data-ttu-id="06021-182"><a name="create-tables"></a>Skapa Hive-databasen och tabeller</span><span class="sxs-lookup"><span data-stu-id="06021-182"><a name="create-tables"></a>Create Hive database and tables</span></span>
<span data-ttu-id="06021-183">hello Hive-frågor delas i hello [GitHub-lagringsplatsen](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) och kan hämtas därifrån.</span><span class="sxs-lookup"><span data-stu-id="06021-183">hello Hive queries are shared in hello [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) and can be downloaded from there.</span></span>

<span data-ttu-id="06021-184">Här är hello Hive-fråga som skapar en Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="06021-184">Here is hello Hive query that creates a Hive table.</span></span>

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

<span data-ttu-id="06021-185">Här följer hello beskrivningar av hello fält som du behöver tooplug i och andra konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="06021-185">Here are hello descriptions of hello fields that you need tooplug in and other configurations:</span></span>

* <span data-ttu-id="06021-186">**&#60; databasnamn >**: hello namnet på hello-databasen som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="06021-186">**&#60;database name>**: hello name of hello database that you want toocreate.</span></span> <span data-ttu-id="06021-187">Om du bara vill toouse hello standarddatabasen hello frågan *Skapa databas...*  kan utelämnas.</span><span class="sxs-lookup"><span data-stu-id="06021-187">If you just want toouse hello default database, hello query *create database...* can be omitted.</span></span>
* <span data-ttu-id="06021-188">**&#60; tabellnamn >**: hello namnet på hello-tabell som du vill toocreate inom hello angivna databasen.</span><span class="sxs-lookup"><span data-stu-id="06021-188">**&#60;table name>**: hello name of hello table that you want toocreate within hello specified database.</span></span> <span data-ttu-id="06021-189">Om du vill toouse hello standarddatabasen hello tabellen kan refereras direkt av *&#60; tabellnamn >* utan &#60; databasnamn >.</span><span class="sxs-lookup"><span data-stu-id="06021-189">If you want toouse hello default database, hello table can be directly referred by *&#60;table name>* without &#60;database name>.</span></span>
* <span data-ttu-id="06021-190">**&#60; fältavgränsaren >**: hello avgränsare som avgränsar fälten i hello data filen toobe överförs toohello Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="06021-190">**&#60;field separator>**: hello separator that delimits fields in hello data file toobe uploaded toohello Hive table.</span></span>
* <span data-ttu-id="06021-191">**&#60; radbrytningstecken >**: hello avgränsare som avgränsar rader i datafilen hello.</span><span class="sxs-lookup"><span data-stu-id="06021-191">**&#60;line separator>**: hello separator that delimits lines in hello data file.</span></span>
* <span data-ttu-id="06021-192">**&#60; lagringsplats >**: hello Azure storage toosave hello lokaliseringsuppgifter för Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="06021-192">**&#60;storage location>**: hello Azure storage location toosave hello data of Hive tables.</span></span> <span data-ttu-id="06021-193">Om du inte anger *plats &#60; lagringsplats >*hello databasen och hello tabeller lagras i *hive/datalager/* katalogen i hello standardbehållaren hello Hive-klustret som standard.</span><span class="sxs-lookup"><span data-stu-id="06021-193">If you do not specify *LOCATION &#60;storage location>*, hello database and hello tables are stored in *hive/warehouse/* directory in hello default container of hello Hive cluster by default.</span></span> <span data-ttu-id="06021-194">Om du vill toospecify hello lagringsplats har hello lagringsplats toobe inom hello standardbehållaren för hello-databasen och tabeller.</span><span class="sxs-lookup"><span data-stu-id="06021-194">If you want toospecify hello storage location, hello storage location has toobe within hello default container for hello database and tables.</span></span> <span data-ttu-id="06021-195">Den här platsen har toobe kallas plats relativa toohello standardbehållaren hello klustret i hello-format för *' wasb: / / / &#60; katalogen 1 > / ”* eller *' wasb: / / / &#60; katalogen 1 > / &#60; katalogen 2 > / ”*osv. När hello-frågan körs skapas hello relativa kataloger inom hello standardbehållaren.</span><span class="sxs-lookup"><span data-stu-id="06021-195">This location has toobe referred as location relative toohello default container of hello cluster in hello format of *'wasb:///&#60;directory 1>/'* or *'wasb:///&#60;directory 1>/&#60;directory 2>/'*, etc. After hello query is executed, hello relative directories are created within hello default container.</span></span>
* <span data-ttu-id="06021-196">**TBLPROPERTIES("Skip.Header.Line.Count"="1")**: om hello datafilen har en rubrikrad, har du tooadd den här egenskapen **hello slutet** av hello *Skapa tabell* frågan.</span><span class="sxs-lookup"><span data-stu-id="06021-196">**TBLPROPERTIES("skip.header.line.count"="1")**: If hello data file has a header line, you have tooadd this property **at hello end** of hello *create table* query.</span></span> <span data-ttu-id="06021-197">Annars hello rubrikraden har lästs in som en tabell med poster toohello.</span><span class="sxs-lookup"><span data-stu-id="06021-197">Otherwise, hello header line is loaded as a record toohello table.</span></span> <span data-ttu-id="06021-198">Om hello datafilen saknar en huvudrad kan den här konfigurationen utelämnas i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="06021-198">If hello data file does not have a header line, this configuration can be omitted in hello query.</span></span>

## <span data-ttu-id="06021-199"><a name="load-data"></a>Läsa in data tooHive tabeller</span><span class="sxs-lookup"><span data-stu-id="06021-199"><a name="load-data"></a>Load data tooHive tables</span></span>
<span data-ttu-id="06021-200">Här är hello Hive-frågan som läser in data i en Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="06021-200">Here is hello Hive query that loads data into a Hive table.</span></span>

    LOAD DATA INPATH '<path tooblob data>' INTO TABLE <database name>.<table name>;

* <span data-ttu-id="06021-201">**&#60; sökvägen tooblob data >**: om hello blob filen har överförts toobe toohello Hive tabellen i hello standardbehållaren för hello HDInsight Hadoop-kluster hello *&#60; sökvägen tooblob data >* måste vara i formatet hello *' wasb: / / / &#60; katalogen i den här behållaren > / &#60; blob filnamn >'*.</span><span class="sxs-lookup"><span data-stu-id="06021-201">**&#60;path tooblob data>**: If hello blob file toobe uploaded toohello Hive table is in hello default container of hello HDInsight Hadoop cluster, hello *&#60;path tooblob data>* should be in hello format *'wasb:///&#60;directory in this container>/&#60;blob file name>'*.</span></span> <span data-ttu-id="06021-202">hello blob-fil kan också vara ytterligare en behållare för hello HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-202">hello blob file can also be in an additional container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="06021-203">I det här fallet *&#60; sökvägen tooblob data >* måste vara i formatet hello *' wasb: / / &#60; behållarens namn > @&#60; lagringskontonamnet >.blob.core.windows.net/ &#60; blob filnamn >'*.</span><span class="sxs-lookup"><span data-stu-id="06021-203">In this case, *&#60;path tooblob data>* should be in hello format *'wasb://&#60;container name>@&#60;storage account name>.blob.core.windows.net/&#60;blob file name>'*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="06021-204">hello har blob data överförs toobe tooHive tabellen toobe i hello standard eller ytterligare en behållare för hello lagringskonto för hello Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="06021-204">hello blob data toobe uploaded tooHive table has toobe in hello default or additional container of hello storage account for hello Hadoop cluster.</span></span> <span data-ttu-id="06021-205">Annars hello *Läs in DATA* frågan inte kunde köras klagande att det går inte att komma åt hello data.</span><span class="sxs-lookup"><span data-stu-id="06021-205">Otherwise, hello *LOAD DATA* query fails complaining that it cannot access hello data.</span></span>
  >
  >

## <span data-ttu-id="06021-206"><a name="partition-orc"></a>Avancerade alternativ: partitionerad tabell och lagra Hive-data i ORC-format</span><span class="sxs-lookup"><span data-stu-id="06021-206"><a name="partition-orc"></a>Advanced topics: partitioned table and store Hive data in ORC format</span></span>
<span data-ttu-id="06021-207">Om hello data är stor, är partitionering hello tabell bra för frågor som bara behöver tooscan några partitioner för hello tabell.</span><span class="sxs-lookup"><span data-stu-id="06021-207">If hello data is large, partitioning hello table is beneficial for queries that only need tooscan a few partitions of hello table.</span></span> <span data-ttu-id="06021-208">Det är exempelvis rimliga toopartition hello loggdata för en webbplats med datum.</span><span class="sxs-lookup"><span data-stu-id="06021-208">For instance, it is reasonable toopartition hello log data of a web site by dates.</span></span>

<span data-ttu-id="06021-209">I tillägg toopartitioning Hive tabeller, det är också bra toostore hello Hive data i hello optimerade raden kolumner (ORC)-format.</span><span class="sxs-lookup"><span data-stu-id="06021-209">In addition toopartitioning Hive tables, it is also beneficial toostore hello Hive data in hello Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="06021-210">Mer information om ORC formatering finns <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">med ORC-filer som förbättrar prestanda när Hive läsning, skrivning och bearbetning av data</a>.</span><span class="sxs-lookup"><span data-stu-id="06021-210">For more information on ORC formatting, see <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">Using ORC files improves performance when Hive is reading, writing, and processing data</a>.</span></span>

### <a name="partitioned-table"></a><span data-ttu-id="06021-211">Partitionerad tabell</span><span class="sxs-lookup"><span data-stu-id="06021-211">Partitioned table</span></span>
<span data-ttu-id="06021-212">Här är hello Hive-fråga som skapar en partitionerad tabell och läser in data i den.</span><span class="sxs-lookup"><span data-stu-id="06021-212">Here is hello Hive query that creates a partitioned table and loads data into it.</span></span>

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

<span data-ttu-id="06021-213">När du frågar partitionerade tabeller bör tooadd hello partition villkor i hello **början** av hello `where` satsen som detta förbättrar hello effekt söker avsevärt.</span><span class="sxs-lookup"><span data-stu-id="06021-213">When querying partitioned tables, it is recommended tooadd hello partition condition in hello **beginning** of hello `where` clause as this improves hello efficacy of searching significantly.</span></span>

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <span data-ttu-id="06021-214"><a name="orc"></a>Hive data lagras i ORC-format</span><span class="sxs-lookup"><span data-stu-id="06021-214"><a name="orc"></a>Store Hive data in ORC format</span></span>
<span data-ttu-id="06021-215">Du kan inte direkt läsa in data från blob storage i Hive-tabeller som är lagrad i hello ORC-format.</span><span class="sxs-lookup"><span data-stu-id="06021-215">You cannot directly load data from blob storage into Hive tables that is stored in hello ORC format.</span></span> <span data-ttu-id="06021-216">Här är hello stegen som hello som du behöver tootake tooload data från Azure BLOB-objekt tooHive tabeller som lagras i ORC-format.</span><span class="sxs-lookup"><span data-stu-id="06021-216">Here are hello steps that hello you need tootake tooload data from Azure blobs tooHive tables stored in ORC format.</span></span>

<span data-ttu-id="06021-217">Skapa en extern tabell **LAGRAS AS TEXTFILE** och läsa in data från blob storage toohello tabell.</span><span class="sxs-lookup"><span data-stu-id="06021-217">Create an external table **STORED AS TEXTFILE** and load data from blob storage toohello table.</span></span>

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path toohello source file>' INTO TABLE <database name>.<table name>;

<span data-ttu-id="06021-218">Skapa en intern tabell med hello samma schema som hello extern tabell i steg 1, med hello samma fältavgränsaren och lagra hello Hive-data i hello ORC format.</span><span class="sxs-lookup"><span data-stu-id="06021-218">Create an internal table with hello same schema as hello external table in step 1, with hello same field delimiter, and store hello Hive data in hello ORC format.</span></span>

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

<span data-ttu-id="06021-219">Välj data från hello extern tabell i steg 1 och infoga i hello ORC tabell</span><span class="sxs-lookup"><span data-stu-id="06021-219">Select data from hello external table in step 1 and insert into hello ORC table</span></span>

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

> [!NOTE]
> <span data-ttu-id="06021-220">Om hello TEXTFILE tabell *&#60; databasnamn >. &#60; externa textfile tabellnamn >* har partitioner i steg3 hello `SELECT * FROM <database name>.<external textfile table name>` kommandot väljer hello partition variabeln som ett fält i hello returneras datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="06021-220">If hello TEXTFILE table *&#60;database name>.&#60;external textfile table name>* has partitions, in STEP 3, hello `SELECT * FROM <database name>.<external textfile table name>` command selects hello partition variable as a field in hello returned data set.</span></span> <span data-ttu-id="06021-221">Infoga den i hello *&#60; databasnamn >. &#60; ORC tabellnamn >* misslyckas eftersom *&#60; databasnamn >. &#60; ORC tabellnamn >* hello partition variabel som inte har en fält i hello tabellens schema.</span><span class="sxs-lookup"><span data-stu-id="06021-221">Inserting it into hello *&#60;database name>.&#60;ORC table name>* fails since *&#60;database name>.&#60;ORC table name>* does not have hello partition variable as a field in hello table schema.</span></span> <span data-ttu-id="06021-222">I det här fallet behöver du toospecifically väljer hello fält toobe infogas för*&#60; databasnamn >. &#60; ORC-tabellnamn >* på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="06021-222">In this case, you need toospecifically select hello fields toobe inserted too*&#60;database name>.&#60;ORC table name>* as follows:</span></span>
>
>

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

<span data-ttu-id="06021-223">Det är säkert toodrop hello *&#60; externa textfile tabellnamn >* när med hello följande fråga efter alla data som har infogats i *&#60; databasnamn >. &#60; ORC-tabellnamn >*:</span><span class="sxs-lookup"><span data-stu-id="06021-223">It is safe toodrop hello *&#60;external textfile table name>* when using hello following query after all data has been inserted into *&#60;database name>.&#60;ORC table name>*:</span></span>

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

<span data-ttu-id="06021-224">När den här proceduren bör du ha en tabell med data i hello ORC format redo toouse.</span><span class="sxs-lookup"><span data-stu-id="06021-224">After following this procedure, you should have a table with data in hello ORC format ready toouse.</span></span>  
