---
title: "Använda Tez-Gränssnittet med Windows-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du använder Tez-UI för att felsöka Tez-jobb på Windows-baserade HDInsight HDInsight."
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
ms.openlocfilehash: 3889fa1c3523eb0330cbe3b7640fd8590a5ceadf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a><span data-ttu-id="f403b-103">Använd Tez-UI för att felsöka Tez-jobb på Windows-baserade HDInsight</span><span class="sxs-lookup"><span data-stu-id="f403b-103">Use the Tez UI to debug Tez Jobs on Windows-based HDInsight</span></span>
<span data-ttu-id="f403b-104">Tez UI är en webbsida som kan användas för att förstå och felsöka jobb som använder Tez som motorn för körning på Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f403b-104">The Tez UI is a web page that can be used to understand and debug jobs that use Tez as the execution engine on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="f403b-105">Tez UI kan du visualisera jobbet som ett diagram över anslutna objekt, detaljer om varje objekt, och hämta statistik och loggningsinformation.</span><span class="sxs-lookup"><span data-stu-id="f403b-105">The Tez UI allows you to visualize the job as a graph of connected items, drill into each item, and retrieve statistics and logging information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f403b-106">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Windows.</span><span class="sxs-lookup"><span data-stu-id="f403b-106">The steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="f403b-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="f403b-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f403b-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f403b-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f403b-109">Krav</span><span class="sxs-lookup"><span data-stu-id="f403b-109">Prerequisites</span></span>
* <span data-ttu-id="f403b-110">Ett Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f403b-110">A Windows-based HDInsight cluster.</span></span> <span data-ttu-id="f403b-111">Anvisningar om hur du skapar ett nytt kluster finns [komma igång med Windows-baserade HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span><span class="sxs-lookup"><span data-stu-id="f403b-111">For steps on creating a new cluster, see [Get started using Windows-based HDInsight](hdinsight-hadoop-tutorial-get-started-windows.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="f403b-112">Tez UI är bara tillgänglig på Windows-baserade HDInsight-kluster som skapas efter den 8 februari 2016.</span><span class="sxs-lookup"><span data-stu-id="f403b-112">The Tez UI is only available on Windows-based HDInsight clusters created after February 8th, 2016.</span></span>
  >
  >
* <span data-ttu-id="f403b-113">En Windows-baserade fjärrskrivbordsklienten.</span><span class="sxs-lookup"><span data-stu-id="f403b-113">A Windows-based Remote Desktop client.</span></span>

## <a name="understanding-tez"></a><span data-ttu-id="f403b-114">Förstå Tez</span><span class="sxs-lookup"><span data-stu-id="f403b-114">Understanding Tez</span></span>
<span data-ttu-id="f403b-115">Tez är ett utökningsbart ramverk för databearbetning i Hadoop och som ger högre hastigheter än traditionella MapReduce-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="f403b-115">Tez is an extensible framework for data processing in Hadoop that provides greater speeds than traditional MapReduce processing.</span></span> <span data-ttu-id="f403b-116">För Windows-baserade HDInsight-kluster är det en valfri motor som du kan aktivera för Hive med hjälp av följande kommando som en del av Hive-fråga:</span><span class="sxs-lookup"><span data-stu-id="f403b-116">For Windows-based HDInsight clusters, it is an optional engine that you can enable for Hive by using the following command as part of your Hive query:</span></span>

    set hive.execution.engine=tez;

<span data-ttu-id="f403b-117">När arbetet skickas till Tez skapar en dirigeras acykliska diagram (DAG) som beskriver ordningen för körningen av åtgärder som krävs av jobbet.</span><span class="sxs-lookup"><span data-stu-id="f403b-117">When work is submitted to Tez, it creates a Directed Acyclic Graph (DAG) that describes the order of execution of the actions required by the job.</span></span> <span data-ttu-id="f403b-118">Enskilda åtgärder kallas formhörnen och köra en del av en övergripande jobbet.</span><span class="sxs-lookup"><span data-stu-id="f403b-118">Individual actions are called vertices, and execute a piece of the overall job.</span></span> <span data-ttu-id="f403b-119">Faktiska körningen av det arbete som beskrivs av en nod kallas för en aktivitet och kan distribueras över flera noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="f403b-119">The actual execution of the work described by a vertex is called a task, and may be distributed across multiple nodes in the cluster.</span></span>

### <a name="understanding-the-tez-ui"></a><span data-ttu-id="f403b-120">Förstå Tez-Gränssnittet</span><span class="sxs-lookup"><span data-stu-id="f403b-120">Understanding the Tez UI</span></span>
<span data-ttu-id="f403b-121">Tez UI är en webbsida ger information om processer som körs eller har kördes tidigare med Tez.</span><span class="sxs-lookup"><span data-stu-id="f403b-121">The Tez UI is a web page provides information on processes that are running, or have previously ran using Tez.</span></span> <span data-ttu-id="f403b-122">Du kan visa den DAG som genererats av Tez, hur den distribueras till kluster, räknare, till exempel minne som används av uppgifter och formhörnen och information om felet.</span><span class="sxs-lookup"><span data-stu-id="f403b-122">It allows you to view the DAG generated by Tez, how it is distributed across clusters, counters such as memory used by tasks and vertices, and error information.</span></span> <span data-ttu-id="f403b-123">Den kan erbjuda användbar information i följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="f403b-123">It may offer useful information in the following scenarios:</span></span>

* <span data-ttu-id="f403b-124">Övervakning tidskrävande processer, visa förloppet för kartan och minska uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f403b-124">Monitoring long-running processes, viewing the progress of map and reduce tasks.</span></span>
* <span data-ttu-id="f403b-125">Analysera historiska data för lyckade eller misslyckade processer att lära dig hur bearbetning kan förbättras eller orsaken till felet.</span><span class="sxs-lookup"><span data-stu-id="f403b-125">Analyzing historical data for successful or failed processes to learn how processing could be improved or why it failed.</span></span>

## <a name="generate-a-dag"></a><span data-ttu-id="f403b-126">Generera en DAG</span><span class="sxs-lookup"><span data-stu-id="f403b-126">Generate a DAG</span></span>
<span data-ttu-id="f403b-127">Tez UI innehåller bara data om ett jobb som använder Tez-motorn körs eller har körts tidigare.</span><span class="sxs-lookup"><span data-stu-id="f403b-127">The Tez UI will only contain data if a job that uses the Tez engine is currently running, or has been ran in the past.</span></span> <span data-ttu-id="f403b-128">Enkel Hive-frågor kan vanligtvis lösas utan att använda Tez, men mer komplexa frågor som gör filtrering, gruppering, sortering, kopplingar, etc. kräver vanligtvis Tez.</span><span class="sxs-lookup"><span data-stu-id="f403b-128">Simple Hive queries can usually be resolved without using Tez, however more complex queries that do filtering, grouping, ordering, joins, etc. will usually require Tez.</span></span>

<span data-ttu-id="f403b-129">Använd följande steg för att köra en Hive-fråga som ska köras med hjälp av Tez.</span><span class="sxs-lookup"><span data-stu-id="f403b-129">Use the following steps to run a Hive query that will execute using Tez.</span></span>

1. <span data-ttu-id="f403b-130">I en webbläsare, navigerar du till https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f403b-130">In a web browser, navigate to https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="f403b-131">På menyn överst på sidan, Välj den **Hive-redigeraren**.</span><span class="sxs-lookup"><span data-stu-id="f403b-131">From the menu at the top of the page, select the **Hive Editor**.</span></span> <span data-ttu-id="f403b-132">En sida med följande exempelfråga visas.</span><span class="sxs-lookup"><span data-stu-id="f403b-132">This will display a page with the following example query.</span></span>

        Select * from hivesampletable

    <span data-ttu-id="f403b-133">Radera exempelfråga och Ersätt den med följande.</span><span class="sxs-lookup"><span data-stu-id="f403b-133">Erase the example query and replace it with the following.</span></span>

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. <span data-ttu-id="f403b-134">Välj den **skicka** knappen.</span><span class="sxs-lookup"><span data-stu-id="f403b-134">Select the **Submit** button.</span></span> <span data-ttu-id="f403b-135">Den **jobbet Session** avsnittet längst ned på sidan visar statusen för frågan.</span><span class="sxs-lookup"><span data-stu-id="f403b-135">The **Job Session** section at the bottom of the page will display the status of the query.</span></span> <span data-ttu-id="f403b-136">När statusen ändras till **slutförd**, Välj den **visa information** länken om du vill visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="f403b-136">Once the status changes to **Completed**, select the **View Details** link to view the results.</span></span> <span data-ttu-id="f403b-137">Den **Jobbutdata** bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="f403b-137">The **Job Output** should be similar to the following:</span></span>

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a><span data-ttu-id="f403b-138">Använda Tez-Gränssnittet</span><span class="sxs-lookup"><span data-stu-id="f403b-138">Use the Tez UI</span></span>
> [!NOTE]
> <span data-ttu-id="f403b-139">Tez UI är endast tillgängligt från skrivbordet head klusternoder, så du måste använda Fjärrskrivbord för att ansluta till huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="f403b-139">The Tez UI is only available from the desktop of the cluster head nodes, so you must use Remote Desktop to connect to the head nodes.</span></span>
>
>

1. <span data-ttu-id="f403b-140">Från den [Azure-portalen](https://portal.azure.com), Välj ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f403b-140">From the [Azure portal](https://portal.azure.com), select your HDInsight cluster.</span></span> <span data-ttu-id="f403b-141">Upp i bladet HDInsight, Välj den **fjärrskrivbord** ikon.</span><span class="sxs-lookup"><span data-stu-id="f403b-141">From the top of the HDInsight blade, select the **Remote Desktop** icon.</span></span> <span data-ttu-id="f403b-142">Detta visar bladet remote desktop</span><span class="sxs-lookup"><span data-stu-id="f403b-142">This will display the remote desktop blade</span></span>

    ![Ikon för Remote desktop](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. <span data-ttu-id="f403b-144">Remote Desktop-bladet välj **Anslut** att ansluta till klustrets huvudnod.</span><span class="sxs-lookup"><span data-stu-id="f403b-144">From the Remote Desktop blade, select **Connect** to connect to the cluster head node.</span></span> <span data-ttu-id="f403b-145">Vid uppmaning används klustret Remote Desktop-användarnamn och lösenord för att autentisera anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f403b-145">When prompted, use the cluster Remote Desktop user name and password to authenticate the connection.</span></span>

    ![Anslutningen till fjärrskrivbord ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > <span data-ttu-id="f403b-147">Om du inte har aktiverat Fjärrskrivbord-anslutningen, ange ett användarnamn, lösenord och upphör att gälla och välj sedan **aktivera** att aktivera Fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="f403b-147">If you have not enabled Remote Desktop connectivity, provide a user name, password, and expiration date, then select **Enable** to enable Remote Desktop.</span></span> <span data-ttu-id="f403b-148">När den har aktiverats, kan du använda de här stegen för att ansluta.</span><span class="sxs-lookup"><span data-stu-id="f403b-148">Once it has been enabled, use the previous steps to connect.</span></span>
   >
   >
3. <span data-ttu-id="f403b-149">När du är ansluten, öppna Internet Explorer på fjärrskrivbordet, väljer du kugghjulsikonen i övre högra hörnet i webbläsaren och välj sedan **Kompatibilitetsvyinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="f403b-149">Once connected, open Internet Explorer on the remote desktop, select the gear icon in the upper right of the browser, and then select **Compatibility View Settings**.</span></span>
4. <span data-ttu-id="f403b-150">Längst ned i **Kompatibilitetsvyinställningarna**, avmarkerar du kryssrutan för **visa intranätplatser i Kompatibilitetsvy** och **Använd Microsoft kompatibilitetslista**, och välj sedan **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="f403b-150">From the bottom of **Compatibility View Settings**, clear the check box for **Display intranet sites in Compatibility View** and **Use Microsoft compatibility lists**, and then select **Close**.</span></span>
5. <span data-ttu-id="f403b-151">I Internet Explorer, bläddra till #-http://headnodehost:8188/tezui /.</span><span class="sxs-lookup"><span data-stu-id="f403b-151">In Internet Explorer, browse to http://headnodehost:8188/tezui/#/.</span></span> <span data-ttu-id="f403b-152">Detta visar Tez UI</span><span class="sxs-lookup"><span data-stu-id="f403b-152">This will display the Tez UI</span></span>

    ![Tez-Gränssnittet](./media/hdinsight-debug-tez-ui/tezui.png)

    <span data-ttu-id="f403b-154">När Tez UI läses in, visas en lista över dag som för närvarande körs eller har körts på klustret.</span><span class="sxs-lookup"><span data-stu-id="f403b-154">When the Tez UI loads, you will see a list of DAGs that are currently running, or have been ran on the cluster.</span></span> <span data-ttu-id="f403b-155">Standardvyn innehåller Dag Name, -Id, runtimenamn, Skickat, Status, starttid, sluttid, varaktighet, program-ID och kön.</span><span class="sxs-lookup"><span data-stu-id="f403b-155">The default view includes the Dag Name, Id, Submitter, Status, Start Time, End Time, Duration, Application ID, and Queue.</span></span> <span data-ttu-id="f403b-156">Fler kolumner läggas till med hjälp av växeln-ikonen längst till höger på sidan.</span><span class="sxs-lookup"><span data-stu-id="f403b-156">More columns can be added using the gear icon at the right of the page.</span></span>

    <span data-ttu-id="f403b-157">Om du har en enda post blir det för den fråga som du körde i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f403b-157">If you have only one entry, it will be for the query that you ran in the previous section.</span></span> <span data-ttu-id="f403b-158">Om du har flera poster kan du söka genom att ange sökvillkor i fälten i dag och sedan klicka på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="f403b-158">If you have multiple entries, you can search by entering search criteria in the fields above the DAGs, then hit **Enter**.</span></span>
6. <span data-ttu-id="f403b-159">Välj den **Dag Name** för den senaste DAG-posten.</span><span class="sxs-lookup"><span data-stu-id="f403b-159">Select the **Dag Name** for the most recent DAG entry.</span></span> <span data-ttu-id="f403b-160">Information om DAG och alternativet för att hämta en zip JSON-filer som innehåller information om gruppen för Databastillgänglighet visas.</span><span class="sxs-lookup"><span data-stu-id="f403b-160">This will display information about the DAG, as well as the option to download a zip of JSON files that contain information about the DAG.</span></span>

    ![DAG-information](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. <span data-ttu-id="f403b-162">Ovanför den **DAG information** är flera länkar som kan användas för att visa information om DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-162">Above the **DAG Details** are several links that can be used to display information about the DAG.</span></span>

   * <span data-ttu-id="f403b-163">**DAG räknare** visar informationen från prestandaräknarna för denna DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-163">**DAG Counters** displays counters information for this DAG.</span></span>
   * <span data-ttu-id="f403b-164">**Grafisk vy** visar en grafisk representation av den här DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-164">**Graphical View** displays a graphical representation of this DAG.</span></span>
   * <span data-ttu-id="f403b-165">**Alla formhörnen** visar en lista över formhörnen i denna DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-165">**All Vertices** displays a list of the vertices in this DAG.</span></span>
   * <span data-ttu-id="f403b-166">**Alla aktiviteter** visar en lista över aktiviteter för alla hörn i denna DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-166">**All Tasks** displays a list of the tasks for all vertices in this DAG.</span></span>
   * <span data-ttu-id="f403b-167">**Alla TaskAttempts** visar information om försöker att köra uppgifter för denna DAG.</span><span class="sxs-lookup"><span data-stu-id="f403b-167">**All TaskAttempts** displays information about the attempts to run tasks for this DAG.</span></span>

     > [!NOTE]
     > <span data-ttu-id="f403b-168">Om du bläddrar i kolumnen visa för formhörnen, uppgifter och TaskAttempts, Observera att det finns länkar för att visa **räknare** och **visa eller hämta loggar** för varje rad.</span><span class="sxs-lookup"><span data-stu-id="f403b-168">If you scroll the column display for Vertices, Tasks and TaskAttempts, notice that there are links to view **counters** and **view or download logs** for each row.</span></span>
     >
     >

     <span data-ttu-id="f403b-169">Om det uppstod ett fel med jobbet, visas DAG information statusen misslyckades, tillsammans med länkar till information om om aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="f403b-169">If there was a failure with the job, the DAG Details will display a status of FAILED, along with links to information about the failed task.</span></span> <span data-ttu-id="f403b-170">Diagnostikinformationen visas under DAG-information.</span><span class="sxs-lookup"><span data-stu-id="f403b-170">Diagnostics information will be displayed beneath the DAG details.</span></span>
8. <span data-ttu-id="f403b-171">Välj **grafisk vy**.</span><span class="sxs-lookup"><span data-stu-id="f403b-171">Select **Graphical View**.</span></span> <span data-ttu-id="f403b-172">Visar en grafisk representation av gruppen för Databastillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="f403b-172">This displays a graphical representation of the DAG.</span></span> <span data-ttu-id="f403b-173">Du kan placera muspekaren över varje nod i vyn för att visa information om den.</span><span class="sxs-lookup"><span data-stu-id="f403b-173">You can place the mouse over each vertex in the view to display information about it.</span></span>

    ![Grafisk vy](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. <span data-ttu-id="f403b-175">Klicka på en nod att läsa in den **Vertex information** för objektet.</span><span class="sxs-lookup"><span data-stu-id="f403b-175">Clicking on a vertex will load the **Vertex Details** for that item.</span></span> <span data-ttu-id="f403b-176">Klicka på den **kartan 1** vertex att visa detaljer för det här objektet.</span><span class="sxs-lookup"><span data-stu-id="f403b-176">Click on the **Map 1** vertex to display details for this item.</span></span> <span data-ttu-id="f403b-177">Välj **Bekräfta** bekräfta navigeringen.</span><span class="sxs-lookup"><span data-stu-id="f403b-177">Select **Confirm** to confirm the navigation.</span></span>

    ![Vertex information](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. <span data-ttu-id="f403b-179">Observera att du nu har länkarna överst på sidan som är relaterade till formhörnen och uppgifter.</span><span class="sxs-lookup"><span data-stu-id="f403b-179">Note that you now have links at the top of the page that are related to vertices and tasks.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f403b-180">Du kan också komma fram till den här sidan genom att gå tillbaka till **DAG information**, välja **Vertex information**, och sedan välja den **kartan 1** hörn.</span><span class="sxs-lookup"><span data-stu-id="f403b-180">You can also arrive at this page by going back to **DAG Details**, selecting **Vertex Details**, and then selecting the **Map 1** vertex.</span></span>
    >
    >

    * <span data-ttu-id="f403b-181">**Vertex räknare** visar räknarinformation för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="f403b-181">**Vertex Counters** displays counter information for this vertex.</span></span>
    * <span data-ttu-id="f403b-182">**Uppgifter** uppgifter för den här vertex visas.</span><span class="sxs-lookup"><span data-stu-id="f403b-182">**Tasks** displays tasks for this vertex.</span></span>
    * <span data-ttu-id="f403b-183">**Uppgift försök** visar information om försök att köra uppgifter för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="f403b-183">**Task Attempts** displays information about attempts to run tasks for this vertex.</span></span>
    * <span data-ttu-id="f403b-184">**Datakällor & egenskaperna** visar datakällor och egenskaperna för den här hörn.</span><span class="sxs-lookup"><span data-stu-id="f403b-184">**Sources & Sinks** displays data sources and sinks for this vertex.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f403b-185">Med den föregående menyn rulla som Visa kolumnen för aktiviteter, aktivitet försök källor och Sinks__ visa länkar till mer information för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="f403b-185">As with the previous menu, you can scroll the column display for Tasks, Task Attempts, and Sources & Sinks__ to display links to more information for each item.</span></span>
      >
      >
11. <span data-ttu-id="f403b-186">Välj **uppgifter**, och välj sedan de objekt med namnet **00_000000**.</span><span class="sxs-lookup"><span data-stu-id="f403b-186">Select **Tasks**, and then select the item named **00_000000**.</span></span> <span data-ttu-id="f403b-187">Detta visar **aktivitetsinformation** för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="f403b-187">This will display **Task Details** for this task.</span></span> <span data-ttu-id="f403b-188">Du kan visa från den här skärmen **aktivitet räknare** och **aktivitet försök**.</span><span class="sxs-lookup"><span data-stu-id="f403b-188">From this screen, you can view **Task Counters** and **Task Attempts**.</span></span>

    ![Uppgiftsinformation](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a><span data-ttu-id="f403b-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f403b-190">Next Steps</span></span>
<span data-ttu-id="f403b-191">Nu när du har lärt dig hur du använder vyn Tez, lär du dig mer om [med hjälp av Hive i HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="f403b-191">Now that you have learned how to use the Tez view, learn more about [Using Hive on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="f403b-192">Mer teknisk information om Tez finns i [Tez sidan vid Hortonworks](http://hortonworks.com/hadoop/tez/).</span><span class="sxs-lookup"><span data-stu-id="f403b-192">For more detailed technical information on Tez, see the [Tez page at Hortonworks](http://hortonworks.com/hadoop/tez/).</span></span>
