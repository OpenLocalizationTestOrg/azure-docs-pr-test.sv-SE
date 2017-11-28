---
title: "Felsöka BizTalk-tjänster med hjälp av åtgärdsloggar | Microsoft Docs"
description: "Felsöka BizTalk-tjänster med hjälp av åtgärdsloggar. MABS WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c0c83361f94ffd9c30d7fcc551ff4b85ad7d6fa5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a><span data-ttu-id="e4d16-104">BizTalk-tjänst: Felsökning med åtgärdsloggar</span><span class="sxs-lookup"><span data-stu-id="e4d16-104">BizTalk Services: Troubleshoot using operation logs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="what-are-the-operation-logs"></a><span data-ttu-id="e4d16-105">Vad är den Åtgärdsloggar</span><span class="sxs-lookup"><span data-stu-id="e4d16-105">What are the Operation Logs</span></span>
<span data-ttu-id="e4d16-106">Åtgärdsloggar är en funktion för tjänster som är tillgängliga i den klassiska Azure-portalen där du kan visa historiska loggar av åtgärder som utförs på Azure-tjänster, inklusive BizTalk-tjänst.</span><span class="sxs-lookup"><span data-stu-id="e4d16-106">Operation Logs is a Management Services feature available in the Azure classic portal that allows you to view historical logs of operations performed on your Azure services, including BizTalk Services.</span></span> <span data-ttu-id="e4d16-107">På så sätt kan du visa historiska data som rör hanteringsåtgärder på prenumerationen BizTalk Service så långt tillbaka 180 dagar.</span><span class="sxs-lookup"><span data-stu-id="e4d16-107">This enables you to view historical data related to management operations on your BizTalk Service subscription as far back as 180 days.</span></span>

> [!NOTE]
> <span data-ttu-id="e4d16-108">Den här funktionen endast samlar in loggar för hanteringsåtgärder på BizTalk-tjänster, till exempel när tjänsten startades backas upp, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="e4d16-108">This feature only captures logs for management operations on BizTalk Services, such as when the service was started, backed up, and so on.</span></span> <span data-ttu-id="e4d16-109">Dessa åtgärder spåras oavsett om de utförs från den klassiska Azure-portalen eller med hjälp av den [BizTalk Service REST API: er](http://msdn.microsoft.com/library/azure/dn232347.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4d16-109">Such operations are tracked irrespective of whether they are performed from the Azure classic portal or by using the [BizTalk Service REST APIs](http://msdn.microsoft.com/library/azure/dn232347.aspx).</span></span> <span data-ttu-id="e4d16-110">En fullständig lista över åtgärder som spåras använder tjänster finns [Operations spårade med hjälp av Azure hanteringstjänster](#bizops).</span><span class="sxs-lookup"><span data-stu-id="e4d16-110">For a complete list of operations that are tracked using Management Services, see [Operations Tracked Using Azure Management Services](#bizops).</span></span><br/><br/>
> <span data-ttu-id="e4d16-111">Detta in inte loggar för aktiviteter som rör BizTalk Service runtime (t.ex (meddelande bearbetas av bryggor osv.).</span><span class="sxs-lookup"><span data-stu-id="e4d16-111">This does not capture the logs for activities related to BizTalk Service runtime (such as message processed by bridges, and so on.).</span></span> <span data-ttu-id="e4d16-112">Om du vill visa loggarna använder du vyn spårning från BizTalk-Services-portalen.</span><span class="sxs-lookup"><span data-stu-id="e4d16-112">To view these logs, use the Tracking view from the BizTalk Services portal.</span></span> <span data-ttu-id="e4d16-113">Mer information finns i [spårning av meddelanden](http://msdn.microsoft.com/library/azure/hh949805.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4d16-113">For more information, see [Tracking Messages](http://msdn.microsoft.com/library/azure/hh949805.aspx).</span></span>
> 
> 

## <a name="view-biztalk-services-operation-logs"></a><span data-ttu-id="e4d16-114">Visa BizTalk Services Åtgärdsloggar</span><span class="sxs-lookup"><span data-stu-id="e4d16-114">View BizTalk Services Operation Logs</span></span>
1. <span data-ttu-id="e4d16-115">I den klassiska Azure-portalen väljer **hanteringstjänster**, och välj sedan den **Åtgärdsloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="e4d16-115">In the Azure classic portal, select **Management Services**, and then select the **Operation Logs** tab.</span></span>
2. <span data-ttu-id="e4d16-116">Du kan filtrera loggarna baserat på olika parametrar som prenumeration, datumintervall, service-typen (t.ex. BizTalk-tjänst), tjänstnamn eller status för åtgärden (lyckades, misslyckades).</span><span class="sxs-lookup"><span data-stu-id="e4d16-116">You can filter the logs based on different parameters like subscription, date range, service type (e.g. BizTalk Services), service name, or status of the operation (Succeeded, Failed).</span></span>
3. <span data-ttu-id="e4d16-117">Välj på bockmarkeringen för att visa en filtrerad lista.</span><span class="sxs-lookup"><span data-stu-id="e4d16-117">Select the checkmark to view the filtered list.</span></span> <span data-ttu-id="e4d16-118">Följande bild visar aktiviteter relaterade till testbiztalkservice: ![visa åtgärdsloggar][ViewLogs]</span><span class="sxs-lookup"><span data-stu-id="e4d16-118">The following image shows activities related to testbiztalkservice: ![View operation logs][ViewLogs]</span></span> 
4. <span data-ttu-id="e4d16-119">Om du vill visa mer om en specifik åtgärd, Välj raden och klicka på **information** i Aktivitetsfältet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e4d16-119">To view more about a specific operation, select the row, and click **Details** in the task bar at the bottom.</span></span>

## <span data-ttu-id="e4d16-120"><a name="bizops"></a>Åtgärder som spåras med hjälp av Azure hanteringstjänster</span><span class="sxs-lookup"><span data-stu-id="e4d16-120"><a name="bizops"></a>Operations Tracked Using Azure Management Services</span></span>
<span data-ttu-id="e4d16-121">I följande tabell visas de åtgärder som spåras med Azure Management Services:</span><span class="sxs-lookup"><span data-stu-id="e4d16-121">The following table lists the operations that are tracked using the Azure Management Services:</span></span>

| <span data-ttu-id="e4d16-122">Åtgärdsnamn</span><span class="sxs-lookup"><span data-stu-id="e4d16-122">Operation Name</span></span> | <span data-ttu-id="e4d16-123">Aktivitet</span><span class="sxs-lookup"><span data-stu-id="e4d16-123">Task</span></span> |
| --- | --- |
| <span data-ttu-id="e4d16-124">CreateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-124">CreateBizTalkService</span></span> |<span data-ttu-id="e4d16-125">Åtgärden för att skapa en ny BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-125">Operation to create a new BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-126">DeleteBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-126">DeleteBizTalkService</span></span> |<span data-ttu-id="e4d16-127">Åtgärden ta bort en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-127">Operation to delete a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-128">RestartBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-128">RestartBizTalkService</span></span> |<span data-ttu-id="e4d16-129">Åtgärden för att starta om en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-129">Operation to restart a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-130">StartBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-130">StartBizTalkService</span></span> |<span data-ttu-id="e4d16-131">Åtgärden för att starta en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-131">Operation to start a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-132">StopBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-132">StopBizTalkService</span></span> |<span data-ttu-id="e4d16-133">Åtgärden för att stoppa en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-133">Operation to stop a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-134">DisableBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-134">DisableBizTalkService</span></span> |<span data-ttu-id="e4d16-135">Att inaktivera en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-135">Operation to disable a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-136">EnableBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-136">EnableBizTalkService</span></span> |<span data-ttu-id="e4d16-137">Åtgärden för att aktivera en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-137">Operation to enable a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-138">BackupBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-138">BackupBizTalkService</span></span> |<span data-ttu-id="e4d16-139">Åtgärden för att säkerhetskopiera en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-139">Operation to back up a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-140">RestoreBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-140">RestoreBizTalkService</span></span> |<span data-ttu-id="e4d16-141">Åtgärden för att återställa en BizTalk Service från angivna säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e4d16-141">Operation to restore a BizTalk Service from specified backup</span></span> |
| <span data-ttu-id="e4d16-142">SuspendBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-142">SuspendBizTalkService</span></span> |<span data-ttu-id="e4d16-143">Åtgärden pausa en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-143">Operation to suspend a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-144">ResumeBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-144">ResumeBizTalkService</span></span> |<span data-ttu-id="e4d16-145">Åtgärden återuppta en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-145">Operation to resume a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-146">ScaleBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-146">ScaleBizTalkService</span></span> |<span data-ttu-id="e4d16-147">Åtgärden att skala en BizTalk Service uppåt eller nedåt</span><span class="sxs-lookup"><span data-stu-id="e4d16-147">Operation to scale a BizTalk Service up or down</span></span> |
| <span data-ttu-id="e4d16-148">ConfigUpdateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-148">ConfigUpdateBizTalkService</span></span> |<span data-ttu-id="e4d16-149">Åtgärden för att uppdatera konfigurationen av en BizTalk Service</span><span class="sxs-lookup"><span data-stu-id="e4d16-149">Operation to update the configuration of a BizTalk Service</span></span> |
| <span data-ttu-id="e4d16-150">ServiceUpdateBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-150">ServiceUpdateBizTalkService</span></span> |<span data-ttu-id="e4d16-151">Åtgärd för att uppgradera eller nedgradera en BizTalk Service till en annan version</span><span class="sxs-lookup"><span data-stu-id="e4d16-151">Operation to upgrade or downgrade a BizTalk Service to a different version</span></span> |
| <span data-ttu-id="e4d16-152">PurgeBackupBizTalkService</span><span class="sxs-lookup"><span data-stu-id="e4d16-152">PurgeBackupBizTalkService</span></span> |<span data-ttu-id="e4d16-153">Åtgärden för att rensa säkerhetskopior av BizTalk Service gjorts utanför kvarhållningsperioden</span><span class="sxs-lookup"><span data-stu-id="e4d16-153">Operation to purge backups of the BizTalk Service outside the retention period</span></span> |

## <a name="see-also"></a><span data-ttu-id="e4d16-154">Se även</span><span class="sxs-lookup"><span data-stu-id="e4d16-154">See Also</span></span>
* [<span data-ttu-id="e4d16-155">Säkerhetskopiera BizTalk-tjänst</span><span class="sxs-lookup"><span data-stu-id="e4d16-155">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="e4d16-156">Återställa BizTalk-tjänst från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e4d16-156">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="e4d16-157">BizTalk-tjänst: Utvecklare, Basic, Standard och Premium-utgåvor diagram</span><span class="sxs-lookup"><span data-stu-id="e4d16-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="e4d16-158">BizTalk-tjänst: Etablering med hjälp av Azure klassiska portal</span><span class="sxs-lookup"><span data-stu-id="e4d16-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="e4d16-159">BizTalk Services: Etablering av statusdiagram</span><span class="sxs-lookup"><span data-stu-id="e4d16-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="e4d16-160">BizTalk Services: Flikarna Instrumentpanel, Övervakare och Skalning</span><span class="sxs-lookup"><span data-stu-id="e4d16-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="e4d16-161">BizTalk Services: Begränsning</span><span class="sxs-lookup"><span data-stu-id="e4d16-161">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="e4d16-162">BizTalk Services: Utfärdarens namn och nyckel</span><span class="sxs-lookup"><span data-stu-id="e4d16-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="e4d16-163">Hur gör jag för att börja använda Azure BizTalk Services SDK?</span><span class="sxs-lookup"><span data-stu-id="e4d16-163">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

