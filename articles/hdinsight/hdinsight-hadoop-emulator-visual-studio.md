---
title: "Data Lake-verktyg för Visual Studio med Hortonworks Sandbox - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Azure Data Lake-verktyg för Visual Studio med Hortonworks sandbox körs på en lokal virtuell dator. Med dessa verktyg kan du skapa och köra Hive och Pig-jobb på begränsat, och visa jobbutdata och historik."
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
ms.openlocfilehash: 574ccaa8b2d9448a60ddf8adc7f92fa3683b1d61
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-the-azure-data-lake-tools-for-visual-studio-with-the-hortonworks-sandbox"></a><span data-ttu-id="1681d-104">Använda Azure Data Lake-verktyg för Visual Studio med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="1681d-104">Use the Azure Data Lake tools for Visual Studio with the Hortonworks Sandbox</span></span>

<span data-ttu-id="1681d-105">Azure Data Lake innehåller verktyg för att arbeta med allmänt Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="1681d-105">Azure Data Lake includes tools for working with generic Hadoop clusters.</span></span> <span data-ttu-id="1681d-106">Det här dokumentet innehåller de steg som behövs för att använda Data Lake-verktyg med Hortonworks Sandbox körs i en lokal virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1681d-106">This document provides the steps needed to use the Data Lake tools with the Hortonworks Sandbox running in a local virtual machine.</span></span>

<span data-ttu-id="1681d-107">Med Hortonworks Sandbox kan du arbeta med Hadoop lokalt på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1681d-107">Using the Hortonworks Sandbox allows you to work with Hadoop locally on your development environment.</span></span> <span data-ttu-id="1681d-108">När du har utvecklat en lösning och vill distribuera i skala, kan du sedan flytta till ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1681d-108">After you have developed a solution and want to deploy it at scale, you can then move to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1681d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="1681d-109">Prerequisites</span></span>

* <span data-ttu-id="1681d-110">Hortonworks Sandbox körs i en virtuell dator på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1681d-110">The Hortonworks Sandbox, running in a virtual machine on your development environment.</span></span> <span data-ttu-id="1681d-111">Dokumentet har skrivits och testas med sandbox körs i Oracle VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="1681d-111">This document was written and tested with the sandbox running in Oracle VirtualBox.</span></span> <span data-ttu-id="1681d-112">Information om hur du konfigurerar sandbox finns det [Kom igång med Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="1681d-112">For information on setting up the sandbox, see the [Get started with the Hortonworks sandbox.](hdinsight-hadoop-emulator-get-started.md)</span></span> <span data-ttu-id="1681d-113">dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1681d-113">document.</span></span>

* <span data-ttu-id="1681d-114">Visual Studio 2013, Visual Studio 2015 eller Visual Studio 2017 (någon utgåva).</span><span class="sxs-lookup"><span data-stu-id="1681d-114">Visual Studio 2013, Visual Studio 2015, or Visual Studio 2017 (any edition).</span></span>

* <span data-ttu-id="1681d-115">Den [Azure SDK för .NET](https://azure.microsoft.com/downloads/) 2.7.1 eller senare.</span><span class="sxs-lookup"><span data-stu-id="1681d-115">The [Azure SDK for .NET](https://azure.microsoft.com/downloads/) 2.7.1 or later.</span></span>

* <span data-ttu-id="1681d-116">Den [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="1681d-116">The [Azure Data Lake tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

## <a name="configure-passwords-for-the-sandbox"></a><span data-ttu-id="1681d-117">Konfigurera lösenord för sandbox</span><span class="sxs-lookup"><span data-stu-id="1681d-117">Configure passwords for the sandbox</span></span>

<span data-ttu-id="1681d-118">Kontrollera att sandlådan Hortonworks körs.</span><span class="sxs-lookup"><span data-stu-id="1681d-118">Make sure that the Hortonworks Sandbox is running.</span></span> <span data-ttu-id="1681d-119">Följ stegen i den [komma igång i sandlådan Hortonworks](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1681d-119">Then follow the steps in the [Get started in the Hortonworks Sandbox](hdinsight-hadoop-emulator-get-started.md#set-sandbox-passwords) document.</span></span> <span data-ttu-id="1681d-120">De här stegen konfigurera lösenordet för SSH `root` konto och Ambari `admin` konto.</span><span class="sxs-lookup"><span data-stu-id="1681d-120">These steps configure the password for the SSH `root` account, and the Ambari `admin` account.</span></span> <span data-ttu-id="1681d-121">Dessa lösenord används när du ansluter till sandbox från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1681d-121">These passwords are used when you connect to the sandbox from Visual Studio.</span></span>

## <a name="connect-the-tools-to-the-sandbox"></a><span data-ttu-id="1681d-122">Ansluta verktygen till sandbox</span><span class="sxs-lookup"><span data-stu-id="1681d-122">Connect the tools to the sandbox</span></span>

1. <span data-ttu-id="1681d-123">Öppna Visual Studio, markera **visa**, och välj sedan **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1681d-123">Open Visual Studio, select **View**, and then select **Server Explorer**.</span></span>

2. <span data-ttu-id="1681d-124">Från **Server Explorer**, högerklicka på den **HDInsight** posten och välj sedan **Anslut till HDInsight-Emulator**.</span><span class="sxs-lookup"><span data-stu-id="1681d-124">From **Server Explorer**, right-click the **HDInsight** entry, and then select **Connect to HDInsight Emulator**.</span></span>

    ![Skärmbild av Server Explorer, med Anslut till HDInsight-Emulator markerat](./media/hdinsight-hadoop-emulator-visual-studio/connect-emulator.png)

3. <span data-ttu-id="1681d-126">Från den **Anslut till HDInsight-Emulator** dialogrutan Ange lösenordet som du har konfigurerat för Ambari.</span><span class="sxs-lookup"><span data-stu-id="1681d-126">From the **Connect to HDInsight Emulator** dialog box, enter the password that you configured for Ambari.</span></span>

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-ambari-password.png)

    <span data-ttu-id="1681d-128">Välj **nästa** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="1681d-128">Select **Next** to continue.</span></span>

4. <span data-ttu-id="1681d-129">Använd den **lösenord** fältet för att ange lösenordet som du har konfigurerat för den `root` konto.</span><span class="sxs-lookup"><span data-stu-id="1681d-129">Use the **Password** field to enter the password you configured for the `root` account.</span></span> <span data-ttu-id="1681d-130">Lämna de andra fälten standardvärdet.</span><span class="sxs-lookup"><span data-stu-id="1681d-130">Leave the other fields at the default value.</span></span>

    ![Skärmbild av dialogrutan med lösenordet i textrutan markerat](./media/hdinsight-hadoop-emulator-visual-studio/enter-root-password.png)

    <span data-ttu-id="1681d-132">Välj **nästa** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="1681d-132">Select **Next** to continue.</span></span>

5. <span data-ttu-id="1681d-133">Vänta på verifiering av tjänster ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="1681d-133">Wait for validation of the services to finish.</span></span> <span data-ttu-id="1681d-134">I vissa fall verifieringen misslyckas och uppmanar dig att uppdatera konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1681d-134">In some cases, validation may fail and prompt you to update the configuration.</span></span> <span data-ttu-id="1681d-135">Om valideringen misslyckas, Välj **uppdatering**, och väntar på att konfigurations- och verifiering för tjänsten för att avsluta.</span><span class="sxs-lookup"><span data-stu-id="1681d-135">If validation fails, select **Update**, and wait for the configuration and verification for the service to finish.</span></span>

    ![Skärmbild av dialogrutan med knappen Uppdatera markerat](./media/hdinsight-hadoop-emulator-visual-studio/fail-and-update.png)

    > [!NOTE]
    > <span data-ttu-id="1681d-137">Uppdateringsprocessen använder Ambari för att ändra konfigurationen för Hortonworks begränsat till vad som förväntas av Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1681d-137">The update process uses Ambari to modify the Hortonworks Sandbox configuration to what is expected by the Data Lake tools for Visual Studio.</span></span>

6. <span data-ttu-id="1681d-138">När verifieringen är klar väljer du **Slutför** att slutföra konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1681d-138">After validation has finished, select **Finish** to complete configuration.</span></span>
    <span data-ttu-id="1681d-139">![Skärmbild av dialogrutan med Slutför markerat](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span><span class="sxs-lookup"><span data-stu-id="1681d-139">![Screenshot of dialog box, with Finish button highlighted](./media/hdinsight-hadoop-emulator-visual-studio/finished-connect.png)</span></span>

     >[!NOTE]
     > <span data-ttu-id="1681d-140">Beroende på din utvecklingsmiljö och mängden minne som allokerats till den virtuella datorn, kan det ta flera minuter att konfigurera och verifiera tjänsterna.</span><span class="sxs-lookup"><span data-stu-id="1681d-140">Depending on the speed of your development environment, and the amount of memory allocated to the virtual machine, it can take several minutes to configure and validate the services.</span></span>

<span data-ttu-id="1681d-141">När du har följt de här stegen, nu har du en **lokala HDInsight-kluster** transaktion i Server Explorer under den **HDInsight** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1681d-141">After following these steps, you now have an **HDInsight local cluster** entry in Server Explorer, under the **HDInsight** section.</span></span>

## <a name="write-a-hive-query"></a><span data-ttu-id="1681d-142">Skriv en Hive-fråga</span><span class="sxs-lookup"><span data-stu-id="1681d-142">Write a Hive query</span></span>

<span data-ttu-id="1681d-143">Strukturen innehåller ett SQL-liknande frågespråk (HiveQL) för att arbeta med strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="1681d-143">Hive provides a SQL-like query language (HiveQL) for working with structured data.</span></span> <span data-ttu-id="1681d-144">Använd följande steg för att lära dig hur du kör på begäran frågor mot det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="1681d-144">Use the following steps to learn how to run on-demand queries against the local cluster.</span></span>

1. <span data-ttu-id="1681d-145">I **Server Explorer**, högerklicka på posten för det lokala klustret som du lagt till tidigare och välj sedan **skriva en Hive-fråga**.</span><span class="sxs-lookup"><span data-stu-id="1681d-145">In **Server Explorer**, right-click the entry for the local cluster that you added previously, and then select **Write a Hive Query**.</span></span>

    ![Skärmbild av Server Explorer med Skriv en Hive-fråga som markerats](./media/hdinsight-hadoop-emulator-visual-studio/write-hive-query.png)

    <span data-ttu-id="1681d-147">Ett nytt frågefönster visas.</span><span class="sxs-lookup"><span data-stu-id="1681d-147">A new query window appears.</span></span> <span data-ttu-id="1681d-148">Här kan du snabbt skriva och skicka en fråga till det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="1681d-148">Here you can quickly write and submit a query to the local cluster.</span></span>

2. <span data-ttu-id="1681d-149">I frågefönstret nytt anger du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1681d-149">In the new query window, enter the following command:</span></span>

        select count(*) from sample_08;

    <span data-ttu-id="1681d-150">Om du vill köra frågan **skicka** längst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="1681d-150">To run the query, select **Submit** at the top of the window.</span></span> <span data-ttu-id="1681d-151">Lämna de andra värdena (**Batch** och namn på servern) sina standardvärden.</span><span class="sxs-lookup"><span data-stu-id="1681d-151">Leave the other values (**Batch** and server name) at the default values.</span></span>

    ![Skärmbild av frågefönstret med knappen Skicka markerat](./media/hdinsight-hadoop-emulator-visual-studio/submit-hive.png)

    <span data-ttu-id="1681d-153">Du kan också använda den nedrullningsbara menyn bredvid **skicka** att välja **Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="1681d-153">You can also use the drop-down menu next to **Submit** to select **Advanced**.</span></span> <span data-ttu-id="1681d-154">Avancerade alternativ kan du ange ytterligare alternativ när du har skickat jobbet.</span><span class="sxs-lookup"><span data-stu-id="1681d-154">Advanced options allow you to provide additional options when you submit the job.</span></span>

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-hive.png)

3. <span data-ttu-id="1681d-156">När du skickar frågan visas jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="1681d-156">After you submit the query, the job status appears.</span></span> <span data-ttu-id="1681d-157">Jobbet har statusen visar information om jobbet som bearbetas av Hadoop.</span><span class="sxs-lookup"><span data-stu-id="1681d-157">The job status displays information about the job as it is processed by Hadoop.</span></span> <span data-ttu-id="1681d-158">**Jobbets status** ger status för jobbet.</span><span class="sxs-lookup"><span data-stu-id="1681d-158">**Job State** provides the status of the job.</span></span> <span data-ttu-id="1681d-159">Tillståndet uppdateras regelbundet och du kan använda den här ikonen för att uppdatera tillståndet manuellt.</span><span class="sxs-lookup"><span data-stu-id="1681d-159">The state is updated periodically, or you can use the refresh icon to refresh the state manually.</span></span>

    ![Skärmbild av jobbet dialogrutan vy med jobbstatus markerat](./media/hdinsight-hadoop-emulator-visual-studio/job-state.png)

    <span data-ttu-id="1681d-161">Efter den **jobbstatus** ändras till **avslutad**, dirigeras acykliska diagram (DAG) visas.</span><span class="sxs-lookup"><span data-stu-id="1681d-161">After the **Job State** changes to **Finished**, a Directed Acyclic Graph (DAG) is displayed.</span></span> <span data-ttu-id="1681d-162">Det här diagrammet beskrivs körning av sökväg som fastställts av Tez vid bearbetning av Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="1681d-162">This diagram describes the execution path that was determined by Tez when processing the Hive query.</span></span> <span data-ttu-id="1681d-163">Tez är standard Körningsmotor för Hive i det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="1681d-163">Tez is the default execution engine for Hive on the local cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1681d-164">Tez är standardalternativet när du använder Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1681d-164">Tez is also the default when you are using Linux-based HDInsight clusters.</span></span> <span data-ttu-id="1681d-165">Det är inte standard på Windows-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1681d-165">It is not the default on Windows-based HDInsight.</span></span> <span data-ttu-id="1681d-166">Du använder den, måste du lägga till raden `set hive.execution.engine = tez;` till början av Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="1681d-166">To use it there, you must add the line `set hive.execution.engine = tez;` to the beginning of your Hive query.</span></span>

    <span data-ttu-id="1681d-167">Använd den **Jobbutdata** länken om du vill visa resultatet.</span><span class="sxs-lookup"><span data-stu-id="1681d-167">Use the **Job Output** link to view the output.</span></span> <span data-ttu-id="1681d-168">I det här fallet är det 823, antalet rader i tabellen sample_08.</span><span class="sxs-lookup"><span data-stu-id="1681d-168">In this case, it is 823, the number of rows in the sample_08 table.</span></span> <span data-ttu-id="1681d-169">Du kan visa diagnostikinformation om jobbet genom att använda den **Jobblogg** och **hämta YARN-logg** länkar.</span><span class="sxs-lookup"><span data-stu-id="1681d-169">You can view diagnostics information about the job by using the **Job Log** and **Download YARN Log** links.</span></span>

4. <span data-ttu-id="1681d-170">Du kan också köra Hive-jobb interaktivt genom att ändra den **Batch** till **interaktiv**.</span><span class="sxs-lookup"><span data-stu-id="1681d-170">You can also run Hive jobs interactively by changing the **Batch** field to **Interactive**.</span></span> <span data-ttu-id="1681d-171">Välj sedan **kör**.</span><span class="sxs-lookup"><span data-stu-id="1681d-171">Then select **Execute**.</span></span>

    ![Skärmbild av interaktiva och kör knappar markerat](./media/hdinsight-hadoop-emulator-visual-studio/interactive-query.png)

    <span data-ttu-id="1681d-173">En interaktiv fråga strömmar utdataloggen genereras under bearbetningen till den **HiveServer2 utdata** fönster.</span><span class="sxs-lookup"><span data-stu-id="1681d-173">An interactive query streams the output log generated during processing to the **HiveServer2 Output** window.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1681d-174">Informationen är samma som är tillgänglig från den **Jobblogg** länka när ett jobb har slutförts.</span><span class="sxs-lookup"><span data-stu-id="1681d-174">The information is the same that is available from the **Job Log** link after a job has finished.</span></span>

    ![Skärmbild av logg för utdata](./media/hdinsight-hadoop-emulator-visual-studio/hiveserver2-output.png)

## <a name="create-a-hive-project"></a><span data-ttu-id="1681d-176">Skapa ett Hive-projekt</span><span class="sxs-lookup"><span data-stu-id="1681d-176">Create a Hive project</span></span>

<span data-ttu-id="1681d-177">Du kan också skapa ett projekt som innehåller flera Hive-skript.</span><span class="sxs-lookup"><span data-stu-id="1681d-177">You can also create a project that contains multiple Hive scripts.</span></span> <span data-ttu-id="1681d-178">Använd ett projekt när du har relaterade skript eller vill lagra skript i ett system för version.</span><span class="sxs-lookup"><span data-stu-id="1681d-178">Use a project when you have related scripts or want to store scripts in a version control system.</span></span>

1. <span data-ttu-id="1681d-179">I Visual Studio väljer **filen**, **ny**, och sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="1681d-179">In Visual Studio, select **File**, **New**, and then **Project**.</span></span>

2. <span data-ttu-id="1681d-180">I listan över projekt, expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **HIVE (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="1681d-180">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **HIVE (HDInsight)**.</span></span> <span data-ttu-id="1681d-181">Välj i listan över mallar **Hive exempel**.</span><span class="sxs-lookup"><span data-stu-id="1681d-181">From the list of templates, select **Hive Sample**.</span></span> <span data-ttu-id="1681d-182">Ange ett namn och plats och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="1681d-182">Enter a name and location, and then select **OK**.</span></span>

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, HIVE, Hive exempel och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-hive-project.png)

<span data-ttu-id="1681d-184">Den **Hive exempel** projektet innehåller två skript **WebLogAnalysis.hql** och **SensorDataAnalysis.hql**.</span><span class="sxs-lookup"><span data-stu-id="1681d-184">The **Hive Sample** project contains two scripts, **WebLogAnalysis.hql** and **SensorDataAnalysis.hql**.</span></span> <span data-ttu-id="1681d-185">Du kan skicka dessa skript med hjälp av samma **skicka** längst upp i fönstret.</span><span class="sxs-lookup"><span data-stu-id="1681d-185">You can submit these scripts by using the same **Submit** button at the top of the window.</span></span>

## <a name="create-a-pig-project"></a><span data-ttu-id="1681d-186">Skapa ett Pig-projekt</span><span class="sxs-lookup"><span data-stu-id="1681d-186">Create a Pig project</span></span>

<span data-ttu-id="1681d-187">Medan Hive innehåller en SQL-liknande språk för att arbeta med strukturerade data, fungerar Pig genom att utföra omvandlingar på data.</span><span class="sxs-lookup"><span data-stu-id="1681d-187">While Hive provides a SQL-like language for working with structured data, Pig works by performing transformations on data.</span></span> <span data-ttu-id="1681d-188">Pig innehåller ett språk (Pig Latin) som hjälper dig att utveckla en pipeline omformningar.</span><span class="sxs-lookup"><span data-stu-id="1681d-188">Pig provides a language (Pig Latin) that allows you to develop a pipeline of transformations.</span></span> <span data-ttu-id="1681d-189">Följ dessa steg om du vill använda Pig med det lokala klustret:</span><span class="sxs-lookup"><span data-stu-id="1681d-189">To use Pig with the local cluster, follow these steps:</span></span>

1. <span data-ttu-id="1681d-190">Öppna Visual Studio och välj **filen**, **ny**, och sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="1681d-190">Open Visual Studio, and select **File**, **New**, and then **Project**.</span></span> <span data-ttu-id="1681d-191">I listan över projekt, expandera **mallar**, expandera **Azure Data Lake**, och välj sedan **Pig (HDInsight)**.</span><span class="sxs-lookup"><span data-stu-id="1681d-191">From the list of projects, expand **Templates**, expand **Azure Data Lake**, and then select **Pig (HDInsight)**.</span></span> <span data-ttu-id="1681d-192">Välj i listan över mallar **Pig programmet**.</span><span class="sxs-lookup"><span data-stu-id="1681d-192">From the list of templates, select **Pig Application**.</span></span> <span data-ttu-id="1681d-193">Ange ett namn, plats, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="1681d-193">Enter a name, location, and then select **OK**.</span></span>

    ![Skärmbild av nytt projekt fönster, med Azure Data Lake, Pig, Pig program och OK markerat](./media/hdinsight-hadoop-emulator-visual-studio/new-pig.png)

2. <span data-ttu-id="1681d-195">Ange följande text som innehållet i den **script.pig** filen som skapades med det här projektet.</span><span class="sxs-lookup"><span data-stu-id="1681d-195">Enter the following text as the contents of the **script.pig** file that was created with this project.</span></span>

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

    <span data-ttu-id="1681d-196">Pig använder ett annat språk än Hive, hur du kör jobben är konsekventa mellan både språk via den **skicka** knappen.</span><span class="sxs-lookup"><span data-stu-id="1681d-196">While Pig uses a different language than Hive, how you run the jobs is consistent between both languages, through the **Submit** button.</span></span> <span data-ttu-id="1681d-197">Att välja i listrutan bredvid **skicka** visar en dialogruta med avancerade skicka för Pig.</span><span class="sxs-lookup"><span data-stu-id="1681d-197">Selecting the drop-down beside **Submit** displays an advanced submit dialog box for Pig.</span></span>

    ![Dialogrutan Skicka skript skärmbild](./media/hdinsight-hadoop-emulator-visual-studio/advanced-pig.png)

3. <span data-ttu-id="1681d-199">Jobbets status och utdata visas också, samma som en Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="1681d-199">The job status and output is also displayed, the same as a Hive query.</span></span>

    ![Skärmbild av ett avslutat Pig-projekt](./media/hdinsight-hadoop-emulator-visual-studio/completed-pig.png)

## <a name="view-jobs"></a><span data-ttu-id="1681d-201">Visa jobb</span><span class="sxs-lookup"><span data-stu-id="1681d-201">View jobs</span></span>

<span data-ttu-id="1681d-202">Data Lake-verktyg kan du enkelt se information om jobb som har körts på Hadoop.</span><span class="sxs-lookup"><span data-stu-id="1681d-202">Data Lake tools also allow you to easily view information about jobs that have been run on Hadoop.</span></span> <span data-ttu-id="1681d-203">Använd följande steg om du vill visa de jobb som har körts på det lokala klustret.</span><span class="sxs-lookup"><span data-stu-id="1681d-203">Use the following steps to see the jobs that have been run on the local cluster.</span></span>

1. <span data-ttu-id="1681d-204">Från **Server Explorer**, högerklicka på det lokala klustret och välj sedan **visa jobb**.</span><span class="sxs-lookup"><span data-stu-id="1681d-204">From **Server Explorer**, right-click the local cluster, and then select **View Jobs**.</span></span> <span data-ttu-id="1681d-205">En lista över jobb som har skickats till klustret visas.</span><span class="sxs-lookup"><span data-stu-id="1681d-205">A list of jobs that have been submitted to the cluster is displayed.</span></span>

    ![Skärmbild av Server Explorer, med Visa jobb markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-jobs.png)

2. <span data-ttu-id="1681d-207">Välj något att visa Jobbinformationen för jobben i listan.</span><span class="sxs-lookup"><span data-stu-id="1681d-207">From the list of jobs, select one to view the job details.</span></span>

    ![Skärmbild av jobbet webbläsare med ett av jobben markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-job-details.png)

    <span data-ttu-id="1681d-209">Informationen liknar det som visas när du har kört en Hive- eller Pig fråga, inklusive länkar om du vill visa utdata och logga information.</span><span class="sxs-lookup"><span data-stu-id="1681d-209">The information displayed is similar to what you see after running a Hive or Pig query, including links to view the output and log information.</span></span>

3. <span data-ttu-id="1681d-210">Du kan också ändra och skicka jobbet från den här.</span><span class="sxs-lookup"><span data-stu-id="1681d-210">You can also modify and resubmit the job from here.</span></span>

## <a name="view-hive-databases"></a><span data-ttu-id="1681d-211">Visa Hive-databaser</span><span class="sxs-lookup"><span data-stu-id="1681d-211">View Hive databases</span></span>

1. <span data-ttu-id="1681d-212">I **Server Explorer**, expandera den **lokala HDInsight-kluster** post, och expandera sedan **Hive-databaser**.</span><span class="sxs-lookup"><span data-stu-id="1681d-212">In **Server Explorer**, expand the **HDInsight local cluster** entry, and then expand **Hive Databases**.</span></span> <span data-ttu-id="1681d-213">Den **standard** och **xademo** databaser i det lokala klustret visas.</span><span class="sxs-lookup"><span data-stu-id="1681d-213">The **Default** and **xademo** databases on the local cluster are displayed.</span></span> <span data-ttu-id="1681d-214">Expandera en databas visar tabeller i databasen.</span><span class="sxs-lookup"><span data-stu-id="1681d-214">Expanding a database shows the tables within the database.</span></span>

    ![Skärmbild av Server Explorer, med databaser expanderas](./media/hdinsight-hadoop-emulator-visual-studio/expanded-databases.png)

2. <span data-ttu-id="1681d-216">Expandera en tabell visar kolumnerna för tabellen.</span><span class="sxs-lookup"><span data-stu-id="1681d-216">Expanding a table displays the columns for that table.</span></span> <span data-ttu-id="1681d-217">Högerklicka på en tabell för att snabbt visa data, och välj **visa de 100 översta raderna**.</span><span class="sxs-lookup"><span data-stu-id="1681d-217">To quickly view the data, right-click a table, and select **View Top 100 Rows**.</span></span>

    ![Skärmbild av Server Explorer, med tabellen expanderas och visa de 100 översta raderna markerat](./media/hdinsight-hadoop-emulator-visual-studio/view-100.png)

### <a name="database-and-table-properties"></a><span data-ttu-id="1681d-219">Egenskaper för databas och tabell</span><span class="sxs-lookup"><span data-stu-id="1681d-219">Database and table properties</span></span>

<span data-ttu-id="1681d-220">Du kan visa egenskaperna för en databas eller tabell.</span><span class="sxs-lookup"><span data-stu-id="1681d-220">You can view the properties of a database or table.</span></span> <span data-ttu-id="1681d-221">Att välja **egenskaper** visar information för det valda objektet i egenskapsfönstret.</span><span class="sxs-lookup"><span data-stu-id="1681d-221">Selecting **Properties** displays details for the selected item in the properties window.</span></span> <span data-ttu-id="1681d-222">Till exempel se informationen som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="1681d-222">For example, see the information shown in the following screenshot:</span></span>

![Skärmbild av egenskapsfönstret](./media/hdinsight-hadoop-emulator-visual-studio/properties.png)

### <a name="create-a-table"></a><span data-ttu-id="1681d-224">Skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="1681d-224">Create a table</span></span>

<span data-ttu-id="1681d-225">Högerklicka på en databas för att skapa en tabell, och välj sedan **Create Table**.</span><span class="sxs-lookup"><span data-stu-id="1681d-225">To create a table, right-click a database, and then select **Create Table**.</span></span>

![Skärmbild av Server Explorer, med Create Table markerat](./media/hdinsight-hadoop-emulator-visual-studio/create-table.png)

<span data-ttu-id="1681d-227">Du kan sedan skapa tabellen med hjälp av ett formulär.</span><span class="sxs-lookup"><span data-stu-id="1681d-227">You can then create the table using a form.</span></span> <span data-ttu-id="1681d-228">Du kan se rådata HiveQL som används för att skapa tabellen längst ned i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="1681d-228">At the bottom of the following screenshot, you can see the raw HiveQL that is used to create the table.</span></span>

![Skärmbild av formuläret som används för att skapa en tabell](./media/hdinsight-hadoop-emulator-visual-studio/create-table-form.png)

## <a name="next-steps"></a><span data-ttu-id="1681d-230">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1681d-230">Next steps</span></span>

* [<span data-ttu-id="1681d-231">Learning linor av sandlådan Hortonworks</span><span class="sxs-lookup"><span data-stu-id="1681d-231">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="1681d-232">Hadoop-vägledning - komma igång med HDP</span><span class="sxs-lookup"><span data-stu-id="1681d-232">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)
