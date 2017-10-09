---
title: aaaInstall Presto i Azure HDInsight Linux-kluster | Microsoft Docs
description: "Lär dig hur tooinstall Presto och Airpal på Linux-baserade HDInsight Hadoop-kluster med hjälp av skriptåtgärder."
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
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="fb83e-103">Installera och använda Presto på HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="fb83e-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="fb83e-104">I det här avsnittet får du lära dig hur tooinstall Presto på HDInsight Hadoop-kluster med hjälp av skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="fb83e-104">In this topic, you learn how tooinstall Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="fb83e-105">Du också lära dig hur tooinstall Airpal på ett befintligt Presto HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="fb83e-105">You also learn how tooinstall Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb83e-106">hello stegen i det här dokumentet kräver en **HDInsight 3.5 Hadoop-kluster** som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="fb83e-106">hello steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="fb83e-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="fb83e-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fb83e-108">Mer information finns i [HDInsight-versioner](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="fb83e-109">Vad är Presto?</span><span class="sxs-lookup"><span data-stu-id="fb83e-109">What is Presto?</span></span>
<span data-ttu-id="fb83e-110">[Presto](https://prestodb.io/overview.html) är en snabb distribuerade SQL-frågemotor för stordata.</span><span class="sxs-lookup"><span data-stu-id="fb83e-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="fb83e-111">Presto är lämplig för interaktiva frågor till petabyte data.</span><span class="sxs-lookup"><span data-stu-id="fb83e-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="fb83e-112">Mer information om hello komponenter av Presto och hur de fungerar finns i [Presto begrepp](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="fb83e-112">For more information on hello components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="fb83e-113">Komponenter som ingår i hello HDInsight-kluster stöds fullt ut och Microsoft Support kommer att tooisolate och lösa problem relaterade toothese komponenter.</span><span class="sxs-lookup"><span data-stu-id="fb83e-113">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
> 
> <span data-ttu-id="fb83e-114">Anpassade komponenter, till exempel Presto kan ta emot kommersiellt rimliga stöd toohelp du toofurther hello felsökning.</span><span class="sxs-lookup"><span data-stu-id="fb83e-114">Custom components, such as Presto, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="fb83e-115">Detta kan resultera i att lösa problemet hello eller be tooengage tillgängliga kanaler för hello öppnas teknikerna där djup expertis för att teknik finns.</span><span class="sxs-lookup"><span data-stu-id="fb83e-115">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="fb83e-116">Det finns till exempel många community-webbplatser som kan användas, t.ex: [MSDN-forum för HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Apache-projekt har också project-webbplatser [http://apache.org](http://apache.org), till exempel: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fb83e-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="fb83e-117">Installera Presto med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="fb83e-117">Install Presto using script action</span></span>

<span data-ttu-id="fb83e-118">Det här avsnittet innehåller instruktioner om hur toouse hello exempelskript när du skapar ett nytt kluster med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb83e-118">This section provides instructions on how toouse hello sample script when creating a new cluster by using hello Azure portal.</span></span> 

1. <span data-ttu-id="fb83e-119">Börja etablera ett kluster med hjälp av hello stegen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-119">Start provisioning a cluster by using hello steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="fb83e-120">Se till att du skapar hello-kluster med hjälp av hello **anpassad** klustret skapas flödet.</span><span class="sxs-lookup"><span data-stu-id="fb83e-120">Make sure you create hello cluster using hello **Custom** cluster creation flow.</span></span> <span data-ttu-id="fb83e-121">Du måste se till att hello-kluster som du skapar uppfyller följande krav hello.</span><span class="sxs-lookup"><span data-stu-id="fb83e-121">You must ensure that hello cluster you create meets hello following requirements.</span></span>

    <span data-ttu-id="fb83e-122">a.</span><span class="sxs-lookup"><span data-stu-id="fb83e-122">a.</span></span> <span data-ttu-id="fb83e-123">Det måste vara ett Hadoop-kluster med HDInsight version 3.5.</span><span class="sxs-lookup"><span data-stu-id="fb83e-123">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="fb83e-124">b.</span><span class="sxs-lookup"><span data-stu-id="fb83e-124">b.</span></span> <span data-ttu-id="fb83e-125">Den måste använda Azure Storage som hello datalager.</span><span class="sxs-lookup"><span data-stu-id="fb83e-125">It must use Azure Storage as hello data store.</span></span> <span data-ttu-id="fb83e-126">Med hjälp av Presto på ett kluster som använder Azure Data Lake Store som hello lagringsalternativ stöds inte ännu.</span><span class="sxs-lookup"><span data-stu-id="fb83e-126">Using Presto on a cluster that uses Azure Data Lake Store as hello storage option is not yet supported.</span></span> 

    ![HDInsight-kluster med anpassade alternativ](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="fb83e-128">På hello **avancerade inställningar** bladet väljer **skriptåtgärder**, och ange hello informationen nedan:</span><span class="sxs-lookup"><span data-stu-id="fb83e-128">On hello **Advanced settings** blade, select **Script Actions**, and provide hello information below:</span></span>
   
   * <span data-ttu-id="fb83e-129">**NAMNET**: Ange ett eget namn för hello skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="fb83e-129">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="fb83e-130">**Bash-skript-URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="fb83e-130">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="fb83e-131">**HEAD**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="fb83e-131">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="fb83e-132">**WORKER**: Markera det här alternativet</span><span class="sxs-lookup"><span data-stu-id="fb83e-132">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="fb83e-133">**ZOOKEEPER**: avmarkerar du kryssrutan</span><span class="sxs-lookup"><span data-stu-id="fb83e-133">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="fb83e-134">**PARAMETRARNA**: lämna fältet tomt</span><span class="sxs-lookup"><span data-stu-id="fb83e-134">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="fb83e-135">Längst ned hello hello **skriptåtgärder** bladet, klickar du på hello **Välj** toosave hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="fb83e-135">At hello bottom of hello **Script Actions** blade, click hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="fb83e-136">Klicka slutligen på hello **Välj** knappen längst ned hello hello **avancerade inställningar** bladet toosave hello konfigurationsinformation.</span><span class="sxs-lookup"><span data-stu-id="fb83e-136">Finally, click  hello **Select** button at hello bottom of hello **Advanced Settings** blade toosave hello configuration information.</span></span>

4. <span data-ttu-id="fb83e-137">Fortsätta etablering hello klustret enligt beskrivningen i [etablera Linux-baserade HDInsight-kluster](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-137">Continue provisioning hello cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="fb83e-138">Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK eller Azure Resource Manager-mallar kan också vara används tooapply skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="fb83e-138">Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK, or Azure Resource Manager templates can also be used tooapply script actions.</span></span> <span data-ttu-id="fb83e-139">Du kan också använda skriptet åtgärder tooalready kluster som körs.</span><span class="sxs-lookup"><span data-stu-id="fb83e-139">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="fb83e-140">Mer information finns i [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-140">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="fb83e-141">Använda Presto med HDInsight</span><span class="sxs-lookup"><span data-stu-id="fb83e-141">Use Presto with HDInsight</span></span>

<span data-ttu-id="fb83e-142">Utföra hello följande steg toouse Presto i ett HDInsight-kluster när du har installerat den med hjälp av hello stegen som beskrivs ovan.</span><span class="sxs-lookup"><span data-stu-id="fb83e-142">Perform hello following steps toouse Presto in an HDInsight cluster after you have installed it using hello steps described above.</span></span>

1. <span data-ttu-id="fb83e-143">Anslut toohello HDInsight-kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="fb83e-143">Connect toohello HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="fb83e-144">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="fb83e-144">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="fb83e-145">Starta hello Presto shell med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="fb83e-145">Start hello Presto shell using hello following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="fb83e-146">Köra en fråga på en exempeltabell **hivesampletable**, som är tillgänglig i alla HDInsight-kluster som standard.</span><span class="sxs-lookup"><span data-stu-id="fb83e-146">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="fb83e-147">Som standard [Hive](https://prestodb.io/docs/current/connector/hive.html) och [TPCH](https://prestodb.io/docs/current/connector/tpch.html) kopplingar för Presto har redan konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="fb83e-147">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="fb83e-148">Hive connector är konfigurerade toouse hello installerat Hive standardinstallation, så alla hello tabeller från Hive visas automatiskt i Presto.</span><span class="sxs-lookup"><span data-stu-id="fb83e-148">Hive connector is configured toouse hello default installed Hive installation, so all hello tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="fb83e-149">En detaljerad beskrivning av hur du kan använda Presto finns [Presto dokumentationen](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="fb83e-149">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="fb83e-150">Använda Airpal med Presto</span><span class="sxs-lookup"><span data-stu-id="fb83e-150">Use Airpal with Presto</span></span>

<span data-ttu-id="fb83e-151">[Airpal](https://github.com/airbnb/airpal#airpal) är ett gränssnitt för öppen källkod webbaserade frågan för Presto.</span><span class="sxs-lookup"><span data-stu-id="fb83e-151">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="fb83e-152">Mer information om Airpal finns [Airpal dokumentationen](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="fb83e-152">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="fb83e-153">I det här avsnittet ska vi titta på hello steg för**installera Airpal på hello edgenode** i ett HDInsight Hadoop-kluster där det redan finns Presto installerad.</span><span class="sxs-lookup"><span data-stu-id="fb83e-153">In this section, we look at hello steps too**install Airpal on hello edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="fb83e-154">Detta säkerställer att hello Airpal frågan webbgränssnitt är tillgänglig över hello Internet.</span><span class="sxs-lookup"><span data-stu-id="fb83e-154">This ensures that hello Airpal web query interface is available over hello Internet.</span></span>

1. <span data-ttu-id="fb83e-155">Med SSH, Anslut toohello headnode av hello HDInsight-kluster som har installerat Presto:</span><span class="sxs-lookup"><span data-stu-id="fb83e-155">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="fb83e-156">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="fb83e-156">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="fb83e-157">Kör följande kommando hello när du är ansluten.</span><span class="sxs-lookup"><span data-stu-id="fb83e-157">Once you are connected, run hello following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="fb83e-158">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="fb83e-158">You should see an output like hello following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="fb83e-159">Observera hello värde för hello från hello utdata **värdet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="fb83e-159">From hello output, note hello value for hello **value** property.</span></span> <span data-ttu-id="fb83e-160">Du behöver detta när du installerar Airpal på hello klustret edgenode.</span><span class="sxs-lookup"><span data-stu-id="fb83e-160">You will need this while installing Airpal on hello cluster edgenode.</span></span> <span data-ttu-id="fb83e-161">Från hello utdata över hello-värde som du behöver är **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="fb83e-161">From hello output above, hello value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="fb83e-162">Använd hello mall  **[här](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate ett HDInsight-kluster edgenode och ange hello värden som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="fb83e-162">Use hello template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** toocreate an HDInsight cluster edgenode and provide hello values as shown in hello following screenshot.</span></span>

    ![Installera HDInsight Airpal på Presto klustret](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="fb83e-164">Klicka på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="fb83e-164">Click **Purchase**.</span></span>

6. <span data-ttu-id="fb83e-165">När det är tillämpade toohello klusterkonfigurationen hello ändringarna kan du komma åt hello Airpal webbgränssnitt med hjälp av följande hello.</span><span class="sxs-lookup"><span data-stu-id="fb83e-165">Once hello changes are applied toohello cluster configuration, you can access hello Airpal web interface by using hello following steps.</span></span>

    <span data-ttu-id="fb83e-166">a.</span><span class="sxs-lookup"><span data-stu-id="fb83e-166">a.</span></span> <span data-ttu-id="fb83e-167">Hello kluster-bladet, klickar du på **program**.</span><span class="sxs-lookup"><span data-stu-id="fb83e-167">From hello cluster blade, click **Applications**.</span></span>

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="fb83e-169">b.</span><span class="sxs-lookup"><span data-stu-id="fb83e-169">b.</span></span> <span data-ttu-id="fb83e-170">Från hello **installerade appar** bladet, klickar du på **Portal** mot airpal.</span><span class="sxs-lookup"><span data-stu-id="fb83e-170">From hello **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![HDInsight systemstart Airpal Presto kluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="fb83e-172">c.</span><span class="sxs-lookup"><span data-stu-id="fb83e-172">c.</span></span> <span data-ttu-id="fb83e-173">När du uppmanas ange hello admin-autentiseringsuppgifter som du angav när du skapar hello HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="fb83e-173">When prompted, enter hello admin credentials that you specified while creating hello HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="fb83e-174">Anpassa en Presto installation på HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="fb83e-174">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="fb83e-175">När du har installerat Presto på ett HDInsight Hadoop-kluster, kan du anpassa hello installation toomake ändringar, till exempel minne uppdateringsinställningar, ändra kopplingar, osv. Utför följande steg toodo så hello.</span><span class="sxs-lookup"><span data-stu-id="fb83e-175">After you have installed Presto on an HDInsight Hadoop cluster, you can customize hello installation toomake changes such as update memory settings, change connectors, etc. Perform hello following steps toodo so.</span></span>

1. <span data-ttu-id="fb83e-176">Med SSH, Anslut toohello headnode av hello HDInsight-kluster som har installerat Presto:</span><span class="sxs-lookup"><span data-stu-id="fb83e-176">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="fb83e-177">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="fb83e-177">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="fb83e-178">Ändra konfigurationen av hello filen `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="fb83e-178">Make your configuration changes in hello file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="fb83e-179">Mer information om Presto konfiguration finns [Presto konfigurationen för YARN-baserade kluster](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="fb83e-179">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="fb83e-180">Stoppa och avsluta hello aktuell session i Presto.</span><span class="sxs-lookup"><span data-stu-id="fb83e-180">Stop and kill hello current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="fb83e-181">Starta en ny instans av Presto med hello anpassning.</span><span class="sxs-lookup"><span data-stu-id="fb83e-181">Start a new instance of Presto with hello customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="fb83e-182">Vänta tills hello ny instans toobe redo och anteckna presto coordinator adress.</span><span class="sxs-lookup"><span data-stu-id="fb83e-182">Wait for hello new instance toobe ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="fb83e-183">Generera benchmark-data för HDInsight-kluster som kör Presto</span><span class="sxs-lookup"><span data-stu-id="fb83e-183">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="fb83e-184">TPC DS är hello branschstandard för att mäta hello prestanda för många beslut support-system, inklusive system för stordata.</span><span class="sxs-lookup"><span data-stu-id="fb83e-184">TPC-DS is hello industry standard for measuring hello performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="fb83e-185">Du kan använda Presto på HDInsight-kluster toogenerate data och utvärdera hur jämförs med dina egna HDInsight benchmark-data.</span><span class="sxs-lookup"><span data-stu-id="fb83e-185">You can use Presto on HDInsight clusters toogenerate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="fb83e-186">Mer information finns i [här](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-186">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="fb83e-187">Se även</span><span class="sxs-lookup"><span data-stu-id="fb83e-187">See also</span></span>
* <span data-ttu-id="fb83e-188">[Installera och använda Hue på HDInsight-kluster](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-188">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="fb83e-189">Hue är ett webbgränssnitt som gör det enkelt toocreate, köra och spara Pig och Hive-jobb, samt som Bläddra hello standardlagring för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="fb83e-189">Hue is a web UI that makes it easy toocreate, run and save Pig and Hive jobs, as well as browse hello default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="fb83e-190">[Installera Giraph på HDInsight-kluster](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-190">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="fb83e-191">Använd kluster anpassning tooinstall Giraph på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="fb83e-191">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="fb83e-192">Giraph kan du tooperform diagrammet bearbetning med hjälp av Hadoop och kan användas med Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="fb83e-192">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="fb83e-193">[Installera Solr på HDInsight-kluster](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fb83e-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="fb83e-194">Använd kluster anpassning tooinstall Solr på HDInsight Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="fb83e-194">Use cluster customization tooinstall Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="fb83e-195">Solr kan tooperform kraftfulla sökningar på lagrade data.</span><span class="sxs-lookup"><span data-stu-id="fb83e-195">Solr allows you tooperform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
