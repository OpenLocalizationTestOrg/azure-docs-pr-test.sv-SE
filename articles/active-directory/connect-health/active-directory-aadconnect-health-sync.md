---
title: aaaUsing Azure AD Connect Health med synkronisering | Microsoft Docs
description: "Detta är hello Azure AD Connect Health sida som innehåller information om hur toomonitor Azure AD Connect synkroniserar."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
ms.assetid: 1dfbeaba-bda2-4f68-ac89-1dbfaf5b4015
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 56f0582be30e664026cedf15350bc23501998bfd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-ad-connect-sync-with-azure-ad-connect-health"></a><span data-ttu-id="f47b8-103">Övervaka Azure AD Connect-synkronisering med Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="f47b8-103">Monitor Azure AD Connect sync with Azure AD Connect Health</span></span>
<span data-ttu-id="f47b8-104">hello följande dokumentation är specifik toomonitoring Azure AD Connect (synkronisering) med Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="f47b8-104">hello following documentation is specific toomonitoring Azure AD Connect (Sync) with Azure AD Connect Health.</span></span>  <span data-ttu-id="f47b8-105">Information om övervakning av AD FS med Azure AD Connect Health finns i [Använda Azure AD Connect Health med AD FS](active-directory-aadconnect-health-adfs.md).</span><span class="sxs-lookup"><span data-stu-id="f47b8-105">For information on monitoring AD FS with Azure AD Connect Health see [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md).</span></span> <span data-ttu-id="f47b8-106">Mer information om övervakning av Active Directory Domain Services med Azure AD Connect Health finns i [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md).</span><span class="sxs-lookup"><span data-stu-id="f47b8-106">Additionally, for information on monitoring Active Directory Domain Services with Azure AD Connect Health see [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md).</span></span>

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a><span data-ttu-id="f47b8-108">Aviseringar för Azure AD Connect Health för synkronisering</span><span class="sxs-lookup"><span data-stu-id="f47b8-108">Alerts for Azure AD Connect Health for sync</span></span>
<span data-ttu-id="f47b8-109">hello Azure AD Connect Health-aviseringar för synkronisering tillhandahåller du hello lista över aktiva aviseringar.</span><span class="sxs-lookup"><span data-stu-id="f47b8-109">hello Azure AD Connect Health Alerts for sync section provides you hello list of active alerts.</span></span> <span data-ttu-id="f47b8-110">Varje avisering innehåller relevant information, Lösningssteg och länkar toorelated dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f47b8-110">Each alert includes relevant information, resolution steps, and links toorelated documentation.</span></span> <span data-ttu-id="f47b8-111">Genom att välja en aktiv eller åtgärdad avisering visas ett nytt blad med ytterligare information, samt vad du kan göra tooresolve hello aviseringen och länkar tooadditional dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f47b8-111">By selecting an active or resolved alert you will see a new blade with additional information, as well as steps you can take tooresolve hello alert, and links tooadditional documentation.</span></span> <span data-ttu-id="f47b8-112">Du kan också visa historiska data om aviseringar som har lösts i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="f47b8-112">You can also view historical data on alerts that were resolved in hello past.</span></span>

<span data-ttu-id="f47b8-113">Genom att markera en avisering som levereras med ytterligare information, samt steg kan du ta tooresolve hello aviseringen liksom länkar tooadditional dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f47b8-113">By selecting an alert you will be provided with additional information as well as steps you can take tooresolve hello alert and links tooadditional documentation.</span></span>

![Azure AD Connect-synkroniseringsfel](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a><span data-ttu-id="f47b8-115">Begränsad utvärdering av aviseringar</span><span class="sxs-lookup"><span data-stu-id="f47b8-115">Limited Evaluation of Alerts</span></span>
<span data-ttu-id="f47b8-116">Om Azure AD Connect inte använder standardkonfigurationen för hello (till exempel om Attributfiltrering ändras från hello standard tooa anpassad konfiguration), sedan hello Azure AD Connect Health-agenten kommer inte att överföra hello fel händelser relaterade tooAzure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f47b8-116">If Azure AD Connect is NOT using hello default configuration (for example, if Attribute Filtering is changed from hello default configuration tooa custom configuration), then hello Azure AD Connect Health agent will not upload hello error events related tooAzure AD Connect.</span></span>

<span data-ttu-id="f47b8-117">Detta begränsar hello utvärdering av aviseringar av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f47b8-117">This limits hello evaluation of alerts by hello service.</span></span> <span data-ttu-id="f47b8-118">Visas en banderoll som anger det här villkoret i hello Azure Portal under din tjänst.</span><span class="sxs-lookup"><span data-stu-id="f47b8-118">You'd will see a banner that indicates this condition in hello Azure Portal under your service.</span></span>

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/banner.png)

<span data-ttu-id="f47b8-120">Du kan ändra detta genom att klicka på ”inställningar” och låta Azure AD Connect Health agent tooupload alla felloggarna.</span><span class="sxs-lookup"><span data-stu-id="f47b8-120">You can change this by clicking "Settings" and allowing Azure AD Connect Health agent tooupload all error logs.</span></span>

![Azure AD Connect Health för synkronisering](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a><span data-ttu-id="f47b8-122">Synkronisera Insight</span><span class="sxs-lookup"><span data-stu-id="f47b8-122">Sync Insight</span></span>
<span data-ttu-id="f47b8-123">Administratörer ofta vill tooknow om hello tid det tar toosync ändringar tooAzure AD och hello mängd ändringar äger rum.</span><span class="sxs-lookup"><span data-stu-id="f47b8-123">Admins Frequently want tooknow about hello time it takes toosync changes tooAzure AD and hello amount of changes taking place.</span></span> <span data-ttu-id="f47b8-124">Den här funktionen ger ett enkelt sätt toovisualize detta med hjälp av hello nedan diagram:</span><span class="sxs-lookup"><span data-stu-id="f47b8-124">This feature provides an easy way toovisualize this using hello below graphs:</span></span>   

* <span data-ttu-id="f47b8-125">Svarstiden för synkroniseringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="f47b8-125">Latency of sync operations</span></span>
* <span data-ttu-id="f47b8-126">Objektändringstrend</span><span class="sxs-lookup"><span data-stu-id="f47b8-126">Object Change trend</span></span>

### <a name="sync-latency"></a><span data-ttu-id="f47b8-127">Synkronisera svarstider</span><span class="sxs-lookup"><span data-stu-id="f47b8-127">Sync Latency</span></span>
<span data-ttu-id="f47b8-128">Denna funktion tillhandahåller en grafisk trend över svarstiderna för hello synkroniseringsåtgärder (import, export osv.) för kopplingar.</span><span class="sxs-lookup"><span data-stu-id="f47b8-128">This feature provides a graphical trend of latency of hello sync operations (import, export, etc.) for connectors.</span></span>  <span data-ttu-id="f47b8-129">Detta ger en snabb och enkelt sätt toounderstand hello inte bara svarstiderna för dina åtgärder (större om du har en stor mängd ändringar äger rum), utan också ett sätt toodetect avvikelser i hello latens som kan kräva ytterligare utredning.</span><span class="sxs-lookup"><span data-stu-id="f47b8-129">This provides a quick and easy way toounderstand not only hello latency of your operations (larger if you have a large set of changes occurring) but also a way toodetect anomalies in hello latency that may require further investigation.</span></span>

![Synkronisera svarstider](./media/active-directory-aadconnect-health-sync/synclatency02.png)

<span data-ttu-id="f47b8-131">Som standard visas endast hello fördröjning av hello ”Export” för hello Azure AD-koppling.</span><span class="sxs-lookup"><span data-stu-id="f47b8-131">By default, only hello latency of hello 'Export' operation for hello Azure AD connector is shown.</span></span>  <span data-ttu-id="f47b8-132">toosee flera åtgärder på hello koppling eller tooview åtgärder från andra anslutningsappar högerklickar du på hello diagram, väljer Redigera diagram eller klicka på ”Redigera latens diagram” Hej och välj hello specifika åtgärden och kopplingar.</span><span class="sxs-lookup"><span data-stu-id="f47b8-132">toosee more operations on hello connector or tooview operations from other connectors, right-click on hello chart,  select Edit Chart or click on hello "Edit Latency Chart" button and choose hello specific operation and connectors.</span></span>

### <a name="sync-object-changes"></a><span data-ttu-id="f47b8-133">Synkronisera objektändringar</span><span class="sxs-lookup"><span data-stu-id="f47b8-133">Sync Object Changes</span></span>
<span data-ttu-id="f47b8-134">Denna funktion tillhandahåller en grafisk trend över hello antalet ändringar som utvärderas och exporteras tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f47b8-134">This feature provides a graphical trend of hello number of changes that are being evaluated and exported tooAzure AD.</span></span>  <span data-ttu-id="f47b8-135">Idag, när toogather är den här informationen från synkroniseringsloggarna hello svårt.</span><span class="sxs-lookup"><span data-stu-id="f47b8-135">Today, trying toogather this information from hello sync logs is difficult.</span></span>  <span data-ttu-id="f47b8-136">hello diagrammet innehåller inte bara ett enklare sätt att övervaka hello antalet ändringar som äger rum i din miljö, utan också en visuell översikt över hello-fel som uppstår.</span><span class="sxs-lookup"><span data-stu-id="f47b8-136">hello chart gives you, not only a simpler way of monitoring hello number of changes that are occurring in your environment, but also a visual view of hello failures that are occurring.</span></span>

![Synkronisera svarstider](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a><span data-ttu-id="f47b8-138">Felrapport för synkronisering på objektnivå (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="f47b8-138">Object Level Synchronization Error Report (Preview)</span></span>
<span data-ttu-id="f47b8-139">Denna funktion tillhandahåller en rapport om synkroniseringsfel som kan uppstå när identitetsdata synkroniseras mellan Windows Server AD och Azure AD med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="f47b8-139">This feature provides a report about synchronization errors that can occur when identity data is synchronized between Windows Server AD and Azure AD using Azure AD Connect.</span></span>

* <span data-ttu-id="f47b8-140">hello rapporten täcker fel som har registrerats av hello synkroniseringsklient (Azure AD Connect version 1.1.281.0 eller senare)</span><span class="sxs-lookup"><span data-stu-id="f47b8-140">hello report covers errors recorded by hello sync client (Azure AD Connect version 1.1.281.0 or higher)</span></span>
* <span data-ttu-id="f47b8-141">Hello senaste synkroniseringen på hello Synkroniseringsmotorn innehåller hello-fel som uppstått.</span><span class="sxs-lookup"><span data-stu-id="f47b8-141">It includes hello errors that occurred in hello last synchronization operation on hello sync engine.</span></span> <span data-ttu-id="f47b8-142">(”Export” på hello Azure AD-koppling.)</span><span class="sxs-lookup"><span data-stu-id="f47b8-142">("Export" on hello Azure AD Connector.)</span></span>
* <span data-ttu-id="f47b8-143">Azure AD Connect Health agent för synkronisering måste ha utgående anslutning toohello krävs slutpunkter för hello tooinclude hello senaste rapportdata.</span><span class="sxs-lookup"><span data-stu-id="f47b8-143">Azure AD Connect Health agent for sync must have outbound connectivity toohello required end points for hello report tooinclude hello latest data.</span></span>
* <span data-ttu-id="f47b8-144">hello rapporten är **uppdaterade efter var 30: e minut** med hello data har laddats upp av Azure AD Connect Health agent för synkronisering. Det ger hello följande viktiga funktioner</span><span class="sxs-lookup"><span data-stu-id="f47b8-144">hello report is **updated after every 30 minutes** using hello data uploaded by Azure AD Connect Health agent for sync. It provides hello following key capabilities</span></span>

  * <span data-ttu-id="f47b8-145">Kategorisering av fel</span><span class="sxs-lookup"><span data-stu-id="f47b8-145">Categorization of errors</span></span>
  * <span data-ttu-id="f47b8-146">Lista över objekt med fel per kategori</span><span class="sxs-lookup"><span data-stu-id="f47b8-146">List of objects with error per category</span></span>
  * <span data-ttu-id="f47b8-147">Alla hello data om hello fel på samma ställe</span><span class="sxs-lookup"><span data-stu-id="f47b8-147">All hello data about hello errors at one place</span></span>
  * <span data-ttu-id="f47b8-148">Sida vid sida-jämförelse av objekt med fel på grund av tooa konflikt</span><span class="sxs-lookup"><span data-stu-id="f47b8-148">Side by side comparison of Objects with error due tooa conflict</span></span>
  * <span data-ttu-id="f47b8-149">Hämta hello felrapport som en CVS (kommer snart)</span><span class="sxs-lookup"><span data-stu-id="f47b8-149">Download hello error report as a CVS (coming soon)</span></span>

### <a name="categorization-of-errors"></a><span data-ttu-id="f47b8-150">Kategorisering av fel</span><span class="sxs-lookup"><span data-stu-id="f47b8-150">Categorization of Errors</span></span>
<span data-ttu-id="f47b8-151">hello rapporten kategoriserar hello befintliga synkroniseringsfel i hello följande kategorier:</span><span class="sxs-lookup"><span data-stu-id="f47b8-151">hello report categorizes hello existing synchronization errors in hello following categories:</span></span>

| <span data-ttu-id="f47b8-152">Kategori</span><span class="sxs-lookup"><span data-stu-id="f47b8-152">Category</span></span> | <span data-ttu-id="f47b8-153">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="f47b8-153">Description</span></span> |
| --- | --- |
| <span data-ttu-id="f47b8-154">Duplicerat attribut</span><span class="sxs-lookup"><span data-stu-id="f47b8-154">Duplicate Attribute</span></span> |<span data-ttu-id="f47b8-155">Fel när Azure AD Connect försöker skapa eller uppdatera objekt med dubblerade värden av ett eller flera attribut i Azure AD som måste vara unika i en klient, till exempel proxyAddresses, UserPrincipalName.</span><span class="sxs-lookup"><span data-stu-id="f47b8-155">Errors when Azure AD Connect attempts create or update objects with duplicated values of one or more attributes in Azure AD that must be unique in a Tenant, such as proxyAddresses, UserPrincipalName.</span></span> |
| <span data-ttu-id="f47b8-156">Felmatchning av data</span><span class="sxs-lookup"><span data-stu-id="f47b8-156">Data Mismatch</span></span> |<span data-ttu-id="f47b8-157">Fel uppstod när hello soft-matchar inte toomatch objekt som resulterar i synkroniseringsfel.</span><span class="sxs-lookup"><span data-stu-id="f47b8-157">Errors when hello soft-match fails toomatch objects that result in synchronization errors.</span></span> |
| <span data-ttu-id="f47b8-158">Verifieringsfel för data</span><span class="sxs-lookup"><span data-stu-id="f47b8-158">Data Validation Failure</span></span> |<span data-ttu-id="f47b8-159">Fel på grund av tooinvalid data, till exempel tecken som inte stöds i viktiga attribut, till exempel UserPrincipalName, formatera fel som inte kan valideras innan de skrivs i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f47b8-159">Errors due tooinvalid data, such as unsupported characters in critical attributes such as UserPrincipalName, format errors that fail validation before being written in Azure AD.</span></span> |
| <span data-ttu-id="f47b8-160">Stora attribut</span><span class="sxs-lookup"><span data-stu-id="f47b8-160">Large Attribute</span></span> |<span data-ttu-id="f47b8-161">Fel när en eller flera attribut är större än hello tillåten storlek, längd eller antal.</span><span class="sxs-lookup"><span data-stu-id="f47b8-161">Errors when one or more attributes are larger than hello allowed size, length or count.</span></span> |
| <span data-ttu-id="f47b8-162">Annat</span><span class="sxs-lookup"><span data-stu-id="f47b8-162">Other</span></span> |<span data-ttu-id="f47b8-163">Alla andra fel som inte får plats i hello ovanför kategorier.</span><span class="sxs-lookup"><span data-stu-id="f47b8-163">All other errors that don't fit in hello above categories.</span></span> <span data-ttu-id="f47b8-164">Baserat på feedback, kommer den här kategorin att delas upp i underkategorier.</span><span class="sxs-lookup"><span data-stu-id="f47b8-164">Based on feedback, this category will be split in sub categories.</span></span> |

<span data-ttu-id="f47b8-165">![ Rapportsammanfattning för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Rapportkategorier för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span><span class="sxs-lookup"><span data-stu-id="f47b8-165">![Sync Error Report Summary](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![Sync Error Report Categories](./media/active-directory-aadconnect-health-sync/errorreport02.png)</span></span>

### <a name="list-of-objects-with-error-per-category"></a><span data-ttu-id="f47b8-166">Lista över objekt med fel per kategori</span><span class="sxs-lookup"><span data-stu-id="f47b8-166">List of objects with error per category</span></span>
<span data-ttu-id="f47b8-167">Vidaresökning i varje kategori får hello listan med objekt med hello fel i den kategorin.</span><span class="sxs-lookup"><span data-stu-id="f47b8-167">Drilling into each category will provide hello list of objects having hello error in that category.</span></span>
<span data-ttu-id="f47b8-168">![Rapportlista för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span><span class="sxs-lookup"><span data-stu-id="f47b8-168">![Sync Error Report List](./media/active-directory-aadconnect-health-sync/errorreport03.png)</span></span>

### <a name="error-details"></a><span data-ttu-id="f47b8-169">Information om fel</span><span class="sxs-lookup"><span data-stu-id="f47b8-169">Error Details</span></span>
<span data-ttu-id="f47b8-170">Följande data är tillgängliga i hello detaljerad vy för varje fel</span><span class="sxs-lookup"><span data-stu-id="f47b8-170">Following data is available in hello detailed view for each error</span></span>

* <span data-ttu-id="f47b8-171">Identifierare för hello *AD-objekt* ingår</span><span class="sxs-lookup"><span data-stu-id="f47b8-171">Identifiers for hello *AD Object* involved</span></span>
* <span data-ttu-id="f47b8-172">Identifierare för hello *Azure AD-objekt* inblandade (som är tillämpligt)</span><span class="sxs-lookup"><span data-stu-id="f47b8-172">Identifiers for hello *Azure AD Object* involved (as applicable)</span></span>
* <span data-ttu-id="f47b8-173">Felbeskrivning och hur toofix</span><span class="sxs-lookup"><span data-stu-id="f47b8-173">Error description and how toofix</span></span>
* <span data-ttu-id="f47b8-174">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="f47b8-174">Related articles</span></span>

![Rapportdetaljer för synkroniseringsfel](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-hello-error-report-as-csv"></a><span data-ttu-id="f47b8-176">Hämta hello felrapport som CSV-fil</span><span class="sxs-lookup"><span data-stu-id="f47b8-176">Download hello error report as CSV</span></span>
<span data-ttu-id="f47b8-177">Genom att välja hello ”Export” knapp som du kan ladda ned en CSV-fil med alla hello detaljer om alla hello-fel.</span><span class="sxs-lookup"><span data-stu-id="f47b8-177">By selecting hello "Export" button you can download a CSV file with all hello details about all hello errors.</span></span>

## <a name="related-links"></a><span data-ttu-id="f47b8-178">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="f47b8-178">Related links</span></span>
* [<span data-ttu-id="f47b8-179">Felsöka fel under synkronisering</span><span class="sxs-lookup"><span data-stu-id="f47b8-179">Troubleshooting Errors during synchronization</span></span>](../connect/active-directory-aadconnect-troubleshoot-sync-errors.md)
* [<span data-ttu-id="f47b8-180">Återhämtning av duplicerat attribut</span><span class="sxs-lookup"><span data-stu-id="f47b8-180">Duplicate Attribute Resiliency</span></span>](../connect/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [<span data-ttu-id="f47b8-181">Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="f47b8-181">Azure AD Connect Health</span></span>](active-directory-aadconnect-health.md)
* [<span data-ttu-id="f47b8-182">Installation av Azure AD Connect Health Agent</span><span class="sxs-lookup"><span data-stu-id="f47b8-182">Azure AD Connect Health Agent Installation</span></span>](active-directory-aadconnect-health-agent-install.md)
* [<span data-ttu-id="f47b8-183">Azure AD Connect Health-åtgärder</span><span class="sxs-lookup"><span data-stu-id="f47b8-183">Azure AD Connect Health Operations</span></span>](active-directory-aadconnect-health-operations.md)
* [<span data-ttu-id="f47b8-184">Använda Azure AD Connect Health med AD FS</span><span class="sxs-lookup"><span data-stu-id="f47b8-184">Using Azure AD Connect Health with AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
* [<span data-ttu-id="f47b8-185">Använda Azure AD Connect Health med AD DS</span><span class="sxs-lookup"><span data-stu-id="f47b8-185">Using Azure AD Connect Health with AD DS</span></span>](active-directory-aadconnect-health-adds.md)
* [<span data-ttu-id="f47b8-186">Vanliga frågor och svar om Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="f47b8-186">Azure AD Connect Health FAQ</span></span>](active-directory-aadconnect-health-faq.md)
* [<span data-ttu-id="f47b8-187">Versionshistorik för Azure AD Connect Health</span><span class="sxs-lookup"><span data-stu-id="f47b8-187">Azure AD Connect Health Version History</span></span>](active-directory-aadconnect-health-version-history.md)