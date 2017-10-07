---
title: "aaaStorm-startexempel som finns på Apache Storm på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toodo stordata och bearbeta data i realtid med Apache Storm och hello storm starter-exempel i HDInsight."
keywords: storm-starter apache storm-exempel
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a><span data-ttu-id="949b7-104">Kom igång med Apache Storm på HDInsight med hello storm starter-exempel</span><span class="sxs-lookup"><span data-stu-id="949b7-104">Get started with Apache Storm on HDInsight using hello storm-starter examples</span></span>

<span data-ttu-id="949b7-105">Lär dig hur toouse Apache Storm i HDInsight med hello storm starter-exempel.</span><span class="sxs-lookup"><span data-stu-id="949b7-105">Learn how toouse Apache Storm in HDInsight using hello storm-starter examples.</span></span>

<span data-ttu-id="949b7-106">Apache Storm är ett skalbart, feltolerant och distribuerat system för beräkningar i realtid för bearbetning av dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="949b7-106">Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data.</span></span> <span data-ttu-id="949b7-107">Du kan skapa ett molnbaserat Storm-kluster som utför analyser av stordata i realtid med Storm på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="949b7-107">With Storm on Azure HDInsight, you can create a cloud-based Storm cluster that performs big data analytics in real time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="949b7-108">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="949b7-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="949b7-109">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="949b7-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="949b7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="949b7-110">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="949b7-111">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="949b7-111">**An Azure subscription**.</span></span> <span data-ttu-id="949b7-112">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="949b7-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="949b7-113">**kunskap om SSH och SCP**.</span><span class="sxs-lookup"><span data-stu-id="949b7-113">**Familiarity with SSH and SCP**.</span></span> <span data-ttu-id="949b7-114">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="949b7-114">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-a-storm-cluster"></a><span data-ttu-id="949b7-115">Skapa ett Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="949b7-115">Create a Storm cluster</span></span>

<span data-ttu-id="949b7-116">Använd hello följa steg toocreate ett Storm på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="949b7-116">Use hello following steps toocreate a Storm on HDInsight cluster:</span></span>

1. <span data-ttu-id="949b7-117">Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny**, **Intelligence + analys**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="949b7-117">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>

    ![Skapa ett HDInsight-kluster](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. <span data-ttu-id="949b7-119">Från hello **grunderna** bladet ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="949b7-119">From hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="949b7-120">**Klusternamn**: hello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="949b7-120">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="949b7-121">**Prenumerationen**: Välj hello prenumeration toouse.</span><span class="sxs-lookup"><span data-stu-id="949b7-121">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="949b7-122">**Klustret inloggning användarnamn** och **klustret inloggningslösenordet**: hello inloggning vid åtkomst till hello klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="949b7-122">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="949b7-123">Du använder dessa autentiseringsuppgifter tooaccess tjänster, till exempel hello Ambari-Webbgränssnittet eller REST API.</span><span class="sxs-lookup"><span data-stu-id="949b7-123">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="949b7-124">**Secure Shell (SSH) användarnamn**: hello-inloggning som används vid åtkomst till hello klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="949b7-124">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="949b7-125">Som standard är hello lösenord hello samma som hello inloggningslösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="949b7-125">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="949b7-126">**Resursgruppen**: hello resurs grupp toocreate hello klustret i.</span><span class="sxs-lookup"><span data-stu-id="949b7-126">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="949b7-127">**Plats**: hello Azure-region toocreate hello klustret i.</span><span class="sxs-lookup"><span data-stu-id="949b7-127">**Location**: hello Azure region toocreate hello cluster in.</span></span>

    ![Välj en prenumeration](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. <span data-ttu-id="949b7-129">Välj **kluster typen**, och sedan ange hello följa värden på hello **klusterkonfigurationen** bladet:</span><span class="sxs-lookup"><span data-stu-id="949b7-129">Select **Cluster type**, and then set hello following values on hello **Cluster configuration** blade:</span></span>

    * <span data-ttu-id="949b7-130">**Typ av kluster**: Storm</span><span class="sxs-lookup"><span data-stu-id="949b7-130">**Cluster Type**: Storm</span></span>

    * <span data-ttu-id="949b7-131">**Operativsystem**: Linux</span><span class="sxs-lookup"><span data-stu-id="949b7-131">**Operating system**: Linux</span></span>

    * <span data-ttu-id="949b7-132">**Version**: Storm 1.1.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="949b7-132">**Version**: Storm 1.1.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="949b7-133">**Klusternivå**: Standard</span><span class="sxs-lookup"><span data-stu-id="949b7-133">**Cluster Tier**: Standard</span></span>

    <span data-ttu-id="949b7-134">Använd slutligen hello **Välj** knappen toosave inställningar.</span><span class="sxs-lookup"><span data-stu-id="949b7-134">Finally, use hello **Select** button toosave settings.</span></span>

    ![Välj klustertyp](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="949b7-136">När du har valt hello typ av kluster, använda hello __Välj__ knappen tooset hello typ av kluster.</span><span class="sxs-lookup"><span data-stu-id="949b7-136">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="949b7-137">Använd sedan hello __nästa__ toofinish grundläggande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="949b7-137">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="949b7-138">Från hello **lagring** bladet Välj eller skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="949b7-138">From hello **Storage** blade, select or create a Storage account.</span></span> <span data-ttu-id="949b7-139">Lämna hello andra fält för hello stegen i det här dokumentet, på det här bladet på hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="949b7-139">For hello steps in this document, leave hello other fields on this blade at hello default values.</span></span> <span data-ttu-id="949b7-140">Använd hello __nästa__ toosave konfiguration för lagring.</span><span class="sxs-lookup"><span data-stu-id="949b7-140">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Ange hello konto lagringsinställningarna för HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. <span data-ttu-id="949b7-142">Från hello **sammanfattning** bladet granska hello konfigurationen för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="949b7-142">From hello **Summary** blade, review hello configuration for hello cluster.</span></span> <span data-ttu-id="949b7-143">Använd hello __redigera__ länkar toochange inställningar som är felaktiga.</span><span class="sxs-lookup"><span data-stu-id="949b7-143">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="949b7-144">Slutligen Använd the__Create__ knappen toocreate hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="949b7-144">Finally, use the__Create__ button toocreate hello cluster.</span></span>

    ![Sammanfattning av klusterkonfiguration](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > <span data-ttu-id="949b7-146">Det kan ta upp too20 minuter toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="949b7-146">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="run-a-storm-starter-sample-on-hdinsight"></a><span data-ttu-id="949b7-147">Köra ett Storm Starter-exempel i HDInsight</span><span class="sxs-lookup"><span data-stu-id="949b7-147">Run a storm-starter sample on HDInsight</span></span>

1. <span data-ttu-id="949b7-148">Anslut toohello HDInsight-kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="949b7-148">Connect toohello HDInsight cluster using SSH:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="949b7-149">Om du har använt ett lösenord toosecure SSH-användarkontot, är du tillfrågas tooenter den.</span><span class="sxs-lookup"><span data-stu-id="949b7-149">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="949b7-150">Om du använder en offentlig nyckel, du måste använda hello `-i` parametern toospecify hello motsvarande privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="949b7-150">If you used a public key, you may need use hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="949b7-151">Till exempel `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="949b7-151">For example, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

    <span data-ttu-id="949b7-152">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="949b7-152">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="949b7-153">Använd följande kommando toostart en exempeltopologi hello:</span><span class="sxs-lookup"><span data-stu-id="949b7-153">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > <span data-ttu-id="949b7-154">I tidigare versioner av HDInsight hello klassnamnet för hello topologin är `storm.starter.WordCountTopology` i stället för `org.apache.storm.starter.WordCountTopology`.</span><span class="sxs-lookup"><span data-stu-id="949b7-154">On previous versions of HDInsight, hello class name of hello topology is `storm.starter.WordCountTopology` instead of `org.apache.storm.starter.WordCountTopology`.</span></span>

    <span data-ttu-id="949b7-155">Detta kommando startar hello exempel WordCount-topologi på hello-kluster med ett eget namn för 'wordcount'.</span><span class="sxs-lookup"><span data-stu-id="949b7-155">This command starts hello example WordCount topology on hello cluster, with a friendly name of 'wordcount'.</span></span> <span data-ttu-id="949b7-156">Den genererar slumpmässigt meningar och antal hello förekomsten av varje ord i hello meningar.</span><span class="sxs-lookup"><span data-stu-id="949b7-156">It randomly generates sentences and count hello occurrence of each word in hello sentences.</span></span>

    > [!NOTE]
    > <span data-ttu-id="949b7-157">När ditt eget topologier toohello kluster, måste du först kopiera hello jar-filen som innehåller hello klustret innan du använder hello `storm` kommando.</span><span class="sxs-lookup"><span data-stu-id="949b7-157">When submitting your own topologies toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="949b7-158">Använd hello `scp` kommandofilen toocopy hello.</span><span class="sxs-lookup"><span data-stu-id="949b7-158">Use hello `scp` command toocopy hello file.</span></span> <span data-ttu-id="949b7-159">Till exempel, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="949b7-159">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
    >
    > <span data-ttu-id="949b7-160">Hej WordCount-exemplet och andra storm starter-exempel finns redan i klustret på `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="949b7-160">hello WordCount example, and other storm-starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

<span data-ttu-id="949b7-161">Om du vill visa hello källa för hello storm-startexempel som finns hittar du hello koden i [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span><span class="sxs-lookup"><span data-stu-id="949b7-161">If you are interested in viewing hello source for hello storm-starter examples, you can find hello code at [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span></span> <span data-ttu-id="949b7-162">Länken är till Storm 1.1.x, som medföljer HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="949b7-162">This link is for Storm 1.1.x, which is provided with HDInsight 3.6.</span></span> <span data-ttu-id="949b7-163">För andra versioner av Storm använda hello __gren__ knappen hello överst i hello sidan tooselect en annan Storm-version.</span><span class="sxs-lookup"><span data-stu-id="949b7-163">For other versions of Storm, use hello __Branch__ button at hello top of hello page tooselect a different Storm version.</span></span>

## <a name="monitor-hello-topology"></a><span data-ttu-id="949b7-164">Övervakaren hello-topologi</span><span class="sxs-lookup"><span data-stu-id="949b7-164">Monitor hello topology</span></span>

<span data-ttu-id="949b7-165">hello Storm-Användargränssnittet innehåller ett webbgränssnitt för att arbeta med topologier som körs och ingår i ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="949b7-165">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span>

<span data-ttu-id="949b7-166">Använd följande steg toomonitor hello topologi med hello Storm-Användargränssnittet hello:</span><span class="sxs-lookup"><span data-stu-id="949b7-166">Use hello following steps toomonitor hello topology using hello Storm UI:</span></span>

1. <span data-ttu-id="949b7-167">toodisplay hello Storm-Användargränssnittet, öppna en webbläsare web-toohttps://CLUSTERNAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="949b7-167">toodisplay hello Storm UI, open a web browser toohttps://CLUSTERNAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="949b7-168">Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="949b7-168">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="949b7-169">Om du blir tillfrågad tooprovide ett användarnamn och lösenord, ange hello Klusteradministratören (admin) och lösenord som du använde när du skapar hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="949b7-169">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

2. <span data-ttu-id="949b7-170">Under **Topology summary**väljer hello **wordcount** post i hello **namn** kolumn.</span><span class="sxs-lookup"><span data-stu-id="949b7-170">Under **Topology summary**, select hello **wordcount** entry in hello **Name** column.</span></span> <span data-ttu-id="949b7-171">Information om hello topologin visas.</span><span class="sxs-lookup"><span data-stu-id="949b7-171">Information about hello topology is displayed.</span></span>

    ![Storm Dashboard med storm-starter, topologisk information om WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    <span data-ttu-id="949b7-173">Den här sidan innehåller hello följande information:</span><span class="sxs-lookup"><span data-stu-id="949b7-173">This page provides hello following information:</span></span>

    * <span data-ttu-id="949b7-174">**Topology stats** – grundläggande information om hello topologins prestanda ordnad i tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="949b7-174">**Topology stats** - Basic information on hello topology performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="949b7-175">Att välja en viss tid fönstret ändringar hello tidsfönstret för information som visas i andra avsnitt i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="949b7-175">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="949b7-176">**Spouts** – grundläggande information om kanaler, inklusive hello senaste fel som returnerats för varje kanal.</span><span class="sxs-lookup"><span data-stu-id="949b7-176">**Spouts** - Basic information about spouts, including hello last error returned by each spout.</span></span>

    * <span data-ttu-id="949b7-177">**Bolts** (Bultar) – grundläggande information om bultar.</span><span class="sxs-lookup"><span data-stu-id="949b7-177">**Bolts** - Basic information about bolts.</span></span>

    * <span data-ttu-id="949b7-178">**Topologi configuration** -detaljerad information om hello topologins konfiguration.</span><span class="sxs-lookup"><span data-stu-id="949b7-178">**Topology configuration** - Detailed information about hello topology configuration.</span></span>

    <span data-ttu-id="949b7-179">Den här sidan innehåller också åtgärder som kan göras på topologin hello:</span><span class="sxs-lookup"><span data-stu-id="949b7-179">This page also provides actions that can be taken on hello topology:</span></span>

    * <span data-ttu-id="949b7-180">**Activate** (Aktivera) – återupptar bearbetningen av en inaktiverad topologi.</span><span class="sxs-lookup"><span data-stu-id="949b7-180">**Activate** - Resumes processing of a deactivated topology.</span></span>

    * <span data-ttu-id="949b7-181">**Deactivate**  (Inaktivera) – pausar en topologi som körs.</span><span class="sxs-lookup"><span data-stu-id="949b7-181">**Deactivate** - Pauses a running topology.</span></span>

    * <span data-ttu-id="949b7-182">**Balansera** -justerar hello parallellitet hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="949b7-182">**Rebalance** - Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="949b7-183">Du bör balansera om topologier som körs när du har ändrat hello antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="949b7-183">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="949b7-184">Ombalansering justerar parallellitet toocompensate för hello ökade/minskade antalet noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="949b7-184">Rebalancing adjusts parallelism toocompensate for hello increased/decreased number of nodes in hello cluster.</span></span> <span data-ttu-id="949b7-185">Mer information finns i [förstå hello parallellitet i en Storm-topologi](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="949b7-185">For more information, see [Understanding hello parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

    * <span data-ttu-id="949b7-186">**Avsluta** -avslutar en Storm-topologi efter hello angivna timeout.</span><span class="sxs-lookup"><span data-stu-id="949b7-186">**Kill** - Terminates a Storm topology after hello specified timeout.</span></span>

3. <span data-ttu-id="949b7-187">Den här sidan väljer du en post från hello **Spouts** eller **Bolts** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="949b7-187">From this page, select an entry from hello **Spouts** or **Bolts** section.</span></span> <span data-ttu-id="949b7-188">Information om hello valda komponenten visas.</span><span class="sxs-lookup"><span data-stu-id="949b7-188">Information about hello selected component is displayed.</span></span>

    ![Storm-instrumentpanelen med information om valda komponenter.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    <span data-ttu-id="949b7-190">Den här sidan visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="949b7-190">This page displays hello following information:</span></span>

    * <span data-ttu-id="949b7-191">**Kanal/bult stats** – grundläggande information om hello komponentens prestanda, ordnad i tidsfönster.</span><span class="sxs-lookup"><span data-stu-id="949b7-191">**Spout/Bolt stats** - Basic information on hello component performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="949b7-192">Att välja en viss tid fönstret ändringar hello tidsfönstret för information som visas i andra avsnitt i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="949b7-192">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="949b7-193">**Input stats** (endast bultar) – Information om komponenter som producerar de data som används av hello bulten.</span><span class="sxs-lookup"><span data-stu-id="949b7-193">**Input stats** (bolt only) - Information on components that produce data consumed by hello bolt.</span></span>

    * <span data-ttu-id="949b7-194">**Output stats** (Statistik för utdata) – information om data som sänds av bulten.</span><span class="sxs-lookup"><span data-stu-id="949b7-194">**Output stats** - Information on data emitted by this bolt.</span></span>

    * <span data-ttu-id="949b7-195">**Executors** (Utförare) – information om komponentens instanser.</span><span class="sxs-lookup"><span data-stu-id="949b7-195">**Executors** - Information on instances of this component.</span></span>

    * <span data-ttu-id="949b7-196">**Errors** (Fel) – fel som komponenten ger upphov till.</span><span class="sxs-lookup"><span data-stu-id="949b7-196">**Errors** - Errors produced by this component.</span></span>

4. <span data-ttu-id="949b7-197">När du visar hello information om en kanal eller en bult väljer du en post från hello **Port** kolumn i hello **Executors** avsnittet tooview information för en specifik instans av hello-komponent.</span><span class="sxs-lookup"><span data-stu-id="949b7-197">When viewing hello details of a spout or bolt, select an entry from hello **Port** column in hello **Executors** section tooview details for a specific instance of hello component.</span></span>

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    <span data-ttu-id="949b7-198">I det här exemplet hello word **sju** uppstod 1493957 gånger.</span><span class="sxs-lookup"><span data-stu-id="949b7-198">In this example, hello word **seven** has occurred 1493957 times.</span></span> <span data-ttu-id="949b7-199">Det här antalet är hur många gånger hello ordet har påträffats sedan topologin startades.</span><span class="sxs-lookup"><span data-stu-id="949b7-199">This count is how many times hello word has been encountered since this topology was started.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="949b7-200">Stoppa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="949b7-200">Stop hello topology</span></span>

<span data-ttu-id="949b7-201">Returnera toohello **Topology summary** för hello ordräkningstopologin och välj sedan hello **Kill** knappen från hello **Topology actions** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="949b7-201">Return toohello **Topology summary** page for hello word-count topology, and then select hello **Kill** button from hello **Topology actions** section.</span></span> <span data-ttu-id="949b7-202">När du uppmanas, anger du 10 för hello sekunder toowait innan du stoppar hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="949b7-202">When prompted, enter 10 for hello seconds toowait before stopping hello topology.</span></span> <span data-ttu-id="949b7-203">Efter hello tidsgränsen hello topologin inte längre när du besöker hello **Storm-Användargränssnittet** avsnittet hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="949b7-203">After hello timeout period, hello topology no longer appears when you visit hello **Storm UI** section of hello dashboard.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="949b7-204">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="949b7-204">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="949b7-205">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="949b7-205">If you run into an issue with creating HDInsight cluster, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <span data-ttu-id="949b7-206"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="949b7-206"><a id="next"></a>Next steps</span></span>

<span data-ttu-id="949b7-207">I den här Apache Storm-kurs berättat hello grunderna i att arbeta med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="949b7-207">In this Apache Storm tutorial, you learned hello basics of working with Storm on HDInsight.</span></span> <span data-ttu-id="949b7-208">Lär dig sedan hur för[utveckla Java-baserade topologier med Maven](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="949b7-208">Next, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="949b7-209">Om du redan är bekant med att utveckla Java-baserad topologier och vill toodeploy en befintlig topologi tooHDInsight finns [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span><span class="sxs-lookup"><span data-stu-id="949b7-209">If you're already familiar with developing Java-based topologies and want toodeploy an existing topology tooHDInsight, see [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span></span>

<span data-ttu-id="949b7-210">Om du är en .NET-utvecklare kan du skapa C#- eller C#/Java-hybridtopologier med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="949b7-210">If you are a .NET developer, you can create C# or hybrid C#/Java topologies using Visual Studio.</span></span> <span data-ttu-id="949b7-211">Mer information finns i [Utveckla C#-topologier för Apache Storm på HDInsight med Hadoop-verktyg för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="949b7-211">For more information, see [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="949b7-212">Till exempel topologier som kan användas med Storm på HDInsight, se följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="949b7-212">For example topologies that can be used with Storm on HDInsight, see hello following examples:</span></span>

* [<span data-ttu-id="949b7-213">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="949b7-213">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
