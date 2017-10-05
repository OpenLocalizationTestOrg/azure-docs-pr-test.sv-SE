---
title: "Installera Presto på Azure HDInsight Linux-kluster | Microsoft Docs"
description: "Lär dig hur du installerar Presto och Airpal på Linux-baserade HDInsight Hadoop-kluster med hjälp av skriptåtgärder."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 406ef84e72d253fec51a0b37c48f326dafd511b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="3e076-103">Installera och använda Presto på HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="3e076-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="3e076-104">Lär dig hur du installerar Presto på HDInsight Hadoop-kluster med hjälp av skriptåtgärder i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3e076-104">In this topic, you learn how to install Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="3e076-105">Du också lära dig hur du installerar Airpal på ett befintligt Presto HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e076-105">You also learn how to install Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e076-106">Stegen i det här dokumentet kräver en **HDInsight 3.5 Hadoop-kluster** som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="3e076-106">The steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="3e076-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="3e076-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3e076-108">Mer information finns i [HDInsight-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="3e076-109">Vad är Presto?</span><span class="sxs-lookup"><span data-stu-id="3e076-109">What is Presto?</span></span>
<span data-ttu-id="3e076-110">[Presto](https://prestodb.io/overview.html) är en snabb distribuerade SQL-frågemotor för stordata.</span><span class="sxs-lookup"><span data-stu-id="3e076-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="3e076-111">Presto är lämplig för interaktiva frågor till petabyte data.</span><span class="sxs-lookup"><span data-stu-id="3e076-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="3e076-112">Mer information om komponenter av Presto och hur de fungerar finns i [Presto begrepp](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="3e076-112">For more information on the components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="3e076-113">Komponenter som ingår i HDInsight-kluster stöds fullt ut och Microsoft-supporten hjälper att isolera och lösa problem relaterade till komponenterna.</span><span class="sxs-lookup"><span data-stu-id="3e076-113">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
> 
> <span data-ttu-id="3e076-114">Anpassade komponenter, till exempel Presto kan få kommersiellt rimliga stöd för att hjälpa dig att felsöka problemet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="3e076-114">Custom components, such as Presto, receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="3e076-115">Detta kan resultera i att lösa problemet eller där du uppmanas att engagera tillgängliga kanaler för öppen källkod där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="3e076-115">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="3e076-116">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="3e076-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="3e076-117">Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3e076-117">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="3e076-118">Installera Presto med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="3e076-118">Install Presto using script action</span></span>

<span data-ttu-id="3e076-119">Det här avsnittet innehåller instruktioner om hur du använder exempelskriptet när du skapar ett nytt kluster med hjälp av Azure portal.</span><span class="sxs-lookup"><span data-stu-id="3e076-119">This section provides instructions on how to use the sample script when creating a new cluster by using the Azure portal.</span></span> 

1. <span data-ttu-id="3e076-120">Börja etablera ett kluster med hjälp av stegen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-120">Start provisioning a cluster by using the steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="3e076-121">Se till att du skapar klustret med den **anpassad** klustret skapas flödet.</span><span class="sxs-lookup"><span data-stu-id="3e076-121">Make sure you create the cluster using the **Custom** cluster creation flow.</span></span> <span data-ttu-id="3e076-122">Du måste se till att du skapar klustret uppfyller följande krav.</span><span class="sxs-lookup"><span data-stu-id="3e076-122">You must ensure that the cluster you create meets the following requirements.</span></span>

    <span data-ttu-id="3e076-123">a.</span><span class="sxs-lookup"><span data-stu-id="3e076-123">a.</span></span> <span data-ttu-id="3e076-124">Det måste vara ett Hadoop-kluster med HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="3e076-124">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="3e076-125">b.</span><span class="sxs-lookup"><span data-stu-id="3e076-125">b.</span></span> <span data-ttu-id="3e076-126">Den måste använda Azure Storage som dataarkiv.</span><span class="sxs-lookup"><span data-stu-id="3e076-126">It must use Azure Storage as the data store.</span></span> <span data-ttu-id="3e076-127">Med Presto på ett kluster som använder Azure Data Lake Store som lagringsalternativet stöds inte ännu.</span><span class="sxs-lookup"><span data-stu-id="3e076-127">Using Presto on a cluster that uses Azure Data Lake Store as the storage option is not yet supported.</span></span> 

    ![HDInsight-kluster med anpassade alternativ](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="3e076-129">På den **avancerade inställningar** bladet väljer **skriptåtgärder**, och ange informationen nedan:</span><span class="sxs-lookup"><span data-stu-id="3e076-129">On the **Advanced settings** blade, select **Script Actions**, and provide the information below:</span></span>
   
   * <span data-ttu-id="3e076-130">**NAMNET**: Ange ett eget namn för skriptåtgärden.</span><span class="sxs-lookup"><span data-stu-id="3e076-130">**NAME**: Enter a friendly name for the script action.</span></span>
   * <span data-ttu-id="3e076-131">**Bash-skript-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="3e076-131">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="3e076-132">**HEAD**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="3e076-132">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="3e076-133">**WORKER**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="3e076-133">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="3e076-134">**ZOOKEEPER**: avmarkerar du kryssrutan</span><span class="sxs-lookup"><span data-stu-id="3e076-134">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="3e076-135">**PARAMETRARNA**: lämna fältet tomt</span><span class="sxs-lookup"><span data-stu-id="3e076-135">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="3e076-136">Längst ned i den **skriptåtgärder** bladet, klickar du på den **Välj** för att spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3e076-136">At the bottom of the **Script Actions** blade, click the **Select** button to save the configuration.</span></span> <span data-ttu-id="3e076-137">Klicka slutligen på den **Välj** knappen längst ned i den **avancerade inställningar** bladet för att spara konfigurationsinformationen.</span><span class="sxs-lookup"><span data-stu-id="3e076-137">Finally, click  the **Select** button at the bottom of the **Advanced Settings** blade to save the configuration information.</span></span>

4. <span data-ttu-id="3e076-138">Fortsätta etablering klustret enligt beskrivningen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-138">Continue provisioning the cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="3e076-139">Azure PowerShell, Azure CLI, HDInsight .NET SDK eller Azure Resource Manager-mallar kan också användas för att tillämpa skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="3e076-139">Azure PowerShell, the Azure CLI, the HDInsight .NET SDK, or Azure Resource Manager templates can also be used to apply script actions.</span></span> <span data-ttu-id="3e076-140">Du kan även gälla skriptåtgärder kluster som körs redan.</span><span class="sxs-lookup"><span data-stu-id="3e076-140">You can also apply script actions to already running clusters.</span></span> <span data-ttu-id="3e076-141">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-141">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="3e076-142">Använda Presto med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3e076-142">Use Presto with HDInsight</span></span>

<span data-ttu-id="3e076-143">Utför följande steg för att använda Presto i ett HDInsight-kluster när du har installerat den med hjälp av stegen som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="3e076-143">Perform the following steps to use Presto in an HDInsight cluster after you have installed it using the steps described above.</span></span>

1. <span data-ttu-id="3e076-144">Anslut till HDInsight-klustret med hjälp av SSH:</span><span class="sxs-lookup"><span data-stu-id="3e076-144">Connect to the HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="3e076-145">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3e076-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="3e076-146">Starta Presto shell med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="3e076-146">Start the Presto shell using the following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="3e076-147">Köra en fråga på en exempeltabell **hivesampletable**, som är tillgänglig i alla HDInsight-kluster som standard.</span><span class="sxs-lookup"><span data-stu-id="3e076-147">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="3e076-148">Som standard [Hive](https://prestodb.io/docs/current/connector/hive.html) och [TPCH](https://prestodb.io/docs/current/connector/tpch.html) kopplingar för Presto har redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="3e076-148">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="3e076-149">Hive-anslutningen är konfigurerad för att använda installerat Hive standardinstallationen, så alla tabeller från Hive visas automatiskt i Presto.</span><span class="sxs-lookup"><span data-stu-id="3e076-149">Hive connector is configured to use the default installed Hive installation, so all the tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="3e076-150">En detaljerad beskrivning av hur du kan använda Presto finns [Presto dokumentationen](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="3e076-150">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="3e076-151">Använda Airpal med Presto</span><span class="sxs-lookup"><span data-stu-id="3e076-151">Use Airpal with Presto</span></span>

<span data-ttu-id="3e076-152">[Airpal](https://github.com/airbnb/airpal#airpal) är ett gränssnitt för öppen källkod webbaserade frågan för Presto.</span><span class="sxs-lookup"><span data-stu-id="3e076-152">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="3e076-153">Mer information om Airpal finns [Airpal dokumentationen](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="3e076-153">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="3e076-154">I det här avsnittet ska vi titta på hur du **installera Airpal på edgenode** i ett HDInsight Hadoop-kluster där det redan finns Presto installerad.</span><span class="sxs-lookup"><span data-stu-id="3e076-154">In this section, we look at the steps to **install Airpal on the edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="3e076-155">Detta säkerställer att Airpal frågan webbgränssnittet är tillgänglig via Internet.</span><span class="sxs-lookup"><span data-stu-id="3e076-155">This ensures that the Airpal web query interface is available over the Internet.</span></span>

1. <span data-ttu-id="3e076-156">Med SSH, Anslut till headnode i HDInsight-kluster som har installerat Presto:</span><span class="sxs-lookup"><span data-stu-id="3e076-156">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="3e076-157">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3e076-157">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3e076-158">När du är ansluten, kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="3e076-158">Once you are connected, run the following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="3e076-159">Du bör se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="3e076-159">You should see an output like the following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="3e076-160">Anteckna värdet för utdata i **värdet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="3e076-160">From the output, note the value for the **value** property.</span></span> <span data-ttu-id="3e076-161">Du behöver detta när du installerar Airpal på edgenode för klustret.</span><span class="sxs-lookup"><span data-stu-id="3e076-161">You will need this while installing Airpal on the cluster edgenode.</span></span> <span data-ttu-id="3e076-162">Från resultatet ovan är det värde som du behöver **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="3e076-162">From the output above, the value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="3e076-163">Använd mallen  **[här](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  att skapa ett HDInsight-kluster edgenode och ange värden som visas i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="3e076-163">Use the template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** to create an HDInsight cluster edgenode and provide the values as shown in the following screenshot.</span></span>

    ![Installera HDInsight Airpal på Presto klustret](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="3e076-165">Klicka på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="3e076-165">Click **Purchase**.</span></span>

6. <span data-ttu-id="3e076-166">När ändringarna tillämpas på klusterkonfigurationen, du kan komma åt webbgränssnittet Airpal med hjälp av följande steg.</span><span class="sxs-lookup"><span data-stu-id="3e076-166">Once the changes are applied to the cluster configuration, you can access the Airpal web interface by using the following steps.</span></span>

    <span data-ttu-id="3e076-167">a.</span><span class="sxs-lookup"><span data-stu-id="3e076-167">a.</span></span> <span data-ttu-id="3e076-168">Klusterbladet klickar du på **program**.</span><span class="sxs-lookup"><span data-stu-id="3e076-168">From the cluster blade, click **Applications**.</span></span>

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="3e076-170">b.</span><span class="sxs-lookup"><span data-stu-id="3e076-170">b.</span></span> <span data-ttu-id="3e076-171">Från den **installerade appar** bladet, klickar du på **Portal** mot airpal.</span><span class="sxs-lookup"><span data-stu-id="3e076-171">From the **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="3e076-173">c.</span><span class="sxs-lookup"><span data-stu-id="3e076-173">c.</span></span> <span data-ttu-id="3e076-174">När du uppmanas ange administratörsautentiseringsuppgifter som du angav när du skapar HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e076-174">When prompted, enter the admin credentials that you specified while creating the HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="3e076-175">Anpassa en Presto installation på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="3e076-175">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="3e076-176">När du har installerat Presto på ett HDInsight Hadoop-kluster kan anpassa du installationen om du vill göra ändringar som uppdatera minnesinställningarna, ändra kopplingar, osv. Utför följande steg för att göra det.</span><span class="sxs-lookup"><span data-stu-id="3e076-176">After you have installed Presto on an HDInsight Hadoop cluster, you can customize the installation to make changes such as update memory settings, change connectors, etc. Perform the following steps to do so.</span></span>

1. <span data-ttu-id="3e076-177">Med SSH, Anslut till headnode i HDInsight-kluster som har installerat Presto:</span><span class="sxs-lookup"><span data-stu-id="3e076-177">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="3e076-178">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="3e076-178">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="3e076-179">Ändrar du konfigurationen i filen `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="3e076-179">Make your configuration changes in the file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="3e076-180">Mer information om Presto konfiguration finns [Presto konfigurationen för YARN-baserade kluster](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="3e076-180">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="3e076-181">Stoppa och avsluta den aktuella körs instansen av Presto.</span><span class="sxs-lookup"><span data-stu-id="3e076-181">Stop and kill the current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="3e076-182">Starta en ny instans av Presto med anpassningen.</span><span class="sxs-lookup"><span data-stu-id="3e076-182">Start a new instance of Presto with the customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="3e076-183">Vänta tills den nya instansen redo och anteckna presto coordinator adress.</span><span class="sxs-lookup"><span data-stu-id="3e076-183">Wait for the new instance to be ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="3e076-184">Generera benchmark-data för HDInsight-kluster som kör Presto</span><span class="sxs-lookup"><span data-stu-id="3e076-184">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="3e076-185">TPC DS är branschstandard för mätning av prestanda i många beslut support-system, inklusive system för stordata.</span><span class="sxs-lookup"><span data-stu-id="3e076-185">TPC-DS is the industry standard for measuring the performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="3e076-186">Du kan använda Presto på HDInsight-kluster att generera data och utvärdera hur jämförs med dina egna HDInsight benchmark-data.</span><span class="sxs-lookup"><span data-stu-id="3e076-186">You can use Presto on HDInsight clusters to generate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="3e076-187">Mer information finns i [här](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-187">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="3e076-188">Se även</span><span class="sxs-lookup"><span data-stu-id="3e076-188">See also</span></span>
* <span data-ttu-id="3e076-189">[Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-189">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="3e076-190">Hue är ett webbgränssnitt som gör det enkelt att skapa, köra och spara Pig och Hive-jobb, samt bläddra standardlagring för din HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e076-190">Hue is a web UI that makes it easy to create, run and save Pig and Hive jobs, as well as browse the default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="3e076-191">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-191">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="3e076-192">Använd anpassning av klustret för att installera Giraph på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e076-192">Use cluster customization to install Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="3e076-193">Giraph kan du utföra bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3e076-193">Giraph allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="3e076-194">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3e076-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="3e076-195">Använd anpassning av klustret för att installera Solr på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="3e076-195">Use cluster customization to install Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="3e076-196">Solr kan du utföra kraftfulla sökningar på lagrade data.</span><span class="sxs-lookup"><span data-stu-id="3e076-196">Solr allows you to perform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
