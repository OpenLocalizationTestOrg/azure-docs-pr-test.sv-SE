---
title: "Övervaka och hantera Azure HDInsight med Ambari-Webbgränssnittet | Microsoft Docs"
description: "Lär dig använda Ambari och övervaka och hantera Linux-baserade HDInsight-kluster. I det här dokumentet lär du dig att använda Ambari-Webbgränssnittet som ingår i HDInsight-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: dc0f9ff030f70985dad0f3b74ba0ee3dda1d9f4b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a><span data-ttu-id="d7ef2-104">Hantera HDInsight-kluster med Ambari-Webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d7ef2-104">Manage HDInsight clusters by using the Ambari Web UI</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="d7ef2-105">Apache Ambari förenklar hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla ett enkelt att använda webbgränssnittet och REST-API.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-105">Apache Ambari simplifies the management and monitoring of a Hadoop cluster by providing an easy to use web UI and REST API.</span></span> <span data-ttu-id="d7ef2-106">Ambari ingår i Linux-baserade HDInsight-kluster och används för att övervaka klustret och gör ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-106">Ambari is included on Linux-based HDInsight clusters, and is used to monitor the cluster and make configuration changes.</span></span>

<span data-ttu-id="d7ef2-107">Lär dig hur du använder Ambari-Webbgränssnittet med ett HDInsight-kluster i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-107">In this document, you learn how to use the Ambari Web UI with an HDInsight cluster.</span></span>

## <span data-ttu-id="d7ef2-108"><a id="whatis"></a>Vad är Ambari?</span><span class="sxs-lookup"><span data-stu-id="d7ef2-108"><a id="whatis"></a>What is Ambari?</span></span>

<span data-ttu-id="d7ef2-109">[Apache Ambari](http://ambari.apache.org) förenklar Hadoop-hanteringen genom att tillhandahålla en enkel att använda webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-109">[Apache Ambari](http://ambari.apache.org) simplifies Hadoop management by providing an easy-to-use web UI.</span></span> <span data-ttu-id="d7ef2-110">Du kan använda Ambari skapa, hantera och övervaka Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-110">You can use Ambari create, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="d7ef2-111">Utvecklare kan integrera dessa funktioner i sina program med hjälp av den [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-111">Developers can integrate these capabilities into their applications by using the [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="d7ef2-112">Ambari-Webbgränssnittet är som standard med HDInsight-kluster som använder Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-112">The Ambari Web UI is provided by default with HDInsight clusters that use the Linux operating system.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7ef2-113">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-113">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d7ef2-114">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-114">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 

## <a name="connectivity"></a><span data-ttu-id="d7ef2-115">Anslutning</span><span class="sxs-lookup"><span data-stu-id="d7ef2-115">Connectivity</span></span>

<span data-ttu-id="d7ef2-116">Ambari-Webbgränssnittet är tillgängligt på ditt HDInsight-kluster på HTTPS://CLUSTERNAME.azurehdidnsight.net, där **KLUSTERNAMN** är namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-116">The Ambari Web UI is available on your HDInsight cluster at HTTPS://CLUSTERNAME.azurehdidnsight.net, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7ef2-117">Ansluter till Ambari på HDInsight kräver HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-117">Connecting to Ambari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="d7ef2-118">När du uppmanas för autentisering, Använd admin-kontonamnet och lösenordet du angav när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-118">When prompted for authentication, use the admin account name and password you provided when the cluster was created.</span></span>

## <a name="ssh-tunnel-proxy"></a><span data-ttu-id="d7ef2-119">SSH-tunnel (proxy)</span><span class="sxs-lookup"><span data-stu-id="d7ef2-119">SSH tunnel (proxy)</span></span>

<span data-ttu-id="d7ef2-120">När Ambari för klustret kan komma åt direkt via Internet, exponeras inte vissa länkar från Ambari-Webbgränssnittet (till exempel JobTracker) på internet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-120">While Ambari for your cluster is accessible directly over the Internet, some links from the Ambari Web UI (such as to the JobTracker) are not exposed on the internet.</span></span> <span data-ttu-id="d7ef2-121">För att komma åt dessa tjänster måste du skapa en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-121">To access these services, you must create an SSH tunnel.</span></span> <span data-ttu-id="d7ef2-122">Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-122">For more information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

## <a name="ambari-web-ui"></a><span data-ttu-id="d7ef2-123">Ambari-webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d7ef2-123">Ambari Web UI</span></span>

<span data-ttu-id="d7ef2-124">När du ansluter till Ambari-Webbgränssnittet, uppmanas du att autentisera till sidan.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-124">When connecting to the Ambari Web UI, you are prompted to authenticate to the page.</span></span> <span data-ttu-id="d7ef2-125">Använd kluster administratörsanvändare (standard Admin) och lösenord som du använde när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-125">Use the cluster admin user (default Admin) and password you used during cluster creation.</span></span>

<span data-ttu-id="d7ef2-126">När sidan öppnas Observera längst upp.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-126">When the page opens, note the bar at the top.</span></span> <span data-ttu-id="d7ef2-127">Det här fältet innehåller följande information och kontroller:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-127">This bar contains the following information and controls:</span></span>

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* <span data-ttu-id="d7ef2-129">**Ambari logotypen** -öppnar instrumentpanelen, som kan användas för att övervaka klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-129">**Ambari logo** - Opens the dashboard, which can be used to monitor the cluster.</span></span>

* <span data-ttu-id="d7ef2-130">**Klustrets namn # ops** -visar antalet pågående Ambari-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-130">**Cluster name # ops** - Displays the number of ongoing Ambari operations.</span></span> <span data-ttu-id="d7ef2-131">Att välja klusternamnet eller **# ops** visar en lista över bakgrundsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-131">Selecting the cluster name or **# ops** displays a list of background operations.</span></span>

* <span data-ttu-id="d7ef2-132">**# aviseringar** -visar varningar eller kritiska varningar för klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-132">**# alerts** - Displays warnings or critical alerts, if any, for the cluster.</span></span>

* <span data-ttu-id="d7ef2-133">**Instrumentpanelen** -instrumentpanelen visas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-133">**Dashboard** - Displays the dashboard.</span></span>

* <span data-ttu-id="d7ef2-134">**Tjänster** - och konfigurationsinformation inställningar för tjänster i klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-134">**Services** - Information and configuration settings for the services in the cluster.</span></span>

* <span data-ttu-id="d7ef2-135">**Värdar** -Information och konfiguration av inställningar för noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-135">**Hosts** - Information and configuration settings for the nodes in the cluster.</span></span>

* <span data-ttu-id="d7ef2-136">**Aviseringar** -en logg över information, varningar och kritiska aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-136">**Alerts** - A log of information, warnings, and critical alerts.</span></span>

* <span data-ttu-id="d7ef2-137">**Admin** -stacken/programtjänster som är installerade på klustret, tjänstens kontoinformation och Kerberos-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-137">**Admin** - Software stack/services that are installed on the cluster, service account information, and Kerberos security.</span></span>

* <span data-ttu-id="d7ef2-138">**Admin-knappen** -Ambari hantering, användarinställningar och logga ut.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-138">**Admin button** - Ambari management, user settings, and logout.</span></span>

## <a name="monitoring"></a><span data-ttu-id="d7ef2-139">Övervakning</span><span class="sxs-lookup"><span data-stu-id="d7ef2-139">Monitoring</span></span>

### <a name="alerts"></a><span data-ttu-id="d7ef2-140">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="d7ef2-140">Alerts</span></span>

<span data-ttu-id="d7ef2-141">Följande lista innehåller vanliga avisering status som används av Ambari:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-141">The following list contains the common alert statuses used by Ambari:</span></span>

* <span data-ttu-id="d7ef2-142">**OKEJ**</span><span class="sxs-lookup"><span data-stu-id="d7ef2-142">**OK**</span></span>
* <span data-ttu-id="d7ef2-143">**Varning**</span><span class="sxs-lookup"><span data-stu-id="d7ef2-143">**Warning**</span></span>
* <span data-ttu-id="d7ef2-144">**KRITISKA**</span><span class="sxs-lookup"><span data-stu-id="d7ef2-144">**CRITICAL**</span></span>
* <span data-ttu-id="d7ef2-145">**OKÄND**</span><span class="sxs-lookup"><span data-stu-id="d7ef2-145">**UNKNOWN**</span></span>

<span data-ttu-id="d7ef2-146">Aviseringar än **OK** orsaka det **# aviseringar** överst på sidan för att visa antalet aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-146">Alerts other than **OK** cause the **# alerts** entry at the top of the page to display the number of alerts.</span></span> <span data-ttu-id="d7ef2-147">Om du markerar den här posten visas aviseringar och deras status.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-147">Selecting this entry displays the alerts and their status.</span></span>

<span data-ttu-id="d7ef2-148">Aviseringar är uppdelade i flera standardgrupper som kan visas från den **aviseringar** sidan.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-148">Alerts are organized into several default groups, which can be viewed from the **Alerts** page.</span></span>

![Sidan varningar](./media/hdinsight-hadoop-manage-ambari/alerts.png)

<span data-ttu-id="d7ef2-150">Du kan hantera grupperna med hjälp av den **åtgärder** menyn och välja **hantera avisering grupper**.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-150">You can manage the groups by using the **Actions** menu and selecting **Manage Alert Groups**.</span></span>

![Hantera aviseringar grupper dialogrutan](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

<span data-ttu-id="d7ef2-152">Du kan också hantera aviseringar metoder och skapa aviseringar från den **åtgärder** menyn genom att välja __hantera avisering meddelanden__.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-152">You can also manage alerting methods, and create alert notifications from the **Actions** menu by selecting __Manage Alert Notifications__.</span></span> <span data-ttu-id="d7ef2-153">De aktuella meddelanden visas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-153">Any current notifications are displayed.</span></span> <span data-ttu-id="d7ef2-154">Du kan också skapa aviseringar här.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-154">You can also create notifications from here.</span></span> <span data-ttu-id="d7ef2-155">Meddelanden kan skickas **e-post** eller **SNMP** när specifika/allvarlighetsgraden för aviseringen kombinationer inträffar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-155">Notifications can be sent via **EMAIL** or **SNMP** when specific alert/severity combinations occur.</span></span> <span data-ttu-id="d7ef2-156">Du kan till exempel skicka ett e-postmeddelande när något av aviseringar i den **YARN standard** grupp har angetts till **kritisk**.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-156">For example, you can send an email message when any of the alerts in the **YARN Default** group is set to **Critical**.</span></span>

![Skapa varningsruta](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

<span data-ttu-id="d7ef2-158">Slutligen väljer __Hantera aviseringsinställningar__ från den __åtgärder__ menyn kan du ange antalet gånger som en avisering måste inträffa innan ett meddelande har skickats.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-158">Finally, selecting __Manage Alert Settings__ from the __Actions__ menu allows you to set the number of times an alert must occur before a notification is sent.</span></span> <span data-ttu-id="d7ef2-159">Den här inställningen kan användas för att förhindra att meddelanden för tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-159">This setting can be used to prevent notifications for transient errors.</span></span>

### <a name="cluster"></a><span data-ttu-id="d7ef2-160">Kluster</span><span class="sxs-lookup"><span data-stu-id="d7ef2-160">Cluster</span></span>

<span data-ttu-id="d7ef2-161">Den **mått** på instrumentpanelen innehåller ett antal widgetar som gör det lättare att övervaka status för klustret i korthet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-161">The **Metrics** tab of the dashboard contains a series of widgets that make it easy to monitor the status of your cluster at a glance.</span></span> <span data-ttu-id="d7ef2-162">Flera widgetar som **CPU-användning**, innehåller ytterligare information när du klickar på.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-162">Several widgets, such as **CPU Usage**, provide additional information when clicked.</span></span>

![instrumentpanel med mått](./media/hdinsight-hadoop-manage-ambari/metrics.png)

<span data-ttu-id="d7ef2-164">Den **Heatmaps** fliken visar mått som färgade heatmaps som kommer från grönt till rött.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-164">The **Heatmaps** tab displays metrics as colored heatmaps, going from green to red.</span></span>

![instrumentpanel med heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

<span data-ttu-id="d7ef2-166">Mer information om noder i klustret, väljer **värdar**.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-166">For more information on the nodes within the cluster, select **Hosts**.</span></span> <span data-ttu-id="d7ef2-167">Välj den specifika nod som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-167">Then select the specific node you are interested in.</span></span>

![information om värden](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a><span data-ttu-id="d7ef2-169">Tjänster</span><span class="sxs-lookup"><span data-stu-id="d7ef2-169">Services</span></span>

<span data-ttu-id="d7ef2-170">Den **Services** sidopanelen på instrumentpanelen ger snabb inblick i status för de tjänster som körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-170">The **Services** sidebar on the dashboard provides quick insight into the status of the services running on the cluster.</span></span> <span data-ttu-id="d7ef2-171">Olika ikoner används för att ange status eller åtgärder som ska vidtas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-171">Various icons are used to indicate status or actions that should be taken.</span></span> <span data-ttu-id="d7ef2-172">Till exempel visas en gul återvinning symbol om en tjänst behöver återvinnas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-172">For example, a yellow recycle symbol is displayed if a service needs to be recycled.</span></span>

![tjänster-sidorutan](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> <span data-ttu-id="d7ef2-174">Tjänster som visas skiljer sig åt mellan HDInsight-klustertyper och versioner.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-174">The services displayed differ between HDInsight cluster types and versions.</span></span> <span data-ttu-id="d7ef2-175">De tjänster som visas här kan skilja sig från de tjänster som visas för klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-175">The services displayed here may be different than the services displayed for your cluster.</span></span>

<span data-ttu-id="d7ef2-176">Att välja en tjänst visar mer detaljerad information om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-176">Selecting a service displays more detailed information on the service.</span></span>

![sammanfattningsinformation för tjänsten](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a><span data-ttu-id="d7ef2-178">Snabblänkar</span><span class="sxs-lookup"><span data-stu-id="d7ef2-178">Quick links</span></span>

<span data-ttu-id="d7ef2-179">Vissa tjänster visas en **snabblänkar** längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-179">Some services display a **Quick Links** link at the top of the page.</span></span> <span data-ttu-id="d7ef2-180">Detta kan användas för att komma åt tjänstspecifika web användargränssnitt, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-180">This can be used to access service-specific web UIs, such as:</span></span>

* <span data-ttu-id="d7ef2-181">**Jobbhistorik** -MapReduce historiken för datalagerjobb.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-181">**Job History** - MapReduce job history.</span></span>
* <span data-ttu-id="d7ef2-182">**Hanteraren för filserverresurser** -YARN ResourceManager-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-182">**Resource Manager** - YARN ResourceManager UI.</span></span>
* <span data-ttu-id="d7ef2-183">**NameNode** -Hadoop Distributed File System (HDFS) NameNode Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-183">**NameNode** - Hadoop Distributed File System (HDFS) NameNode UI.</span></span>
* <span data-ttu-id="d7ef2-184">**Oozie webbgränssnittet** -Oozie-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-184">**Oozie Web UI** - Oozie UI.</span></span>

<span data-ttu-id="d7ef2-185">När du väljer ett av dessa länkar öppnas en ny flik i webbläsaren, som visar den valda sidan.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-185">Selecting any of these links opens a new tab in your browser, which displays the selected page.</span></span>

> [!NOTE]
> <span data-ttu-id="d7ef2-186">Att välja den **snabblänkar** post för en tjänst kan returnera ett ”det gick inte att hitta” serverfel.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-186">Selecting the **Quick Links** entry for a service may return a "server not found" error.</span></span> <span data-ttu-id="d7ef2-187">Om felet uppstår, måste du använda en SSH-tunnel när du använder den **snabblänkar** post för den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-187">If you encounter this error, you must use an SSH tunnel when using the **Quick Links** entry for this service.</span></span> <span data-ttu-id="d7ef2-188">Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="d7ef2-188">For information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

## <a name="management"></a><span data-ttu-id="d7ef2-189">Hantering</span><span class="sxs-lookup"><span data-stu-id="d7ef2-189">Management</span></span>

### <a name="ambari-users-groups-and-permissions"></a><span data-ttu-id="d7ef2-190">Ambari användare, grupper och behörigheter</span><span class="sxs-lookup"><span data-stu-id="d7ef2-190">Ambari users, groups, and permissions</span></span>

<span data-ttu-id="d7ef2-191">Arbeta med användare, grupper och behörigheter stöds när du använder en [domänanslutna](hdinsight-domain-joined-introduction.md) HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-191">Working with users, groups, and permissions are supported when using a [domain joined](hdinsight-domain-joined-introduction.md) HDInsight cluster.</span></span> <span data-ttu-id="d7ef2-192">Mer information om med hjälp av Användargränssnittet för hantering av Ambari på en domänansluten klustret finns [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-192">For information on using the Ambari Management UI on a domain-joined cluster, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-introduction.md).</span></span>

> [!WARNING]
> <span data-ttu-id="d7ef2-193">Ändra inte lösenordet för Ambari watchdog (hdinsightwatchdog) på Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-193">Do not change the password of the Ambari watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="d7ef2-194">Ändra lösenord bryts möjligheten att använda script actions eller utföra skalning åtgärder med ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-194">Changing the password breaks the ability to use script actions or perform scaling operations with your cluster.</span></span>

### <a name="hosts"></a><span data-ttu-id="d7ef2-195">Värdar</span><span class="sxs-lookup"><span data-stu-id="d7ef2-195">Hosts</span></span>

<span data-ttu-id="d7ef2-196">Den **värdar** sidan listar alla värdar i klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-196">The **Hosts** page lists all hosts in the cluster.</span></span> <span data-ttu-id="d7ef2-197">Följ dessa steg för att hantera värdar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-197">To manage hosts, follow these steps.</span></span>

![sidan för värdar](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> <span data-ttu-id="d7ef2-199">Lägga till, inaktivering och recommissioning en värd får inte användas med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-199">Adding, decommissioning, and recommissioning a host should not be used with HDInsight clusters.</span></span>

1. <span data-ttu-id="d7ef2-200">Välj den värd som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-200">Select the host that you wish to manage.</span></span>

2. <span data-ttu-id="d7ef2-201">Använd den **åtgärder** -menyn och välj den åtgärd som du vill utföra:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-201">Use the **Actions** menu to select the action that you wish to perform:</span></span>

   * <span data-ttu-id="d7ef2-202">**Starta alla komponenter** -starta alla komponenter på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-202">**Start all components** - Start all components on the host.</span></span>

   * <span data-ttu-id="d7ef2-203">**Stoppa alla komponenter** -stoppa alla komponenter på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-203">**Stop all components** - Stop all components on the host.</span></span>

   * <span data-ttu-id="d7ef2-204">**Starta om alla komponenter** - stoppa och starta alla komponenter på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-204">**Restart all components** - Stop and start all components on the host.</span></span>

   * <span data-ttu-id="d7ef2-205">**Aktivera underhållsläget** -Undertrycker aviseringar för värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-205">**Turn on maintenance mode** - Suppresses alerts for the host.</span></span> <span data-ttu-id="d7ef2-206">Det här läget måste vara aktiverat om du utför åtgärder som genererar aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-206">This mode should be enabled if you are performing actions that generate alerts.</span></span> <span data-ttu-id="d7ef2-207">Exempelvis stoppa och starta en tjänst.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-207">For example, stopping and starting a service.</span></span>

   * <span data-ttu-id="d7ef2-208">**Inaktivera underhållsläge** -returnerar värden till normal aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-208">**Turn off maintenance mode** - Returns the host to normal alerting.</span></span>

   * <span data-ttu-id="d7ef2-209">**Stoppa** -stoppar DataNode eller NodeManagers på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-209">**Stop** - Stops DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d7ef2-210">**Starta** -startar DataNode eller NodeManagers på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-210">**Start** - Starts DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d7ef2-211">**Starta om** -stoppar och startar DataNode eller NodeManagers på värden.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-211">**Restart** - Stops and starts DataNode or NodeManagers on the host.</span></span>

   * <span data-ttu-id="d7ef2-212">**Inaktivera** -tar bort en värd från klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-212">**Decommission** - Removes a host from the cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d7ef2-213">Använd inte den här åtgärden på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-213">Do not use this action on HDInsight clusters.</span></span>

   * <span data-ttu-id="d7ef2-214">**Recommission** -lägger till en tidigare inaktiverade värd i klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-214">**Recommission** - Adds a previously decommissioned host to the cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="d7ef2-215">Använd inte den här åtgärden på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-215">Do not use this action on HDInsight clusters.</span></span>

### <span data-ttu-id="d7ef2-216"><a id="service"></a>Tjänster</span><span class="sxs-lookup"><span data-stu-id="d7ef2-216"><a id="service"></a>Services</span></span>

<span data-ttu-id="d7ef2-217">Från den **instrumentpanelen** eller **Services** använder den **åtgärder** knappen längst ned i listan över tjänster att stoppa och starta alla tjänster.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-217">From the **Dashboard** or **Services** page, use the **Actions** button at the bottom of the list of services to stop and start all services.</span></span>

![tjänståtgärder](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> <span data-ttu-id="d7ef2-219">Medan **lägga till tjänsten** visas i den här menyn den bör inte användas för att lägga till tjänster till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-219">While **Add Service** is listed in this menu, it should not be used to add services to the HDInsight cluster.</span></span> <span data-ttu-id="d7ef2-220">Nya tjänster ska läggas till med en skriptåtgärd under klusteretablering.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-220">New services should be added using a Script Action during cluster provisioning.</span></span> <span data-ttu-id="d7ef2-221">Mer information om hur du använder skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-221">For more information on using Script Actions, see [Customize HDInsight clusters using Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="d7ef2-222">När den **åtgärder** knappen kan starta om alla tjänster, ofta du vill starta, stoppa eller starta om en specifik tjänst.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-222">While the **Actions** button can restart all services, often you want to start, stop, or restart a specific service.</span></span> <span data-ttu-id="d7ef2-223">Använd följande steg för att utföra åtgärder på en enskild tjänst:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-223">Use the following steps to perform actions on an individual service:</span></span>

1. <span data-ttu-id="d7ef2-224">Från den **instrumentpanelen** eller **Services** väljer du en tjänst.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-224">From the **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="d7ef2-225">Från början av den **sammanfattning** använder den **tjänståtgärder** och välj åtgärden som ska vidtas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-225">From the top of the **Summary** tab, use the **Service Actions** button and select the action to take.</span></span> <span data-ttu-id="d7ef2-226">Detta startar om tjänsten på alla noder.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-226">This restarts the service on all nodes.</span></span>

    ![åtgärden på tjänsten](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > <span data-ttu-id="d7ef2-228">Starta om vissa tjänster när klustret är igång kan generera aviseringar.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-228">Restarting some services while the cluster is running may generate alerts.</span></span> <span data-ttu-id="d7ef2-229">Om du vill undvika aviseringar kan du använda den **tjänståtgärder** för att aktivera **underhållsläge** för tjänsten innan du utför omstarten.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-229">To avoid alerts, you can use the **Service Actions** button to enable **Maintenance mode** for the service before performing the restart.</span></span>

3. <span data-ttu-id="d7ef2-230">När en åtgärd har valts i **# op** post överst på sidan steg att visa att en bakgrundsåtgärden pågår.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-230">Once an action has been selected, the **# op** entry at the top of the page increments to show that a background operation is occurring.</span></span> <span data-ttu-id="d7ef2-231">Om konfigurerad för att visa visas listan över bakgrundsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-231">If configured to display, the list of background operations is displayed.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d7ef2-232">Om du har aktiverat **underhållsläge** för tjänsten, Kom ihåg att inaktivera med hjälp av den **tjänståtgärder** knappen när åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-232">If you enabled **Maintenance mode** for the service, remember to disable it by using the **Service Actions** button once the operation has finished.</span></span>

<span data-ttu-id="d7ef2-233">Använd följande steg för att konfigurera en tjänst:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-233">To configure a service, use the following steps:</span></span>

1. <span data-ttu-id="d7ef2-234">Från den **instrumentpanelen** eller **Services** väljer du en tjänst.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-234">From the **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="d7ef2-235">Välj den **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-235">Select the **Configs** tab.</span></span> <span data-ttu-id="d7ef2-236">Den aktuella konfigurationen visas.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-236">The current configuration is displayed.</span></span> <span data-ttu-id="d7ef2-237">En lista över tidigare konfigurationer visas också.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-237">A list of previous configurations is also displayed.</span></span>

    ![Konfigurationer](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. <span data-ttu-id="d7ef2-239">Använd de fält som visas om du vill ändra konfigurationen och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-239">Use the fields displayed to modify the configuration, and then select **Save**.</span></span> <span data-ttu-id="d7ef2-240">Eller välj en tidigare konfiguration och välj sedan **se aktuella** att återställa de tidigare inställningarna.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-240">Or select a previous configuration and then select **Make current** to roll back to the previous settings.</span></span>

## <a name="ambari-views"></a><span data-ttu-id="d7ef2-241">Ambari-vyer</span><span class="sxs-lookup"><span data-stu-id="d7ef2-241">Ambari views</span></span>

<span data-ttu-id="d7ef2-242">Ambari Views kan utvecklare plugin-UI-element i en Ambari-Webbgränssnittet med hjälp av den [Ambari vyer Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-242">Ambari Views allow developers to plug UI elements into the Ambari Web UI using the [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span></span> <span data-ttu-id="d7ef2-243">HDInsight innehåller följande vyer med Hadoop-klustertyper:</span><span class="sxs-lookup"><span data-stu-id="d7ef2-243">HDInsight provides the following views with Hadoop cluster types:</span></span>

* <span data-ttu-id="d7ef2-244">Yarn Queue Manager: köhanteraren ger ett enkelt gränssnitt för att visa och ändra YARN köer.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-244">Yarn Queue Manager: The queue manager provides a simple UI for viewing and modifying YARN queues.</span></span>

* <span data-ttu-id="d7ef2-245">Hive-vyn: Hive-vy kan du köra Hive-frågor direkt från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-245">Hive View: The Hive View allows you to run Hive queries directly from your web browser.</span></span> <span data-ttu-id="d7ef2-246">Du kan spara frågor, visa resultat, spara resultaten för klusterlagring eller hämta resultaten till din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-246">You can save queries, view results, save results to the cluster storage, or download results to your local system.</span></span> <span data-ttu-id="d7ef2-247">Mer information om hur du använder Hive vyer finns [Använd Hive-vyer med HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="d7ef2-247">For more information on using Hive Views, see [Use Hive Views with HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>

* <span data-ttu-id="d7ef2-248">Tez vy: Visa Tez kan du bättre förstå och optimera jobb.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-248">Tez View: The Tez View allows you to better understand and optimize jobs.</span></span> <span data-ttu-id="d7ef2-249">Du kan visa information om hur Tez-jobb körs och vilka resurser som används.</span><span class="sxs-lookup"><span data-stu-id="d7ef2-249">You can view information on how Tez jobs are executed and what resources are used.</span></span>
