---
title: "Instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutningar i BizTalk-tjänst | Microsoft Docs"
description: "Läs mer om kontroller och övervaka prestanda på flikarna klassiska portalen för BizTalk-tjänst: instrumentpanelen, övervaka, skala, konfigurera och Hybridanslutningar. MABS WABS"
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
ms.openlocfilehash: 4ec88d9a681a5692b31f7e3990d1c153296b18ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="a6e20-104">Granska flikarna instrumentpanel, övervaka, skala, konfigurera och hybridanslutning</span><span class="sxs-lookup"><span data-stu-id="a6e20-104">Review the Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="a6e20-105">När du skapar BizTalk Service och distribuera ditt program kan du ändra vissa inställningar BizTalk Service och övervaka programprestanda.</span><span class="sxs-lookup"><span data-stu-id="a6e20-105">After you create your BizTalk Service and deploy your application, you can change some of the BizTalk Service settings and monitor the application performance.</span></span> 

<span data-ttu-id="a6e20-106">När du öppnar den klassiska Azure-portalen kan du placeras automatiskt på den **alla objekt** fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-106">When you open the Azure classic portal, you are automatically placed at the **ALL ITEMS** tab.</span></span> <span data-ttu-id="a6e20-107">Om du vill visa BizTalk Service, Välj din BizTalk Service i den **alla objekt** fliken eller Välj den **BIZTALK-tjänst** fliken; och välj sedan namnet på din BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-107">To view your BizTalk Service, select your BizTalk Service in the **ALL ITEMS** tab or select the **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="a6e20-108">Detta öppnar ett nytt fönster med följande flikar.</span><span class="sxs-lookup"><span data-stu-id="a6e20-108">This opens a new window with the following tabs.</span></span> <span data-ttu-id="a6e20-109">Det här avsnittet beskrivs de här flikarna.</span><span class="sxs-lookup"><span data-stu-id="a6e20-109">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="a6e20-110">Snabbstart (</span><span class="sxs-lookup"><span data-stu-id="a6e20-110">Quickstart (</span></span>![Snabbstart][Quickstart]<span data-ttu-id="a6e20-112">)</span><span class="sxs-lookup"><span data-stu-id="a6e20-112">)</span></span>
<span data-ttu-id="a6e20-113">Beroende på utgåva BizTalk-tjänster kanske inte alla alternativ som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="a6e20-113">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="a6e20-114"><strong>Skaffa dig verktyg</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-114"><strong>Get the tools</strong></span></span></td>
        <td><span data-ttu-id="a6e20-115">Hämta BizTalk Services SDK för att installera Visual Studio-projektmallar på utvecklingsdatorn lokalt.</span><span class="sxs-lookup"><span data-stu-id="a6e20-115">Download the BizTalk Services SDK to install the Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="a6e20-116">De här mallarna skapar den <strong>BizTalk-tjänst</strong> (brygga) och <strong>BizTalk-tjänstens artefakter</strong> (Transform) Visual Studio-projekt som har distribuerats till BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-116">These templates create the <strong>BizTalk Services</strong> (bridge) and the <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed to your BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="a6e20-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hur börjar jag använda Azure BizTalk Services SDK </a> och <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">installerar Azure BizTalk Services SDK</a> innehåller instruktioner om hur du kommer igång.</span><span class="sxs-lookup"><span data-stu-id="a6e20-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using the Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing the Azure BizTalk Services SDK</a> lists the steps to get started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="a6e20-118"><strong>Skapa partner avtal</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-118"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="a6e20-119">Öppnar Azure BizTalk-Services-portalen i Azure där du lägger till partner och skapa X12 AS2-, och EDI EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="a6e20-119">Opens the Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="a6e20-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> innehåller instruktioner om hur du kommer igång.</span><span class="sxs-lookup"><span data-stu-id="a6e20-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists the steps to get started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="a6e20-121"><strong>Mer information om BizTalk-tjänst</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-121"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="a6e20-122">Gå till den <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> lära dig mer om Azure BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a6e20-122">Go to the <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> to learn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="a6e20-123">I Aktivitetsfältet längst ned kan du:</span><span class="sxs-lookup"><span data-stu-id="a6e20-123">In the task bar at the bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="a6e20-124"><strong>Hantera</strong> programdistributionen</span><span class="sxs-lookup"><span data-stu-id="a6e20-124"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="a6e20-125">Öppnar BizTalk-tjänst för Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-125">Opens the Azure BizTalk Services portal.</span></span> <span data-ttu-id="a6e20-126">BizTalk-Services-portalen är ingången till EDI-konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="a6e20-126">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-127">Det här är samma som <strong>skapa partner avtal</strong> på den <strong>Snabbstart</strong> fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-127">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="a6e20-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="a6e20-129"><strong>Anslutningsinformationen</strong> av Access Control Namespace</span><span class="sxs-lookup"><span data-stu-id="a6e20-129"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="a6e20-130">När du väljer anslutningsinformationen visas Access Control Namespace, standard utfärdaren och som standard nyckel.</span><span class="sxs-lookup"><span data-stu-id="a6e20-130">When you select Connection Information, then the Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="a6e20-131">Du kan kopiera dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a6e20-131">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-132">Du kan också öppna Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-132">You can also open the Access Control Portal.</span></span> <span data-ttu-id="a6e20-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Skapa en åtkomstkontroll Namespace</a> finns mer information om Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="a6e20-134"><strong>Synkronisera nycklar</strong> i Storage-konto</span><span class="sxs-lookup"><span data-stu-id="a6e20-134"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="a6e20-135">När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel.</span><span class="sxs-lookup"><span data-stu-id="a6e20-135">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="a6e20-136">Dessa krypteringsnycklar styra åtkomsten till ditt Lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6e20-136">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="a6e20-137">BizTalk Service använder automatiskt den primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-137">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="a6e20-138"><strong>Synkronisera nycklar</strong> användarna att växla mellan primärnyckeln och sekundärnyckeln utan att störa BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-138"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-139">Till exempel du BizTalk Service att använda en ny primärnyckel för Lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a6e20-139">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="a6e20-140">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="a6e20-140">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="a6e20-141">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-141">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="a6e20-142">Välj den sekundära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-142">Select the Secondary Key.</span></span> <span data-ttu-id="a6e20-143">När du gör detta startar BizTalk Service med hjälp av den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-143">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="a6e20-144">Välj ditt lagringskonto och återskapa den primärnyckeln i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-144">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="a6e20-145">Kom ihåg att BizTalk Service med hjälp av den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-145">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="a6e20-146">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-146">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="a6e20-147">Nu väljer du den primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-147">Now, select the Primary Key.</span></span> <span data-ttu-id="a6e20-148">Det här är den nya primärnyckeln du återskapas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-148">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="a6e20-149">Välj ditt lagringskonto i den klassiska Azure-portalen och återskapa den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-149">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="a6e20-150">Den här processen kallas ”förnyelse nycklar”.</span><span class="sxs-lookup"><span data-stu-id="a6e20-150">This process is called "rollover keys".</span></span> <span data-ttu-id="a6e20-151">Syftet är att användarna ska växla mellan primärnyckeln och sekundärnyckeln utan att störa BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-151">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="a6e20-152"><strong>Ta bort</strong> ditt program</span><span class="sxs-lookup"><span data-stu-id="a6e20-152"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="a6e20-153">När du väljer Ta bort BizTalk Service och alla objekt som har distribuerats till det tas bort.</span><span class="sxs-lookup"><span data-stu-id="a6e20-153">When you select Delete, your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="a6e20-154">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="a6e20-154">Dashboard</span></span>
<span data-ttu-id="a6e20-155">Beroende på utgåva BizTalk-tjänster kanske inte alla alternativ som finns tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="a6e20-155">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="a6e20-156">När du väljer BizTalk Service namn visas fliken instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="a6e20-156">When you select your BizTalk Service name, the Dashboard tab is displayed.</span></span> <span data-ttu-id="a6e20-157">I instrumentpanelen kan du:</span><span class="sxs-lookup"><span data-stu-id="a6e20-157">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a><span data-ttu-id="a6e20-158">Översikt för användning: Visar antalet använda Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="a6e20-158">Usage Overview: Shows the number of used Hybrid Connections</span></span>
<span data-ttu-id="a6e20-159">Visar även hur data i GB.</span><span class="sxs-lookup"><span data-stu-id="a6e20-159">Also displays the data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="a6e20-160">Mått diagrammet: Visar en fast lista med prestandamått</span><span class="sxs-lookup"><span data-stu-id="a6e20-160">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="a6e20-161">De här måtten ange realtid värden om hälsotillståndet för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-161">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="a6e20-162">Du kan också välja den **relativa** eller **absolut** värden och tidsintervallet **intervall** mätvärden som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-162">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed in the graph.</span></span> 

<span data-ttu-id="a6e20-163">En beskrivning av dessa prestandamått, gå till [tillgängliga mått](#Metrics) i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-163">For a description of these performance metrics, go to [Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="a6e20-164">Snabböversikten: Visar en lista över BizTalk Service-egenskaper</span><span class="sxs-lookup"><span data-stu-id="a6e20-164">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="a6e20-165"><strong>Uppdatera autentiseringsuppgifterna för spårning av databasen</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-165"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="a6e20-166">Ändrar användarnamn och lösenord som används för att logga in på databasen för spårning.</span><span class="sxs-lookup"><span data-stu-id="a6e20-166">Changes the user name and password used to log into the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-167"><strong>Uppdatera SSL-certifikat</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-167"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="a6e20-168">Uppdatera BizTalk Service om du vill använda ett annat SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="a6e20-168">Can update the BizTalk Service to use a different SSL certificate.</span></span> <span data-ttu-id="a6e20-169">Ett självsignerat certifikat för SSL skapas automatiskt när du <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">skapa BizTalk Service</a>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-169">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create the BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-170"><strong>Hämta certifikatet</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-170"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="a6e20-171">Du kan hämta SSL-certifikatet som används av din BizTalk Service till en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="a6e20-171">You can download the SSL certificate used by your BizTalk Service to a local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-172"><strong>Status</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-172"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="a6e20-173">Visar den aktuella statusen för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-173">Displays the current status of your BizTalk Service.</span></span> <span data-ttu-id="a6e20-174">Se <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk-tjänst: tjänsten tillstånd diagram</a>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-174">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-175"><strong>Tjänst-URL</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-175"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="a6e20-176">URL till BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a6e20-176">The URL for your BizTalk Service.</span></span> <span data-ttu-id="a6e20-177">Det här är samma som den <strong>domän-URL</strong> anges när BizTalk Service skapas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-177">This is the same as the <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-178"><strong>Offentliga virtuella IP (VIP)-adressen</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-178"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="a6e20-179">Den IP-adress som tilldelats BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-179">The IP address assigned to your BizTalk Service.</span></span> <span data-ttu-id="a6e20-180">Den används för alla inkommande slutpunkter och är källadress för utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="a6e20-180">It is used for all input endpoints and is the source address for outbound traffic.</span></span> <span data-ttu-id="a6e20-181">IP-adressen hör till BizTalk Service så länge den har skapats.</span><span class="sxs-lookup"><span data-stu-id="a6e20-181">This IP address belongs to your BizTalk Service as long as it is created.</span></span> <span data-ttu-id="a6e20-182">Om du tar bort BizTalk Service har IP-adress tilldelats en annan BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-182">If you delete the BizTalk Service, the IP address is assigned to another BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-183"><strong>ACS-Namespace</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-183"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="a6e20-184">Verifierar med BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a6e20-184">Authenticates with the BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-185"><strong>Utgåva</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-185"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="a6e20-186">Visar utgåvan anges när BizTalk Service skapas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-186">Lists the Edition entered when the BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-187"><strong>Plats</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-187"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="a6e20-188">Visar den geografiska region som är värd för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-188">Displays the geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-189"><strong>Skapa</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-189"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="a6e20-190">Visar datum och tid BizTalk Service skapades.</span><span class="sxs-lookup"><span data-stu-id="a6e20-190">Displays the date and time the BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-191"><strong>Spårning av databasen</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-191"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="a6e20-192">Azure SQL Database-namnet som lagrar spårningstabeller som används av BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-192">The Azure SQL Database name that stores the tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="a6e20-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om spårning av databasen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-194"><strong>Övervaka/arkivering lagring</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-194"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="a6e20-195">Azure Storage-kontonamnet som lagrar övervakning utdata från BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-195">The Azure Storage account name that stores the monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="a6e20-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Krav för Explained</a> innehåller information om lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a6e20-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-197"><strong>Prenumerationsnamn</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-197"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="a6e20-198">Visar den prenumeration som är värd för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-198">Lists the subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="a6e20-199">Prenumerationen styr åtkomsten till den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-199">The subscription governs access to the Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-200"><strong>Prenumerations-ID</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-200"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="a6e20-201">Ett prenumerations-ID genereras automatiskt när en prenumeration har skapats.</span><span class="sxs-lookup"><span data-stu-id="a6e20-201">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="a6e20-202">När du använder REST API: er kan behöva du ange prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="a6e20-202">When using REST APIs, you may need to enter the Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="a6e20-203">[BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) visar hur du skapar en BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-203">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists the steps to create a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a><span data-ttu-id="a6e20-204">Hantera anslutningsinformationen, synkronisera nycklar, och ta bort i Aktivitetsfältet:</span><span class="sxs-lookup"><span data-stu-id="a6e20-204">Manage, Connection Information, Sync Keys, and Delete in the task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="a6e20-205"><strong>Hantera</strong> programdistributionen</span><span class="sxs-lookup"><span data-stu-id="a6e20-205"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="a6e20-206">Öppnar Azure BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-206">Opens the Azure BizTalk Services Portal.</span></span> <span data-ttu-id="a6e20-207">BizTalk-Services-portalen är ingången till EDI-konfigurationen, inklusive lägga till partner och skapa X12 AS2-, och EDIFACT-avtal.</span><span class="sxs-lookup"><span data-stu-id="a6e20-207">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-208">Det här är samma som <strong>skapa partner avtal</strong> på den <strong>Snabbstart</strong> fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-208">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="a6e20-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Konfigurera komponenter för EDI-meddelanden på BizTalk-Services-portalen</a> finns mer information om BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-210"><strong>Anslutningsinformationen</strong> av Access Control Namespace</span><span class="sxs-lookup"><span data-stu-id="a6e20-210"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="a6e20-211">Visar Access Control Namespace, standard utfärdaren och som standard nyckeln värden. som kan kopieras.</span><span class="sxs-lookup"><span data-stu-id="a6e20-211">Displays the Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-212">Du kan också öppna Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-212">You can also open the Access Control Portal.</span></span> <span data-ttu-id="a6e20-213">Access Control portalen är detsamma som att använda Active Directory-alternativet i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="a6e20-213">This Access Control Portal is the same as using the Active Directory option in the left navigation pane.</span></span>
<br/><br/><span data-ttu-id="a6e20-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hantera din ACS-Namespace</a> finns mer information om Access Control-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-215"><strong>Synkronisera nycklar</strong> i Storage-konto</span><span class="sxs-lookup"><span data-stu-id="a6e20-215"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="a6e20-216">När du skapar ett lagringskonto skapas automatiskt en primär nyckel och en sekundär nyckel.</span><span class="sxs-lookup"><span data-stu-id="a6e20-216">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="a6e20-217">Dessa krypteringsnycklar styra åtkomsten till ditt Lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a6e20-217">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="a6e20-218">BizTalk Service använder automatiskt den primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-218">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="a6e20-219"><strong>Synkronisera nycklar</strong> användarna att växla mellan primärnyckeln och sekundärnyckeln utan att störa BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-219"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="a6e20-220">Till exempel du BizTalk Service att använda en ny primärnyckel för Lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="a6e20-220">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="a6e20-221">Gör så här:</span><span class="sxs-lookup"><span data-stu-id="a6e20-221">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="a6e20-222">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-222">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="a6e20-223">Välj den sekundära nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-223">Select the Secondary Key.</span></span> <span data-ttu-id="a6e20-224">När du gör detta startar BizTalk Service med hjälp av den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-224">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="a6e20-225">Välj ditt lagringskonto och återskapa den primärnyckeln i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-225">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="a6e20-226">Kom ihåg att BizTalk Service med hjälp av den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-226">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="a6e20-227">Välj BizTalk Service och välj <strong>synkronisera nycklar</strong>.</span><span class="sxs-lookup"><span data-stu-id="a6e20-227">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="a6e20-228">Nu väljer du den primärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-228">Now, select the Primary Key.</span></span> <span data-ttu-id="a6e20-229">Det här är den nya primärnyckeln du återskapas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-229">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="a6e20-230">Välj ditt lagringskonto i den klassiska Azure-portalen och återskapa den sekundärnyckeln.</span><span class="sxs-lookup"><span data-stu-id="a6e20-230">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="a6e20-231">Den här processen kallas ”förnyelse nycklar”.</span><span class="sxs-lookup"><span data-stu-id="a6e20-231">This process is called "rollover keys".</span></span> <span data-ttu-id="a6e20-232">Syftet är att användarna ska växla mellan primärnyckeln och sekundärnyckeln utan att störa BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-232">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="a6e20-233"><strong>Ta bort</strong> ditt program</span><span class="sxs-lookup"><span data-stu-id="a6e20-233"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="a6e20-234">BizTalk Service och alla objekt som har distribuerats till det tas bort.</span><span class="sxs-lookup"><span data-stu-id="a6e20-234">Your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="a6e20-235">Övervaka</span><span class="sxs-lookup"><span data-stu-id="a6e20-235">Monitor</span></span>
<span data-ttu-id="a6e20-236">Gäller inte för den kostnadsfria versionen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-236">Does not apply to the Free Edition.</span></span>

<span data-ttu-id="a6e20-237">När du markerar du namnet på BizTalk Service övervakningsfliken är tillgängligt och visar följande:</span><span class="sxs-lookup"><span data-stu-id="a6e20-237">When you select your BizTalk Service name, the Monitor tab is available and displays the following:</span></span>

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a><span data-ttu-id="a6e20-238">: Mått visas valda prestandamått</span><span class="sxs-lookup"><span data-stu-id="a6e20-238">Metric Graph: Displays the selected performance metrics</span></span>
<span data-ttu-id="a6e20-239">De här måtten ange realtid värden om hälsotillståndet för BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-239">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="a6e20-240">Du kan välja vilka prestandamått visas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-240">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="a6e20-241">Maximalt sex prestandamått kan visas samtidigt.</span><span class="sxs-lookup"><span data-stu-id="a6e20-241">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="a6e20-242">Du kan också välja den **relativa** eller **absolut** värden och tidsintervallet **intervall** mätvärden som visas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-242">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed.</span></span> 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a><span data-ttu-id="a6e20-243">Att ta bort eller visa mått i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="a6e20-243">To remove or display metrics in the graph:</span></span>
1. <span data-ttu-id="a6e20-244">Välj den **övervakaren** fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-244">Select the **Monitor** tab.</span></span>
2. <span data-ttu-id="a6e20-245">Välj **lägga till mätvärden** i Aktivitetsfältet:</span><span class="sxs-lookup"><span data-stu-id="a6e20-245">Select **Add Metrics** in the task bar:</span></span>  
   <span data-ttu-id="a6e20-246">![Välj Lägg till mått][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="a6e20-246">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="a6e20-247">Kontrollera prestandamått som du vill visa.</span><span class="sxs-lookup"><span data-stu-id="a6e20-247">Check the performance metrics you want to display.</span></span>
4. <span data-ttu-id="a6e20-248">Välj på bockmarkeringen för att återgå till den **övervakaren** fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-248">Select the checkmark to return to the **Monitor** tab.</span></span>
5. <span data-ttu-id="a6e20-249">Välj cirkel bredvid mått att visa den mått i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-249">Select the circle next to the metric to display that metric's value in the graph.</span></span>  
   
    <span data-ttu-id="a6e20-250">Till exempel den **CPU-användning** mått är nedtonad; utdata visas inte i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="a6e20-250">For example, the **CPU Usage** metric is grayed out; its output is not displayed in the graph:</span></span>  
   <span data-ttu-id="a6e20-251">![CPU-användning mått är nedtonad][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="a6e20-251">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="a6e20-252">Välj den nedtonad ut cirkel om du vill aktivera den **CPU-användning** mått att visa dess utdata i diagrammet:</span><span class="sxs-lookup"><span data-stu-id="a6e20-252">Select the grayed out circle to enable the **CPU Usage** metric to display its output in the graph:</span></span>  
   <span data-ttu-id="a6e20-253">![CPU-användning mått är aktiverad][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="a6e20-253">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="a6e20-254">Om du vill ta bort ett mått från visa diagrammet och listan, Välj **ta bort måttet** i Aktivitetsfältet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-254">To remove a metric from the display graph and the list, select **Delete Metric** in the task bar.</span></span> <span data-ttu-id="a6e20-255">Om du vill lägga till mått tillbaka i listan, Välj **lägga till mätvärden** Kontrollera måttet i Aktivitetsfältet och välj på bockmarkeringen för att återgå till den **övervakaren** fliken.</span><span class="sxs-lookup"><span data-stu-id="a6e20-255">To add the metric back to the list, select **Add Metrics** in the task bar, check the metric, and select the checkmark to return to the **Monitor** tab.</span></span> <span data-ttu-id="a6e20-256">Välj den nedtonad ut cirkel om du vill aktivera mått.</span><span class="sxs-lookup"><span data-stu-id="a6e20-256">Select the grayed out circle to enable the metric.</span></span>

## <span data-ttu-id="a6e20-257"><a name="Metrics"></a>Tillgängliga mått</span><span class="sxs-lookup"><span data-stu-id="a6e20-257"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="a6e20-258">Följande räknare/prestandamåtten är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="a6e20-258">The following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="a6e20-259"><strong>RountdTrip svarstid</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-259"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="a6e20-260">Visar Genomsnittlig tid i millisekunder (ms) för att bearbeta ett meddelande från tidpunkten som meddelandet tas emot förrän meddelandet bearbetas fullständigt av BizTalk Service alla bryggor.</span><span class="sxs-lookup"><span data-stu-id="a6e20-260">Displays the average time taken in milliseconds (ms) to process a message from the time the message is received until the message is fully processed by the BizTalk Service across all bridges.</span></span> <span data-ttu-id="a6e20-261">Endast meddelanden som har bearbetat räknas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-261">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="a6e20-262">När följande händelser inträffar, skapas en tidsstämpel:</span><span class="sxs-lookup"><span data-stu-id="a6e20-262">When the following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="a6e20-263">Meddelandet anger gatewayen</span><span class="sxs-lookup"><span data-stu-id="a6e20-263">Message enters the gateway</span></span></li>
<li><span data-ttu-id="a6e20-264">Meddelandet dirigeras till målet</span><span class="sxs-lookup"><span data-stu-id="a6e20-264">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="a6e20-265">Mål-svar har tagits emot</span><span class="sxs-lookup"><span data-stu-id="a6e20-265">Destination response is received</span></span></li>
<li><span data-ttu-id="a6e20-266">Mål bekräftelse svar som har skickats till gatewayen</span><span class="sxs-lookup"><span data-stu-id="a6e20-266">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="a6e20-267">Det här måttet visar resultatet av följande beräkning:</span><span class="sxs-lookup"><span data-stu-id="a6e20-267">This metric shows the result of the following calculation:</span></span>
<br/><br/>
<span data-ttu-id="a6e20-268">[Mål bekräftelse svar som har skickats till gateway] - [meddelandet anger gatewayen]</span><span class="sxs-lookup"><span data-stu-id="a6e20-268">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-269"><strong>Fel vid källan</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-269"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="a6e20-270">Visar det totala antalet meddelanden som inte godkänts av BizTalk Service när hämta meddelanden från käll-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a6e20-270">Displays the total number of messages that failed by the BizTalk Service when pulling messages from the source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-271"><strong>CPU-användning</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-271"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="a6e20-272">Visar den genomsnittliga % processortid för alla rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="a6e20-272">Lists the average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-273"><strong>Svarstid för bearbetning</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-273"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="a6e20-274">Visar Genomsnittlig tid i millisekunder (ms) för att bearbeta ett meddelande av BizTalk Service alla bryggor, exklusive tid som mål.</span><span class="sxs-lookup"><span data-stu-id="a6e20-274">Displays the average time taken In milliseconds (ms) to process a message by the BizTalk Service across all bridges, excluding the time spent in destinations.</span></span> <span data-ttu-id="a6e20-275">Endast meddelanden som har bearbetat räknas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-275">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="a6e20-276">En tidsstämpel skapas när var och en av följande händelser inträffar:</span><span class="sxs-lookup"><span data-stu-id="a6e20-276">When each of the following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="a6e20-277">Meddelandet anger gatewayen</span><span class="sxs-lookup"><span data-stu-id="a6e20-277">Message enters the gateway</span></span></li>
<li><span data-ttu-id="a6e20-278">Meddelandet dirigeras till målet</span><span class="sxs-lookup"><span data-stu-id="a6e20-278">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="a6e20-279">Mål-svar har tagits emot</span><span class="sxs-lookup"><span data-stu-id="a6e20-279">Destination response is received</span></span></li>
<li><span data-ttu-id="a6e20-280">Mål bekräftelse svar som har skickats till gatewayen</span><span class="sxs-lookup"><span data-stu-id="a6e20-280">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/><span data-ttu-id="a6e20-281">Det här måttet visar resultatet av följande beräkning:</span><span class="sxs-lookup"><span data-stu-id="a6e20-281">This metric shows the result of the following calculation:</span></span><br/><br/>
<span data-ttu-id="a6e20-282">[Mål bekräftelse svar som har skickats till gateway] - [meddelandet anger gatewayen] - [mål svar] + [meddelandet dirigeras till målet]</span><span class="sxs-lookup"><span data-stu-id="a6e20-282">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway] - [Destination response is received] + [Message is routed to the destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-283"><strong>Fel i processen</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-283"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="a6e20-284">Visar det totala antalet meddelanden som misslyckades under bearbetning av BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a6e20-284">Displays the total number of messages that failed during processing by the BizTalk Service across all the bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-285"><strong>Meddelanden som skickas</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-285"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="a6e20-286">Visar det totala antalet meddelanden som skickas av BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a6e20-286">Displays the total number of messages sent by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="a6e20-287">Det här måttet ökar stegvis när ett meddelande som skickas från en pipeline når väg mål.</span><span class="sxs-lookup"><span data-stu-id="a6e20-287">This metric is incremented when a message sent from a pipeline reaches the route destination.</span></span> <span data-ttu-id="a6e20-288">Det här måttet visar inte att ett meddelande har bearbetat.</span><span class="sxs-lookup"><span data-stu-id="a6e20-288">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="a6e20-289">Måttet är i ett scenario med Request-Reply stegvis när väg mål skickar en bekräftelse för inleverans tillbaka till pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-289">In a Request-Reply scenario, the metric is incremented when the route destination sends a receipt acknowledgement back to the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-290"><strong>Mottagna meddelanden</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-290"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="a6e20-291">Visar det totala antalet meddelanden som tagits emot av BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a6e20-291">Displays the total number of messages received by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="a6e20-292">Det här måttet ökar stegvis när ett nytt meddelande tas emot av pipeline.</span><span class="sxs-lookup"><span data-stu-id="a6e20-292">This metric is incremented when a new message is received by the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-293"><strong>Meddelanden i processen</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-293"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="a6e20-294">Visar det totala antalet meddelanden som för närvarande bearbetas av BizTalk Service inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a6e20-294">Displays the total number of messages currently being processed by the BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="a6e20-295"><strong>Meddelanden som bearbetas</strong></span><span class="sxs-lookup"><span data-stu-id="a6e20-295"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="a6e20-296">Visar det totala antalet meddelanden som har behandlats av BizTalk Service över alla bryggor inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="a6e20-296">Displays the total number of messages successfully processed by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="a6e20-297">Det här måttet ökar stegvis när ett meddelande har tagits emot av pipeline och kunna vidarebefordras till målet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-297">This metric is incremented when a message is successfully received by the pipeline and successfully routed to the destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="a6e20-298">Skala</span><span class="sxs-lookup"><span data-stu-id="a6e20-298">Scale</span></span>
<span data-ttu-id="a6e20-299">Du kan lägga till eller subtraheras antalet enheter som används av BizTalk-Service på fliken Skala.</span><span class="sxs-lookup"><span data-stu-id="a6e20-299">In the Scale tab, you can add or subtract the number of units used by your BizTalk Service.</span></span> <span data-ttu-id="a6e20-300">Som standard finns en enhet är konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="a6e20-300">By default, there is one Unit configured.</span></span> <span data-ttu-id="a6e20-301">Ytterligare enheter kan läggas till skala BizTalk Service.</span><span class="sxs-lookup"><span data-stu-id="a6e20-301">Additional Units can be added to scale your BizTalk Service.</span></span> <span data-ttu-id="a6e20-302">Om du ökar skalan ökar genomströmningen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-302">When you increase the scale, you are increasing throughput.</span></span> <span data-ttu-id="a6e20-303">Mängden resurser som ökar också inklusive distribuerade platslänkbryggor, avtal, LOB-anslutningar och processorkraft.</span><span class="sxs-lookup"><span data-stu-id="a6e20-303">The amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="a6e20-304">Exempelvis kan du öka skala från 1 enhet till 2 enheter.</span><span class="sxs-lookup"><span data-stu-id="a6e20-304">For example, you increase the scale from 1 Unit to 2 Units.</span></span> <span data-ttu-id="a6e20-305">I så fall kan du distribuera dubbelt så många bryggor, dubbla avtalen, dubbla LOB-anslutningar och dubbla processorkraften.</span><span class="sxs-lookup"><span data-stu-id="a6e20-305">In this situation, you can deploy double the number of bridges, double the agreements, double the LOB connections, and double the processing power.</span></span>

<span data-ttu-id="a6e20-306">Vissa versioner av BizTalk ger inte ett alternativ för skala.</span><span class="sxs-lookup"><span data-stu-id="a6e20-306">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="a6e20-307">I så fall kan är en enhet tillåtet.</span><span class="sxs-lookup"><span data-stu-id="a6e20-307">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="a6e20-308">För att avgöra hur många enheter som den här versionen kan skalas avser [BizTalk-tjänst: utgåvor diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="a6e20-308">To determine how many units your edition can be scaled, refer to [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="a6e20-309">Öka antalet enheter som kan påverka priser.</span><span class="sxs-lookup"><span data-stu-id="a6e20-309">Increasing the number of units may impact pricing.</span></span> <span data-ttu-id="a6e20-310">Om du ökar enheterna, välja **spara** visar ett meddelande som talar om fakturering påverkas.</span><span class="sxs-lookup"><span data-stu-id="a6e20-310">If you increase the Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="a6e20-311">Du sedan välja att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="a6e20-311">You then choose to continue.</span></span> <span data-ttu-id="a6e20-312">När du ökar antalet enheter, BizTalk Service status ändras från aktiv till uppdatering.</span><span class="sxs-lookup"><span data-stu-id="a6e20-312">When you increase the number of Units, the BizTalk Service status changes from Active to Updating.</span></span> <span data-ttu-id="a6e20-313">BizTalk Service fortsätter att köras i läget uppdatering.</span><span class="sxs-lookup"><span data-stu-id="a6e20-313">In the Updating state, your BizTalk Service continues to run.</span></span>

<span data-ttu-id="a6e20-314">[BizTalk-tjänst: Utgåvor diagram](biztalk-editions-feature-chart.md) definierar ”enhet”.</span><span class="sxs-lookup"><span data-stu-id="a6e20-314">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="a6e20-315">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="a6e20-315">Configure</span></span>
<span data-ttu-id="a6e20-316">Gäller inte för Hybridanslutningar.</span><span class="sxs-lookup"><span data-stu-id="a6e20-316">Does not apply to Hybrid Connections.</span></span>

<span data-ttu-id="a6e20-317">Anger Status för säkerhetskopiering till None eller automatisk.</span><span class="sxs-lookup"><span data-stu-id="a6e20-317">Sets the Backup Status to None or Automatic.</span></span> <span data-ttu-id="a6e20-318">När inställd på None, skapas inga säkerhetskopior automatiskt.</span><span class="sxs-lookup"><span data-stu-id="a6e20-318">When set to None, no backups are automatically created.</span></span> <span data-ttu-id="a6e20-319">När inställd på automatisk, konfigurera du platsen för säkerhetskopiering, frekvens för säkerhetskopiering och hur lång tid att behålla de säkerhetskopiera filerna.</span><span class="sxs-lookup"><span data-stu-id="a6e20-319">When set to Automatic, you configure the backup location, the frequency of the backup, and how long to keep the backup files.</span></span> 

<span data-ttu-id="a6e20-320">[BizTalk-tjänst: Säkerhetskopiera och återställa](biztalk-backup-restore.md) visar information.</span><span class="sxs-lookup"><span data-stu-id="a6e20-320">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides the details.</span></span> 

## <span data-ttu-id="a6e20-321"><a name="HybridConnections"></a>Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="a6e20-321"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="a6e20-322">Hybridanslutningar ansluter ett Azure-program, t.ex. Webbappar eller Mobilappar i Azure App Service till en lokal resurs som använder en statisk TCP-port, till exempel SQL Server, MySQL, http-webb-API: er och de flesta anpassade webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a6e20-322">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, to an on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="a6e20-323">Hybridanslutningar hanteras i BizTalk-tjänst i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a6e20-323">Hybrid Connections are managed in  BizTalk Services in the Azure classic portal.</span></span>

<span data-ttu-id="a6e20-324">Om du vill skapa Hybridanslutningar i Azure App Service finns [åtkomst till lokala resurser genom att använda hybridanslutningar i Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a6e20-324">To create Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="a6e20-325">Om du vill skapa eller hantera Hybridanslutningar i Azure BizTalk Services, se [Hybridanslutningar](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a6e20-325">To create or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="a6e20-326">Nästa</span><span class="sxs-lookup"><span data-stu-id="a6e20-326">Next</span></span>
<span data-ttu-id="a6e20-327">Nu när du är bekant med de olika flikarna kan du lära dig mer om Azure BizTalk-Services-funktioner:</span><span class="sxs-lookup"><span data-stu-id="a6e20-327">Now that you're familiar with the different tabs, you can learn more about the Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="a6e20-328">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="a6e20-328">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="a6e20-329">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="a6e20-329">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="a6e20-330">BizTalk Services: Säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="a6e20-330">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="a6e20-331">Se även</span><span class="sxs-lookup"><span data-stu-id="a6e20-331">See Also</span></span>
* [<span data-ttu-id="a6e20-332">Hybridanslutningar</span><span class="sxs-lookup"><span data-stu-id="a6e20-332">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="a6e20-333">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="a6e20-333">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="a6e20-334">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="a6e20-334">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="a6e20-335">BizTalk-tjänst: BizTalk-tjänstens tillstånd-diagram</span><span class="sxs-lookup"><span data-stu-id="a6e20-335">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="a6e20-336">Hur gör jag för att börja använda Azure BizTalk Services SDK?</span><span class="sxs-lookup"><span data-stu-id="a6e20-336">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

