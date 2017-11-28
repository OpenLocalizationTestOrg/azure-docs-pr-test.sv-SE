---
title: "aaaIT Service Management-anslutningstjänsten i OMS | Microsoft Docs"
description: "Använd hello IT Service Management-anslutningstjänsten toocentrally övervaka och hantera hello ITSM arbetsobjekt i OMS och att snabbt lösa eventuella problem."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a><span data-ttu-id="bc610-103">Centralt hantera ITSM arbetsobjekt med hjälp av IT Service Management-anslutningstjänsten (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="bc610-103">Centrally manage ITSM work items using IT Service Management Connector (Preview)</span></span>

![IT Service Management-anslutningstjänsten symbol](./media/log-analytics-itsmc/itsmc-symbol.png)

<span data-ttu-id="bc610-105">Du kan använda hello IT Service Management koppling (ITSMC) i OMS logganalys toocentrally övervaka och hantera arbetsuppgifter i din ITSM produkter och tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc610-105">You can use hello IT Service Management Connector (ITSMC) in OMS Log Analytics toocentrally monitor and manage work items across your ITSM products/services.</span></span>

<span data-ttu-id="bc610-106">hello IT Service Management-anslutningstjänsten integrerar dina befintliga IT Service Management (ITSM) produkter och tjänster med OMS logganalys.</span><span class="sxs-lookup"><span data-stu-id="bc610-106">hello IT Service Management Connector integrates your existing IT Service Management (ITSM) products and services with OMS Log Analytics.</span></span>  <span data-ttu-id="bc610-107">hello-lösningen har dubbelriktad integrering med ITSM produkter och tjänster, där det ger hello OMS användare en alternativet toocreate incidenter, aviseringar och händelser i ITSM lösning.</span><span class="sxs-lookup"><span data-stu-id="bc610-107">hello solution has bidirectional integration with ITSM products/services, where it provides hello OMS users an option toocreate incidents, alerts, or events in ITSM solution.</span></span> <span data-ttu-id="bc610-108">hello-anslutningen också importerar data som incidenter och tjänstbegäranden från ITSM lösning i OMS logganalys.</span><span class="sxs-lookup"><span data-stu-id="bc610-108">hello connector also  imports data such as incidents, and change requests from ITSM solution into OMS Log Analytics.</span></span>

<span data-ttu-id="bc610-109">Med IT Service Management-anslutningstjänsten kan du:</span><span class="sxs-lookup"><span data-stu-id="bc610-109">With IT Service Management Connector, you can:</span></span>

  - <span data-ttu-id="bc610-110">Centralt övervaka och hantera arbetsobjekt för ITSM produkter och tjänster som används inom organisationen.</span><span class="sxs-lookup"><span data-stu-id="bc610-110">Centrally monitor and manage work items for ITSM products/services used across your organization.</span></span>
  - <span data-ttu-id="bc610-111">Skapa ITSM arbetsuppgifter (t.ex. aviseringen, händelse, incident) i ITSM från OMS-varningar och via loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="bc610-111">Create ITSM work items (like alert, event, incident) in ITSM from OMS alerts and through log search.</span></span>
  - <span data-ttu-id="bc610-112">Läs incidenter och tjänstbegäranden från din lösning för ITSM och korrelera med relevanta loggdata i logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="bc610-112">Read incidents and change requests from your ITSM solution and correlate with relevant log data in Log Analytics workspace.</span></span>
  - <span data-ttu-id="bc610-113">Sök efter eventuella oväntade och ovanliga händelser och lösa dem, även innan hello slutanvändare anropa och rapportera dem toohello supportavdelningen.</span><span class="sxs-lookup"><span data-stu-id="bc610-113">Find any unexpected and unusual events and resolve them, even before hello end users call and report them toohello helpdesk.</span></span>
  - <span data-ttu-id="bc610-114">Importera objekt arbetsdata till logganalys och skapa rapporter för KPI-KPI-indikatorn.</span><span class="sxs-lookup"><span data-stu-id="bc610-114">Import work items data into Log Analytics and create key performance indicator (KPI) reports.</span></span>  <span data-ttu-id="bc610-115">Med dessa rapporter kan du identifiera, utvärdera och arbeta med exempel på viktiga saker, till exempel kontroll av skadlig kod.</span><span class="sxs-lookup"><span data-stu-id="bc610-115">Using these reports, you can identify, assess and act on several important items such as malware assessment.</span></span>
  - <span data-ttu-id="bc610-116">Visa granskat instrumentpaneler för djupare insikter om incidenter, ändringsbegäranden och berörda system.</span><span class="sxs-lookup"><span data-stu-id="bc610-116">View curated dashboards for deeper insights on incidents, change requests and impacted systems.</span></span>
  - <span data-ttu-id="bc610-117">Felsöka snabbare genom att sammanföra med andra hanteringslösningar i hello logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="bc610-117">Troubleshoot faster by correlating with other management solutions in hello Log Analytics workspace.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="bc610-118">Krav</span><span class="sxs-lookup"><span data-stu-id="bc610-118">Prerequisites</span></span>

<span data-ttu-id="bc610-119">tooimport hello ITSM arbetsobjekt i OMS logganalys hello lösning kräver en anslutning mellan hello IT Service Management-anslutningstjänsten i hello OMS och hello IT SM produkter eller tjänster som du importerar från hello arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="bc610-119">tooimport hello ITSM work items into OMS Log Analytics, hello solution requires a connection between hello IT Service Management Connector in hello OMS and hello IT SM product/service from which you import hello work items.</span></span>


## <a name="configuration"></a><span data-ttu-id="bc610-120">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bc610-120">Configuration</span></span>

<span data-ttu-id="bc610-121">Lägg till hello IT Service Management-anslutningstjänsten lösning tooyour OMS arbetsutrymme med hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="bc610-121">Add hello IT Service Management Connector solution tooyour OMS work space, using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>

<span data-ttu-id="bc610-122">IT Service Management-anslutningstjänsten panelen som du ser i hello lösningsgalleriet:</span><span class="sxs-lookup"><span data-stu-id="bc610-122">IT Service Management Connector tile as you see in hello Solutions gallery:</span></span>

![kopplingen sida vid sida](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

<span data-ttu-id="bc610-124">Efter lyckad dessutom visas hello IT Service Management-anslutningstjänsten under **OMS** > **inställningar** > **anslutna källor.**</span><span class="sxs-lookup"><span data-stu-id="bc610-124">After successful addition, you will see hello IT Service Management Connector under **OMS** > **Settings** > **Connected Sources.**</span></span>

![ITSMC ansluten](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> <span data-ttu-id="bc610-126">Som standard uppdaterar hello IT Service Management-anslutningstjänsten hello anslutning data en gång i en gång per dygn.</span><span class="sxs-lookup"><span data-stu-id="bc610-126">By default, hello IT Service Management Connector refreshes hello connection's data once in every 24 hours.</span></span> <span data-ttu-id="bc610-127">toorefresh anslutningsdata direkt för redigering eller mall uppdaterar du gör på hello uppdatering visas knappen Nästa tooyour anslutning.</span><span class="sxs-lookup"><span data-stu-id="bc610-127">toorefresh your connection's data instantly for any edits or template updates that you make, click hello refresh button displayed next tooyour connection.</span></span>

 ![ITSMC uppdatering](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a><span data-ttu-id="bc610-129">Hanteringspaket</span><span class="sxs-lookup"><span data-stu-id="bc610-129">Management packs</span></span>
<span data-ttu-id="bc610-130">Denna lösning kräver inte några management packs.</span><span class="sxs-lookup"><span data-stu-id="bc610-130">This solution does not require any management packs.</span></span>

## <a name="connected-sources"></a><span data-ttu-id="bc610-131">Anslutna källor</span><span class="sxs-lookup"><span data-stu-id="bc610-131">Connected sources</span></span>

<span data-ttu-id="bc610-132">hello som följande ITSM produkter och tjänster stöds av hello IT Service Management-anslutningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="bc610-132">hello following ITSM products/services are supported by hello IT Service Management Connector:</span></span>

- [<span data-ttu-id="bc610-133">System Center Service Manager (SCSM)</span><span class="sxs-lookup"><span data-stu-id="bc610-133">System Center Service Manager (SCSM)</span></span>](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="bc610-134">ServiceNow</span><span class="sxs-lookup"><span data-stu-id="bc610-134">ServiceNow</span></span>](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [<span data-ttu-id="bc610-135">Provance</span><span class="sxs-lookup"><span data-stu-id="bc610-135">Provance</span></span>](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [<span data-ttu-id="bc610-136">Cherwell</span><span class="sxs-lookup"><span data-stu-id="bc610-136">Cherwell</span></span>](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a><span data-ttu-id="bc610-137">Med hello-lösning</span><span class="sxs-lookup"><span data-stu-id="bc610-137">Using hello solution</span></span>

<span data-ttu-id="bc610-138">När du har anslutit hello OMS IT Service Management-anslutningstjänsten med ITSM-tjänsten startar hello logganalys services hello datainsamlingen från hello anslutna ITSM produkter eller tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc610-138">Once you connect hello OMS IT Service Management Connector with your ITSM service, hello Log Analytics services starts gathering hello data from hello connected ITSM products/service.</span></span>

> [!NOTE]
> - <span data-ttu-id="bc610-139">Data som importeras av IT Service Management-anslutningstjänsten lösning visas i logganalys som händelser med namnet **ServiceDesk_CL**.</span><span class="sxs-lookup"><span data-stu-id="bc610-139">Data imported by IT Service Management Connector solution appears in Log Analytics as events named **ServiceDesk_CL**.</span></span>
- <span data-ttu-id="bc610-140">Händelsemeddelandet innehåller ett fält med namnet **ServiceDeskWorkItemType_s**.</span><span class="sxs-lookup"><span data-stu-id="bc610-140">Event contains a field named **ServiceDeskWorkItemType_s**.</span></span> <span data-ttu-id="bc610-141">som ta dess värde som incident eller ändringsbegäran, beroende på hello arbetsobjekt data i hello **ServiceDesk_CL** händelse.</span><span class="sxs-lookup"><span data-stu-id="bc610-141">which can take its value as incident, or change request, depending on hello work item data contained in hello **ServiceDesk_CL** event.</span></span>

## <a name="input-data"></a><span data-ttu-id="bc610-142">Indata</span><span class="sxs-lookup"><span data-stu-id="bc610-142">Input data</span></span>
<span data-ttu-id="bc610-143">Arbetsobjekt som har importerats från hello ITSM produkter och tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc610-143">Work items imported from hello ITSM products/services.</span></span>

<span data-ttu-id="bc610-144">hello visas följande information exempel på data som samlas in av hello IT Service Management-anslutningstjänsten:</span><span class="sxs-lookup"><span data-stu-id="bc610-144">hello following information shows examples of data gathered by hello IT Service Management connector:</span></span>

> [!NOTE]
> <span data-ttu-id="bc610-145">Beroende på hello typ av arbetsobjekt importeras till Log Analytics **ServiceDesk_CL** innehåller hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="bc610-145">Depending on hello work item type imported into Log Analytics, **ServiceDesk_CL** contains hello following fields:</span></span>

<span data-ttu-id="bc610-146">**Arbetsobjekt:** **incidenter**</span><span class="sxs-lookup"><span data-stu-id="bc610-146">**Work item:** **Incidents**</span></span>  
<span data-ttu-id="bc610-147">ServiceDeskWorkItemType_s = ”Incident”</span><span class="sxs-lookup"><span data-stu-id="bc610-147">ServiceDeskWorkItemType_s="Incident"</span></span>

<span data-ttu-id="bc610-148">**Fält**</span><span class="sxs-lookup"><span data-stu-id="bc610-148">**Fields**</span></span>

- <span data-ttu-id="bc610-149">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="bc610-149">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="bc610-150">ID för skrivbord</span><span class="sxs-lookup"><span data-stu-id="bc610-150">Service Desk ID</span></span>
- <span data-ttu-id="bc610-151">Status</span><span class="sxs-lookup"><span data-stu-id="bc610-151">State</span></span>
- <span data-ttu-id="bc610-152">Angelägenhetsgrad</span><span class="sxs-lookup"><span data-stu-id="bc610-152">Urgency</span></span>
- <span data-ttu-id="bc610-153">Påverkan</span><span class="sxs-lookup"><span data-stu-id="bc610-153">Impact</span></span>
- <span data-ttu-id="bc610-154">Prioritet</span><span class="sxs-lookup"><span data-stu-id="bc610-154">Priority</span></span>
- <span data-ttu-id="bc610-155">Eskalering</span><span class="sxs-lookup"><span data-stu-id="bc610-155">Escalation</span></span>
- <span data-ttu-id="bc610-156">Skapat av</span><span class="sxs-lookup"><span data-stu-id="bc610-156">Created By</span></span>
- <span data-ttu-id="bc610-157">Löst av</span><span class="sxs-lookup"><span data-stu-id="bc610-157">Resolved By</span></span>
- <span data-ttu-id="bc610-158">Stängts av</span><span class="sxs-lookup"><span data-stu-id="bc610-158">Closed By</span></span>
- <span data-ttu-id="bc610-159">Källa</span><span class="sxs-lookup"><span data-stu-id="bc610-159">Source</span></span>
- <span data-ttu-id="bc610-160">Tilldelad till</span><span class="sxs-lookup"><span data-stu-id="bc610-160">Assigned To</span></span>
- <span data-ttu-id="bc610-161">Kategori</span><span class="sxs-lookup"><span data-stu-id="bc610-161">Category</span></span>
- <span data-ttu-id="bc610-162">Rubrik</span><span class="sxs-lookup"><span data-stu-id="bc610-162">Title</span></span>
- <span data-ttu-id="bc610-163">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc610-163">Description</span></span>
- <span data-ttu-id="bc610-164">Skapad datum</span><span class="sxs-lookup"><span data-stu-id="bc610-164">Created Date</span></span>
- <span data-ttu-id="bc610-165">Stängningsdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-165">Closed Date</span></span>
- <span data-ttu-id="bc610-166">Lösningsdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-166">Resolved Date</span></span>
- <span data-ttu-id="bc610-167">Senast ändrad datum</span><span class="sxs-lookup"><span data-stu-id="bc610-167">Last Modified Date</span></span>
- <span data-ttu-id="bc610-168">Dator</span><span class="sxs-lookup"><span data-stu-id="bc610-168">Computer</span></span>


<span data-ttu-id="bc610-169">**Arbetsobjekt:** **ändringsbegäranden**</span><span class="sxs-lookup"><span data-stu-id="bc610-169">**Work item:** **Change Requests**</span></span>

<span data-ttu-id="bc610-170">ServiceDeskWorkItemType_s = ”ändra begäran”</span><span class="sxs-lookup"><span data-stu-id="bc610-170">ServiceDeskWorkItemType_s="ChangeRequest"</span></span>

<span data-ttu-id="bc610-171">**Fält**</span><span class="sxs-lookup"><span data-stu-id="bc610-171">**Fields**</span></span>
- <span data-ttu-id="bc610-172">ServiceDeskConnectionName</span><span class="sxs-lookup"><span data-stu-id="bc610-172">ServiceDeskConnectionName</span></span>
- <span data-ttu-id="bc610-173">ID för skrivbord</span><span class="sxs-lookup"><span data-stu-id="bc610-173">Service Desk ID</span></span>
- <span data-ttu-id="bc610-174">Skapat av</span><span class="sxs-lookup"><span data-stu-id="bc610-174">Created By</span></span>
- <span data-ttu-id="bc610-175">Stängts av</span><span class="sxs-lookup"><span data-stu-id="bc610-175">Closed By</span></span>
- <span data-ttu-id="bc610-176">Källa</span><span class="sxs-lookup"><span data-stu-id="bc610-176">Source</span></span>
- <span data-ttu-id="bc610-177">Tilldelad till</span><span class="sxs-lookup"><span data-stu-id="bc610-177">Assigned To</span></span>
- <span data-ttu-id="bc610-178">Rubrik</span><span class="sxs-lookup"><span data-stu-id="bc610-178">Title</span></span>
- <span data-ttu-id="bc610-179">Typ</span><span class="sxs-lookup"><span data-stu-id="bc610-179">Type</span></span>
- <span data-ttu-id="bc610-180">Kategori</span><span class="sxs-lookup"><span data-stu-id="bc610-180">Category</span></span>
- <span data-ttu-id="bc610-181">Status</span><span class="sxs-lookup"><span data-stu-id="bc610-181">State</span></span>
- <span data-ttu-id="bc610-182">Eskalering</span><span class="sxs-lookup"><span data-stu-id="bc610-182">Escalation</span></span>
- <span data-ttu-id="bc610-183">Konflikt Status</span><span class="sxs-lookup"><span data-stu-id="bc610-183">Conflict Status</span></span>
- <span data-ttu-id="bc610-184">Angelägenhetsgrad</span><span class="sxs-lookup"><span data-stu-id="bc610-184">Urgency</span></span>
- <span data-ttu-id="bc610-185">Prioritet</span><span class="sxs-lookup"><span data-stu-id="bc610-185">Priority</span></span>
- <span data-ttu-id="bc610-186">Risk</span><span class="sxs-lookup"><span data-stu-id="bc610-186">Risk</span></span>
- <span data-ttu-id="bc610-187">Påverkan</span><span class="sxs-lookup"><span data-stu-id="bc610-187">Impact</span></span>
- <span data-ttu-id="bc610-188">Tilldelad till</span><span class="sxs-lookup"><span data-stu-id="bc610-188">Assigned To</span></span>
- <span data-ttu-id="bc610-189">Skapad datum</span><span class="sxs-lookup"><span data-stu-id="bc610-189">Created Date</span></span>
- <span data-ttu-id="bc610-190">Stängningsdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-190">Closed Date</span></span>
- <span data-ttu-id="bc610-191">Senast ändrad datum</span><span class="sxs-lookup"><span data-stu-id="bc610-191">Last Modified Date</span></span>
- <span data-ttu-id="bc610-192">Begärt datum</span><span class="sxs-lookup"><span data-stu-id="bc610-192">Requested Date</span></span>
- <span data-ttu-id="bc610-193">Planerat startdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-193">Planned Start Date</span></span>
- <span data-ttu-id="bc610-194">Planerat slutdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-194">Planned End Date</span></span>
- <span data-ttu-id="bc610-195">Startdatum för arbete</span><span class="sxs-lookup"><span data-stu-id="bc610-195">Work Start Date</span></span>
- <span data-ttu-id="bc610-196">Slutdatum för arbete</span><span class="sxs-lookup"><span data-stu-id="bc610-196">Work End Date</span></span>
- <span data-ttu-id="bc610-197">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc610-197">Description</span></span>
- <span data-ttu-id="bc610-198">Dator</span><span class="sxs-lookup"><span data-stu-id="bc610-198">Computer</span></span>

## <a name="output-data-for-a-servicenow-incident"></a><span data-ttu-id="bc610-199">Utdata för en ServiceNow-incident</span><span class="sxs-lookup"><span data-stu-id="bc610-199">Output data for a ServiceNow incident</span></span>

| <span data-ttu-id="bc610-200">OMS-fält</span><span class="sxs-lookup"><span data-stu-id="bc610-200">OMS field</span></span> | <span data-ttu-id="bc610-201">ITSM fält</span><span class="sxs-lookup"><span data-stu-id="bc610-201">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="bc610-202">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="bc610-202">ServiceDeskId_s</span></span>| <span data-ttu-id="bc610-203">Tal</span><span class="sxs-lookup"><span data-stu-id="bc610-203">Number</span></span> |
| <span data-ttu-id="bc610-204">IncidentState_s</span><span class="sxs-lookup"><span data-stu-id="bc610-204">IncidentState_s</span></span> | <span data-ttu-id="bc610-205">Status</span><span class="sxs-lookup"><span data-stu-id="bc610-205">State</span></span> |
| <span data-ttu-id="bc610-206">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="bc610-206">Urgency_s</span></span> |<span data-ttu-id="bc610-207">Angelägenhetsgrad</span><span class="sxs-lookup"><span data-stu-id="bc610-207">Urgency</span></span> |
| <span data-ttu-id="bc610-208">Impact_s</span><span class="sxs-lookup"><span data-stu-id="bc610-208">Impact_s</span></span> |<span data-ttu-id="bc610-209">Påverkan</span><span class="sxs-lookup"><span data-stu-id="bc610-209">Impact</span></span>|
| <span data-ttu-id="bc610-210">Priority_s</span><span class="sxs-lookup"><span data-stu-id="bc610-210">Priority_s</span></span> | <span data-ttu-id="bc610-211">Prioritet</span><span class="sxs-lookup"><span data-stu-id="bc610-211">Priority</span></span> |
| <span data-ttu-id="bc610-212">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="bc610-212">CreatedBy_s</span></span> | <span data-ttu-id="bc610-213">Öppnas av</span><span class="sxs-lookup"><span data-stu-id="bc610-213">Opened by</span></span> |
| <span data-ttu-id="bc610-214">ResolvedBy_s</span><span class="sxs-lookup"><span data-stu-id="bc610-214">ResolvedBy_s</span></span> | <span data-ttu-id="bc610-215">Löst av</span><span class="sxs-lookup"><span data-stu-id="bc610-215">Resolved by</span></span>|
| <span data-ttu-id="bc610-216">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="bc610-216">ClosedBy_s</span></span>  | <span data-ttu-id="bc610-217">Stängts av</span><span class="sxs-lookup"><span data-stu-id="bc610-217">Closed by</span></span> |
| <span data-ttu-id="bc610-218">Source_s</span><span class="sxs-lookup"><span data-stu-id="bc610-218">Source_s</span></span>| <span data-ttu-id="bc610-219">Kontakta typ</span><span class="sxs-lookup"><span data-stu-id="bc610-219">Contact type</span></span> |
| <span data-ttu-id="bc610-220">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="bc610-220">AssignedTo_s</span></span> | <span data-ttu-id="bc610-221">Tilldelade för</span><span class="sxs-lookup"><span data-stu-id="bc610-221">Assigned too</span></span> |
| <span data-ttu-id="bc610-222">Category_s</span><span class="sxs-lookup"><span data-stu-id="bc610-222">Category_s</span></span> | <span data-ttu-id="bc610-223">Kategori</span><span class="sxs-lookup"><span data-stu-id="bc610-223">Category</span></span> |
| <span data-ttu-id="bc610-224">Title_s</span><span class="sxs-lookup"><span data-stu-id="bc610-224">Title_s</span></span>|  <span data-ttu-id="bc610-225">Kort beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc610-225">Short description</span></span> |
| <span data-ttu-id="bc610-226">Description_s</span><span class="sxs-lookup"><span data-stu-id="bc610-226">Description_s</span></span>|  <span data-ttu-id="bc610-227">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bc610-227">Notes</span></span> |
| <span data-ttu-id="bc610-228">CreatedDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-228">CreatedDate_t</span></span>|  <span data-ttu-id="bc610-229">Öppna</span><span class="sxs-lookup"><span data-stu-id="bc610-229">Opened</span></span> |
| <span data-ttu-id="bc610-230">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-230">ClosedDate_t</span></span>| <span data-ttu-id="bc610-231">Stängd</span><span class="sxs-lookup"><span data-stu-id="bc610-231">closed</span></span>|
| <span data-ttu-id="bc610-232">ResolvedDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-232">ResolvedDate_t</span></span>|<span data-ttu-id="bc610-233">Löst</span><span class="sxs-lookup"><span data-stu-id="bc610-233">Resolved</span></span>|
| <span data-ttu-id="bc610-234">Dator</span><span class="sxs-lookup"><span data-stu-id="bc610-234">Computer</span></span>  | <span data-ttu-id="bc610-235">konfigurationsobjekt</span><span class="sxs-lookup"><span data-stu-id="bc610-235">Configuration item</span></span> |

## <a name="output-data-for-a-servicenow-change-request"></a><span data-ttu-id="bc610-236">Utdata för en ServiceNow ändringsbegäran</span><span class="sxs-lookup"><span data-stu-id="bc610-236">Output data for a ServiceNow change request</span></span>

| <span data-ttu-id="bc610-237">OMS-fält</span><span class="sxs-lookup"><span data-stu-id="bc610-237">OMS field</span></span> | <span data-ttu-id="bc610-238">ITSM fält</span><span class="sxs-lookup"><span data-stu-id="bc610-238">ITSM field</span></span> |
|:--- |:--- |
| <span data-ttu-id="bc610-239">ServiceDeskId_s</span><span class="sxs-lookup"><span data-stu-id="bc610-239">ServiceDeskId_s</span></span>| <span data-ttu-id="bc610-240">Tal</span><span class="sxs-lookup"><span data-stu-id="bc610-240">Number</span></span> |
| <span data-ttu-id="bc610-241">CreatedBy_s</span><span class="sxs-lookup"><span data-stu-id="bc610-241">CreatedBy_s</span></span> | <span data-ttu-id="bc610-242">Begärd av</span><span class="sxs-lookup"><span data-stu-id="bc610-242">Requested by</span></span> |
| <span data-ttu-id="bc610-243">ClosedBy_s</span><span class="sxs-lookup"><span data-stu-id="bc610-243">ClosedBy_s</span></span> | <span data-ttu-id="bc610-244">Stängts av</span><span class="sxs-lookup"><span data-stu-id="bc610-244">Closed by</span></span> |
| <span data-ttu-id="bc610-245">AssignedTo_s</span><span class="sxs-lookup"><span data-stu-id="bc610-245">AssignedTo_s</span></span> | <span data-ttu-id="bc610-246">Tilldelade för</span><span class="sxs-lookup"><span data-stu-id="bc610-246">Assigned too</span></span> |
| <span data-ttu-id="bc610-247">Title_s</span><span class="sxs-lookup"><span data-stu-id="bc610-247">Title_s</span></span>|  <span data-ttu-id="bc610-248">Kort beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc610-248">Short description</span></span> |
| <span data-ttu-id="bc610-249">Type_s</span><span class="sxs-lookup"><span data-stu-id="bc610-249">Type_s</span></span>|  <span data-ttu-id="bc610-250">Typ</span><span class="sxs-lookup"><span data-stu-id="bc610-250">Type</span></span> |
| <span data-ttu-id="bc610-251">Category_s</span><span class="sxs-lookup"><span data-stu-id="bc610-251">Category_s</span></span>|  <span data-ttu-id="bc610-252">Catgory</span><span class="sxs-lookup"><span data-stu-id="bc610-252">Catgory</span></span> |
| <span data-ttu-id="bc610-253">CRState_s</span><span class="sxs-lookup"><span data-stu-id="bc610-253">CRState_s</span></span>|  <span data-ttu-id="bc610-254">Status</span><span class="sxs-lookup"><span data-stu-id="bc610-254">State</span></span>|
| <span data-ttu-id="bc610-255">Urgency_s</span><span class="sxs-lookup"><span data-stu-id="bc610-255">Urgency_s</span></span>|  <span data-ttu-id="bc610-256">Angelägenhetsgrad</span><span class="sxs-lookup"><span data-stu-id="bc610-256">Urgency</span></span> |
| <span data-ttu-id="bc610-257">Priority_s</span><span class="sxs-lookup"><span data-stu-id="bc610-257">Priority_s</span></span>| <span data-ttu-id="bc610-258">Prioritet</span><span class="sxs-lookup"><span data-stu-id="bc610-258">Priority</span></span>|
| <span data-ttu-id="bc610-259">Risk_s</span><span class="sxs-lookup"><span data-stu-id="bc610-259">Risk_s</span></span>| <span data-ttu-id="bc610-260">Risk</span><span class="sxs-lookup"><span data-stu-id="bc610-260">Risk</span></span>|
| <span data-ttu-id="bc610-261">Impact_s</span><span class="sxs-lookup"><span data-stu-id="bc610-261">Impact_s</span></span>| <span data-ttu-id="bc610-262">Påverkan</span><span class="sxs-lookup"><span data-stu-id="bc610-262">Impact</span></span>|
| <span data-ttu-id="bc610-263">RequestedDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-263">RequestedDate_t</span></span>  | <span data-ttu-id="bc610-264">Begärt efter datum</span><span class="sxs-lookup"><span data-stu-id="bc610-264">Requested by date</span></span> |
| <span data-ttu-id="bc610-265">ClosedDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-265">ClosedDate_t</span></span> | <span data-ttu-id="bc610-266">Stängningsdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-266">Closed date</span></span> |
| <span data-ttu-id="bc610-267">PlannedStartDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-267">PlannedStartDate_t</span></span>  |     <span data-ttu-id="bc610-268">Planerat startdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-268">Planned start date</span></span> |
| <span data-ttu-id="bc610-269">PlannedEndDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-269">PlannedEndDate_t</span></span>  |   <span data-ttu-id="bc610-270">Planerat slutdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-270">Planned end date</span></span> |
| <span data-ttu-id="bc610-271">WorkStartDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-271">WorkStartDate_t</span></span>  | <span data-ttu-id="bc610-272">Verkligt startdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-272">Actual start date</span></span> |
| <span data-ttu-id="bc610-273">WorkEndDate_t</span><span class="sxs-lookup"><span data-stu-id="bc610-273">WorkEndDate_t</span></span> | <span data-ttu-id="bc610-274">Verkligt slutdatum</span><span class="sxs-lookup"><span data-stu-id="bc610-274">Actual end date</span></span>|
| <span data-ttu-id="bc610-275">Description_s</span><span class="sxs-lookup"><span data-stu-id="bc610-275">Description_s</span></span> | <span data-ttu-id="bc610-276">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="bc610-276">Description</span></span> |
| <span data-ttu-id="bc610-277">Dator</span><span class="sxs-lookup"><span data-stu-id="bc610-277">Computer</span></span>  | <span data-ttu-id="bc610-278">Konfigurationsobjekt</span><span class="sxs-lookup"><span data-stu-id="bc610-278">Configuration Item</span></span> |

<span data-ttu-id="bc610-279">**Exempel logganalys skärmen för ITSM data:**</span><span class="sxs-lookup"><span data-stu-id="bc610-279">**Sample Log Analytics screen for ITSM data:**</span></span>

![Log Analytics skärmen](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a><span data-ttu-id="bc610-281">IT Service Management-anslutningstjänsten – integrering med andra OMS-lösningar</span><span class="sxs-lookup"><span data-stu-id="bc610-281">IT Service Management connector – integration with other OMS solutions</span></span>

<span data-ttu-id="bc610-282">IT Service Management-anslutningstjänsten stöder för närvarande integrering med hello Tjänstkarta lösning.</span><span class="sxs-lookup"><span data-stu-id="bc610-282">IT Service Management Connector, currently supports integration with hello Service Map solution.</span></span>

<span data-ttu-id="bc610-283">Tjänstkarta upptäcker automatiskt hello programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc610-283">Service Map automatically discovers hello application components on Windows and Linux systems and maps hello communication between services.</span></span> <span data-ttu-id="bc610-284">Det gör att du tooview dina servrar som du betrakta dem – som sammanlänkade system som levererar kritiska tjänster.</span><span class="sxs-lookup"><span data-stu-id="bc610-284">It allows you tooview your servers as you think of them – as interconnected systems that deliver critical services.</span></span> <span data-ttu-id="bc610-285">Tjänstkarta visar anslutningar mellan servrar, processer och portar över en TCP-ansluten arkitektur med ingen konfiguration krävs för andra än installation av en agent.</span><span class="sxs-lookup"><span data-stu-id="bc610-285">Service Map shows connections between servers, processes, and ports across any TCP-connected architecture with no configuration required other than installation of an agent.</span></span> <span data-ttu-id="bc610-286">Mer information: [Tjänstkarta](../operations-management-suite/operations-management-suite-service-map.md).</span><span class="sxs-lookup"><span data-stu-id="bc610-286">More information: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).</span></span>

<span data-ttu-id="bc610-287">Du kan visa hello service supportavdelningen som har skapats i hello ITSM lösningar som visas i följande exempel hello med den här integreringen:</span><span class="sxs-lookup"><span data-stu-id="bc610-287">With this integration, you can view hello service desk items created in hello ITSM solutions as shown in hello following example:</span></span>

![<span data-ttu-id="bc610-288">Integrerad lösning</span><span class="sxs-lookup"><span data-stu-id="bc610-288">Integrated solution</span></span> ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a><span data-ttu-id="bc610-289">Skapa ITSM arbetsobjekt för OMS-aviseringar</span><span class="sxs-lookup"><span data-stu-id="bc610-289">Create ITSM work items for OMS alerts</span></span>

<span data-ttu-id="bc610-290">Du kan skapa tillhörande arbetsobjekt i hello anslutna ITSM källor för hello OMS-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="bc610-290">For hello OMS alerts, you can create associated work items in hello connected ITSM sources.</span></span>  <span data-ttu-id="bc610-291">toodo detta, Använd hello nedan:</span><span class="sxs-lookup"><span data-stu-id="bc610-291">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="bc610-292">Från **loggen Sök** och köra en sökning frågan tooview loggdata.</span><span class="sxs-lookup"><span data-stu-id="bc610-292">From **Log Search** window, run a log search query tooview data.</span></span> <span data-ttu-id="bc610-293">Frågeresultatet är hello källan för arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="bc610-293">Query results are hello source for work items.</span></span>
2. <span data-ttu-id="bc610-294">I **loggen Sök**, klickar du på **avisering** tooopen hello **lägga till Varningsregeln** sidan.</span><span class="sxs-lookup"><span data-stu-id="bc610-294">In **Log Search**, click **Alert** tooopen hello **Add Alert Rule** page.</span></span>

    ![Log Analytics skärmen](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. <span data-ttu-id="bc610-296">På hello **lägga till Varningsregeln** fönster, ange information om hello som krävs för **namn**, **allvarlighetsgrad**, **sökfråga**, och  **Varna kriterier** (fönstret/Tidsmått för mätning).</span><span class="sxs-lookup"><span data-stu-id="bc610-296">On hello **Add Alert Rule** window, provide hello required details for **Name**, **Severity**,  **Search query**, and **Alert criteria** (Time Window/Metric measurement).</span></span>
4. <span data-ttu-id="bc610-297">Välj **Ja** för **ITSM åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="bc610-297">Select **Yes** for **ITSM Actions**.</span></span>
5. <span data-ttu-id="bc610-298">Välja ITSM anslutning hello **Välj anslutning** lista.</span><span class="sxs-lookup"><span data-stu-id="bc610-298">Select your ITSM connection from hello **Select Connection** list.</span></span>
6. <span data-ttu-id="bc610-299">Ange hello information som krävs.</span><span class="sxs-lookup"><span data-stu-id="bc610-299">Provide hello details as required.</span></span>
7. <span data-ttu-id="bc610-300">toocreate ett separat arbetsobjekt för varje loggpost för den här aviseringen, Välj hello **skapa enskilda arbetsobjekt för varje loggpost** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="bc610-300">toocreate a separate work item for each log entry of this alert, select hello **Create individual work items for each log entry** checkbox.</span></span>

    <span data-ttu-id="bc610-301">Eller</span><span class="sxs-lookup"><span data-stu-id="bc610-301">Or</span></span>

    <span data-ttu-id="bc610-302">Låt kryssrutan avmarkerad toocreate endast en arbetsuppgiften för valfritt antal loggposter i den här aviseringen.</span><span class="sxs-lookup"><span data-stu-id="bc610-302">leave this checkbox unselected toocreate only one work item for any number of log entries under this alert.</span></span>

7. <span data-ttu-id="bc610-303">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bc610-303">Click **Save**.</span></span>

<span data-ttu-id="bc610-304">hello OMS aviseringen kommer att skapas under **aviseringar**.</span><span class="sxs-lookup"><span data-stu-id="bc610-304">hello OMS alert will be created under **Alerts**.</span></span> <span data-ttu-id="bc610-305">Hej motsvarande ITSM anslutning arbete objekt skapas när hello angivna aviseringen är uppfyllt.</span><span class="sxs-lookup"><span data-stu-id="bc610-305">hello corresponding ITSM connection's work items are created when hello specified alert's condition is met.</span></span>

## <a name="create-itsm-work-items-from-oms-logs"></a><span data-ttu-id="bc610-306">Skapa ITSM arbetsobjekt från OMS-loggar</span><span class="sxs-lookup"><span data-stu-id="bc610-306">Create ITSM work items from OMS logs</span></span>

<span data-ttu-id="bc610-307">Du kan skapa arbetsobjekt i hello anslutna ITSM källor med hjälp av OMS loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="bc610-307">You can create work items in hello connected ITSM sources by using OMS Log Search.</span></span> <span data-ttu-id="bc610-308">toodo detta, Använd hello nedan:</span><span class="sxs-lookup"><span data-stu-id="bc610-308">toodo this, use hello following procedure:</span></span>

1. <span data-ttu-id="bc610-309">Från **loggen Sök**söka hello krävs data, välja hello information och klicka på **skapa arbetsobjekt**.</span><span class="sxs-lookup"><span data-stu-id="bc610-309">From **Log Search**,  search hello required data, select hello detail, and click **Create work item**.</span></span>

    <span data-ttu-id="bc610-310">Hej **skapa ITSM arbetsobjekt** visas:</span><span class="sxs-lookup"><span data-stu-id="bc610-310">hello **Create ITSM Work item** window appears:</span></span>

    ![Log Analytics skärmen](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   <span data-ttu-id="bc610-312">Lägg till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="bc610-312">Add hello following details:</span></span>

  - <span data-ttu-id="bc610-313">**Rubrik för arbetsobjekt**: titel hello arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="bc610-313">**Work item Title**: Title for hello work item.</span></span>
  - <span data-ttu-id="bc610-314">**Beskrivning av arbetsuppgift**: beskrivning för hello nya arbetsuppgiften.</span><span class="sxs-lookup"><span data-stu-id="bc610-314">**Work item Description**: Description for hello new work item.</span></span>
  - <span data-ttu-id="bc610-315">**Påverkas datorn**: namnet på hello dator där den här loggdata hittades.</span><span class="sxs-lookup"><span data-stu-id="bc610-315">**Affected Computer**: Name of hello computer where this log data was found.</span></span>
  - <span data-ttu-id="bc610-316">**Välj anslutning**: ITSM anslutning som du vill toocreate arbetsuppgiften.</span><span class="sxs-lookup"><span data-stu-id="bc610-316">**Select Connection**:  ITSM connection in which you want toocreate this work item.</span></span>
  - <span data-ttu-id="bc610-317">**Arbetsobjektet**: typ av arbetsobjekt.</span><span class="sxs-lookup"><span data-stu-id="bc610-317">**Work item**:  Type of work item.</span></span>

3. <span data-ttu-id="bc610-318">toouse en befintlig mall för arbetsobjekt för en incident, klickar du på **Ja** under **generera arbetsobjekt utifrån hello mallen** alternativ och klickar sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bc610-318">toouse an existing work item template for an incident, click **Yes** under **Generate work item based on hello template** option and then click **Create**.</span></span>

    <span data-ttu-id="bc610-319">Eller:</span><span class="sxs-lookup"><span data-stu-id="bc610-319">Or,</span></span>

    <span data-ttu-id="bc610-320">Klicka på **nr** om du vill tooprovide din anpassade värden.</span><span class="sxs-lookup"><span data-stu-id="bc610-320">Click **No** if you want tooprovide your customized values.</span></span>

4. <span data-ttu-id="bc610-321">Ange hello lämpliga värden i hello **kontakta typen**, **inverkan**, **angelägenhetsgrad**, **kategori**, och **underkategori**  textrutor och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="bc610-321">Provide hello appropriate values in hello **Contact Type**, **Impact**, **Urgency**, **Category**, and **Sub Category** text boxes, and then click **Create**.</span></span>

<span data-ttu-id="bc610-322">hello arbetsobjekt skapas i hello ITSM som du kan också visa i OMS.</span><span class="sxs-lookup"><span data-stu-id="bc610-322">hello work item will be created in hello ITSM, which you can also view in OMS.</span></span>

## <a name="troubleshoot-itsm-connections-in-oms"></a><span data-ttu-id="bc610-323">Felsöka ITSM anslutningar i OMS</span><span class="sxs-lookup"><span data-stu-id="bc610-323">Troubleshoot ITSM connections in OMS</span></span>
1.  <span data-ttu-id="bc610-324">Om anslutningen misslyckas från Gränssnittet för anslutna källa och du får hello **fel spara anslutningen** visas, hello följande:</span><span class="sxs-lookup"><span data-stu-id="bc610-324">If connection fails from connected source's UI and you get hello **Error in saving connection** message, do hello following:</span></span>
 - <span data-ttu-id="bc610-325">Vid ServiceNow, Cherwell och Provance anslutningar, se till att du korrekt angivna hello användarnamn/lösenord och klient-ID: T/klienthemlighet för varje hello anslutningar.</span><span class="sxs-lookup"><span data-stu-id="bc610-325">In case of ServiceNow, Cherwell and Provance connections, ensure you correctly entered  hello username/password and  client ID/client secret  for each of hello connections.</span></span> <span data-ttu-id="bc610-326">Om hello problemet kvarstår bör du kontrollera om du har tillräckliga privilegier i hello motsvarande ITSM produkten toomake hello-anslutningen.</span><span class="sxs-lookup"><span data-stu-id="bc610-326">If hello error persists, check if you have sufficient privileges  in hello corresponding ITSM product toomake hello connection.</span></span>
 - <span data-ttu-id="bc610-327">Om Service Manager, kontrollera att hello webbprogrammet har distribuerats och hybridanslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="bc610-327">In case of Service Manager, ensure that hello Web app is successfully deployed and hybrid connection is created.</span></span> <span data-ttu-id="bc610-328">tooverify hello finns har anslutning med hello lokal Service Manager-datorn, gå hello Web app-URL som anges i dokumentationen för hello för att göra hello [hybridanslutning](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span><span class="sxs-lookup"><span data-stu-id="bc610-328">tooverify hello connection is successfully established with hello on-prem Service Manager machine, visit hello  Web app URL as detailed in hello documentation for making hello [hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).</span></span>

2.  <span data-ttu-id="bc610-329">Om inte komma har synkroniserats data från ServiceNow i OMS, se till att den hello ServiceNow-instansen inte är i viloläge.</span><span class="sxs-lookup"><span data-stu-id="bc610-329">If data from ServiceNow is not getting synced in OMS, ensure that hello ServiceNow instance is not sleeping.</span></span> <span data-ttu-id="bc610-330">Detta kan senare inträffa i hello ServiceNow Dev instanser vid inaktivitet.</span><span class="sxs-lookup"><span data-stu-id="bc610-330">This might sometime happen in hello ServiceNow Dev instances, when idle.</span></span> <span data-ttu-id="bc610-331">Annan, rapporten hello problemet.</span><span class="sxs-lookup"><span data-stu-id="bc610-331">Else, report hello issue.</span></span>
3.  <span data-ttu-id="bc610-332">Om aviseringar komma skickas från OMS men arbetsobjekt komma skapas inte i ITSM produkt eller konfigurationsobjekt inte får skapas/länkade toowork objekt eller en allmän information, hello följande:</span><span class="sxs-lookup"><span data-stu-id="bc610-332">If Alerts are getting fired from OMS but work items are not getting created in ITSM product or configuration items are not getting created/linked toowork items or for any generic information, do hello following:</span></span>
 -  <span data-ttu-id="bc610-333">IT Service Management-anslutningstjänsten lösning i OMS-portalen kan vara används tooget en sammanfattning av anslutningar/artiklar/arbetsdatorer osv. Klicka på hello felmeddelande hello status-bladet, navigera för**loggen Sök** och visa hello-anslutning som har hello fel med hjälp av hello information i hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="bc610-333">IT Service Management Connector solution in OMS portal could be used tooget a summary of connections/work items/computers etc. Click hello error message in hello status blade, navigate too**Log Search** and view hello connection that has hello error by using hello details in hello error message.</span></span>
 - <span data-ttu-id="bc610-334">Du kan visa hello fel/relaterad information direkt i hello **loggen Sök** sidan med *typ = ServiceDeskLog_CL*.</span><span class="sxs-lookup"><span data-stu-id="bc610-334">you can directly view hello errors/related information in hello **Log Search** page using *Type=ServiceDeskLog_CL*.</span></span>

## <a name="troubleshoot-service-manager-web-app-deployment"></a><span data-ttu-id="bc610-335">Felsöka Service Manager Web App-distribution</span><span class="sxs-lookup"><span data-stu-id="bc610-335">Troubleshoot Service Manager Web App deployment</span></span>
1.  <span data-ttu-id="bc610-336">Vid problem med web app-distribution, se till att du har tillräckliga behörigheter i hello prenumeration nämns toocreate/distribuera resurser.</span><span class="sxs-lookup"><span data-stu-id="bc610-336">In case of any trouble with web app deployment, ensure you have sufficient permissions in hello subscription mentioned toocreate/deploy resources.</span></span>
2.  <span data-ttu-id="bc610-337">Om **objektreferens har angetts tooinstance objektets** felmeddelande när du kör hello [skriptet](log-analytics-itsmc-service-manager-script.md) se till att du har angett giltiga värden under **Användarkonfiguration**avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bc610-337">If **Object reference not set tooinstance of an object** error message appears while running hello [script](log-analytics-itsmc-service-manager-script.md) ensure that you entered valid values  under **User Configuration** section.</span></span>
3.  <span data-ttu-id="bc610-338">Se till att hello begärda resursprovidern har registrerats i hello prenumeration om du inte toocreate service bus relay-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="bc610-338">If you fail toocreate service bus relay namespace, ensure that hello required resource provider is registered in hello subscription.</span></span> <span data-ttu-id="bc610-339">Om du inte har registrerats, skapar du det manuellt från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc610-339">If not registered, manually create it from hello Azure portal.</span></span> <span data-ttu-id="bc610-340">Du kan också skapa den när [skapar hello hybridanslutning](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc610-340">You can also create it while [creating hello hybrid connection](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) from hello Azure portal.</span></span>


## <a name="contact-us"></a><span data-ttu-id="bc610-341">Kontakta oss</span><span class="sxs-lookup"><span data-stu-id="bc610-341">Contact us</span></span>

<span data-ttu-id="bc610-342">För alla frågor eller feedback om hello IT Service Management-anslutningstjänsten, kontaktar du oss på [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="bc610-342">For any queries or feedback on hello IT Service Management Connector, contact us at [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc610-343">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc610-343">Next steps</span></span>
<span data-ttu-id="bc610-344">[Lägg till ITSM produkter och tjänster tooIT Service Management-anslutningstjänsten](log-analytics-itsmc-connections.md).</span><span class="sxs-lookup"><span data-stu-id="bc610-344">[Add ITSM products/services tooIT Service Management Connector](log-analytics-itsmc-connections.md).</span></span>
