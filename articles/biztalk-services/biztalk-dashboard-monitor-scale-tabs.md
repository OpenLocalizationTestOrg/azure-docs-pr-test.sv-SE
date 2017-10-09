---
title: "aaaDashboard, övervaka, skala, konfigurera och Hybrid-anslutningar i BizTalk-tjänst | Microsoft Docs"
description: "Läs mer om hello kontroller och övervaka prestanda på hello klassiska portal flikar för BizTalk-tjänst: instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutningar. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="53128-104">Granska hello instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutning flikar</span><span class="sxs-lookup"><span data-stu-id="53128-104">Review hello Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="53128-105">När du skapar BizTalk Service och distribuera ditt program kan du ändra vissa inställningar för hello BizTalk-tjänst och övervaka hello programprestanda.</span><span class="sxs-lookup"><span data-stu-id="53128-105">After you create your BizTalk Service and deploy your application, you can change some of hello BizTalk Service settings and monitor hello application performance.</span></span> 

<span data-ttu-id="53128-106">När du öppnar hello klassiska Azure-portalen kan du placeras automatiskt på hello **alla objekt** fliken tooview din BizTalk Service väljer BizTalk Service i hello **alla objekt** TABB eller välj hello **BIZTALK-tjänst** fliken; och välj sedan namnet på din BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-106">When you open hello Azure classic portal, you are automatically placed at hello **ALL ITEMS** tab. tooview your BizTalk Service, select your BizTalk Service in hello **ALL ITEMS** tab or select hello **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="53128-107">Med följande flikar hello öppnas ett nytt fönster.</span><span class="sxs-lookup"><span data-stu-id="53128-107">This opens a new window with hello following tabs.</span></span> <span data-ttu-id="53128-108">Det här avsnittet beskrivs de här flikarna.</span><span class="sxs-lookup"><span data-stu-id="53128-108">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="53128-109">Snabbstart (</span><span class="sxs-lookup"><span data-stu-id="53128-109">Quickstart (</span></span>![Snabbstart][Quickstart]<span data-ttu-id="53128-111">)</span><span class="sxs-lookup"><span data-stu-id="53128-111">)</span></span>
<span data-ttu-id="53128-112">Beroende på hello BizTalk Services Edition kanske inte alla alternativ som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="53128-112">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="53128-113"><strong>Hämta hello-verktyg</strong></span><span class="sxs-lookup"><span data-stu-id="53128-113"><strong>Get hello tools</strong></span></span></td>
        <td><span data-ttu-id="53128-114">Hämta hello BizTalk Services SDK tooinstall hello Visual Studio-projektmallar på utvecklingsdatorn lokalt.</span><span class="sxs-lookup"><span data-stu-id="53128-114">Download hello BizTalk Services SDK tooinstall hello Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="53128-115">De här mallarna skapar hello <strong>BizTalk-tjänst</strong> (brygga) och hello <strong>BizTalk-tjänstens artefakter</strong> (Transform) Visual Studio-projekt som är distribuerade tooyour BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-115">These templates create hello <strong>BizTalk Services</strong> (bridge) and hello <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed tooyour BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="53128-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hur jag börja använda hello Azure BizTalk Services SDK </a> och <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installerar hello Azure BizTalk Services SDK</a> visar hello steg tooget igång.</span><span class="sxs-lookup"><span data-stu-id="53128-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using hello Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing hello Azure BizTalk Services SDK</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="53128-117"><strong>Skapa partner avtal</strong></span><span class="sxs-lookup"><span data-stu-id="53128-117"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="53128-118">Öppnas hello Azure BizTalk-Services-portalen i Azure där du lägger till partner och skapa X12 AS2-, och EDI EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="53128-118">Opens hello Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="53128-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> visar hello steg tooget igång.</span><span class="sxs-lookup"><span data-stu-id="53128-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="53128-120"><strong>Mer information om BizTalk-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="53128-120"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="53128-121">Gå toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> toolearn mer om Azure BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="53128-121">Go toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> toolearn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="53128-122">I Aktivitetsfältet längst ned hello hello kan du:</span><span class="sxs-lookup"><span data-stu-id="53128-122">In hello task bar at hello bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="53128-123"><strong>Hantera</strong> programdistributionen</span><span class="sxs-lookup"><span data-stu-id="53128-123"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="53128-124">Öppnar hello Azure BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-124">Opens hello Azure BizTalk Services portal.</span></span> <span data-ttu-id="53128-125">hello BizTalk-Services-portalen är hello ingång tooEDI konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="53128-125">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="53128-126">Detta är hello samma som <strong>skapa partner avtal</strong> på hello <strong>Snabbstart</strong> fliken.</span><span class="sxs-lookup"><span data-stu-id="53128-126">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="53128-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om hello BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="53128-128"><strong>Anslutningsinformationen</strong> av hello Access Control Namespace</span><span class="sxs-lookup"><span data-stu-id="53128-128"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="53128-129">När du väljer anslutningsinformationen hello Access Control Namespace, standard utfärdaren och som standard nyckeln visas.</span><span class="sxs-lookup"><span data-stu-id="53128-129">When you select Connection Information, then hello Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="53128-130">Du kan kopiera dessa värden.</span><span class="sxs-lookup"><span data-stu-id="53128-130">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="53128-131">Du kan också öppna hello Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-131">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="53128-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Skapa en åtkomstkontroll Namespace</a> finns mer information om hello Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="53128-133"><strong>Synkronisera nycklar</strong> i hello Storage-konto</span><span class="sxs-lookup"><span data-stu-id="53128-133"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="53128-134">När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-134">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="53128-135">Dessa krypteringsnycklar styra åtkomst tooyour Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="53128-135">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="53128-136">BizTalk Service används automatiskt hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-136">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="53128-137"><strong>Synkronisera nycklar</strong> aktivera användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-137"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="53128-138">Till exempel vill du hello BizTalk Service toouse en ny primärnyckel för hello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="53128-138">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="53128-139">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="53128-139">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="53128-140">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="53128-140">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="53128-141">Välj hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-141">Select hello Secondary Key.</span></span> <span data-ttu-id="53128-142">När du gör detta startar hello BizTalk Service med hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-142">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="53128-143">Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-143">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="53128-144">Kom ihåg att BizTalk Service använder hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-144">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="53128-145">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="53128-145">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="53128-146">Nu Välj hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-146">Now, select hello Primary Key.</span></span> <span data-ttu-id="53128-147">Detta är hello nya primärnyckeln som du har återskapats.</span><span class="sxs-lookup"><span data-stu-id="53128-147">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="53128-148">Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-148">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="53128-149">Den här processen kallas ”förnyelse nycklar”.</span><span class="sxs-lookup"><span data-stu-id="53128-149">This process is called "rollover keys".</span></span> <span data-ttu-id="53128-150">hello syftet är tooenable användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-150">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="53128-151"><strong>Ta bort</strong> ditt program</span><span class="sxs-lookup"><span data-stu-id="53128-151"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="53128-152">När du väljer Ta bort BizTalk Service och alla objekt som har distribuerats tooit tas bort.</span><span class="sxs-lookup"><span data-stu-id="53128-152">When you select Delete, your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="53128-153">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="53128-153">Dashboard</span></span>
<span data-ttu-id="53128-154">Beroende på hello BizTalk Services Edition kanske inte alla alternativ som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="53128-154">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="53128-155">När du markerar du namnet på BizTalk Service visas hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="53128-155">When you select your BizTalk Service name, hello Dashboard tab is displayed.</span></span> <span data-ttu-id="53128-156">I instrumentpanelen kan du:</span><span class="sxs-lookup"><span data-stu-id="53128-156">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a><span data-ttu-id="53128-157">Översikt för användning: Visar hello antalet använda Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="53128-157">Usage Overview: Shows hello number of used Hybrid Connections</span></span>
<span data-ttu-id="53128-158">Visar även hello dataanvändning i GB.</span><span class="sxs-lookup"><span data-stu-id="53128-158">Also displays hello data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="53128-159">Mått diagrammet: Visar en fast lista med prestandamått</span><span class="sxs-lookup"><span data-stu-id="53128-159">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="53128-160">De här måtten ange realtid värden om hello hälsotillstånd hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-160">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="53128-161">Du kan också välja hello **relativa** eller **absolut** värden och hello tidsintervallet **intervall** av hello mått som visas i diagrammet hello.</span><span class="sxs-lookup"><span data-stu-id="53128-161">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed in hello graph.</span></span> 

<span data-ttu-id="53128-162">En beskrivning av dessa prestandamått gå för[tillgängliga mått](#Metrics) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="53128-162">For a description of these performance metrics, go too[Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="53128-163">Snabböversikten: Visar en lista över BizTalk Service-egenskaper</span><span class="sxs-lookup"><span data-stu-id="53128-163">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="53128-164"><strong>Uppdatera autentiseringsuppgifterna för spårning av databasen</strong></span><span class="sxs-lookup"><span data-stu-id="53128-164"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="53128-165">Ändringar hello användarnamnet och lösenordet som används för toolog i hello spårning av databasen.</span><span class="sxs-lookup"><span data-stu-id="53128-165">Changes hello user name and password used toolog into hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-166"><strong>Uppdatera SSL-certifikat</strong></span><span class="sxs-lookup"><span data-stu-id="53128-166"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="53128-167">Uppdatera hello BizTalk Service toouse ett annat SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="53128-167">Can update hello BizTalk Service toouse a different SSL certificate.</span></span> <span data-ttu-id="53128-168">Ett självsignerat certifikat för SSL skapas automatiskt när du <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">skapa hello BizTalk Service</a>.</span><span class="sxs-lookup"><span data-stu-id="53128-168">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create hello BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-169"><strong>Hämta certifikatet</strong></span><span class="sxs-lookup"><span data-stu-id="53128-169"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="53128-170">Du kan hämta hello SSL-certifikatet som används av din lokala dator BizTalk Service tooa.</span><span class="sxs-lookup"><span data-stu-id="53128-170">You can download hello SSL certificate used by your BizTalk Service tooa local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-171"><strong>Status</strong></span><span class="sxs-lookup"><span data-stu-id="53128-171"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="53128-172">Visar hello aktuell status för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-172">Displays hello current status of your BizTalk Service.</span></span> <span data-ttu-id="53128-173">Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-tjänst: tjänsten tillstånd diagram</a>.</span><span class="sxs-lookup"><span data-stu-id="53128-173">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="53128-174"><strong>Tjänst-URL</strong></span><span class="sxs-lookup"><span data-stu-id="53128-174"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="53128-175">hello-URL för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-175">hello URL for your BizTalk Service.</span></span> <span data-ttu-id="53128-176">Detta är hello samma som hello <strong>domän-URL</strong> anges när BizTalk Service skapas.</span><span class="sxs-lookup"><span data-stu-id="53128-176">This is hello same as hello <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-177"><strong>Offentliga virtuella IP (VIP)-adressen</strong></span><span class="sxs-lookup"><span data-stu-id="53128-177"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="53128-178">hello IP-adress som tilldelats tooyour BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-178">hello IP address assigned tooyour BizTalk Service.</span></span> <span data-ttu-id="53128-179">Den används för alla inkommande slutpunkter och är hello källadress för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="53128-179">It is used for all input endpoints and is hello source address for outbound traffic.</span></span> <span data-ttu-id="53128-180">IP-adressen hör tooyour BizTalk Service så länge den har skapats.</span><span class="sxs-lookup"><span data-stu-id="53128-180">This IP address belongs tooyour BizTalk Service as long as it is created.</span></span> <span data-ttu-id="53128-181">Om du tar bort hello BizTalk Service tilldelas hello IP-adress tooanother BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-181">If you delete hello BizTalk Service, hello IP address is assigned tooanother BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-182"><strong>ACS-Namespace</strong></span><span class="sxs-lookup"><span data-stu-id="53128-182"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="53128-183">Verifierar med hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-183">Authenticates with hello BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-184"><strong>Utgåva</strong></span><span class="sxs-lookup"><span data-stu-id="53128-184"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="53128-185">Visar hello Edition anges när hello BizTalk Service skapas.</span><span class="sxs-lookup"><span data-stu-id="53128-185">Lists hello Edition entered when hello BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-186"><strong>Plats</strong></span><span class="sxs-lookup"><span data-stu-id="53128-186"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="53128-187">Visar hello geografiska region som är värd för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-187">Displays hello geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-188"><strong>Skapa</strong></span><span class="sxs-lookup"><span data-stu-id="53128-188"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="53128-189">Visar hello datum och tid hello BizTalk Service har skapats.</span><span class="sxs-lookup"><span data-stu-id="53128-189">Displays hello date and time hello BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-190"><strong>Spårning av databasen</strong></span><span class="sxs-lookup"><span data-stu-id="53128-190"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="53128-191">hello Azure SQL Database namn som lagrar hello spårning tabeller som används av BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-191">hello Azure SQL Database name that stores hello tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="53128-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om hello spårning av databasen.</span><span class="sxs-lookup"><span data-stu-id="53128-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-193"><strong>Övervaka/arkivering lagring</strong></span><span class="sxs-lookup"><span data-stu-id="53128-193"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="53128-194">hello Azure Storage kontonamn som lagrar hello övervakning utdata från BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-194">hello Azure Storage account name that stores hello monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="53128-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om hello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="53128-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-196"><strong>Prenumerationsnamn</strong></span><span class="sxs-lookup"><span data-stu-id="53128-196"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="53128-197">Visar hello-prenumeration som är värd för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-197">Lists hello subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="53128-198">hello prenumeration styr åtkomst toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-198">hello subscription governs access toohello Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-199"><strong>Prenumerations-ID</strong></span><span class="sxs-lookup"><span data-stu-id="53128-199"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="53128-200">Ett prenumerations-ID genereras automatiskt när en prenumeration har skapats.</span><span class="sxs-lookup"><span data-stu-id="53128-200">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="53128-201">När du använder REST API: er kan behöva du tooenter hello prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="53128-201">When using REST APIs, you may need tooenter hello Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="53128-202">[BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) visar hello steg toocreate en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-202">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists hello steps toocreate a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a><span data-ttu-id="53128-203">Hantera anslutningsinformationen, synkronisera nycklar, och ta bort i Aktivitetsfältet hello:</span><span class="sxs-lookup"><span data-stu-id="53128-203">Manage, Connection Information, Sync Keys, and Delete in hello task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="53128-204"><strong>Hantera</strong> programdistributionen</span><span class="sxs-lookup"><span data-stu-id="53128-204"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="53128-205">Öppnar hello Azure BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-205">Opens hello Azure BizTalk Services Portal.</span></span> <span data-ttu-id="53128-206">hello BizTalk-Services-portalen är hello ingång tooEDI konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="53128-206">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="53128-207">Detta är hello samma som <strong>skapa partner avtal</strong> på hello <strong>Snabbstart</strong> fliken.</span><span class="sxs-lookup"><span data-stu-id="53128-207">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="53128-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om hello BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-209"><strong>Anslutningsinformationen</strong> av hello Access Control Namespace</span><span class="sxs-lookup"><span data-stu-id="53128-209"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="53128-210">Visar hello Access Control Namespace, standard utfärdare och standard nyckeln värden. som kan kopieras.</span><span class="sxs-lookup"><span data-stu-id="53128-210">Displays hello Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="53128-211">Du kan också öppna hello Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-211">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="53128-212">Access Control portalen är hello samma som med hello Active Directory på hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="53128-212">This Access Control Portal is hello same as using hello Active Directory option in hello left navigation pane.</span></span>
<br/><br/><span data-ttu-id="53128-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hantera din ACS-Namespace</a> finns mer information om hello Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-214"><strong>Synkronisera nycklar</strong> i hello Storage-konto</span><span class="sxs-lookup"><span data-stu-id="53128-214"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="53128-215">När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-215">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="53128-216">Dessa krypteringsnycklar styra åtkomst tooyour Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="53128-216">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="53128-217">BizTalk Service används automatiskt hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-217">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="53128-218"><strong>Synkronisera nycklar</strong> aktivera användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-218"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="53128-219">Till exempel vill du hello BizTalk Service toouse en ny primärnyckel för hello Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="53128-219">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="53128-220">toodo detta:</span><span class="sxs-lookup"><span data-stu-id="53128-220">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="53128-221">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="53128-221">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="53128-222">Välj hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-222">Select hello Secondary Key.</span></span> <span data-ttu-id="53128-223">När du gör detta startar hello BizTalk Service med hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-223">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="53128-224">Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-224">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="53128-225">Kom ihåg att BizTalk Service använder hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-225">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="53128-226">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="53128-226">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="53128-227">Nu Välj hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="53128-227">Now, select hello Primary Key.</span></span> <span data-ttu-id="53128-228">Detta är hello nya primärnyckeln som du har återskapats.</span><span class="sxs-lookup"><span data-stu-id="53128-228">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="53128-229">Välj ditt lagringskonto i hello klassiska Azure-portalen och återskapa hello sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="53128-229">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="53128-230">Den här processen kallas ”förnyelse nycklar”.</span><span class="sxs-lookup"><span data-stu-id="53128-230">This process is called "rollover keys".</span></span> <span data-ttu-id="53128-231">hello syftet är tooenable användare tooswitch mellan hello primärnyckel och hello sekundärnyckeln utan att störa hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-231">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="53128-232"><strong>Ta bort</strong> ditt program</span><span class="sxs-lookup"><span data-stu-id="53128-232"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="53128-233">BizTalk Service och alla objekt som har distribuerats tooit tas bort.</span><span class="sxs-lookup"><span data-stu-id="53128-233">Your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="53128-234">Övervaka</span><span class="sxs-lookup"><span data-stu-id="53128-234">Monitor</span></span>
<span data-ttu-id="53128-235">Gäller inte toohello Free Edition.</span><span class="sxs-lookup"><span data-stu-id="53128-235">Does not apply toohello Free Edition.</span></span>

<span data-ttu-id="53128-236">När du markerar du namnet på BizTalk Service hello övervakningsfliken är tillgängligt och visar hello följande:</span><span class="sxs-lookup"><span data-stu-id="53128-236">When you select your BizTalk Service name, hello Monitor tab is available and displays hello following:</span></span>

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a><span data-ttu-id="53128-237">Mått diagram: Visar hello valt prestandamått</span><span class="sxs-lookup"><span data-stu-id="53128-237">Metric Graph: Displays hello selected performance metrics</span></span>
<span data-ttu-id="53128-238">De här måtten ange realtid värden om hello hälsotillstånd hello BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-238">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="53128-239">Du kan välja vilka prestandamått visas.</span><span class="sxs-lookup"><span data-stu-id="53128-239">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="53128-240">Maximalt sex prestandamått kan visas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="53128-240">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="53128-241">Du kan också välja hello **relativa** eller **absolut** värden och hello tidsintervallet **intervall** av hello mått som visas.</span><span class="sxs-lookup"><span data-stu-id="53128-241">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed.</span></span> 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a><span data-ttu-id="53128-242">tooremove eller visa mått i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="53128-242">tooremove or display metrics in hello graph:</span></span>
1. <span data-ttu-id="53128-243">Välj hello **övervakaren** fliken.</span><span class="sxs-lookup"><span data-stu-id="53128-243">Select hello **Monitor** tab.</span></span>
2. <span data-ttu-id="53128-244">Välj **lägga till mätvärden** i Aktivitetsfältet hello:</span><span class="sxs-lookup"><span data-stu-id="53128-244">Select **Add Metrics** in hello task bar:</span></span>  
   <span data-ttu-id="53128-245">![Välj Lägg till mått][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="53128-245">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="53128-246">Kontrollera hello prestandamått som du vill toodisplay.</span><span class="sxs-lookup"><span data-stu-id="53128-246">Check hello performance metrics you want toodisplay.</span></span>
4. <span data-ttu-id="53128-247">Välj hello markering tooreturn toohello **övervakaren** fliken.</span><span class="sxs-lookup"><span data-stu-id="53128-247">Select hello checkmark tooreturn toohello **Monitor** tab.</span></span>
5. <span data-ttu-id="53128-248">Välj hello cirkel nästa toohello mått toodisplay som mått värde i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="53128-248">Select hello circle next toohello metric toodisplay that metric's value in hello graph.</span></span>  
   
    <span data-ttu-id="53128-249">Till exempel hello **CPU-användning** mått är nedtonad; utdata visas inte i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="53128-249">For example, hello **CPU Usage** metric is grayed out; its output is not displayed in hello graph:</span></span>  
   <span data-ttu-id="53128-250">![CPU-användning mått är nedtonad][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="53128-250">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="53128-251">Välj hello nedtonad cirkel tooenable hello **CPU-användning** mått toodisplay utdata i hello diagram:</span><span class="sxs-lookup"><span data-stu-id="53128-251">Select hello grayed out circle tooenable hello **CPU Usage** metric toodisplay its output in hello graph:</span></span>  
   <span data-ttu-id="53128-252">![CPU-användning mått är aktiverad][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="53128-252">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="53128-253">tooremove ett mått från hello visa diagrammet och hello listan, Välj **ta bort måttet** i hello Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="53128-253">tooremove a metric from hello display graph and hello list, select **Delete Metric** in hello task bar.</span></span> <span data-ttu-id="53128-254">tooadd hello mått tillbaka toohello listan markerar **lägga till mätvärden** i Aktivitetsfältet hello Kontrollera hello mått och välj hello markering tooreturn toohello **övervakaren** fliken. Välj hello nedtonade cirkel tooenable hello mått.</span><span class="sxs-lookup"><span data-stu-id="53128-254">tooadd hello metric back toohello list, select **Add Metrics** in hello task bar, check hello metric, and select hello checkmark tooreturn toohello **Monitor** tab. Select hello grayed out circle tooenable hello metric.</span></span>

## <span data-ttu-id="53128-255"><a name="Metrics"></a>Tillgängliga mått</span><span class="sxs-lookup"><span data-stu-id="53128-255"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="53128-256">hello följande räknare/prestandamått är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="53128-256">hello following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="53128-257"><strong>RountdTrip svarstid</strong></span><span class="sxs-lookup"><span data-stu-id="53128-257"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="53128-258">Visar hello Genomsnittlig tid i millisekunder (ms) tooprocess ett meddelande från hello tid hello-meddelande tas emot förrän hello-meddelande bearbetas fullständigt av hello BizTalk Service över alla bryggor.</span><span class="sxs-lookup"><span data-stu-id="53128-258">Displays hello average time taken in milliseconds (ms) tooprocess a message from hello time hello message is received until hello message is fully processed by hello BizTalk Service across all bridges.</span></span> <span data-ttu-id="53128-259">Endast meddelanden som har bearbetat räknas.</span><span class="sxs-lookup"><span data-stu-id="53128-259">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="53128-260">En tidsstämpel skapas när hello följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="53128-260">When hello following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="53128-261">Meddelandet anger hello gateway</span><span class="sxs-lookup"><span data-stu-id="53128-261">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="53128-262">Meddelandet är routade toohello mål</span><span class="sxs-lookup"><span data-stu-id="53128-262">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="53128-263">Mål-svar har tagits emot</span><span class="sxs-lookup"><span data-stu-id="53128-263">Destination response is received</span></span></li>
<li><span data-ttu-id="53128-264">Mål bekräftelse svaret toohello gateway</span><span class="sxs-lookup"><span data-stu-id="53128-264">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="53128-265">Det här måttet visar hello resultatet av hello följande beräkning:</span><span class="sxs-lookup"><span data-stu-id="53128-265">This metric shows hello result of hello following calculation:</span></span>
<br/><br/>
<span data-ttu-id="53128-266">[Mål bekräftelse svaret toohello gateway] - [meddelandet anger hello gateway]</span><span class="sxs-lookup"><span data-stu-id="53128-266">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-267"><strong>Fel vid källan</strong></span><span class="sxs-lookup"><span data-stu-id="53128-267"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="53128-268">Visar hello sammanlagt antal meddelanden som inte godkänts av hello BizTalk Service när hämta meddelanden från hello källa slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="53128-268">Displays hello total number of messages that failed by hello BizTalk Service when pulling messages from hello source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-269"><strong>CPU-användning</strong></span><span class="sxs-lookup"><span data-stu-id="53128-269"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="53128-270">Visar hello genomsnittlig processortid i procent för alla rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="53128-270">Lists hello average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-271"><strong>Svarstid för bearbetning</strong></span><span class="sxs-lookup"><span data-stu-id="53128-271"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="53128-272">Visar hello Genomsnittlig tid i millisekunder (ms) tooprocess ett meddelande av hello BizTalk Service över alla bryggor, exklusive hello tid som ägnats åt mål.</span><span class="sxs-lookup"><span data-stu-id="53128-272">Displays hello average time taken In milliseconds (ms) tooprocess a message by hello BizTalk Service across all bridges, excluding hello time spent in destinations.</span></span> <span data-ttu-id="53128-273">Endast meddelanden som har bearbetat räknas.</span><span class="sxs-lookup"><span data-stu-id="53128-273">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="53128-274">En tidsstämpel skapas när var och en av hello följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="53128-274">When each of hello following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="53128-275">Meddelandet anger hello gateway</span><span class="sxs-lookup"><span data-stu-id="53128-275">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="53128-276">Meddelandet är routade toohello mål</span><span class="sxs-lookup"><span data-stu-id="53128-276">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="53128-277">Mål-svar har tagits emot</span><span class="sxs-lookup"><span data-stu-id="53128-277">Destination response is received</span></span></li>
<li><span data-ttu-id="53128-278">Mål bekräftelse svaret toohello gateway</span><span class="sxs-lookup"><span data-stu-id="53128-278">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/><span data-ttu-id="53128-279">Det här måttet visar hello resultatet av hello följande beräkning:</span><span class="sxs-lookup"><span data-stu-id="53128-279">This metric shows hello result of hello following calculation:</span></span><br/><br/>
<span data-ttu-id="53128-280">[Mål bekräftelse svaret toohello gateway] - [meddelandet anger hello gateway] - [mål svar] + [meddelandet är routade toohello mål]</span><span class="sxs-lookup"><span data-stu-id="53128-280">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway] - [Destination response is received] + [Message is routed toohello destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-281"><strong>Fel i processen</strong></span><span class="sxs-lookup"><span data-stu-id="53128-281"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="53128-282">Visar hello sammanlagt antal meddelanden som misslyckades under bearbetning av hello BizTalk Service över alla hello bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="53128-282">Displays hello total number of messages that failed during processing by hello BizTalk Service across all hello bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-283"><strong>Meddelanden som skickas</strong></span><span class="sxs-lookup"><span data-stu-id="53128-283"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="53128-284">Visar hello Totalt antal meddelanden som skickas av hello BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="53128-284">Displays hello total number of messages sent by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="53128-285">Det här måttet ökar stegvis när ett meddelande som skickas från en pipeline når hello väg mål.</span><span class="sxs-lookup"><span data-stu-id="53128-285">This metric is incremented when a message sent from a pipeline reaches hello route destination.</span></span> <span data-ttu-id="53128-286">Det här måttet visar inte att ett meddelande har bearbetat.</span><span class="sxs-lookup"><span data-stu-id="53128-286">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="53128-287">I ett scenario med Request-Reply ökas hello mått när hello väg mål skickar en inleverans bekräftelse tillbaka toohello pipeline.</span><span class="sxs-lookup"><span data-stu-id="53128-287">In a Request-Reply scenario, hello metric is incremented when hello route destination sends a receipt acknowledgement back toohello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-288"><strong>Mottagna meddelanden</strong></span><span class="sxs-lookup"><span data-stu-id="53128-288"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="53128-289">Visar hello Totalt antal meddelanden mottagna genom hello BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="53128-289">Displays hello total number of messages received by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="53128-290">Det här måttet ökar stegvis när ett nytt meddelande tas emot av hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="53128-290">This metric is incremented when a new message is received by hello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-291"><strong>Meddelanden i processen</strong></span><span class="sxs-lookup"><span data-stu-id="53128-291"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="53128-292">Visar hello sammanlagt antal meddelanden som för närvarande bearbetas av hello BizTalk Service inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="53128-292">Displays hello total number of messages currently being processed by hello BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="53128-293"><strong>Meddelanden som bearbetas</strong></span><span class="sxs-lookup"><span data-stu-id="53128-293"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="53128-294">Visar hello Totalt antal meddelanden som har behandlats av hello BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="53128-294">Displays hello total number of messages successfully processed by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="53128-295">Det här måttet ökar stegvis när ett meddelande har tas emot av hello pipeline och har routade toohello mål.</span><span class="sxs-lookup"><span data-stu-id="53128-295">This metric is incremented when a message is successfully received by hello pipeline and successfully routed toohello destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="53128-296">Skala</span><span class="sxs-lookup"><span data-stu-id="53128-296">Scale</span></span>
<span data-ttu-id="53128-297">I hello skala, du lägger till eller subtrahera hello antalet enheter som används av BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-297">In hello Scale tab, you can add or subtract hello number of units used by your BizTalk Service.</span></span> <span data-ttu-id="53128-298">Som standard finns en enhet är konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="53128-298">By default, there is one Unit configured.</span></span> <span data-ttu-id="53128-299">Ytterligare enheter kan läggas till tooscale BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="53128-299">Additional Units can be added tooscale your BizTalk Service.</span></span> <span data-ttu-id="53128-300">Om du ökar hello skala ökar genomströmningen.</span><span class="sxs-lookup"><span data-stu-id="53128-300">When you increase hello scale, you are increasing throughput.</span></span> <span data-ttu-id="53128-301">hello mängden resurser ökar också inklusive distribuerade platslänkbryggor, avtal, LOB-anslutningar och processorkraft.</span><span class="sxs-lookup"><span data-stu-id="53128-301">hello amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="53128-302">Exempelvis kan du öka hello skala från 1 enhet too2 enheter.</span><span class="sxs-lookup"><span data-stu-id="53128-302">For example, you increase hello scale from 1 Unit too2 Units.</span></span> <span data-ttu-id="53128-303">I så fall kan distribuera du dubbla hello antalet platslänkbryggor, dubbla hello avtal, dubbla hello LOB-anslutningar och dubbla hello processorkraft.</span><span class="sxs-lookup"><span data-stu-id="53128-303">In this situation, you can deploy double hello number of bridges, double hello agreements, double hello LOB connections, and double hello processing power.</span></span>

<span data-ttu-id="53128-304">Vissa versioner av BizTalk ger inte ett alternativ för skala.</span><span class="sxs-lookup"><span data-stu-id="53128-304">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="53128-305">I så fall kan är en enhet tillåtet.</span><span class="sxs-lookup"><span data-stu-id="53128-305">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="53128-306">toodetermine hur många enheter som den här versionen kan skalas för referera[BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="53128-306">toodetermine how many units your edition can be scaled, refer too[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="53128-307">Öka hello antalet enheter som kan påverka priser.</span><span class="sxs-lookup"><span data-stu-id="53128-307">Increasing hello number of units may impact pricing.</span></span> <span data-ttu-id="53128-308">Om du ökar hello enheter, väljer du **spara** visar ett meddelande som talar om fakturering påverkas.</span><span class="sxs-lookup"><span data-stu-id="53128-308">If you increase hello Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="53128-309">Sedan kan du välja toocontinue.</span><span class="sxs-lookup"><span data-stu-id="53128-309">You then choose toocontinue.</span></span> <span data-ttu-id="53128-310">Om du ökar hello antalet enheter ändras hello BizTalk Service status från aktivt tooUpdating.</span><span class="sxs-lookup"><span data-stu-id="53128-310">When you increase hello number of Units, hello BizTalk Service status changes from Active tooUpdating.</span></span> <span data-ttu-id="53128-311">I hello uppdaterar tillstånd fortsätter BizTalk Service toorun.</span><span class="sxs-lookup"><span data-stu-id="53128-311">In hello Updating state, your BizTalk Service continues toorun.</span></span>

<span data-ttu-id="53128-312">[BizTalk-tjänst: Utgåvor diagram](biztalk-editions-feature-chart.md) definierar ”enhet”.</span><span class="sxs-lookup"><span data-stu-id="53128-312">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="53128-313">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="53128-313">Configure</span></span>
<span data-ttu-id="53128-314">Gäller inte tooHybrid anslutningar.</span><span class="sxs-lookup"><span data-stu-id="53128-314">Does not apply tooHybrid Connections.</span></span>

<span data-ttu-id="53128-315">Anger hello Status för säkerhetskopiering tooNone eller automatisk.</span><span class="sxs-lookup"><span data-stu-id="53128-315">Sets hello Backup Status tooNone or Automatic.</span></span> <span data-ttu-id="53128-316">Om värdet är tooNone, inga säkerhetskopior skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="53128-316">When set tooNone, no backups are automatically created.</span></span> <span data-ttu-id="53128-317">Ange tooAutomatic, när du konfigurerar hello säkerhetskopieringsplatsen, hello ofta hello säkerhetskopiering och hur länge tookeep hello säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="53128-317">When set tooAutomatic, you configure hello backup location, hello frequency of hello backup, and how long tookeep hello backup files.</span></span> 

<span data-ttu-id="53128-318">[BizTalk-tjänst: Säkerhetskopiera och återställa](biztalk-backup-restore.md) innehåller hello information.</span><span class="sxs-lookup"><span data-stu-id="53128-318">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides hello details.</span></span> 

## <span data-ttu-id="53128-319"><a name="HybridConnections"></a>Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="53128-319"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="53128-320">Hybridanslutningar ansluter ett Azure-program, som Webbappar eller Mobilappar i Azure App Service, tooan lokala resursen som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och mest anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="53128-320">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, tooan on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="53128-321">Hybridanslutningar hanteras i BizTalk-tjänst i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="53128-321">Hybrid Connections are managed in  BizTalk Services in hello Azure classic portal.</span></span>

<span data-ttu-id="53128-322">toocreate Hybridanslutningar i Azure App Service finns [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="53128-322">toocreate Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="53128-323">toocreate eller hantera Hybridanslutningar i Azure BizTalk Services, se [Hybridanslutningar](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="53128-323">toocreate or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="53128-324">Nästa</span><span class="sxs-lookup"><span data-stu-id="53128-324">Next</span></span>
<span data-ttu-id="53128-325">Nu när du är bekant med hello olika flikarna, du kan lära dig mer om hello Azure BizTalk-Services-funktioner:</span><span class="sxs-lookup"><span data-stu-id="53128-325">Now that you're familiar with hello different tabs, you can learn more about hello Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="53128-326">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="53128-326">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="53128-327">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="53128-327">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="53128-328">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="53128-328">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="53128-329">Se även</span><span class="sxs-lookup"><span data-stu-id="53128-329">See Also</span></span>
* [<span data-ttu-id="53128-330">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="53128-330">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="53128-331">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="53128-331">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="53128-332">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="53128-332">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="53128-333">BizTalk-tjänst: BizTalk-tjänstens tillstånd-diagram</span><span class="sxs-lookup"><span data-stu-id="53128-333">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="53128-334">Hur jag börja använda hello Azure BizTalk Services SDK</span><span class="sxs-lookup"><span data-stu-id="53128-334">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

