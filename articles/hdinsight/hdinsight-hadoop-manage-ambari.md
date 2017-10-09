---
title: "aaaMonitor och hantera Azure HDInsight med Ambari-Webbgränssnittet | Microsoft Docs"
description: "Lär dig hur toouse Ambari toomonitor och hantera Linux-baserade HDInsight-kluster. I det här dokumentet beskrivs hur toouse hello Ambari-Webbgränssnittet som ingår i HDInsight-kluster."
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
ms.openlocfilehash: d422c40e63345d7054839a625e115c50dad040f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-web-ui"></a><span data-ttu-id="29c59-104">Hantera HDInsight-kluster med hjälp av hello Ambari-Webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="29c59-104">Manage HDInsight clusters by using hello Ambari Web UI</span></span>

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

<span data-ttu-id="29c59-105">Apache Ambari förenklar hello hantering och övervakning av ett Hadoop-kluster genom att tillhandahålla en enkel toouse web användargränssnitt och REST API.</span><span class="sxs-lookup"><span data-stu-id="29c59-105">Apache Ambari simplifies hello management and monitoring of a Hadoop cluster by providing an easy toouse web UI and REST API.</span></span> <span data-ttu-id="29c59-106">Ambari ingår i Linux-baserade HDInsight-kluster och har använt toomonitor hello klustret och gör ändringar i konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="29c59-106">Ambari is included on Linux-based HDInsight clusters, and is used toomonitor hello cluster and make configuration changes.</span></span>

<span data-ttu-id="29c59-107">I det här dokumentet beskrivs hur toouse hello Ambari-Webbgränssnittet med ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-107">In this document, you learn how toouse hello Ambari Web UI with an HDInsight cluster.</span></span>

## <span data-ttu-id="29c59-108"><a id="whatis"></a>Vad är Ambari?</span><span class="sxs-lookup"><span data-stu-id="29c59-108"><a id="whatis"></a>What is Ambari?</span></span>

<span data-ttu-id="29c59-109">[Apache Ambari](http://ambari.apache.org) förenklar Hadoop-hanteringen genom att tillhandahålla en enkel att använda webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="29c59-109">[Apache Ambari](http://ambari.apache.org) simplifies Hadoop management by providing an easy-to-use web UI.</span></span> <span data-ttu-id="29c59-110">Du kan använda Ambari skapa, hantera och övervaka Hadoop-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-110">You can use Ambari create, manage, and monitor Hadoop clusters.</span></span> <span data-ttu-id="29c59-111">Utvecklare kan integrera dessa funktioner i sina program med hjälp av hello [Ambari REST API: er](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span><span class="sxs-lookup"><span data-stu-id="29c59-111">Developers can integrate these capabilities into their applications by using hello [Ambari REST APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).</span></span>

<span data-ttu-id="29c59-112">Hej Ambari-Webbgränssnittet är som standard med HDInsight-kluster som använder hello Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="29c59-112">hello Ambari Web UI is provided by default with HDInsight clusters that use hello Linux operating system.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29c59-113">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="29c59-113">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="29c59-114">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="29c59-114">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 

## <a name="connectivity"></a><span data-ttu-id="29c59-115">Anslutning</span><span class="sxs-lookup"><span data-stu-id="29c59-115">Connectivity</span></span>

<span data-ttu-id="29c59-116">Hej Ambari-Webbgränssnittet är tillgängligt på ditt HDInsight-kluster på HTTPS://CLUSTERNAME.azurehdidnsight.net, där **KLUSTERNAMN** är hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-116">hello Ambari Web UI is available on your HDInsight cluster at HTTPS://CLUSTERNAME.azurehdidnsight.net, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29c59-117">Ansluta tooAmbari på HDInsight kräver HTTPS.</span><span class="sxs-lookup"><span data-stu-id="29c59-117">Connecting tooAmbari on HDInsight requires HTTPS.</span></span> <span data-ttu-id="29c59-118">När du uppmanas att autentisera använda hello admin kontonamn och lösenord som du angav när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="29c59-118">When prompted for authentication, use hello admin account name and password you provided when hello cluster was created.</span></span>

## <a name="ssh-tunnel-proxy"></a><span data-ttu-id="29c59-119">SSH-tunnel (proxy)</span><span class="sxs-lookup"><span data-stu-id="29c59-119">SSH tunnel (proxy)</span></span>

<span data-ttu-id="29c59-120">När Ambari för klustret kan komma åt direkt via hello Internet, hello vissa länkar från hello Ambari-Webbgränssnittet (till exempel toohello JobTracker) inte är tillgängliga på internet.</span><span class="sxs-lookup"><span data-stu-id="29c59-120">While Ambari for your cluster is accessible directly over hello Internet, some links from hello Ambari Web UI (such as toohello JobTracker) are not exposed on hello internet.</span></span> <span data-ttu-id="29c59-121">tooaccess dessa tjänster, måste du skapa en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="29c59-121">tooaccess these services, you must create an SSH tunnel.</span></span> <span data-ttu-id="29c59-122">Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="29c59-122">For more information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

## <a name="ambari-web-ui"></a><span data-ttu-id="29c59-123">Ambari-webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="29c59-123">Ambari Web UI</span></span>

<span data-ttu-id="29c59-124">När du ansluter toohello Ambari-Webbgränssnittet är ange tooauthenticate toohello sidan.</span><span class="sxs-lookup"><span data-stu-id="29c59-124">When connecting toohello Ambari Web UI, you are prompted tooauthenticate toohello page.</span></span> <span data-ttu-id="29c59-125">Använd hello klustret administratörsanvändare (standard Admin) och lösenord som du använde när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="29c59-125">Use hello cluster admin user (default Admin) and password you used during cluster creation.</span></span>

<span data-ttu-id="29c59-126">Observera hello fältet hello överst när hello sidan öppnas.</span><span class="sxs-lookup"><span data-stu-id="29c59-126">When hello page opens, note hello bar at hello top.</span></span> <span data-ttu-id="29c59-127">Det här fältet innehåller hello följande information och kontroller:</span><span class="sxs-lookup"><span data-stu-id="29c59-127">This bar contains hello following information and controls:</span></span>

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* <span data-ttu-id="29c59-129">**Ambari logotypen** -öppnas hello instrumentpanelen, som kan vara används toomonitor hello klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-129">**Ambari logo** - Opens hello dashboard, which can be used toomonitor hello cluster.</span></span>

* <span data-ttu-id="29c59-130">**Klustrets namn # ops** -visar hello Antal pågående Ambari-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="29c59-130">**Cluster name # ops** - Displays hello number of ongoing Ambari operations.</span></span> <span data-ttu-id="29c59-131">Du väljer hello klusternamnet eller **# ops** visar en lista över bakgrundsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="29c59-131">Selecting hello cluster name or **# ops** displays a list of background operations.</span></span>

* <span data-ttu-id="29c59-132">**# aviseringar** -visar varningar eller kritiska varningar för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-132">**# alerts** - Displays warnings or critical alerts, if any, for hello cluster.</span></span>

* <span data-ttu-id="29c59-133">**Instrumentpanelen** -visar hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="29c59-133">**Dashboard** - Displays hello dashboard.</span></span>

* <span data-ttu-id="29c59-134">**Tjänster** - och konfigurationsinformation inställningar för hello tjänster i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-134">**Services** - Information and configuration settings for hello services in hello cluster.</span></span>

* <span data-ttu-id="29c59-135">**Värdar** - och konfigurationsinformation inställningar för hello noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="29c59-135">**Hosts** - Information and configuration settings for hello nodes in hello cluster.</span></span>

* <span data-ttu-id="29c59-136">**Aviseringar** -en logg över information, varningar och kritiska aviseringar.</span><span class="sxs-lookup"><span data-stu-id="29c59-136">**Alerts** - A log of information, warnings, and critical alerts.</span></span>

* <span data-ttu-id="29c59-137">**Admin** -stacken/programtjänster som är installerade på hello kluster, tjänstens kontoinformation och Kerberos-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="29c59-137">**Admin** - Software stack/services that are installed on hello cluster, service account information, and Kerberos security.</span></span>

* <span data-ttu-id="29c59-138">**Admin-knappen** -Ambari hantering, användarinställningar och logga ut.</span><span class="sxs-lookup"><span data-stu-id="29c59-138">**Admin button** - Ambari management, user settings, and logout.</span></span>

## <a name="monitoring"></a><span data-ttu-id="29c59-139">Övervakning</span><span class="sxs-lookup"><span data-stu-id="29c59-139">Monitoring</span></span>

### <a name="alerts"></a><span data-ttu-id="29c59-140">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="29c59-140">Alerts</span></span>

<span data-ttu-id="29c59-141">hello innehåller följande lista hello vanliga avisering statusar används av Ambari:</span><span class="sxs-lookup"><span data-stu-id="29c59-141">hello following list contains hello common alert statuses used by Ambari:</span></span>

* <span data-ttu-id="29c59-142">**OKEJ**</span><span class="sxs-lookup"><span data-stu-id="29c59-142">**OK**</span></span>
* <span data-ttu-id="29c59-143">**Varning**</span><span class="sxs-lookup"><span data-stu-id="29c59-143">**Warning**</span></span>
* <span data-ttu-id="29c59-144">**KRITISKA**</span><span class="sxs-lookup"><span data-stu-id="29c59-144">**CRITICAL**</span></span>
* <span data-ttu-id="29c59-145">**OKÄND**</span><span class="sxs-lookup"><span data-stu-id="29c59-145">**UNKNOWN**</span></span>

<span data-ttu-id="29c59-146">Aviseringar än **OK** orsaka hello **# aviseringar** post hello överst i hello sidan toodisplay hello antalet aviseringar.</span><span class="sxs-lookup"><span data-stu-id="29c59-146">Alerts other than **OK** cause hello **# alerts** entry at hello top of hello page toodisplay hello number of alerts.</span></span> <span data-ttu-id="29c59-147">Om du markerar den här posten visas hello aviseringar och deras status.</span><span class="sxs-lookup"><span data-stu-id="29c59-147">Selecting this entry displays hello alerts and their status.</span></span>

<span data-ttu-id="29c59-148">Aviseringar är uppdelade i flera standardgrupper som kan visas från hello **aviseringar** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c59-148">Alerts are organized into several default groups, which can be viewed from hello **Alerts** page.</span></span>

![Sidan varningar](./media/hdinsight-hadoop-manage-ambari/alerts.png)

<span data-ttu-id="29c59-150">Du kan hantera hello grupper med hjälp av hello **åtgärder** menyn och välja **hantera avisering grupper**.</span><span class="sxs-lookup"><span data-stu-id="29c59-150">You can manage hello groups by using hello **Actions** menu and selecting **Manage Alert Groups**.</span></span>

![Hantera aviseringar grupper dialogrutan](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

<span data-ttu-id="29c59-152">Du kan också hantera aviseringar metoder och skapa aviseringar från hello **åtgärder** menyn genom att välja __hantera avisering meddelanden__.</span><span class="sxs-lookup"><span data-stu-id="29c59-152">You can also manage alerting methods, and create alert notifications from hello **Actions** menu by selecting __Manage Alert Notifications__.</span></span> <span data-ttu-id="29c59-153">De aktuella meddelanden visas.</span><span class="sxs-lookup"><span data-stu-id="29c59-153">Any current notifications are displayed.</span></span> <span data-ttu-id="29c59-154">Du kan också skapa aviseringar här.</span><span class="sxs-lookup"><span data-stu-id="29c59-154">You can also create notifications from here.</span></span> <span data-ttu-id="29c59-155">Meddelanden kan skickas **e-post** eller **SNMP** när specifika/allvarlighetsgraden för aviseringen kombinationer inträffar.</span><span class="sxs-lookup"><span data-stu-id="29c59-155">Notifications can be sent via **EMAIL** or **SNMP** when specific alert/severity combinations occur.</span></span> <span data-ttu-id="29c59-156">Du kan exempelvis skicka ett e-postmeddelande när något av hello aviseringar i hello **YARN standard** grupp har angetts för**kritisk**.</span><span class="sxs-lookup"><span data-stu-id="29c59-156">For example, you can send an email message when any of hello alerts in hello **YARN Default** group is set too**Critical**.</span></span>

![Skapa varningsruta](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

<span data-ttu-id="29c59-158">Slutligen väljer __Hantera aviseringsinställningar__ från hello __åtgärder__ menyn kan du tooset hello antalet gånger som en avisering måste inträffa innan ett meddelande har skickats.</span><span class="sxs-lookup"><span data-stu-id="29c59-158">Finally, selecting __Manage Alert Settings__ from hello __Actions__ menu allows you tooset hello number of times an alert must occur before a notification is sent.</span></span> <span data-ttu-id="29c59-159">Den här inställningen kan vara används tooprevent meddelanden för tillfälliga fel.</span><span class="sxs-lookup"><span data-stu-id="29c59-159">This setting can be used tooprevent notifications for transient errors.</span></span>

### <a name="cluster"></a><span data-ttu-id="29c59-160">Kluster</span><span class="sxs-lookup"><span data-stu-id="29c59-160">Cluster</span></span>

<span data-ttu-id="29c59-161">Hej **mått** fliken hello instrumentpanelen innehåller ett antal widgetar som gör det enkelt toomonitor hello status på klustret i korthet.</span><span class="sxs-lookup"><span data-stu-id="29c59-161">hello **Metrics** tab of hello dashboard contains a series of widgets that make it easy toomonitor hello status of your cluster at a glance.</span></span> <span data-ttu-id="29c59-162">Flera widgetar som **CPU-användning**, innehåller ytterligare information när du klickar på.</span><span class="sxs-lookup"><span data-stu-id="29c59-162">Several widgets, such as **CPU Usage**, provide additional information when clicked.</span></span>

![instrumentpanel med mått](./media/hdinsight-hadoop-manage-ambari/metrics.png)

<span data-ttu-id="29c59-164">Hej **Heatmaps** fliken visar mått som färgade heatmaps, från grön toored.</span><span class="sxs-lookup"><span data-stu-id="29c59-164">hello **Heatmaps** tab displays metrics as colored heatmaps, going from green toored.</span></span>

![instrumentpanel med heatmaps](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

<span data-ttu-id="29c59-166">Mer information om hello noder i hello kluster, Välj **värdar**.</span><span class="sxs-lookup"><span data-stu-id="29c59-166">For more information on hello nodes within hello cluster, select **Hosts**.</span></span> <span data-ttu-id="29c59-167">Välj sedan hello viss nod som du är intresserad av.</span><span class="sxs-lookup"><span data-stu-id="29c59-167">Then select hello specific node you are interested in.</span></span>

![information om värden](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a><span data-ttu-id="29c59-169">Tjänster</span><span class="sxs-lookup"><span data-stu-id="29c59-169">Services</span></span>

<span data-ttu-id="29c59-170">Hej **Services** sidopanelen på hello instrumentpanelen ger snabb inblick i hello status för hello-tjänster som körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-170">hello **Services** sidebar on hello dashboard provides quick insight into hello status of hello services running on hello cluster.</span></span> <span data-ttu-id="29c59-171">Olika ikoner är används tooindicate status eller åtgärder som ska vidtas.</span><span class="sxs-lookup"><span data-stu-id="29c59-171">Various icons are used tooindicate status or actions that should be taken.</span></span> <span data-ttu-id="29c59-172">Till exempel visas en gul återvinning symbol om en tjänst behöver toobe återanvänds.</span><span class="sxs-lookup"><span data-stu-id="29c59-172">For example, a yellow recycle symbol is displayed if a service needs toobe recycled.</span></span>

![tjänster-sidorutan](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> <span data-ttu-id="29c59-174">hello-tjänster som visas skiljer sig åt mellan HDInsight-klustertyper och versioner.</span><span class="sxs-lookup"><span data-stu-id="29c59-174">hello services displayed differ between HDInsight cluster types and versions.</span></span> <span data-ttu-id="29c59-175">hello-tjänster som visas här är annorlunda än hello-tjänster som visas för klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-175">hello services displayed here may be different than hello services displayed for your cluster.</span></span>

<span data-ttu-id="29c59-176">Att välja en tjänst visar mer detaljerad information på hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="29c59-176">Selecting a service displays more detailed information on hello service.</span></span>

![sammanfattningsinformation för tjänsten](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a><span data-ttu-id="29c59-178">Snabblänkar</span><span class="sxs-lookup"><span data-stu-id="29c59-178">Quick links</span></span>

<span data-ttu-id="29c59-179">Vissa tjänster visas en **snabblänkar** länk hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="29c59-179">Some services display a **Quick Links** link at hello top of hello page.</span></span> <span data-ttu-id="29c59-180">Detta kan vara används tooaccess tjänstspecifika web användargränssnitt, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="29c59-180">This can be used tooaccess service-specific web UIs, such as:</span></span>

* <span data-ttu-id="29c59-181">**Jobbhistorik** -MapReduce historiken för datalagerjobb.</span><span class="sxs-lookup"><span data-stu-id="29c59-181">**Job History** - MapReduce job history.</span></span>
* <span data-ttu-id="29c59-182">**Hanteraren för filserverresurser** -YARN ResourceManager-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="29c59-182">**Resource Manager** - YARN ResourceManager UI.</span></span>
* <span data-ttu-id="29c59-183">**NameNode** -Hadoop Distributed File System (HDFS) NameNode Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="29c59-183">**NameNode** - Hadoop Distributed File System (HDFS) NameNode UI.</span></span>
* <span data-ttu-id="29c59-184">**Oozie webbgränssnittet** -Oozie-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="29c59-184">**Oozie Web UI** - Oozie UI.</span></span>

<span data-ttu-id="29c59-185">När du väljer ett av dessa länkar öppnas en ny flik i webbläsaren, som visar hello valda sidan.</span><span class="sxs-lookup"><span data-stu-id="29c59-185">Selecting any of these links opens a new tab in your browser, which displays hello selected page.</span></span>

> [!NOTE]
> <span data-ttu-id="29c59-186">Du väljer hello **snabblänkar** post för en tjänst kan returnera ett ”det gick inte att hitta” serverfel.</span><span class="sxs-lookup"><span data-stu-id="29c59-186">Selecting hello **Quick Links** entry for a service may return a "server not found" error.</span></span> <span data-ttu-id="29c59-187">Om felet uppstår, måste du använda en SSH-tunnel när du använder hello **snabblänkar** post för den här tjänsten.</span><span class="sxs-lookup"><span data-stu-id="29c59-187">If you encounter this error, you must use an SSH tunnel when using hello **Quick Links** entry for this service.</span></span> <span data-ttu-id="29c59-188">Mer information finns i [använda SSH-tunnlar med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span><span class="sxs-lookup"><span data-stu-id="29c59-188">For information, see [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md)</span></span>

## <a name="management"></a><span data-ttu-id="29c59-189">Hantering</span><span class="sxs-lookup"><span data-stu-id="29c59-189">Management</span></span>

### <a name="ambari-users-groups-and-permissions"></a><span data-ttu-id="29c59-190">Ambari användare, grupper och behörigheter</span><span class="sxs-lookup"><span data-stu-id="29c59-190">Ambari users, groups, and permissions</span></span>

<span data-ttu-id="29c59-191">Arbeta med användare, grupper och behörigheter stöds när du använder en [domänanslutna](hdinsight-domain-joined-introduction.md) HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-191">Working with users, groups, and permissions are supported when using a [domain joined](hdinsight-domain-joined-introduction.md) HDInsight cluster.</span></span> <span data-ttu-id="29c59-192">Information om hur du använder hello Ambari Management Användargränssnittet på en domänansluten klustret finns [hantera domänanslutna HDInsight-kluster](hdinsight-domain-joined-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="29c59-192">For information on using hello Ambari Management UI on a domain-joined cluster, see [Manage domain-joined HDInsight clusters](hdinsight-domain-joined-introduction.md).</span></span>

> [!WARNING]
> <span data-ttu-id="29c59-193">Ändra inte hello lösenordet för hello Ambari watchdog (hdinsightwatchdog) på Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-193">Do not change hello password of hello Ambari watchdog (hdinsightwatchdog) on your Linux-based HDInsight cluster.</span></span> <span data-ttu-id="29c59-194">Ändra hello lösenord radbrytningar hello möjlighet toouse skriptåtgärder eller utföra skalning åtgärder med ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-194">Changing hello password breaks hello ability toouse script actions or perform scaling operations with your cluster.</span></span>

### <a name="hosts"></a><span data-ttu-id="29c59-195">Värdar</span><span class="sxs-lookup"><span data-stu-id="29c59-195">Hosts</span></span>

<span data-ttu-id="29c59-196">Hej **värdar** sidan listar alla värdar i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="29c59-196">hello **Hosts** page lists all hosts in hello cluster.</span></span> <span data-ttu-id="29c59-197">toomanage värdar, Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="29c59-197">toomanage hosts, follow these steps.</span></span>

![sidan för värdar](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> <span data-ttu-id="29c59-199">Lägga till, inaktivering och recommissioning en värd får inte användas med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-199">Adding, decommissioning, and recommissioning a host should not be used with HDInsight clusters.</span></span>

1. <span data-ttu-id="29c59-200">Välj hello-värd som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="29c59-200">Select hello host that you wish toomanage.</span></span>

2. <span data-ttu-id="29c59-201">Använd hello **åtgärder** menyn tooselect hello åtgärd tooperform gärna:</span><span class="sxs-lookup"><span data-stu-id="29c59-201">Use hello **Actions** menu tooselect hello action that you wish tooperform:</span></span>

   * <span data-ttu-id="29c59-202">**Starta alla komponenter** -starta alla komponenter på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-202">**Start all components** - Start all components on hello host.</span></span>

   * <span data-ttu-id="29c59-203">**Stoppa alla komponenter** -stoppa alla komponenter på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-203">**Stop all components** - Stop all components on hello host.</span></span>

   * <span data-ttu-id="29c59-204">**Starta om alla komponenter** - stoppa och starta alla komponenter på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-204">**Restart all components** - Stop and start all components on hello host.</span></span>

   * <span data-ttu-id="29c59-205">**Aktivera underhållsläget** -Undertrycker aviseringar för hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-205">**Turn on maintenance mode** - Suppresses alerts for hello host.</span></span> <span data-ttu-id="29c59-206">Det här läget måste vara aktiverat om du utför åtgärder som genererar aviseringar.</span><span class="sxs-lookup"><span data-stu-id="29c59-206">This mode should be enabled if you are performing actions that generate alerts.</span></span> <span data-ttu-id="29c59-207">Exempelvis stoppa och starta en tjänst.</span><span class="sxs-lookup"><span data-stu-id="29c59-207">For example, stopping and starting a service.</span></span>

   * <span data-ttu-id="29c59-208">**Inaktivera underhållsläge** -returnerar hello värden toonormal aviseringar.</span><span class="sxs-lookup"><span data-stu-id="29c59-208">**Turn off maintenance mode** - Returns hello host toonormal alerting.</span></span>

   * <span data-ttu-id="29c59-209">**Stoppa** -stoppar DataNode eller NodeManagers på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-209">**Stop** - Stops DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="29c59-210">**Starta** -startar DataNode eller NodeManagers på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-210">**Start** - Starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="29c59-211">**Starta om** -stoppar och startar DataNode eller NodeManagers på hello värden.</span><span class="sxs-lookup"><span data-stu-id="29c59-211">**Restart** - Stops and starts DataNode or NodeManagers on hello host.</span></span>

   * <span data-ttu-id="29c59-212">**Inaktivera** -tar bort en värd från hello klustret.</span><span class="sxs-lookup"><span data-stu-id="29c59-212">**Decommission** - Removes a host from hello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="29c59-213">Använd inte den här åtgärden på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-213">Do not use this action on HDInsight clusters.</span></span>

   * <span data-ttu-id="29c59-214">**Recommission** -lägger till ett tidigare inaktiverade toohello värdkluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-214">**Recommission** - Adds a previously decommissioned host toohello cluster.</span></span>

     > [!NOTE]
     > <span data-ttu-id="29c59-215">Använd inte den här åtgärden på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-215">Do not use this action on HDInsight clusters.</span></span>

### <span data-ttu-id="29c59-216"><a id="service"></a>Tjänster</span><span class="sxs-lookup"><span data-stu-id="29c59-216"><a id="service"></a>Services</span></span>

<span data-ttu-id="29c59-217">Från hello **instrumentpanelen** eller **Services** använder hello **åtgärder** knappen längst ned hello hello lista över tjänster toostop och starta alla tjänster.</span><span class="sxs-lookup"><span data-stu-id="29c59-217">From hello **Dashboard** or **Services** page, use hello **Actions** button at hello bottom of hello list of services toostop and start all services.</span></span>

![tjänståtgärder](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> <span data-ttu-id="29c59-219">Medan **lägga till tjänsten** visas i den här menyn ska inte använda tooadd tjänster toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="29c59-219">While **Add Service** is listed in this menu, it should not be used tooadd services toohello HDInsight cluster.</span></span> <span data-ttu-id="29c59-220">Nya tjänster ska läggas till med en skriptåtgärd under klusteretablering.</span><span class="sxs-lookup"><span data-stu-id="29c59-220">New services should be added using a Script Action during cluster provisioning.</span></span> <span data-ttu-id="29c59-221">Mer information om hur du använder skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="29c59-221">For more information on using Script Actions, see [Customize HDInsight clusters using Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="29c59-222">När hello **åtgärder** knappen kan starta om alla tjänster, ofta du vill toostart, stoppa eller starta om en specifik tjänst.</span><span class="sxs-lookup"><span data-stu-id="29c59-222">While hello **Actions** button can restart all services, often you want toostart, stop, or restart a specific service.</span></span> <span data-ttu-id="29c59-223">Använd följande steg tooperform åtgärder på en enskild tjänst hello:</span><span class="sxs-lookup"><span data-stu-id="29c59-223">Use hello following steps tooperform actions on an individual service:</span></span>

1. <span data-ttu-id="29c59-224">Från hello **instrumentpanelen** eller **Services** väljer du en tjänst.</span><span class="sxs-lookup"><span data-stu-id="29c59-224">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="29c59-225">Från hello överkant hello **sammanfattning** använder hello **tjänståtgärder** knappen och markera hello åtgärden tootake.</span><span class="sxs-lookup"><span data-stu-id="29c59-225">From hello top of hello **Summary** tab, use hello **Service Actions** button and select hello action tootake.</span></span> <span data-ttu-id="29c59-226">Detta startar om hello-tjänsten på alla noder.</span><span class="sxs-lookup"><span data-stu-id="29c59-226">This restarts hello service on all nodes.</span></span>

    ![åtgärden på tjänsten](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > <span data-ttu-id="29c59-228">Starta om vissa tjänster medan hello klustret körs kan generera aviseringar.</span><span class="sxs-lookup"><span data-stu-id="29c59-228">Restarting some services while hello cluster is running may generate alerts.</span></span> <span data-ttu-id="29c59-229">tooavoid aviseringar, du kan använda hello **tjänståtgärder** knappen tooenable **underhållsläge** för hello-tjänsten innan du utför hello omstart.</span><span class="sxs-lookup"><span data-stu-id="29c59-229">tooavoid alerts, you can use hello **Service Actions** button tooenable **Maintenance mode** for hello service before performing hello restart.</span></span>

3. <span data-ttu-id="29c59-230">När en åtgärd har valts, hello **# op** post hello överst i hello sidan steg tooshow som en bakgrundsåtgärden pågår.</span><span class="sxs-lookup"><span data-stu-id="29c59-230">Once an action has been selected, hello **# op** entry at hello top of hello page increments tooshow that a background operation is occurring.</span></span> <span data-ttu-id="29c59-231">Om konfigurerat toodisplay visas hello listan över bakgrundsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="29c59-231">If configured toodisplay, hello list of background operations is displayed.</span></span>

   > [!NOTE]
   > <span data-ttu-id="29c59-232">Om du har aktiverat **underhållsläge** hello tjänsten kommer ihåg toodisable den med hjälp av hello **tjänståtgärder** knappen när hello-åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="29c59-232">If you enabled **Maintenance mode** for hello service, remember toodisable it by using hello **Service Actions** button once hello operation has finished.</span></span>

<span data-ttu-id="29c59-233">tooconfigure en tjänst använda hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="29c59-233">tooconfigure a service, use hello following steps:</span></span>

1. <span data-ttu-id="29c59-234">Från hello **instrumentpanelen** eller **Services** väljer du en tjänst.</span><span class="sxs-lookup"><span data-stu-id="29c59-234">From hello **Dashboard** or **Services** page, select a service.</span></span>

2. <span data-ttu-id="29c59-235">Välj hello **konfigurationerna** fliken hello aktuell konfiguration visas.</span><span class="sxs-lookup"><span data-stu-id="29c59-235">Select hello **Configs** tab. hello current configuration is displayed.</span></span> <span data-ttu-id="29c59-236">En lista över tidigare konfigurationer visas också.</span><span class="sxs-lookup"><span data-stu-id="29c59-236">A list of previous configurations is also displayed.</span></span>

    ![Konfigurationer](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. <span data-ttu-id="29c59-238">Använd hello fält visas toomodify hello konfiguration och välj sedan **spara**.</span><span class="sxs-lookup"><span data-stu-id="29c59-238">Use hello fields displayed toomodify hello configuration, and then select **Save**.</span></span> <span data-ttu-id="29c59-239">Eller välj en tidigare konfiguration och välj sedan **se aktuella** tooroll tillbaka toohello tidigare inställningar.</span><span class="sxs-lookup"><span data-stu-id="29c59-239">Or select a previous configuration and then select **Make current** tooroll back toohello previous settings.</span></span>

## <a name="ambari-views"></a><span data-ttu-id="29c59-240">Ambari-vyer</span><span class="sxs-lookup"><span data-stu-id="29c59-240">Ambari views</span></span>

<span data-ttu-id="29c59-241">Ambari-vyer kan utvecklare tooplug UI-element i hello Ambari-Webbgränssnittet med hello [Ambari vyer Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span><span class="sxs-lookup"><span data-stu-id="29c59-241">Ambari Views allow developers tooplug UI elements into hello Ambari Web UI using hello [Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views).</span></span> <span data-ttu-id="29c59-242">HDInsight tillhandahåller följande vyer med Hadoop klustertyper hello:</span><span class="sxs-lookup"><span data-stu-id="29c59-242">HDInsight provides hello following views with Hadoop cluster types:</span></span>

* <span data-ttu-id="29c59-243">Yarn Queue Manager: hello köhanteraren ger ett enkelt gränssnitt för att visa och ändra YARN köer.</span><span class="sxs-lookup"><span data-stu-id="29c59-243">Yarn Queue Manager: hello queue manager provides a simple UI for viewing and modifying YARN queues.</span></span>

* <span data-ttu-id="29c59-244">Hive vy: hello Hive-vy kan du toorun Hive-frågor direkt från din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="29c59-244">Hive View: hello Hive View allows you toorun Hive queries directly from your web browser.</span></span> <span data-ttu-id="29c59-245">Du kan spara frågor, visa resultat, och spara resultatet toohello klusterlagringen eller hämta resultaten tooyour lokalt system.</span><span class="sxs-lookup"><span data-stu-id="29c59-245">You can save queries, view results, save results toohello cluster storage, or download results tooyour local system.</span></span> <span data-ttu-id="29c59-246">Mer information om hur du använder Hive vyer finns [Använd Hive-vyer med HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span><span class="sxs-lookup"><span data-stu-id="29c59-246">For more information on using Hive Views, see [Use Hive Views with HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).</span></span>

* <span data-ttu-id="29c59-247">Tez vy: hello Tez vyn kan du toobetter förstå och optimera jobb.</span><span class="sxs-lookup"><span data-stu-id="29c59-247">Tez View: hello Tez View allows you toobetter understand and optimize jobs.</span></span> <span data-ttu-id="29c59-248">Du kan visa information om hur Tez-jobb körs och vilka resurser som används.</span><span class="sxs-lookup"><span data-stu-id="29c59-248">You can view information on how Tez jobs are executed and what resources are used.</span></span>
