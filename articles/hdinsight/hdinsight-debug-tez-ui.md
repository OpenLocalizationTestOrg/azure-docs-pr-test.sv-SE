---
title: aaaUse Tez UI med Windows-baserade HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toouse hello Tez UI toodebug Tez jobb på Windows-baserade HDInsight HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="7c27a-103">Använda hello Tez UI toodebug Tez jobb på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="7c27a-103">Use hello Tez UI toodebug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="7c27a-104">Hej Tez UI är en webbsida som kan använda toounderstand och felsöka jobb som använder Tez som motorn för körning av hello på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c27a-104">hello Tez UI is a web page that can be used toounderstand and debug jobs that use Tez as hello execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="7c27a-105">Hej Tez UI kan toovisualize hello jobb som ett diagram över anslutna objekt detaljer om varje objekt och hämta statistik och loggningsinformation.</span><span class="sxs-lookup"><span data-stu-id="7c27a-105">hello Tez UI allows you toovisualize hello job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c27a-106">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Windows.</span><span class="sxs-lookup"><span data-stu-id="7c27a-106">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="7c27a-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7c27a-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7c27a-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7c27a-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c27a-109">Krav</span><span class="sxs-lookup"><span data-stu-id="7c27a-109">Prerequisites</span></span>
* <span data-ttu-id="7c27a-110">Ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c27a-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="7c27a-111">Anvisningar om hur du skapar ett nytt kluster finns [komma igång med Windows-baserade HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="7c27a-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7c27a-112">Hej Tez UI är bara tillgängligt på Windows-baserade HDInsight-kluster som skapas efter den 8 februari 2016.</span><span class="sxs-lookup"><span data-stu-id="7c27a-112">hello Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="7c27a-113">En Windows-baserade fjärrskrivbordsklienten.</span><span class="sxs-lookup"><span data-stu-id="7c27a-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="7c27a-114">Förstå Tez</span><span class="sxs-lookup"><span data-stu-id="7c27a-114">Understanding Tez</span></span>
<span data-ttu-id="7c27a-115">Tez är ett utökningsbart ramverk för databearbetning i Hadoop och som ger högre hastigheter än traditionella MapReduce-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7c27a-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="7c27a-116">För Windows-baserade HDInsight-kluster är det en valfri motor som du kan aktivera för Hive med hjälp av följande kommando som en del av Hive-fråga hello:</span><span class="sxs-lookup"><span data-stu-id="7c27a-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using hello following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="7c27a-117">När arbetet är skickade tooTez, skapar en dirigeras acykliska diagram (DAG) som beskriver hello ordningen för körningen av hello-åtgärder som krävs av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="7c27a-117">When work is submitted tooTez, it creates a Directed Acyclic Graph (DAG) that describes hello order of execution of hello actions required by hello job.</span></span> <span data-ttu-id="7c27a-118">Enskilda åtgärder kallas formhörnen och köra en typ av hello övergripande jobb.</span><span class="sxs-lookup"><span data-stu-id="7c27a-118">Individual actions are called vertices, and execute a piece of hello overall job.</span></span> <span data-ttu-id="7c27a-119">hello faktiska utförande av hello beskrivs av en nod kallas för en aktivitet och kan distribueras över flera noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="7c27a-119">hello actual execution of hello work described by a vertex is called a task, and may be distributed across multiple nodes in hello cluster.</span></span>

### <a name="understanding-hello-tez-ui"></a><span data-ttu-id="7c27a-120">Förstå hello Tez UI</span><span class="sxs-lookup"><span data-stu-id="7c27a-120">Understanding hello Tez UI</span></span>
<span data-ttu-id="7c27a-121">Hej Tez UI är en webbsida ger information om processer som körs eller har kördes tidigare med Tez.</span><span class="sxs-lookup"><span data-stu-id="7c27a-121">hello Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="7c27a-122">Det gör att du tooview hello DAG som genererats av Tez, hur den distribueras till kluster, räknare, till exempel minne som används av uppgifter och formhörnen och information om felet.</span><span class="sxs-lookup"><span data-stu-id="7c27a-122">It allows you tooview hello DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="7c27a-123">Den kan erbjuda användbar information i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="7c27a-123">It may offer useful information in hello following scenarios:</span></span>

* <span data-ttu-id="7c27a-124">Övervaka tidskrävande processer, visa hello förloppet för kartan och minska uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7c27a-124">Monitoring long-running processes, viewing hello progress of map and reduce tasks.</span></span>
* <span data-ttu-id="7c27a-125">Analysera historiska data för lyckade eller misslyckade processer toolearn hur bearbetning kan förbättras eller orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="7c27a-125">Analyzing historical data for successful or failed processes toolearn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="7c27a-126">Generera en DAG</span><span class="sxs-lookup"><span data-stu-id="7c27a-126">Generate a DAG</span></span>
<span data-ttu-id="7c27a-127">Hej Tez UI innehåller bara data om ett jobb som använder hello Tez motorn körs eller har varit kördes i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="7c27a-127">hello Tez UI will only contain data if a job that uses hello Tez engine is currently running, or has been ran in hello past.</span></span> <span data-ttu-id="7c27a-128">Enkel Hive-frågor kan vanligtvis lösas utan att använda Tez, men mer komplexa frågor som gör filtrering, gruppering, sortering, kopplingar, etc. kräver vanligtvis Tez.</span><span class="sxs-lookup"><span data-stu-id="7c27a-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="7c27a-129">Använd följande steg toorun en Hive-fråga som ska köras med hjälp av Tez hello.</span><span class="sxs-lookup"><span data-stu-id="7c27a-129">Use hello following steps toorun a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="7c27a-130">I en webbläsare, navigerar du toohttps://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c27a-130">In a web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="7c27a-131">Välj hello hello menyn hello överst på sidan hello **Hive-redigeraren**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-131">From hello menu at hello top of hello page, select hello **Hive Editor**.</span></span> <span data-ttu-id="7c27a-132">Då visas en sida med hello följande exempelfråga.</span><span class="sxs-lookup"><span data-stu-id="7c27a-132">This will display a page with hello following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="7c27a-133">Radera hello exempelfråga och Ersätt den med hello följande.</span><span class="sxs-lookup"><span data-stu-id="7c27a-133">Erase hello example query and replace it with hello following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="7c27a-134">Välj hello **skicka** knappen.</span><span class="sxs-lookup"><span data-stu-id="7c27a-134">Select hello **Submit** button.</span></span> <span data-ttu-id="7c27a-135">Hej **jobbet Session** avsnittet längst ned hello hello sidan visar hello status för hello fråga.</span><span class="sxs-lookup"><span data-stu-id="7c27a-135">hello **Job Session** section at hello bottom of hello page will display hello status of hello query.</span></span> <span data-ttu-id="7c27a-136">En gång hello status ändras för**slutförd**väljer hello **visa information** länka tooview hello resultat.</span><span class="sxs-lookup"><span data-stu-id="7c27a-136">Once hello status changes too**Completed**, select hello **View Details** link tooview hello results.</span></span> <span data-ttu-id="7c27a-137">Hej **Jobbutdata** ska vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7c27a-137">hello **Job Output** should be similar toohello following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a><span data-ttu-id="7c27a-138">Använd hello Tez UI</span><span class="sxs-lookup"><span data-stu-id="7c27a-138">Use hello Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="7c27a-139">Hej Tez UI är endast tillgänglig från hello desktop head hello klusternoder, så du måste använda Fjärrskrivbord tooconnect toohello huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="7c27a-139">hello Tez UI is only available from hello desktop of hello cluster head nodes, so you must use Remote Desktop tooconnect toohello head nodes.</span></span>
>
>

1. <span data-ttu-id="7c27a-140">Från hello [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c27a-140">From hello [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="7c27a-141">Hello överkant hello HDInsight bladet välj hello **fjärrskrivbord** ikon.</span><span class="sxs-lookup"><span data-stu-id="7c27a-141">From hello top of hello HDInsight blade, select hello **Remote Desktop** icon.</span></span> <span data-ttu-id="7c27a-142">Detta visar hello remote desktop-bladet</span><span class="sxs-lookup"><span data-stu-id="7c27a-142">This will display hello remote desktop blade</span></span>

    ![Ikon för Remote desktop](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="7c27a-144">Hello fjärrskrivbord bladet välj **Anslut** tooconnect toohello klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="7c27a-144">From hello Remote Desktop blade, select **Connect** tooconnect toohello cluster head node.</span></span> <span data-ttu-id="7c27a-145">Vid uppmaning används hello klustret användarens namn och lösenord tooauthenticate hello fjärrskrivbordsanslutning.</span><span class="sxs-lookup"><span data-stu-id="7c27a-145">When prompted, use hello cluster Remote Desktop user name and password tooauthenticate hello connection.</span></span>

    ![Anslutningen till fjärrskrivbord ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="7c27a-147">Om du inte har aktiverat Fjärrskrivbord-anslutningen, ange ett användarnamn, lösenord och upphör att gälla och välj sedan **aktivera** tooenable fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7c27a-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** tooenable Remote Desktop.</span></span> <span data-ttu-id="7c27a-148">Använd hello föregående steg tooconnect när den har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="7c27a-148">Once it has been enabled, use hello previous steps tooconnect.</span></span>
   >
   >
3. <span data-ttu-id="7c27a-149">När du är ansluten, öppna Internet Explorer på hello fjärrskrivbordet väljer hello växeln ikonen i hello övre högra hörnet på hello webbläsaren och välj sedan **Kompatibilitetsvyinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-149">Once connected, open Internet Explorer on hello remote desktop, select hello gear icon in hello upper right of hello browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="7c27a-150">Hello längst ned i **Kompatibilitetsvyinställningarna**avmarkerar hello kryssrutan för **visa intranätplatser i Kompatibilitetsvy** och **Använd Microsoft kompatibilitetslista**, och välj sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-150">From hello bottom of **Compatibility View Settings**, clear hello check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="7c27a-151">Bläddra i Internet Explorer, toohttp://headnodehost:8188/tezui / #/.</span><span class="sxs-lookup"><span data-stu-id="7c27a-151">In Internet Explorer, browse toohttp://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="7c27a-152">Detta visar hello Tez UI</span><span class="sxs-lookup"><span data-stu-id="7c27a-152">This will display hello Tez UI</span></span>

    ![Tez-Gränssnittet](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="7c27a-154">När hello Tez UI läses in, visas en lista över dag som för närvarande körs eller har körts på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="7c27a-154">When hello Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on hello cluster.</span></span> <span data-ttu-id="7c27a-155">hello standardvyn innehåller hello Dag Name, -Id, runtimenamn, Skickat, Status, starttid, sluttid, varaktighet, program-ID och kön.</span><span class="sxs-lookup"><span data-stu-id="7c27a-155">hello default view includes hello Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="7c27a-156">Fler kolumner läggas med hello växeln-ikonen på hello höger i hello sidan.</span><span class="sxs-lookup"><span data-stu-id="7c27a-156">More columns can be added using hello gear icon at hello right of hello page.</span></span>

    <span data-ttu-id="7c27a-157">Om du har en enda post blir för hello-fråga som du körde hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7c27a-157">If you have only one entry, it will be for hello query that you ran in hello previous section.</span></span> <span data-ttu-id="7c27a-158">Om du har flera poster kan du söka genom att ange sökvillkor i hello fälten ovanför hello dag och sedan klicka på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-158">If you have multiple entries, you can search by entering search criteria in hello fields above hello DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="7c27a-159">Välj hello **Dag Name** för hello senaste DAG posten.</span><span class="sxs-lookup"><span data-stu-id="7c27a-159">Select hello **Dag Name** for hello most recent DAG entry.</span></span> <span data-ttu-id="7c27a-160">Information om hello DAG, samt hello alternativet toodownload en zip JSON-filer som innehåller information om hello DAG visas.</span><span class="sxs-lookup"><span data-stu-id="7c27a-160">This will display information about hello DAG, as well as hello option toodownload a zip of JSON files that contain information about hello DAG.</span></span>

    ![DAG-information](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="7c27a-162">Ovan hello **DAG information** är flera länkar som kan vara används toodisplay information om hello DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-162">Above hello **DAG Details** are several links that can be used toodisplay information about hello DAG.</span></span>

   * <span data-ttu-id="7c27a-163">**DAG räknare** visar informationen från prestandaräknarna för denna DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="7c27a-164">**Grafisk vy** visar en grafisk representation av den här DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="7c27a-165">**Alla formhörnen** visar en lista över hello formhörnen i denna DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-165">**All Vertices** displays a list of hello vertices in this DAG.</span></span>
   * <span data-ttu-id="7c27a-166">**Alla aktiviteter** visar en lista över hello aktiviteter för alla hörn i denna DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-166">**All Tasks** displays a list of hello tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="7c27a-167">**Alla TaskAttempts** visar information om hello försöker toorun uppgifter för denna DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-167">**All TaskAttempts** displays information about hello attempts toorun tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="7c27a-168">Om du bläddrar hello kolumnen visas för formhörnen, uppgifter och TaskAttempts Observera att det finns länkar tooview **räknare** och **visa eller hämta loggar** för varje rad.</span><span class="sxs-lookup"><span data-stu-id="7c27a-168">If you scroll hello column display for Vertices, Tasks and TaskAttempts, notice that there are links tooview **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="7c27a-169">Om det uppstod ett fel med hello jobb, visas hello DAG information statusen misslyckades, tillsammans med länkar tooinformation om hello om aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="7c27a-169">If there was a failure with hello job, hello DAG Details will display a status of FAILED, along with links tooinformation about hello failed task.</span></span> <span data-ttu-id="7c27a-170">Diagnostikinformationen visas under hello DAG information.</span><span class="sxs-lookup"><span data-stu-id="7c27a-170">Diagnostics information will be displayed beneath hello DAG details.</span></span>
8. <span data-ttu-id="7c27a-171">Välj **grafisk vy**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-171">Select **Graphical View**.</span></span> <span data-ttu-id="7c27a-172">Visar en grafisk representation av hello DAG.</span><span class="sxs-lookup"><span data-stu-id="7c27a-172">This displays a graphical representation of hello DAG.</span></span> <span data-ttu-id="7c27a-173">Du kan placera hello muspekaren över varje nod i hello visa toodisplay information om den.</span><span class="sxs-lookup"><span data-stu-id="7c27a-173">You can place hello mouse over each vertex in hello view toodisplay information about it.</span></span>

    ![Grafisk vy](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="7c27a-175">Klicka på en brytpunkt läses hello **Vertex information** för objektet.</span><span class="sxs-lookup"><span data-stu-id="7c27a-175">Clicking on a vertex will load hello **Vertex Details** for that item.</span></span> <span data-ttu-id="7c27a-176">Klicka på hello **kartan 1** vertex toodisplay information för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7c27a-176">Click on hello **Map 1** vertex toodisplay details for this item.</span></span> <span data-ttu-id="7c27a-177">Välj **Bekräfta** tooconfirm hello navigering.</span><span class="sxs-lookup"><span data-stu-id="7c27a-177">Select **Confirm** tooconfirm hello navigation.</span></span>

    ![Vertex information](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="7c27a-179">Observera att du nu har länkar hello överst på hello sidan som är relaterade toovertices och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="7c27a-179">Note that you now have links at hello top of hello page that are related toovertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7c27a-180">Du kan också komma fram till den här sidan genom att gå tillbaka för**DAG information**, välja **Vertex information**, och sedan välja hello **kartan 1** hörn.</span><span class="sxs-lookup"><span data-stu-id="7c27a-180">You can also arrive at this page by going back too**DAG Details**, selecting **Vertex Details**, and then selecting hello **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="7c27a-181">**Vertex räknare** visar räknarinformation för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="7c27a-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="7c27a-182">**Uppgifter** uppgifter för den här vertex visas.</span><span class="sxs-lookup"><span data-stu-id="7c27a-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="7c27a-183">**Uppgift försök** visar information om försök toorun uppgifter för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="7c27a-183">**Task Attempts** displays information about attempts toorun tasks for this vertex.</span></span>
    * <span data-ttu-id="7c27a-184">**Datakällor & egenskaperna** visar datakällor och egenskaperna för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="7c27a-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="7c27a-185">Som med tidigare hello-menyn kan du bläddrar hello kolumnen visas för aktiviteter, länkar aktivitet försök och källor & Sinks__ toodisplay toomore information för varje artikel.</span><span class="sxs-lookup"><span data-stu-id="7c27a-185">As with hello previous menu, you can scroll hello column display for Tasks, Task Attempts, and Sources & Sinks__ toodisplay links toomore information for each item.</span></span>
      >
      >
11. <span data-ttu-id="7c27a-186">Välj **uppgifter**, och sedan väljer hello-objekt med namnet **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-186">Select **Tasks**, and then select hello item named **00_000000**.</span></span> <span data-ttu-id="7c27a-187">Detta visar **aktivitetsinformation** för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="7c27a-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="7c27a-188">Du kan visa från den här skärmen **aktivitet räknare** och **aktivitet försök**.</span><span class="sxs-lookup"><span data-stu-id="7c27a-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Uppgiftsinformation](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="7c27a-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c27a-190">Next Steps</span></span>
<span data-ttu-id="7c27a-191">Nu när du har lärt dig hur toouse hello Tez visa, lär du dig mer om [med hjälp av Hive i HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="7c27a-191">Now that you have learned how toouse hello Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="7c27a-192">Mer teknisk information om Tez finns hello [Tez sidan vid Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="7c27a-192">For more detailed technical information on Tez, see hello [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
