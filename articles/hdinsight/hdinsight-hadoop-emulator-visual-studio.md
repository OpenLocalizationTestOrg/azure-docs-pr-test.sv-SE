---
title: "aaaData Lake-verktyg för Visual Studio med Hortonworks Sandbox - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Data Lake-verktyg för Visual Studio med hello Hortonworks sandbox körs på en lokal virtuell dator. Med dessa verktyg kan du skapa och köra Hive och Pig-jobb på hello sandbox och visa jobbutdata och historik."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e3434c45-95d1-4b96-ad4c-fb59870e2ff0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/11/2017
ms.author: larryfr
ms.openlocfilehash: 7a3406b28d87d953e9b5be387faa86268f47b935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-data-lake-tools-for-visual-studio-with-hello-hortonworks-sandbox"></a><span data-ttu-id="48533-104">Använda hello Azure Data Lake-verktyg för Visual Studio med hello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="48533-104">Use hello Azure Data Lake tools for Visual Studio with hello Hortonworks Sandbox</span></span>

<span data-ttu-id="48533-105">Azure Data Lake innehåller verktyg för att arbeta med allmänt Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="48533-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="48533-106">Det här dokumentet innehåller hello steg behövs toouse hello Data Lake-verktyg med hello Hortonworks Sandbox körs i en lokal virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="48533-106">This document provides hello steps needed toouse hello Data Lake tools with hello Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="48533-107">Om du använder hello Hortonworks Sandbox kan du toowork med Hadoop lokalt på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="48533-107">Using hello Hortonworks Sandbox allows you toowork with Hadoop locally on your development environment.</span></span> <span data-ttu-id="48533-108">När du har utvecklat en lösning och vill toodeploy den i skala, kan du flytta tooan HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="48533-108">After you have developed a solution and want toodeploy it at scale, you can then move tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48533-109">Krav</span><span class="sxs-lookup"><span data-stu-id="48533-109">Prerequisites</span></span>

* <span data-ttu-id="48533-110">Hej Hortonworks Sandbox, körs i en virtuell dator på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="48533-110">hello Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="48533-111">Det här dokumentet har skrivits och testas med hello sandbox körs i Oracle VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="48533-111">This document was written and tested with hello sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="48533-112">Information om hur du konfigurerar hello sandbox finns hello [Kom igång med hello Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="48533-112">For information on setting up hello sandbox, see hello [Get started with hello Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="48533-113">dokumentet.</span><span class="sxs-lookup"><span data-stu-id="48533-113">document.</span></span>

* <span data-ttu-id="48533-114">Visual Studio 2013, Visual Studio 2015 eller Visual Studio 2017 (någon utgåva).</span><span class="sxs-lookup"><span data-stu-id="48533-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="48533-115">Hej [Azure SDK för .NET](https://azure.microsoft.com/downloads/) 2.7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="48533-115">hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="48533-116">Hej [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="48533-116">hello [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-hello-sandbox"></a><span data-ttu-id="48533-117">Konfigurera lösenord för hello begränsat läge</span><span class="sxs-lookup"><span data-stu-id="48533-117">Configure passwords for hello sandbox</span></span>

<span data-ttu-id="48533-118">Se till att hello Hortonworks Sandbox körs.</span><span class="sxs-lookup"><span data-stu-id="48533-118">Make sure that hello Hortonworks Sandbox is running.</span></span> <span data-ttu-id="48533-119">Följ stegen hello i hello [komma igång i hello Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="48533-119">Then follow hello steps in hello [Get started in hello Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="48533-120">De här stegen konfigurerar hello lösenordet för hello SSH `root` konto och hello Ambari `admin` konto.</span><span class="sxs-lookup"><span data-stu-id="48533-120">These steps configure hello password for hello SSH `root` account, and hello Ambari `admin` account.</span></span> <span data-ttu-id="48533-121">Dessa lösenord används när du ansluter toohello sandbox från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48533-121">These passwords are used when you connect toohello sandbox from Visual Studio.</span></span>

## <a name="connect-hello-tools-toohello-sandbox"></a><span data-ttu-id="48533-122">Ansluta hello verktyg toohello sandbox</span><span class="sxs-lookup"><span data-stu-id="48533-122">Connect hello tools toohello sandbox</span></span>

1. <span data-ttu-id="48533-123">Öppna Visual Studio, markera **visa**, och välj sedan **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="48533-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="48533-124">Från **Server Explorer**, högerklicka på hello **HDInsight** posten och välj sedan **ansluta tooHDInsight emulatorn**.</span><span class="sxs-lookup"><span data-stu-id="48533-124">From **Server Explorer**, right-click hello **HDInsight** entry, and then select **Connect tooHDInsight Emulator**.</span></span>

    ![Skärmbild av Server Explorer, med Connect tooHDInsight emulatorn markerat](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="48533-126">Från hello **ansluta tooHDInsight emulatorn** dialogrutan Ange hello-lösenord som du konfigurerade för Ambari.</span><span class="sxs-lookup"><span data-stu-id="48533-126">From hello **Connect tooHDInsight Emulator** dialog box, enter hello password that you configured for Ambari.</span></span>

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="48533-128">Välj **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="48533-128">Select **Next** toocontinue.</span></span>

4. <span data-ttu-id="48533-129">Använd hello **lösenord** fältet tooenter hello lösenordet som du konfigurerade för hello `root` konto.</span><span class="sxs-lookup"><span data-stu-id="48533-129">Use hello **Password** field tooenter hello password you configured for hello `root` account.</span></span> <span data-ttu-id="48533-130">Lämna hello andra fälten i hello standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="48533-130">Leave hello other fields at hello default value.</span></span>

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="48533-132">Välj **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="48533-132">Select **Next** toocontinue.</span></span>

5. <span data-ttu-id="48533-133">Vänta på verifiering av hello services toofinish.</span><span class="sxs-lookup"><span data-stu-id="48533-133">Wait for validation of hello services toofinish.</span></span> <span data-ttu-id="48533-134">I vissa fall verifieringen misslyckas och du uppmanas tooupdate hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="48533-134">In some cases, validation may fail and prompt you tooupdate hello configuration.</span></span> <span data-ttu-id="48533-135">Om valideringen misslyckas, Välj **uppdatering**, och vänta tills hello konfiguration och verifiering för hello tjänsten toofinish.</span><span class="sxs-lookup"><span data-stu-id="48533-135">If validation fails, select **Update**, and wait for hello configuration and verification for hello service toofinish.</span></span>

    ![Skärmbild av dialogrutan med knappen Uppdatera markerat](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="48533-137">hello uppdateringsprocessen använder Ambari toomodify hello Hortonworks Sandbox configuration toowhat förväntas av hello Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="48533-137">hello update process uses Ambari toomodify hello Hortonworks Sandbox configuration toowhat is expected by hello Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="48533-138">När verifieringen är klar väljer du **Slutför** toocomplete konfiguration.</span><span class="sxs-lookup"><span data-stu-id="48533-138">After validation has finished, select **Finish** toocomplete configuration.</span></span>
    <span data-ttu-id="48533-139">![Skärmbild av dialogrutan med Slutför markerat](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="48533-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="48533-140">Beroende hello hastighet utvecklingsmiljö och hello mängden minne som allokerats toohello virtuell dator, kan det ta flera minuter tooconfigure och verifiera hello tjänster.</span><span class="sxs-lookup"><span data-stu-id="48533-140">Depending on hello speed of your development environment, and hello amount of memory allocated toohello virtual machine, it can take several minutes tooconfigure and validate hello services.</span></span>

<span data-ttu-id="48533-141">När du har följt de här stegen, nu har du en **lokala HDInsight-kluster** posten i Server Explorer under hello **HDInsight** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="48533-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under hello **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="48533-142">Skriv en Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="48533-142">Write a Hive query</span></span>

<span data-ttu-id="48533-143">Strukturen innehåller ett SQL-liknande frågespråk (HiveQL) för att arbeta med strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="48533-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="48533-144">Använd hello följande steg toolearn hur toorun på begäran frågor mot hello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="48533-144">Use hello following steps toolearn how toorun on-demand queries against hello local cluster.</span></span>

1. <span data-ttu-id="48533-145">I **Server Explorer**Högerklicka hello posten för lokala hello-klustret som du lagt till tidigare och välj sedan **skriva en Hive-fråga**.</span><span class="sxs-lookup"><span data-stu-id="48533-145">In **Server Explorer**, right-click hello entry for hello local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Skärmbild av Server Explorer med Skriv en Hive-fråga som markerats](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="48533-147">Ett nytt frågefönster visas.</span><span class="sxs-lookup"><span data-stu-id="48533-147">A new query window appears.</span></span> <span data-ttu-id="48533-148">Här kan du snabbt skriva och skicka en fråga toohello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="48533-148">Here you can quickly write and submit a query toohello local cluster.</span></span>

2. <span data-ttu-id="48533-149">Ange följande kommando hello i hello nytt frågefönster:</span><span class="sxs-lookup"><span data-stu-id="48533-149">In hello new query window, enter hello following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="48533-150">toorun hello fråga, Välj **skicka** hello överst i fönstret hello.</span><span class="sxs-lookup"><span data-stu-id="48533-150">toorun hello query, select **Submit** at hello top of hello window.</span></span> <span data-ttu-id="48533-151">Lämna hello andra värden (**Batch** och namn på servern) till hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="48533-151">Leave hello other values (**Batch** and server name) at hello default values.</span></span>

    ![Skärmbild av frågefönstret med hello Skicka-knapp markerat](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="48533-153">Du kan också använda hello listrutan bredvid för**skicka** tooselect **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="48533-153">You can also use hello drop-down menu next too**Submit** tooselect **Advanced**.</span></span> <span data-ttu-id="48533-154">Avancerade alternativ kan du tooprovide ytterligare alternativ när du skickar hello jobb.</span><span class="sxs-lookup"><span data-stu-id="48533-154">Advanced options allow you tooprovide additional options when you submit hello job.</span></span>

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="48533-156">När du har skickat hello frågan visas hello jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="48533-156">After you submit hello query, hello job status appears.</span></span> <span data-ttu-id="48533-157">hello jobbstatus visar information om hello jobb som bearbetas av Hadoop.</span><span class="sxs-lookup"><span data-stu-id="48533-157">hello job status displays information about hello job as it is processed by Hadoop.</span></span> <span data-ttu-id="48533-158">**Jobbets status** ger hello hello jobbets status.</span><span class="sxs-lookup"><span data-stu-id="48533-158">**Job State** provides hello status of hello job.</span></span> <span data-ttu-id="48533-159">hello tillstånd uppdateras regelbundet eller du kan använda hello uppdatera ikonen toorefresh hello tillstånd manuellt.</span><span class="sxs-lookup"><span data-stu-id="48533-159">hello state is updated periodically, or you can use hello refresh icon toorefresh hello state manually.</span></span>

    ![Skärmbild av jobbet dialogrutan vy med jobbstatus markerat](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="48533-161">Efter hello **jobbstatus** ändras för**avslutad**, dirigeras acykliska diagram (DAG) visas.</span><span class="sxs-lookup"><span data-stu-id="48533-161">After hello **Job State** changes too**Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="48533-162">Det här diagrammet beskrivs hello körning av sökväg som fastställts av Tez vid bearbetning av hello Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="48533-162">This diagram describes hello execution path that was determined by Tez when processing hello Hive query.</span></span> <span data-ttu-id="48533-163">Tez är hello standard Körningsmotor för Hive i hello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="48533-163">Tez is hello default execution engine for Hive on hello local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="48533-164">Tez är också hello standard när du använder Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="48533-164">Tez is also hello default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="48533-165">Det är inte hello standard på Windows-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="48533-165">It is not hello default on Windows-based HDInsight.</span></span> <span data-ttu-id="48533-166">toouse den där, måste du lägga till hello rad `set hive.execution.engine = tez;` toohello början av Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="48533-166">toouse it there, you must add hello line `set hive.execution.engine = tez;` toohello beginning of your Hive query.</span></span>

    <span data-ttu-id="48533-167">Använd hello **Jobbutdata** länka tooview hello utdata.</span><span class="sxs-lookup"><span data-stu-id="48533-167">Use hello **Job Output** link tooview hello output.</span></span> <span data-ttu-id="48533-168">I det här fallet är det 823 hello antalet rader i hello sample_08 tabell.</span><span class="sxs-lookup"><span data-stu-id="48533-168">In this case, it is 823, hello number of rows in hello sample_08 table.</span></span> <span data-ttu-id="48533-169">Du kan visa diagnostikinformation om hello jobb med hjälp av hello **Jobblogg** och **hämta YARN-logg** länkar.</span><span class="sxs-lookup"><span data-stu-id="48533-169">You can view diagnostics information about hello job by using hello **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="48533-170">Du kan också köra Hive-jobb interaktivt genom att ändra hello **Batch** fältet för**interaktiv**.</span><span class="sxs-lookup"><span data-stu-id="48533-170">You can also run Hive jobs interactively by changing hello **Batch** field too**Interactive**.</span></span> <span data-ttu-id="48533-171">Välj sedan **kör**.</span><span class="sxs-lookup"><span data-stu-id="48533-171">Then select **Execute**.</span></span>

    ![Skärmbild av interaktiva och kör knappar markerat](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="48533-173">En interaktiv fråga dataströmmar hello utdataloggen genereras under bearbetningen toohello **HiveServer2 utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="48533-173">An interactive query streams hello output log generated during processing toohello **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="48533-174">hello information är hello samma som är tillgänglig från hello **Jobblogg** länka när ett jobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="48533-174">hello information is hello same that is available from hello **Job Log** link after a job has finished.</span></span>

    ![Skärmbild av logg för utdata](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="48533-176">Skapa ett Hive-projekt</span><span class="sxs-lookup"><span data-stu-id="48533-176">Create a Hive project</span></span>

<span data-ttu-id="48533-177">Du kan också skapa ett projekt som innehåller flera Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="48533-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="48533-178">Använd ett projekt när du har relaterade skript eller toostore skript i ett system för version.</span><span class="sxs-lookup"><span data-stu-id="48533-178">Use a project when you have related scripts or want toostore scripts in a version control system.</span></span>

1. <span data-ttu-id="48533-179">I Visual Studio väljer **filen**, **ny**, och sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="48533-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="48533-180">Hello listan över projekt och expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **HIVE (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="48533-180">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="48533-181">Välj hello listan över mallar **Hive exempel**.</span><span class="sxs-lookup"><span data-stu-id="48533-181">From hello list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="48533-182">Ange ett namn och plats och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="48533-182">Enter a name and location, and then select **OK**.</span></span>

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, HIVE, Hive exempel och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="48533-184">Hej **Hive exempel** projektet innehåller två skript **WebLogAnalysis.hql** och **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="48533-184">hello **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="48533-185">Du kan skicka dessa skript med hjälp av hello samma **skicka** knappen hello överst i fönstret hello.</span><span class="sxs-lookup"><span data-stu-id="48533-185">You can submit these scripts by using hello same **Submit** button at hello top of hello window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="48533-186">Skapa ett Pig-projekt</span><span class="sxs-lookup"><span data-stu-id="48533-186">Create a Pig project</span></span>

<span data-ttu-id="48533-187">Medan Hive innehåller en SQL-liknande språk för att arbeta med strukturerade data, fungerar Pig genom att utföra omvandlingar på data.</span><span class="sxs-lookup"><span data-stu-id="48533-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="48533-188">Pig innehåller ett språk (Pig Latin) som gör att du toodevelop en pipeline omformningar.</span><span class="sxs-lookup"><span data-stu-id="48533-188">Pig provides a language (Pig Latin) that allows you toodevelop a pipeline of transformations.</span></span> <span data-ttu-id="48533-189">toouse Pig med hello lokala klustret och gör följande:</span><span class="sxs-lookup"><span data-stu-id="48533-189">toouse Pig with hello local cluster, follow these steps:</span></span>

1. <span data-ttu-id="48533-190">Öppna Visual Studio och välj **filen**, **ny**, och sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="48533-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="48533-191">Hello listan över projekt och expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="48533-191">From hello list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="48533-192">Välj hello listan över mallar **Pig programmet**.</span><span class="sxs-lookup"><span data-stu-id="48533-192">From hello list of templates, select **Pig Application**.</span></span> <span data-ttu-id="48533-193">Ange ett namn, plats, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="48533-193">Enter a name, location, and then select **OK**.</span></span>

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, Pig, Pig program och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="48533-195">Ange följande text som hello hello hello **script.pig** filen som skapades med det här projektet.</span><span class="sxs-lookup"><span data-stu-id="48533-195">Enter hello following text as hello contents of hello **script.pig** file that was created with this project.</span></span>

        a = LOAD '/demo/data/Website/Website-Logs' AS (
            log_id:int,
            ip_address:chararray,
            date:chararray,
            time:chararray,
            landing_page:chararray,
            source:chararray);
        b = FILTER a BY (log_id > 100);
        c = GROUP b BY ip_address;
        DUMP c;

    <span data-ttu-id="48533-196">Pig använder ett annat språk än Hive, hur du kör hello-jobben är konsekventa mellan både språk via hello **skicka** knappen.</span><span class="sxs-lookup"><span data-stu-id="48533-196">While Pig uses a different language than Hive, how you run hello jobs is consistent between both languages, through hello **Submit** button.</span></span> <span data-ttu-id="48533-197">Du väljer hello listrutan bredvid **skicka** visar en dialogruta med avancerade skicka för Pig.</span><span class="sxs-lookup"><span data-stu-id="48533-197">Selecting hello drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="48533-199">Hej jobbstatus och utdata visas också, hello samma som en Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="48533-199">hello job status and output is also displayed, hello same as a Hive query.</span></span>

    ![Skärmbild av ett avslutat Pig-projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="48533-201">Visa jobb</span><span class="sxs-lookup"><span data-stu-id="48533-201">View jobs</span></span>

<span data-ttu-id="48533-202">Data Lake-verktyg kan du också tooeasily visa information om jobb som har körts på Hadoop.</span><span class="sxs-lookup"><span data-stu-id="48533-202">Data Lake tools also allow you tooeasily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="48533-203">Använd hello följande steg toosee hello jobb som har körts på hello lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="48533-203">Use hello following steps toosee hello jobs that have been run on hello local cluster.</span></span>

1. <span data-ttu-id="48533-204">Från **Server Explorer**, högerklicka på hello lokala klustret och väljer sedan **visa jobb**.</span><span class="sxs-lookup"><span data-stu-id="48533-204">From **Server Explorer**, right-click hello local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="48533-205">En lista över jobb som har skickats toohello kluster visas.</span><span class="sxs-lookup"><span data-stu-id="48533-205">A list of jobs that have been submitted toohello cluster is displayed.</span></span>

    ![Skärmbild av Server Explorer, med Visa jobb markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="48533-207">Välj en tooview hello jobbinformation hello listan över jobb.</span><span class="sxs-lookup"><span data-stu-id="48533-207">From hello list of jobs, select one tooview hello job details.</span></span>

    ![Skärmbild av jobbet webbläsare med en hello jobb markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="48533-209">hello information som visas är liknande toowhat som visas när du har kört en Hive- eller Pig fråga, inklusive länkar tooview hello utdata och loggar information.</span><span class="sxs-lookup"><span data-stu-id="48533-209">hello information displayed is similar toowhat you see after running a Hive or Pig query, including links tooview hello output and log information.</span></span>

3. <span data-ttu-id="48533-210">Du kan också ändra och skicka jobbet hello härifrån.</span><span class="sxs-lookup"><span data-stu-id="48533-210">You can also modify and resubmit hello job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="48533-211">Visa Hive-databaser</span><span class="sxs-lookup"><span data-stu-id="48533-211">View Hive databases</span></span>

1. <span data-ttu-id="48533-212">I **Server Explorer**, expandera hello **lokala HDInsight-kluster** post, och expandera sedan **Hive-databaser**.</span><span class="sxs-lookup"><span data-stu-id="48533-212">In **Server Explorer**, expand hello **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="48533-213">Hej **standard** och **xademo** databaser på hello lokala klustret visas.</span><span class="sxs-lookup"><span data-stu-id="48533-213">hello **Default** and **xademo** databases on hello local cluster are displayed.</span></span> <span data-ttu-id="48533-214">Expandera en databas visar hello tabeller hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="48533-214">Expanding a database shows hello tables within hello database.</span></span>

    ![Skärmbild av Server Explorer, med databaser expanderas](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="48533-216">Expandera en tabell visar hello kolumner för tabellen.</span><span class="sxs-lookup"><span data-stu-id="48533-216">Expanding a table displays hello columns for that table.</span></span> <span data-ttu-id="48533-217">tooquickly visa hello data, högerklicka på en tabell och välj **visa de 100 översta raderna**.</span><span class="sxs-lookup"><span data-stu-id="48533-217">tooquickly view hello data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Skärmbild av Server Explorer, med tabellen expanderas och visa de 100 översta raderna markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="48533-219">Egenskaper för databas och tabell</span><span class="sxs-lookup"><span data-stu-id="48533-219">Database and table properties</span></span>

<span data-ttu-id="48533-220">Du kan visa hello egenskaperna för en databas eller tabell.</span><span class="sxs-lookup"><span data-stu-id="48533-220">You can view hello properties of a database or table.</span></span> <span data-ttu-id="48533-221">Att välja **egenskaper** visar information för hello markerade objekt i egenskapsfönstret för hello.</span><span class="sxs-lookup"><span data-stu-id="48533-221">Selecting **Properties** displays details for hello selected item in hello properties window.</span></span> <span data-ttu-id="48533-222">Se exempelvis hello informationen som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="48533-222">For example, see hello information shown in hello following screenshot:</span></span>

![Skärmbild av egenskapsfönstret](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="48533-224">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="48533-224">Create a table</span></span>

<span data-ttu-id="48533-225">toocreate en tabell, högerklicka på en databas och välj sedan **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="48533-225">toocreate a table, right-click a database, and then select **Create Table**.</span></span>

![Skärmbild av Server Explorer, med Create Table markerat](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="48533-227">Du kan sedan skapa hello tabellen med hjälp av ett formulär.</span><span class="sxs-lookup"><span data-stu-id="48533-227">You can then create hello table using a form.</span></span> <span data-ttu-id="48533-228">Längst ned hello i följande skärmbild hello, kan du se hello rådata HiveQL som är används toocreate hello tabell.</span><span class="sxs-lookup"><span data-stu-id="48533-228">At hello bottom of hello following screenshot, you can see hello raw HiveQL that is used toocreate hello table.</span></span>

![Skärmbild av hello formuläret används toocreate en tabell](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="48533-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48533-230">Next steps</span></span>

* [<span data-ttu-id="48533-231">Learning hello linor av hello Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="48533-231">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="48533-232">Hadoop-vägledning - komma igång med HDP</span><span class="sxs-lookup"><span data-stu-id="48533-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
