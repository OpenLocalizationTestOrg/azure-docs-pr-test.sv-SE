---
title: "aaaAzure Toolkit för IntelliJ - Debug-program på HDInsight Spark | Microsoft Docs"
description: "Lär dig hur använda HDInsight Tools i Azure Toolkit för IntelliJ tooremotely debug program som körs på HDInsight Spark-kluster via vpn."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 55fb454f-c7dc-46de-a978-e242e9a94f4c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ad67d23bf609d0a7afb38b2acb110062f8b27b39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toodebug-applications-remotely-on-hdinsight-spark-through-vpn"></a><span data-ttu-id="5daa4-103">Använd Azure Toolkit för IntelliJ toodebug program på HDInsight Spark via VPN-anslutning</span><span class="sxs-lookup"><span data-stu-id="5daa4-103">Use Azure Toolkit for IntelliJ toodebug applications remotely on HDInsight Spark through VPN</span></span>

<span data-ttu-id="5daa4-104">Vi rekommenderar hello sätt att felsöka spark applicaltion via fjärranslutning via ssh.</span><span class="sxs-lookup"><span data-stu-id="5daa4-104">We recommend hello way of debugging spark applicaltion remotely through ssh.</span></span> <span data-ttu-id="5daa4-105">Instruktioner finns i [felsöka Spark-program på ett HDInsight-kluster med Azure Toolkit för IntelliJ via SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="5daa4-105">For instructions, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

<span data-ttu-id="5daa4-106">Den här artikeln innehåller stegvisa anvisningar om hur toouse hello HDInsight-verktyg i Azure Toolkit för IntelliJ toosubmit ett Spark-jobb på HDInsight Spark-kluster och felsöka den via fjärranslutning från din dator.</span><span class="sxs-lookup"><span data-stu-id="5daa4-106">This article provides step-by-step guidance on how toouse hello HDInsight Tools in Azure Toolkit for IntelliJ toosubmit a Spark job on HDInsight Spark cluster and then debug it remotely from your desktop computer.</span></span> <span data-ttu-id="5daa4-107">toodo så du måste utföra hello följande anvisningar:</span><span class="sxs-lookup"><span data-stu-id="5daa4-107">toodo so, you must perform hello following high-level steps:</span></span>

1. <span data-ttu-id="5daa4-108">Skapa en plats-till-plats eller en punkt-till-plats Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="5daa4-108">Create a site-to-site or point-to-site Azure Virtual Network.</span></span> <span data-ttu-id="5daa4-109">hello stegen i det här dokumentet förutsätter att du använder ett plats-till-plats-nätverk.</span><span class="sxs-lookup"><span data-stu-id="5daa4-109">hello steps in this document assume that you use a site-to-site network.</span></span>
2. <span data-ttu-id="5daa4-110">Skapa ett Spark-kluster i Azure HDInsight som ingår i hello plats-till-plats Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="5daa4-110">Create a Spark cluster in Azure HDInsight that is part of hello site-to-site Azure Virtual Network.</span></span>
3. <span data-ttu-id="5daa4-111">Kontrollera hello anslutningen mellan hello klustret headnode och ditt skrivbord.</span><span class="sxs-lookup"><span data-stu-id="5daa4-111">Verify hello connectivity between hello cluster headnode and your desktop.</span></span>
4. <span data-ttu-id="5daa4-112">Skapa en Scala-program i IntelliJ IDEA och konfigurera den för fjärrfelsökning.</span><span class="sxs-lookup"><span data-stu-id="5daa4-112">Create a Scala application in IntelliJ IDEA and configure it for remote debugging.</span></span>
5. <span data-ttu-id="5daa4-113">Kör och felsöka hello program.</span><span class="sxs-lookup"><span data-stu-id="5daa4-113">Run and debug hello application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5daa4-114">Krav</span><span class="sxs-lookup"><span data-stu-id="5daa4-114">Prerequisites</span></span>
* <span data-ttu-id="5daa4-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5daa4-115">An Azure subscription.</span></span> <span data-ttu-id="5daa4-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="5daa4-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="5daa4-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5daa4-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="5daa4-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="5daa4-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="5daa4-119">Oracle Java Development kit.</span><span class="sxs-lookup"><span data-stu-id="5daa4-119">Oracle Java Development kit.</span></span> <span data-ttu-id="5daa4-120">Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="5daa4-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="5daa4-121">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="5daa4-121">IntelliJ IDEA.</span></span> <span data-ttu-id="5daa4-122">Den här artikeln använder version 2017.1.</span><span class="sxs-lookup"><span data-stu-id="5daa4-122">This article uses version 2017.1.</span></span> <span data-ttu-id="5daa4-123">Du kan installera den från [här](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="5daa4-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>
* <span data-ttu-id="5daa4-124">HDInsight-verktyg i Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="5daa4-124">HDInsight Tools in Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="5daa4-125">HDInsight-verktyg för IntelliJ är tillgängliga som en del av hello Azure Toolkit för IntelliJ.</span><span class="sxs-lookup"><span data-stu-id="5daa4-125">HDInsight tools for IntelliJ are available as part of hello Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="5daa4-126">Anvisningar för hur tooinstall hello Azure Toolkit finns [installerar hello Azure Toolkit för IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="5daa4-126">For instructions on how tooinstall hello Azure Toolkit, see [Installing hello Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>
* <span data-ttu-id="5daa4-127">Logga in på Azure-prenumerationen från IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="5daa4-127">Log into your Azure Subscription from IntelliJ IDEA.</span></span> <span data-ttu-id="5daa4-128">Följ instruktionerna för hello [här](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="5daa4-128">Follow hello instructions [here](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
* <span data-ttu-id="5daa4-129">När du kör Spark Scala program för fjärrfelsökning i en Windows-dator, kan du få ett undantag som beskrivs i [SPARK 2356](https://issues.apache.org/jira/browse/SPARK-2356) som uppstår på grund av tooa saknas WinUtils.exe i Windows.</span><span class="sxs-lookup"><span data-stu-id="5daa4-129">While running Spark Scala application for remote debugging on a Windows computer, you might get an exception as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356) that occurs due tooa missing WinUtils.exe on Windows.</span></span> <span data-ttu-id="5daa4-130">toowork runt det här felet måste du [hämta hello körbara härifrån](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa plats som **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-130">toowork around this error, you must [download hello executable from here](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa location like **C:\WinUtils\bin**.</span></span> <span data-ttu-id="5daa4-131">Sedan måste du lägga till en miljövariabel **HADOOP_HOME** och ange hello hello variabels värde för**C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-131">You must then add an environment variable **HADOOP_HOME** and set hello value of hello variable too**C\WinUtils**.</span></span>

## <a name="step-1-create-an-azure-virtual-network"></a><span data-ttu-id="5daa4-132">Steg 1: Skapa ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="5daa4-132">Step 1: Create an Azure Virtual Network</span></span>
<span data-ttu-id="5daa4-133">Följ anvisningarna för hello från hello nedan länkar toocreate ett Azure Virtual Network och kontrollera hello anslutningen mellan hello fjärrskrivbord och Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="5daa4-133">Follow hello instructions from hello below links toocreate an Azure Virtual Network and then verify hello connectivity between hello desktop and Azure Virtual Network.</span></span>

* [<span data-ttu-id="5daa4-134">Skapa ett VNet med en plats-till-plats VPN-anslutning med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5daa4-134">Create a VNet with a site-to-site VPN connection using Azure Portal</span></span>](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [<span data-ttu-id="5daa4-135">Skapa ett VNet med en plats-till-plats VPN-anslutning med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5daa4-135">Create a VNet with a site-to-site VPN connection using PowerShell</span></span>](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [<span data-ttu-id="5daa4-136">Konfigurera en punkt-till-plats-anslutning tooa virtuellt nätverk med PowerShell</span><span class="sxs-lookup"><span data-stu-id="5daa4-136">Configure a point-to-site connection tooa virtual network using PowerShell</span></span>](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

## <a name="step-2-create-an-hdinsight-spark-cluster"></a><span data-ttu-id="5daa4-137">Steg 2: Skapa ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="5daa4-137">Step 2: Create an HDInsight Spark cluster</span></span>
<span data-ttu-id="5daa4-138">Du bör också skapa ett Apache Spark-kluster i Azure HDInsight som ingår i hello Azure-nätverk som du skapade.</span><span class="sxs-lookup"><span data-stu-id="5daa4-138">You should also create an Apache Spark cluster on Azure HDInsight that is part of hello Azure Virtual Network that you created.</span></span> <span data-ttu-id="5daa4-139">Använd hello information i [skapa Linux-baserade kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="5daa4-139">Use hello information available at [Create Linux-based clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span> <span data-ttu-id="5daa4-140">Välj hello Azure-nätverk som du skapade i föregående steg i hello som del av valfri konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5daa4-140">As part of optional configuration, select hello Azure Virtual Network that you created in hello previous step.</span></span>

## <a name="step-3-verify-hello-connectivity-between-hello-cluster-headnode-and-your-desktop"></a><span data-ttu-id="5daa4-141">Steg 3: Kontrollera hello anslutningen mellan hello klustret headnode och skrivbordet</span><span class="sxs-lookup"><span data-stu-id="5daa4-141">Step 3: Verify hello connectivity between hello cluster headnode and your desktop</span></span>
1. <span data-ttu-id="5daa4-142">Hämta hello hello headnode IP-adress.</span><span class="sxs-lookup"><span data-stu-id="5daa4-142">Get hello IP address of hello headnode.</span></span> <span data-ttu-id="5daa4-143">Öppna Ambari UI för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="5daa4-143">Open Ambari UI for hello cluster.</span></span> <span data-ttu-id="5daa4-144">Hello kluster-bladet, klickar du på **instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-144">From hello cluster blade, click **Dashboard**.</span></span>

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/launch-ambari-ui.png)
2. <span data-ttu-id="5daa4-146">Hej Ambari UI från hello övre högra hörnet och klicka på **värdar**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-146">From hello Ambari UI, from hello top-right corner, click **Hosts**.</span></span>

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/ambari-hosts.png)
3. <span data-ttu-id="5daa4-148">Du bör se en lista över headnodes, arbetarnoder och zookeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="5daa4-148">You should see a list of headnodes, worker nodes, and zookeeper nodes.</span></span> <span data-ttu-id="5daa4-149">Hej headnodes har hello **hn*** prefix.</span><span class="sxs-lookup"><span data-stu-id="5daa4-149">hello headnodes have hello **hn*** prefix.</span></span> <span data-ttu-id="5daa4-150">Klicka på hello första headnode.</span><span class="sxs-lookup"><span data-stu-id="5daa4-150">Click hello first headnode.</span></span>

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/cluster-headnodes.png)
4. <span data-ttu-id="5daa4-152">Längst ned hello hello-sidan som öppnas från hello **sammanfattning** rutan Kopiera hello IP-adress hello headnode och hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="5daa4-152">At hello bottom of hello page that opens, from hello **Summary** box, copy hello IP address of hello headnode and hello host name.</span></span>

    ![Hitta headnode IP](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/headnode-ip-address.png)
5. <span data-ttu-id="5daa4-154">Inkludera hello IP-adress och hello värdnamn för hello headnode toohello **värdar** fil på hello dator från där du vill toorun och felsöka hello Spark jobb.</span><span class="sxs-lookup"><span data-stu-id="5daa4-154">Include hello IP address and hello host name of hello headnode toohello **hosts** file on hello computer from where you want toorun and remotely debug hello Spark jobs.</span></span> <span data-ttu-id="5daa4-155">Detta gör att du toocommunicate med hello headnode med hjälp av hello IP-adress samt hello värdnamn.</span><span class="sxs-lookup"><span data-stu-id="5daa4-155">This will enable you toocommunicate with hello headnode using hello IP address as well as hello hostname.</span></span>

   1. <span data-ttu-id="5daa4-156">Öppna en anteckningar med förhöjda behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5daa4-156">Open a notepad with elevated permissions.</span></span> <span data-ttu-id="5daa4-157">Hello Arkiv-menyn klickar du på **öppna** och gå sedan toohello platsen för hello hosts-filen.</span><span class="sxs-lookup"><span data-stu-id="5daa4-157">From hello file menu, click **Open** and then navigate toohello location of hello hosts file.</span></span> <span data-ttu-id="5daa4-158">På en Windows-dator är det `C:\Windows\System32\Drivers\etc\hosts`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-158">On a Windows computer, it is `C:\Windows\System32\Drivers\etc\hosts`.</span></span>
   2. <span data-ttu-id="5daa4-159">Lägg till följande toohello hello **värdar** fil.</span><span class="sxs-lookup"><span data-stu-id="5daa4-159">Add hello following toohello **hosts** file.</span></span>

           # For headnode0
           192.xxx.xx.xx hn0-nitinp
           192.xxx.xx.xx hn0-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net

           # For headnode1
           192.xxx.xx.xx hn1-nitinp
           192.xxx.xx.xx hn1-nitinp.lhwwghjkpqejawpqbwcdyp3.gx.internal.cloudapp.net
6. <span data-ttu-id="5daa4-160">Kontrollera att du kan pinga båda hello headnodes med hjälp av hello IP-adress samt hello hostname från hello-dator som du har anslutit toohello virtuella Azure-nätverk som används av hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="5daa4-160">From hello computer that you connected toohello Azure Virtual Network that is used by hello HDInsight cluster, verify that you can ping both hello headnodes using hello IP address as well as hello hostname.</span></span>
7. <span data-ttu-id="5daa4-161">SSH i hello klustret headnode använder hello instruktioner på [Anslut tooan HDInsight-kluster med SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5daa4-161">SSH into hello cluster headnode using hello instructions at [Connect tooan HDInsight cluster using SSH](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span> <span data-ttu-id="5daa4-162">Pinga hello IP-adressen för hello stationär dator från hello klustret headnode.</span><span class="sxs-lookup"><span data-stu-id="5daa4-162">From hello cluster headnode, ping hello IP address of hello desktop computer.</span></span> <span data-ttu-id="5daa4-163">Du bör testa anslutning tooboth hello IP-adresser tilldelas toohello dator, en för hello nätverksanslutning och hello andra för hello Azure Virtual Network som hello datorn är ansluten till.</span><span class="sxs-lookup"><span data-stu-id="5daa4-163">You should test connectivity tooboth hello IP addresses assigned toohello computer, one for hello network connection and hello other for hello Azure Virtual Network that hello computer is connected to.</span></span>
8. <span data-ttu-id="5daa4-164">Upprepa hello steg för hello samt andra headnode.</span><span class="sxs-lookup"><span data-stu-id="5daa4-164">Repeat hello steps for hello other headnode as well.</span></span>

## <a name="step-4-create-a-spark-scala-application-using-hello-hdinsight-tools-in-azure-toolkit-for-intellij-and-configure-it-for-remote-debugging"></a><span data-ttu-id="5daa4-165">Steg 4: Skapa ett Spark Scala-program med Azure Toolkit hello HDInsight-verktyg för IntelliJ och konfigurera den för fjärrfelsökning</span><span class="sxs-lookup"><span data-stu-id="5daa4-165">Step 4: Create a Spark Scala application using hello HDInsight Tools in Azure Toolkit for IntelliJ and configure it for remote debugging</span></span>
1. <span data-ttu-id="5daa4-166">Öppna IntelliJ IDEA och skapa ett nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="5daa4-166">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="5daa4-167">Kontrollera hello följande alternativ och klickar sedan på i hello dialogrutan Nytt projekt, **nästa**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-167">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>

    ![Skapa Spark Scala-program](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-hdi-scala-app.png)

   * <span data-ttu-id="5daa4-169">Hello till vänster och välj **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-169">From hello left pane, select **HDInsight**.</span></span>
   * <span data-ttu-id="5daa4-170">Hello högra rutan, Välj **Spark i HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-170">From hello right pane, select **Spark on HDInsight (Scala)**.</span></span>
   * <span data-ttu-id="5daa4-171">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-171">Click **Next**.</span></span>
2. <span data-ttu-id="5daa4-172">I nästa hello-fönstret hello följande projektinformation, och klickar sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-172">In hello next window, provide hello following project details, and then click **Finish**.</span></span>  
   - <span data-ttu-id="5daa4-173">Ange ett projektnamn och projektets plats.</span><span class="sxs-lookup"><span data-stu-id="5daa4-173">Provide a project name and project location.</span></span>
   - <span data-ttu-id="5daa4-174">För **projekt SDK**, Använd Java 1.8 för spark-kluster för 2.x, Java 1.7 för 1.x spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="5daa4-174">For **Project SDK**, Use Java 1.8 for spark 2.x cluster, Java 1.7 for spark 1.x cluster.</span></span>
   - <span data-ttu-id="5daa4-175">För **Spark Version**, Scala guide för att skapa projektet integreras rätt version för Spark SDK och Scala SDK.</span><span class="sxs-lookup"><span data-stu-id="5daa4-175">For **Spark Version**, Scala project creation wizard integrates proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="5daa4-176">Om hello spark-kluster av version är lägre 2.0 väljer Väck 1.x.</span><span class="sxs-lookup"><span data-stu-id="5daa4-176">If hello spark cluster version is lower 2.0, choose spark 1.x.</span></span> <span data-ttu-id="5daa4-177">I annat fall bör du välja spark2.x.</span><span class="sxs-lookup"><span data-stu-id="5daa4-177">Otherwise, you should select spark2.x.</span></span> <span data-ttu-id="5daa4-178">Det här exemplet använder Spark2.0.2 (Scala 2.11.8).</span><span class="sxs-lookup"><span data-stu-id="5daa4-178">This example uses Spark2.0.2(Scala 2.11.8).</span></span>
       <span data-ttu-id="5daa4-179">![Skapa Spark Scala-program](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span><span class="sxs-lookup"><span data-stu-id="5daa4-179">![Create Spark Scala application](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-scala-project-details.png)</span></span>
  
3. <span data-ttu-id="5daa4-180">hello Spark projekt skapar automatiskt en artefakt åt dig.</span><span class="sxs-lookup"><span data-stu-id="5daa4-180">hello Spark project will automatically create an artifact for you.</span></span> <span data-ttu-id="5daa4-181">toosee hello artefakt, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="5daa4-181">toosee hello artifact, follow these steps.</span></span>

   1. <span data-ttu-id="5daa4-182">Från hello **filen** -menyn klickar du på **projektstruktur**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-182">From hello **File** menu, click **Project Structure**.</span></span>
   2. <span data-ttu-id="5daa4-183">I hello **projektstruktur** dialogrutan klickar du på **artefakter** toosee hello standard artefakt som har skapats.</span><span class="sxs-lookup"><span data-stu-id="5daa4-183">In hello **Project Structure** dialog box, click **Artifacts** toosee hello default artifact that is created.</span></span>
   <span data-ttu-id="5daa4-184">![Skapa JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span><span class="sxs-lookup"><span data-stu-id="5daa4-184">![Create JAR](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/default-artifact.png)</span></span>

      <span data-ttu-id="5daa4-185">Du kan också skapa egna artefakt bly när du klickar på hello  **+**  ikon, markerad i hello bilden ovan.</span><span class="sxs-lookup"><span data-stu-id="5daa4-185">You can also create your own artifact bly clicking on hello **+** icon, highlighted in hello image above.</span></span>

4. <span data-ttu-id="5daa4-186">Lägg till bibliotek tooyour projektet.</span><span class="sxs-lookup"><span data-stu-id="5daa4-186">Add libraries tooyour project.</span></span> <span data-ttu-id="5daa4-187">tooadd ett bibliotek Högerklicka hello projektnamn i trädet för hello-projektet och klicka på **öppna Modulinställningar**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-187">tooadd a library, right-click hello project name in hello project tree, and then click **Open Module Settings**.</span></span> <span data-ttu-id="5daa4-188">I hello **projektstruktur** i dialogrutan hello till vänster och klicka på **bibliotek**, klicka på symbolen hello (+) och klicka sedan på **från Maven**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-188">In hello **Project Structure** dialog box, from hello left pane, click **Libraries**, click hello (+) symbol, and then click **From Maven**.</span></span>

    ![Lägga till bibliotek](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/add-library.png)

    <span data-ttu-id="5daa4-190">I hello **Download Library från Maven databasen** dialogrutan, söka och ange hello följande bibliotek.</span><span class="sxs-lookup"><span data-stu-id="5daa4-190">In hello **Download Library from Maven Repository** dialog box, search and add hello following libraries.</span></span>

   * `org.scalatest:scalatest_2.10:2.2.1`
   * `org.apache.hadoop:hadoop-azure:2.7.1`
5. <span data-ttu-id="5daa4-191">Kopiera `yarn-site.xml` och `core-site.xml` från hello kluster headnode och lägga till den toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="5daa4-191">Copy `yarn-site.xml` and `core-site.xml` from hello cluster headnode and add it toohello project.</span></span> <span data-ttu-id="5daa4-192">Använd följande kommandon toocopy hello filer hello.</span><span class="sxs-lookup"><span data-stu-id="5daa4-192">Use hello following commands toocopy hello files.</span></span> <span data-ttu-id="5daa4-193">Du kan använda [Cygwin](https://cygwin.com/install.html) toorun hello följande `scp` toocopy hello filer från hello klustret headnodes-kommandon.</span><span class="sxs-lookup"><span data-stu-id="5daa4-193">You can use [Cygwin](https://cygwin.com/install.html) toorun hello following `scp` commands toocopy hello files from hello cluster headnodes.</span></span>

        scp <ssh user name>@<headnode IP address or host name>://etc/hadoop/conf/core-site.xml .

    <span data-ttu-id="5daa4-194">Eftersom vi har redan lagts till hello klustret headnode IP-adress och värddatornamn för hello värdfilen på hello skrivbordet, kan vi använda hello **scp** kommandon i hello följande sätt.</span><span class="sxs-lookup"><span data-stu-id="5daa4-194">Because we already added hello cluster headnode IP address and hostnames fo hello hosts file on hello desktop, we can use hello **scp** commands in hello following manner.</span></span>

        scp sshuser@hn0-nitinp:/etc/hadoop/conf/core-site.xml .
        scp sshuser@hn0-nitinp:/etc/hadoop/conf/yarn-site.xml .

    <span data-ttu-id="5daa4-195">Lägga till dessa filer tooyour projekt genom att kopiera dem under hello **/src** mapp i ditt projektträdet, till exempel `<your project directory>\src`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-195">Add these files tooyour project by copying them under hello **/src** folder in your project tree, for example `<your project directory>\src`.</span></span>
6. <span data-ttu-id="5daa4-196">Uppdatera hello `core-site.xml` toomake hello följande ändringar.</span><span class="sxs-lookup"><span data-stu-id="5daa4-196">Update hello `core-site.xml` toomake hello following changes.</span></span>

   1. <span data-ttu-id="5daa4-197">`core-site.xml`innehåller hello krypterade nyckeln toohello storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="5daa4-197">`core-site.xml` includes hello encrypted key toohello storage account associated with hello cluster.</span></span> <span data-ttu-id="5daa4-198">I hello `core-site.xml` du lagt till toohello projekt, Ersätt hello-krypteringsnyckeln med hello faktiska lagringsnyckeln som associeras med hello standardkontot för lagring.</span><span class="sxs-lookup"><span data-stu-id="5daa4-198">In hello `core-site.xml` that you added toohello project, replace hello encrypted key with hello actual storage key associated with hello default storage account.</span></span> <span data-ttu-id="5daa4-199">Se [hantera åtkomstnycklar för lagring](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5daa4-199">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

           <property>
                 <name>fs.azure.account.key.hdistoragecentral.blob.core.windows.net</name>
                 <value>access-key-associated-with-the-account</value>
           </property>
   2. <span data-ttu-id="5daa4-200">Ta bort hello efter poster från hello `core-site.xml`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-200">Remove hello following entries from hello `core-site.xml`.</span></span>

           <property>
                 <name>fs.azure.account.keyprovider.hdistoragecentral.blob.core.windows.net</name>
                 <value>org.apache.hadoop.fs.azure.ShellDecryptionKeyProvider</value>
           </property>

           <property>
                 <name>fs.azure.shellkeyprovider.script</name>
                 <value>/usr/lib/python2.7/dist-packages/hdinsight_common/decrypt.sh</value>
           </property>

           <property>
                 <name>net.topology.script.file.name</name>
                 <value>/etc/hadoop/conf/topology_script.py</value>
           </property>
   3. <span data-ttu-id="5daa4-201">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="5daa4-201">Save hello file.</span></span>
7. <span data-ttu-id="5daa4-202">Lägg till hello Main-klass för ditt program.</span><span class="sxs-lookup"><span data-stu-id="5daa4-202">Add hello Main class for your application.</span></span> <span data-ttu-id="5daa4-203">Från hello **Projektutforskaren**, högerklicka på **src**, peka för**ny**, och klicka sedan på **Scala klassen**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-203">From hello **Project Explorer**, right-click **src**, point too**New**, and then click **Scala class**.</span></span>

    ![Lägg till källkod](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code.png)
8. <span data-ttu-id="5daa4-205">I hello **skapa nya Scala klass** dialogrutan Ange ett namn för **typ** Välj **objekt**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-205">In hello **Create New Scala Class** dialog box, provide a name, for **Kind** select **Object**, and then click **OK**.</span></span>

    ![Lägg till källkod](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/hdi-spark-scala-code-object.png)
9. <span data-ttu-id="5daa4-207">I hello `MyClusterAppMain.scala` fil, klistra in följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="5daa4-207">In hello `MyClusterAppMain.scala` file, paste hello following code.</span></span> <span data-ttu-id="5daa4-208">Den här koden skapar hello Spark kontext och startar en `executeJob` metod från hello `SparkSample` objekt.</span><span class="sxs-lookup"><span data-stu-id="5daa4-208">This code creates hello Spark context and launches an `executeJob` method from hello `SparkSample` object.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}

        object SparkSampleMain {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkSample")
                                      .set("spark.hadoop.validateOutputSpecs", "false")
            val sc = new SparkContext(conf)

            SparkSample.executeJob(sc,
                                   "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
                                   "wasb:///HVACOut")
          }
        }

10. <span data-ttu-id="5daa4-209">Upprepa steg 8 och 9 ovan tooadd kallas för ett nytt objekt i Scala `SparkSample`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-209">Repeat steps 8 and 9 above tooadd a new Scala object called `SparkSample`.</span></span> <span data-ttu-id="5daa4-210">toothis klassen Lägg till följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="5daa4-210">toothis class add hello following code.</span></span> <span data-ttu-id="5daa4-211">Den här koden läser hello data från hello HVAC.csv (finns i alla HDInsight Spark-kluster), hämtar hello rader som bara har en siffra i hello sjunde kolumnen i hello CSV och skriver hello utdata för**/HVACOut** under hello standard lagringsbehållare som klustret hello.</span><span class="sxs-lookup"><span data-stu-id="5daa4-211">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello seventh column in hello CSV, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>

        import org.apache.spark.SparkContext

        object SparkSample {
         def executeJob (sc: SparkContext, input: String, output: String): Unit = {
           val rdd = sc.textFile(input)

           //find hello rows which have only one digit in hello 7th column in hello CSV
           val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)

           val s = sc.parallelize(rdd.take(5)).cartesian(rdd).count()
           println(s)

           rdd1.saveAsTextFile(output)
           //rdd1.collect().foreach(println)
         }
        }
11. <span data-ttu-id="5daa4-212">Upprepa steg 8 och 9 ovan tooadd kallas för en ny klass `RemoteClusterDebugging`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-212">Repeat steps 8 and 9 above tooadd a new class called `RemoteClusterDebugging`.</span></span> <span data-ttu-id="5daa4-213">Den här klassen implementerar hello Spark test framework som används för att felsöka program.</span><span class="sxs-lookup"><span data-stu-id="5daa4-213">This class implements hello Spark test framework that is used for debugging applications.</span></span> <span data-ttu-id="5daa4-214">Lägg till följande kod toohello hello `RemoteClusterDebugging` klass.</span><span class="sxs-lookup"><span data-stu-id="5daa4-214">Add hello following code toohello `RemoteClusterDebugging` class.</span></span>

        import org.apache.spark.{SparkConf, SparkContext}
        import org.scalatest.FunSuite

        class RemoteClusterDebugging extends FunSuite {

         test("Remote run") {
           val conf = new SparkConf().setAppName("SparkSample")
                                     .setMaster("yarn-client")
                                     .set("spark.yarn.am.extraJavaOptions", "-Dhdp.version=2.4")
                                     .set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")
                                     .setJars(Seq("""C:\workspace\IdeaProjects\MyClusterApp\out\artifacts\MyClusterApp_DefaultArtifact\default_artifact.jar"""))
                                     .set("spark.hadoop.validateOutputSpecs", "false")
           val sc = new SparkContext(conf)

           SparkSample.executeJob(sc,
             "wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv",
             "wasb:///HVACOut")
         }
        }

     <span data-ttu-id="5daa4-215">Några viktiga saker toonote här:</span><span class="sxs-lookup"><span data-stu-id="5daa4-215">Couple of important things toonote here:</span></span>

   * <span data-ttu-id="5daa4-216">För `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, kontrollera hello Spark sammansättningen JAR är tillgänglig på hello klusterlagringen på hello angivna sökvägen.</span><span class="sxs-lookup"><span data-stu-id="5daa4-216">For `.set("spark.yarn.jar", "wasb:///hdp/apps/2.4.2.0-258/spark-assembly-1.6.1.2.4.2.0-258-hadoop2.7.1.2.4.2.0-258.jar")`, make sure hello Spark assembly JAR is available on hello cluster storage at hello specified path.</span></span>
   * <span data-ttu-id="5daa4-217">För `setJars`, ange hello plats där hello artefakt jar kommer att skapas.</span><span class="sxs-lookup"><span data-stu-id="5daa4-217">For `setJars`, specify hello location where hello artifact jar will be created.</span></span> <span data-ttu-id="5daa4-218">Det är vanligtvis `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-218">Typically it is `<Your IntelliJ project directory>\out\<project name>_DefaultArtifact\default_artifact.jar`.</span></span>
12. <span data-ttu-id="5daa4-219">I hello `RemoteClusterDebugging` klassen, högerklicka på hello `test` nyckelord och välj **skapa RemoteClusterDebugging Configuration**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-219">In hello `RemoteClusterDebugging` class, right-click hello `test` keyword and select **Create RemoteClusterDebugging Configuration**.</span></span>

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-remote-config.png)

13. <span data-ttu-id="5daa4-221">Ange ett namn för hello configuration hello i dialogrutan och välj hello **testa typ** som **testnamnet**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-221">In hello dialog box, provide a name for hello configuration, and select hello **Test kind** as **Test name**.</span></span> <span data-ttu-id="5daa4-222">Lämnar du alla andra värden som standard, klickar du på **tillämpa**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-222">Leave all other values as default, click **Apply**, and then click **OK**.</span></span>

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/provide-config-value.png)
14. <span data-ttu-id="5daa4-224">Du bör nu se en **Remote kör** configuration listrutan i hello menyraden.</span><span class="sxs-lookup"><span data-stu-id="5daa4-224">You should now see a **Remote Run** configuration drop-down in hello menu bar.</span></span>

    ![Skapa konfiguration för fjärråtkomst](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/config-run.png)

## <a name="step-5-run-hello-application-in-debug-mode"></a><span data-ttu-id="5daa4-226">Steg 5: Kör programmet hello i felsökningsläge</span><span class="sxs-lookup"><span data-stu-id="5daa4-226">Step 5: Run hello application in debug mode</span></span>
1. <span data-ttu-id="5daa4-227">Öppna i projektet IntelliJ IDEA `SparkSample.scala` och skapa en brytpunkt nästa too'val rdd1'.</span><span class="sxs-lookup"><span data-stu-id="5daa4-227">In your IntelliJ IDEA project, open `SparkSample.scala` and create a breakpoint next too\`val rdd1'.</span></span> <span data-ttu-id="5daa4-228">Välj hello popup-menyn för att skapa en brytpunkt **rad i funktionen executeJob**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-228">In hello pop-up menu for creating a breakpoint, select **line in function executeJob**.</span></span>

    ![Lägga till en brytpunkt](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/create-breakpoint.png)
2. <span data-ttu-id="5daa4-230">Klicka på hello **Debug kör** knappen Nästa toohello **Remote kör** configuration nedrullningsbara toostart hello program körs.</span><span class="sxs-lookup"><span data-stu-id="5daa4-230">Click hello **Debug Run** button next toohello **Remote Run** configuration drop-down toostart running hello application.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-run-mode.png)
3. <span data-ttu-id="5daa4-232">När hello programkörningen nått hello brytpunkt bör du se en **felsökare** fliken i hello längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="5daa4-232">When hello program execution reaches hello breakpoint, you should see a **Debugger** tab in hello bottom pane.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch.png)
4. <span data-ttu-id="5daa4-234">Klicka på hello (**+**) ikonen tooadd en titta på enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="5daa4-234">Click hello (**+**) icon tooadd a watch as shown in hello image below.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable.png)

    <span data-ttu-id="5daa4-236">Här, eftersom programmet hello bröt mot innan hello variabeln `rdd1` har skapats med den här titta på kan vi se vad hello 5 första raderna i hello variabeln `rdd`.</span><span class="sxs-lookup"><span data-stu-id="5daa4-236">Here, because hello application broke before hello variable `rdd1` was created, using this watch we can see what are hello first 5 rows in hello variable `rdd`.</span></span> <span data-ttu-id="5daa4-237">Tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="5daa4-237">Press **ENTER**.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-add-watch-variable-value.png)

    <span data-ttu-id="5daa4-239">Du ser i hello bilden ovan är att du kan fråga terrabytes av data och felsökning vid körning, hur dina program pågår.</span><span class="sxs-lookup"><span data-stu-id="5daa4-239">What you see in hello image above is that at runtime, you could query terrabytes of data and debug how your application progresses.</span></span> <span data-ttu-id="5daa4-240">Till exempel hello utdata som visas i hello bilden ovan och du kan se att hello första raden i hello utdata är en rubrik.</span><span class="sxs-lookup"><span data-stu-id="5daa4-240">For example, in hello output shown in hello image above, you can see that hello first row of hello output is a header.</span></span> <span data-ttu-id="5daa4-241">Baserat på det kan du ändra ditt program kod tooskip hello rubrikrad om det behövs.</span><span class="sxs-lookup"><span data-stu-id="5daa4-241">Based on this, you can modify your application code tooskip hello header row if required.</span></span>
5. <span data-ttu-id="5daa4-242">Nu kan du klicka på hello **återuppta programmet** ikonen tooproceed med ditt program körs.</span><span class="sxs-lookup"><span data-stu-id="5daa4-242">You can now click hello **Resume Program** icon tooproceed with your application run.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-continue-run.png)
6. <span data-ttu-id="5daa4-244">Du bör se utdata som liknar följande hello om programmet hello är klar.</span><span class="sxs-lookup"><span data-stu-id="5daa4-244">If hello application completes successfully, you should see an output like hello following.</span></span>

    ![Kör hello programmet i felsökningsläge](./media/hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely-through-vpn/debug-complete.png)

## <span data-ttu-id="5daa4-246"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="5daa4-246"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="5daa4-247">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-247">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="5daa4-248">Demo</span><span class="sxs-lookup"><span data-stu-id="5daa4-248">Demo</span></span>
* <span data-ttu-id="5daa4-249">Skapa Scala projekt (Video): [skapa Spark Scala-program](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="5daa4-249">Create Scala Project (Video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="5daa4-250">Fjärråtkomst Debug (Video): [Använd Azure Toolkit för IntelliJ toodebug Spark-program på HDInsight-kluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="5daa4-250">Remote Debug (Video): [Use Azure Toolkit for IntelliJ toodebug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="5daa4-251">Scenarier</span><span class="sxs-lookup"><span data-stu-id="5daa4-251">Scenarios</span></span>
* [<span data-ttu-id="5daa4-252">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="5daa4-252">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="5daa4-253">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="5daa4-253">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="5daa4-254">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="5daa4-254">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="5daa4-255">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="5daa4-255">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="5daa4-256">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-256">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="5daa4-257">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="5daa4-257">Create and run applications</span></span>
* [<span data-ttu-id="5daa4-258">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="5daa4-258">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="5daa4-259">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="5daa4-259">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="5daa4-260">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="5daa4-260">Tools and extensions</span></span>
* [<span data-ttu-id="5daa4-261">Använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="5daa4-261">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="5daa4-262">Använd Azure Toolkit för IntelliJ toodebug Spark-program via fjärranslutning via SSH</span><span class="sxs-lookup"><span data-stu-id="5daa4-262">Use Azure Toolkit for IntelliJ toodebug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="5daa4-263">Använda HDInsight Tools för IntelliJ med Hortonworks Sandbox</span><span class="sxs-lookup"><span data-stu-id="5daa4-263">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="5daa4-264">Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program</span><span class="sxs-lookup"><span data-stu-id="5daa4-264">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="5daa4-265">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-265">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="5daa4-266">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-266">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="5daa4-267">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="5daa4-267">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="5daa4-268">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="5daa4-268">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="5daa4-269">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="5daa4-269">Manage resources</span></span>
* [<span data-ttu-id="5daa4-270">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-270">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="5daa4-271">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="5daa4-271">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
