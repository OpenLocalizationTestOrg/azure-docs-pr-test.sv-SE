---
title: "aaaOptimize din SQL Server-miljö med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda hello SQL-bedömning lösning tooassess hello risk och hälsotillståndet för din SQL server-miljöer med regelbundna intervall med Azure logganalys."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e297eb57-1718-4cfe-a241-b9e84b2c42ac
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f31326d8cdad3ef5d5a190614d1a18c1dac826ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-sql-server-environment-with-hello-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="8e536-103">Optimera SQL Server-miljön med hello SQL lösning i logganalys</span><span class="sxs-lookup"><span data-stu-id="8e536-103">Optimize your SQL Server environment with hello SQL Assessment solution in Log Analytics</span></span>

![SQL-bedömning symbol](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="8e536-105">Du kan använda hello SQL-bedömning lösning tooassess hello risk och hälsotillståndet för server-miljöer med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="8e536-105">You can use hello SQL Assessment solution tooassess hello risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="8e536-106">Den här artikeln hjälper dig att installera hello lösning så att du kan vidta åtgärder för möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="8e536-106">This article will help you install hello solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="8e536-107">Den här lösningen ger en prioriterad lista med rekommendationer för specifika tooyour distribuerad serverinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="8e536-107">This solution provides a prioritized list of recommendations specific tooyour deployed server infrastructure.</span></span> <span data-ttu-id="8e536-108">hello rekommendationer kategoriseras i sex fokus områden som hjälper dig att snabbt förstå hello risk och vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="8e536-108">hello recommendations are categorized across six focus areas which help you quickly understand hello risk and take corrective action.</span></span>

<span data-ttu-id="8e536-109">hello rekommendationer baserat på hello kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="8e536-109">hello recommendations made are based on hello knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="8e536-110">Varje rekommendation vägledning om ett problem kan varför betydelse tooyou och hur tooimplement hello föreslagna ändringar.</span><span class="sxs-lookup"><span data-stu-id="8e536-110">Each recommendation provides guidance about why an issue might matter tooyou and how tooimplement hello suggested changes.</span></span>

<span data-ttu-id="8e536-111">Du kan välja fokusområden som är den viktigaste tooyour organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.</span><span class="sxs-lookup"><span data-stu-id="8e536-111">You can choose focus areas that are most important tooyour organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="8e536-112">När du har lagt till hello lösning och är slutförd, Sammanfattning visas information för fokusområden på hello **SQL-bedömning** instrumentpanelen för hello infrastrukturen i din miljö.</span><span class="sxs-lookup"><span data-stu-id="8e536-112">After you've added hello solution and an assessment is completed, summary information for focus areas is shown on hello **SQL Assessment** dashboard for hello infrastructure in your environment.</span></span> <span data-ttu-id="8e536-113">hello följande avsnitt beskrivs hur toouse hello information om hello **SQL-bedömning** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din SQL server-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="8e536-113">hello following sections describe how toouse hello information on hello **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![Bild av SQL-bedömning sida vid sida](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Bild av SQL-bedömning instrumentpanelen](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-hello-solution"></a><span data-ttu-id="8e536-116">Installera och konfigurera hello lösning</span><span class="sxs-lookup"><span data-stu-id="8e536-116">Installing and configuring hello solution</span></span>
<span data-ttu-id="8e536-117">SQL-bedömning fungerar med alla versioner av SQL Server för hello Standard, Developer eller Enterprise Edition stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="8e536-117">SQL Assessment works with all currently supported versions of SQL Server for hello Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="8e536-118">Använd följande information tooinstall hello och konfigurera hello lösning.</span><span class="sxs-lookup"><span data-stu-id="8e536-118">Use hello following information tooinstall and configure hello solution.</span></span>

* <span data-ttu-id="8e536-119">Agenterna måste installeras på servrar som har installerat SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e536-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="8e536-120">hello SQL lösning kräver en version av .NET Framework 4 installeras på varje dator som har en OMS-agent som stöds.</span><span class="sxs-lookup"><span data-stu-id="8e536-120">hello SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="8e536-121">I ordning tooinstall hello lösning måste hello användaren vara en administratör eller deltagare toohello Azure-prenumeration när med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8e536-121">In order tooinstall hello solution, hello user must be an administrator or contributor toohello Azure subscription when using hello Azure portal.</span></span> <span data-ttu-id="8e536-122">Hello användaren måste dessutom vara Rollmedlem hello OMS arbetsytan deltagare eller administratör i hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="8e536-122">In addition, hello user must be a member of hello OMS workspace contributor or administrator role in hello OMS portal.</span></span>
* <span data-ttu-id="8e536-123">När du använder hello Operations Manager-agenten med SQL-bedömning behöver toouse ett Run-As för Operations Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="8e536-123">When using hello Operations Manager agent with SQL Assessment, you'll need toouse an Operations Manager Run-As account.</span></span> <span data-ttu-id="8e536-124">Se [Operations Manager kör som-konton för OMS](#operations-manager-run-as-accounts-for-oms) nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="8e536-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8e536-125">hello MMA agenten stöder inte Run-As för Operations Manager-konton.</span><span class="sxs-lookup"><span data-stu-id="8e536-125">hello MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="8e536-126">Lägg till hello SQL-bedömning lösning tooyour OMS-arbetsyta med hjälp av hello process beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="8e536-126">Add hello SQL Assessment solution tooyour OMS workspace using hello process described in [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="8e536-127">Det krävs ingen ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8e536-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="8e536-128">När du har lagt till hello lösning läggs hello AdvisorAssessment.exe filen tooservers med agenter.</span><span class="sxs-lookup"><span data-stu-id="8e536-128">After you've added hello solution, hello AdvisorAssessment.exe file is added tooservers with agents.</span></span> <span data-ttu-id="8e536-129">Konfigurationsdata Läs och skickas sedan toohello OMS-tjänsten i hello molnet för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="8e536-129">Configuration data is read and then sent toohello OMS service in hello cloud for processing.</span></span> <span data-ttu-id="8e536-130">Logik är tillämpade toohello mottagna data och hello Molntjänsten innehåller hello-data.</span><span class="sxs-lookup"><span data-stu-id="8e536-130">Logic is applied toohello received data and hello cloud service records hello data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="8e536-131">SQL-bedömning information för samlingen</span><span class="sxs-lookup"><span data-stu-id="8e536-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="8e536-132">SQL-bedömning samlar in WMI-data, registerdata, prestandadata och SQL Server hantering av dynamiska visa resultat med hjälp av hello agenter som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="8e536-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using hello agents that you have enabled.</span></span>

<span data-ttu-id="8e536-133">hello följande tabell visas data collection metoder för agenter, om Operations Manager (SCOM) krävs och hur ofta data samlas in av en agent.</span><span class="sxs-lookup"><span data-stu-id="8e536-133">hello following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="8e536-134">Plattform</span><span class="sxs-lookup"><span data-stu-id="8e536-134">platform</span></span> | <span data-ttu-id="8e536-135">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="8e536-135">Direct Agent</span></span> | <span data-ttu-id="8e536-136">SCOM-agent</span><span class="sxs-lookup"><span data-stu-id="8e536-136">SCOM agent</span></span> | <span data-ttu-id="8e536-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="8e536-137">Azure Storage</span></span> | <span data-ttu-id="8e536-138">SCOM krävs?</span><span class="sxs-lookup"><span data-stu-id="8e536-138">SCOM required?</span></span> | <span data-ttu-id="8e536-139">SCOM-agent data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="8e536-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="8e536-140">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="8e536-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="8e536-141">Windows</span><span class="sxs-lookup"><span data-stu-id="8e536-141">Windows</span></span> | <span data-ttu-id="8e536-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="8e536-142">&#8226;</span></span> | <span data-ttu-id="8e536-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="8e536-143">&#8226;</span></span> |  |  | <span data-ttu-id="8e536-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="8e536-144">&#8226;</span></span> |<span data-ttu-id="8e536-145">7 dagar</span><span class="sxs-lookup"><span data-stu-id="8e536-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="8e536-146">Operations Manager kör som-konton för OMS</span><span class="sxs-lookup"><span data-stu-id="8e536-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="8e536-147">Logganalys i OMS använder hello Operations Manager-agenten och hantering av grupp toocollect och skicka data toohello OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8e536-147">Log Analytics in OMS uses hello Operations Manager agent and management group toocollect and send data toohello OMS service.</span></span> <span data-ttu-id="8e536-148">OMS-versioner av hanteringspaket för arbetsbelastningar tooprovide mervärde tjänster.</span><span class="sxs-lookup"><span data-stu-id="8e536-148">OMS builds upon management packs for workloads tooprovide value-add services.</span></span> <span data-ttu-id="8e536-149">Varje arbetsbelastning kräver belastningsspecifikt privilegier toorun hanteringspaket i en annan säkerhetskontext, till exempel ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="8e536-149">Each workload requires workload-specific privileges toorun management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="8e536-150">Du måste tooprovide autentiseringsuppgifter genom att konfigurera en Operations Manager kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="8e536-150">You need tooprovide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="8e536-151">Använd följande information tooset hello Operations Manager kör som-konto för SQL-bedömning hello.</span><span class="sxs-lookup"><span data-stu-id="8e536-151">Use hello following information tooset hello Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-hello-run-as-account-for-sql-assessment"></a><span data-ttu-id="8e536-152">Ange hello kör som-konto för SQL-bedömning</span><span class="sxs-lookup"><span data-stu-id="8e536-152">Set hello Run As account for SQL assessment</span></span>
 <span data-ttu-id="8e536-153">Om du redan använder hello hanteringspaket för SQL Server, bör du använda Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="8e536-153">If you are already using hello SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="tooconfigure-hello-sql-run-as-account-in-hello-operations-console"></a><span data-ttu-id="8e536-154">tooconfigure hello SQL kör som-konto i hello Operations-konsolen</span><span class="sxs-lookup"><span data-stu-id="8e536-154">tooconfigure hello SQL Run As account in hello Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="8e536-155">Om du använder hello OMS direkt agent, i stället för hello SCOM-agent, hello hanteringspaket körs alltid i hello säkerhetskontexten för hello kontot Lokalt System.</span><span class="sxs-lookup"><span data-stu-id="8e536-155">If you are using hello OMS direct agent, rather than hello SCOM agent, hello management pack always runs in hello security context of hello Local System account.</span></span> <span data-ttu-id="8e536-156">Hoppa över steg 1 – 5 nedan och kör hello antingen T-SQL eller Powershell exemplet anger att NT AUTHORITY\SYSTEM hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="8e536-156">Skip steps 1-5 below, and run either hello T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as hello user name.</span></span>
>
>

1. <span data-ttu-id="8e536-157">Öppna hello Operations-konsolen i Operations Manager, och klicka sedan på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="8e536-157">In Operations Manager, open hello Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="8e536-158">Under **kör som-konfiguration**, klickar du på **profiler**, och öppna **OMS SQL Assessment kör som-profilen**.</span><span class="sxs-lookup"><span data-stu-id="8e536-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="8e536-159">På hello **kör som-konton** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="8e536-159">On hello **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="8e536-160">Välj en Windows kör som-konto som innehåller hello autentiseringsuppgifter krävs för SQL Server, eller klicka på **ny** toocreate en.</span><span class="sxs-lookup"><span data-stu-id="8e536-160">Select a Windows Run As account that contains hello credentials needed for SQL Server, or click **New** toocreate one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8e536-161">hello kör som-kontotyp måste vara Windows.</span><span class="sxs-lookup"><span data-stu-id="8e536-161">hello Run As account type must be Windows.</span></span> <span data-ttu-id="8e536-162">hello kör som-kontot måste också ingå i gruppen lokala administratörer på alla Windows-servrar som är värd för SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="8e536-162">hello Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="8e536-163">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="8e536-163">Click **Save**.</span></span>
6. <span data-ttu-id="8e536-164">Ändra och kör följande T-SQL-exempel på varje krävs tooRun för SQL Server-instans toogrant minsta möjliga behörigheter på som-konto tooperform SQL-bedömning hello.</span><span class="sxs-lookup"><span data-stu-id="8e536-164">Modify and then execute hello following T-SQL sample on each SQL Server Instance toogrant minimum permissions required tooRun As Account tooperform SQL Assessment.</span></span> <span data-ttu-id="8e536-165">Dock behöver du inte toodo det om ett kör som-konto redan ingår i serverrollen för hello sysadmin på SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="8e536-165">However, you don’t need toodo this if a Run As Account is already part of hello sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with hello actual user name being used as Run As Account.
    USE master

    -- Create login for hello user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions toouser.
    GRANT VIEW SERVER STATE too[<UserName>]
    GRANT VIEW ANY DEFINITION too[<UserName>]
    GRANT VIEW ANY DATABASE too[<UserName>]

    -- Add database user for all hello databases on SQL Server Instance, this is required for connecting tooindividual databases.
    -- NOTE: This command must be run anytime new databases are added tooSQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="tooconfigure-hello-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="8e536-166">tooconfigure hello SQL kör som-konto med hjälp av Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="8e536-166">tooconfigure hello SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="8e536-167">Öppna ett PowerShell-fönster och kör följande skript när du har uppdaterat den med din information hello:</span><span class="sxs-lookup"><span data-stu-id="8e536-167">Open a PowerShell window and run hello following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="8e536-168">Förstå hur rekommendationer prioriteras</span><span class="sxs-lookup"><span data-stu-id="8e536-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="8e536-169">Varje rekommendation är ett givet värde som identifierar hello relativa betydelse hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-169">Every recommendation made is given a weighting value that identifies hello relative importance of hello recommendation.</span></span> <span data-ttu-id="8e536-170">Endast hello tio viktigaste rekommendationer visas.</span><span class="sxs-lookup"><span data-stu-id="8e536-170">Only hello ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="8e536-171">Hur vikterna beräknas</span><span class="sxs-lookup"><span data-stu-id="8e536-171">How weights are calculated</span></span>
<span data-ttu-id="8e536-172">Viktningar är sammanställda värden som baseras på tre viktiga faktorer:</span><span class="sxs-lookup"><span data-stu-id="8e536-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="8e536-173">Hej *sannolikhet* att ett problem som identifieras ska orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="8e536-173">hello *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="8e536-174">En högre sannolikhet är lika tooa större hela poängen för hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-174">A higher probability equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="8e536-175">Hej *inverkan* av hello problemet på din organisation om det uppstår ett problem.</span><span class="sxs-lookup"><span data-stu-id="8e536-175">hello *impact* of hello issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="8e536-176">En högre inverkan är lika tooa större hela poängen för hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-176">A higher impact equates tooa larger overall score for hello recommendation.</span></span>
* <span data-ttu-id="8e536-177">Hej *arbete* krävs tooimplement hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-177">hello *effort* required tooimplement hello recommendation.</span></span> <span data-ttu-id="8e536-178">En högre prestanda är lika tooa mindre hela poängen för hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-178">A higher effort equates tooa smaller overall score for hello recommendation.</span></span>

<span data-ttu-id="8e536-179">hello viktning för varje rekommendation uttrycks i procent av hello sammanlagda poängen för varje fokusområde.</span><span class="sxs-lookup"><span data-stu-id="8e536-179">hello weighting for each recommendation is expressed as a percentage of hello total score available for each focus area.</span></span> <span data-ttu-id="8e536-180">Till exempel om en rekommendation i hello säkerhet och efterlevnad Fokusområde har ett resultat på 5%, ökar implementera denna rekommendation din övergripande säkerhet och efterlevnad poäng av 5%.</span><span class="sxs-lookup"><span data-stu-id="8e536-180">For example, if a recommendation in hello Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="8e536-181">Fokusområden</span><span class="sxs-lookup"><span data-stu-id="8e536-181">Focus areas</span></span>
<span data-ttu-id="8e536-182">**Säkerhet och efterlevnad** -fokus visas här rekommendationer för potentiella säkerhetshot och överträdelser, företagets principer och krav på teknisk, juridisk och regelmässig efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="8e536-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="8e536-183">**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.</span><span class="sxs-lookup"><span data-stu-id="8e536-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="8e536-184">**Prestanda och skalbarhet** -fokus visas här rekommendationer toohelp din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan toorespond toochanging infrastrukturen måste.</span><span class="sxs-lookup"><span data-stu-id="8e536-184">**Performance and Scalability** - This focus area shows recommendations toohelp your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able toorespond toochanging infrastructure needs.</span></span>

<span data-ttu-id="8e536-185">**Uppgradera migrering och distribution** – fokus visas här rekommendationer toohelp uppgradering, migrera och distribuera SQL Server tooyour befintlig infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="8e536-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations toohelp you upgrade, migrate, and deploy SQL Server tooyour existing infrastructure.</span></span>

<span data-ttu-id="8e536-186">**Åtgärder och övervakning** – fokus visas här rekommendationer toohelp förenklad IT-avdelningen implementera förebyggande underhåll och maximera prestanda.</span><span class="sxs-lookup"><span data-stu-id="8e536-186">**Operations and Monitoring** - This focus area shows recommendations toohelp streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="8e536-187">**Ändrings- och konfigurationshantering** -fokus visas här rekommendationer toohelp skydda dagliga uppgifter, se till att ändringar inte negativt påverka din infrastruktur, upprätta ändringskontroll och tootrack och granska systemkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="8e536-187">**Change and Configuration Management** - This focus area shows recommendations toohelp protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and tootrack and audit system configurations.</span></span>

### <a name="should-you-aim-tooscore-100-in-every-focus-area"></a><span data-ttu-id="8e536-188">Bör du sträva tooscore 100% överallt fokus i?</span><span class="sxs-lookup"><span data-stu-id="8e536-188">Should you aim tooscore 100% in every focus area?</span></span>
<span data-ttu-id="8e536-189">Inte nödvändigtvis.</span><span class="sxs-lookup"><span data-stu-id="8e536-189">Not necessarily.</span></span> <span data-ttu-id="8e536-190">hello rekommendationer baserat på hello kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="8e536-190">hello recommendations are based on hello knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="8e536-191">Inga två server-infrastrukturen är dock hello samma och specifika rekommendationer kan vara mer eller mindre relevanta tooyou.</span><span class="sxs-lookup"><span data-stu-id="8e536-191">However, no two server infrastructures are hello same, and specific recommendations may be more or less relevant tooyou.</span></span> <span data-ttu-id="8e536-192">Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte är synliga toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="8e536-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed toohello Internet.</span></span> <span data-ttu-id="8e536-193">Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering.</span><span class="sxs-lookup"><span data-stu-id="8e536-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="8e536-194">Problem som är viktiga tooa mogen företag kanske mindre viktiga tooa uppstart.</span><span class="sxs-lookup"><span data-stu-id="8e536-194">Issues that are important tooa mature business may be less important tooa start-up.</span></span> <span data-ttu-id="8e536-195">Du kanske vill tooidentify vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="8e536-195">You may want tooidentify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="8e536-196">Varje rekommendation innehåller information om varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="8e536-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="8e536-197">Du bör använda den här vägledningen tooevaluate om implementera hello rekommendation är lämplig för dig, eftersom hello IT-tjänster och hello affärsbehoven i organisationen.</span><span class="sxs-lookup"><span data-stu-id="8e536-197">You should use this guidance tooevaluate whether implementing hello recommendation is appropriate for you, given hello nature of your IT services and hello business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="8e536-198">Använd assessment fokus området rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8e536-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="8e536-199">Innan du kan använda en lösning i OMS, måste du ha hello redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="8e536-199">Before you can use an assessment solution in OMS, you must have hello solution installed.</span></span> <span data-ttu-id="8e536-200">tooread mer om hur du installerar lösningar, se [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="8e536-200">tooread more about installing solutions, see [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="8e536-201">När den har installerats ser du hello sammanfattning av rekommendationer med hjälp av hello SQL-bedömning panelen på översiktssidan för hello i OMS.</span><span class="sxs-lookup"><span data-stu-id="8e536-201">After it is installed, you can view hello summary of recommendations by using hello SQL Assessment tile on hello Overview page in OMS.</span></span>

<span data-ttu-id="8e536-202">Visa hello sammanfattas efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8e536-202">View hello summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="tooview-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="8e536-203">tooview rekommendationer för en fokus området och vidtar korrigeringsåtgärder</span><span class="sxs-lookup"><span data-stu-id="8e536-203">tooview recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="8e536-204">På hello **översikt** klickar du på hello **SQL-bedömning** panelen.</span><span class="sxs-lookup"><span data-stu-id="8e536-204">On hello **Overview** page, click hello **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="8e536-205">På hello **SQL-bedömning** granskar hello sammanfattningsinformation i ett hello fokus området blad och klicka sedan på ett tooview rekommendationer om fokus.</span><span class="sxs-lookup"><span data-stu-id="8e536-205">On hello **SQL Assessment** page, review hello summary information in one of hello focus area blades and then click one tooview recommendations for that focus area.</span></span>
3. <span data-ttu-id="8e536-206">På någon av hello fokus Områdessidor, kan du visa hello prioriteras rekommendationer för din miljö.</span><span class="sxs-lookup"><span data-stu-id="8e536-206">On any of hello focus area pages, you can view hello prioritized recommendations made for your environment.</span></span> <span data-ttu-id="8e536-207">Klicka på en rekommendation enligt **påverkade objekt** tooview information om varför hello rekommendation.</span><span class="sxs-lookup"><span data-stu-id="8e536-207">Click a recommendation under **Affected Objects** tooview details about why hello recommendation is made.</span></span>  
    <span data-ttu-id="8e536-208">![Bild av SQL-bedömning rekommendationer](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="8e536-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="8e536-209">Du kan vidta åtgärder i **föreslagna åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="8e536-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="8e536-210">När hello objektet åtgärdats registrerar senare bedömningar den rekommenderade åtgärder som utförts och kompatibilitet poängen ökar.</span><span class="sxs-lookup"><span data-stu-id="8e536-210">When hello item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="8e536-211">Korrigerade objekt visas som **skickades objekt**.</span><span class="sxs-lookup"><span data-stu-id="8e536-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="8e536-212">Ignorera rekommendationer</span><span class="sxs-lookup"><span data-stu-id="8e536-212">Ignore recommendations</span></span>
<span data-ttu-id="8e536-213">Om du har rekommendationer som du vill tooignore kan skapa du en textfil som OMS använder tooprevent rekommendationer visas i sökresultatet assessment.</span><span class="sxs-lookup"><span data-stu-id="8e536-213">If you have recommendations that you want tooignore, you can create a text file that OMS will use tooprevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="tooidentify-recommendations-that-you-will-ignore"></a><span data-ttu-id="8e536-214">tooidentify rekommendationer som kommer att ignorera</span><span class="sxs-lookup"><span data-stu-id="8e536-214">tooidentify recommendations that you will ignore</span></span>
1. <span data-ttu-id="8e536-215">Logga in tooyour arbetsytan och öppna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="8e536-215">Sign in tooyour workspace and open Log Search.</span></span> <span data-ttu-id="8e536-216">Använd hello följande fråga toolist rekommendationer som har misslyckats för datorer i din miljö.</span><span class="sxs-lookup"><span data-stu-id="8e536-216">Use hello following query toolist recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="8e536-217">Här är en skärmbild som visar hello loggen sökfråga: ![misslyckades rekommendationer](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="8e536-217">Here's a screen shot showing hello Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="8e536-218">Välj rekommendationer som du vill tooignore.</span><span class="sxs-lookup"><span data-stu-id="8e536-218">Choose recommendations that you want tooignore.</span></span> <span data-ttu-id="8e536-219">Du ska använda hello värden för RecommendationId i hello nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="8e536-219">You’ll use hello values for RecommendationId in hello next procedure.</span></span>

### <a name="toocreate-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="8e536-220">toocreate och använda en IgnoreRecommendations.txt textfil</span><span class="sxs-lookup"><span data-stu-id="8e536-220">toocreate and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="8e536-221">Skapa en fil med namnet IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="8e536-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="8e536-222">Klistra in eller ange varje RecommendationId för varje rekommendation att OMS tooignore på en separat rad och spara och Stäng hello-filen.</span><span class="sxs-lookup"><span data-stu-id="8e536-222">Paste or type each RecommendationId for each recommendation that you want OMS tooignore on a separate line and then save and close hello file.</span></span>
3. <span data-ttu-id="8e536-223">Placera hello-filen i hello följande mapp på varje dator där du vill att OMS tooignore rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8e536-223">Put hello file in hello following folder on each computer where you want OMS tooignore recommendations.</span></span>
   * <span data-ttu-id="8e536-224">På datorer med hello Microsoft Monitoring Agent (ansluten direkt eller via Operations Manager) - *SystemDrive*: \Program\Microsoft övervakning Agent\Agent</span><span class="sxs-lookup"><span data-stu-id="8e536-224">On computers with hello Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="8e536-225">På hello hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="8e536-225">On hello Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="tooverify-that-recommendations-are-ignored"></a><span data-ttu-id="8e536-226">tooverify att rekommendationer ignoreras</span><span class="sxs-lookup"><span data-stu-id="8e536-226">tooverify that recommendations are ignored</span></span>
1. <span data-ttu-id="8e536-227">När hello nästa schemalagda utvärdering körs som standard var sjunde dag angetts hello rekommendationer markeras ignoreras och visas inte på hello assessment instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="8e536-227">After hello next scheduled assessment runs, by default every 7 days, hello specified recommendations are marked Ignored and will not appear on hello assessment dashboard.</span></span>
2. <span data-ttu-id="8e536-228">Du kan använda hello följande loggen Sök frågor toolist alla hello ignoreras rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8e536-228">You can use hello following Log Search queries toolist all hello ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="8e536-229">Om du senare vill toosee ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.</span><span class="sxs-lookup"><span data-stu-id="8e536-229">If you decide later that you want toosee ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="8e536-230">SQL lösning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8e536-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="8e536-231">*Hur ofta körs en utvärdering?*</span><span class="sxs-lookup"><span data-stu-id="8e536-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="8e536-232">hello assessment körs var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="8e536-232">hello assessment runs every 7 days.</span></span>

<span data-ttu-id="8e536-233">*Finns det ett sätt tooconfigure hur ofta hello assessment körs?*</span><span class="sxs-lookup"><span data-stu-id="8e536-233">*Is there a way tooconfigure how often hello assessment runs?*</span></span>

* <span data-ttu-id="8e536-234">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="8e536-234">Not at this time.</span></span>

<span data-ttu-id="8e536-235">*Om en annan server identifieras när jag har lagt till hello SQL lösning, kommer utvärderas det?*</span><span class="sxs-lookup"><span data-stu-id="8e536-235">*If another server is discovered after I’ve added hello SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="8e536-236">Ja, när det upptäcks att det bedöms från sedan, var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="8e536-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="8e536-237">*Om en server tas ur drift, när den tas bort från hello assessment?*</span><span class="sxs-lookup"><span data-stu-id="8e536-237">*If a server is decommissioned, when will it be removed from hello assessment?*</span></span>

* <span data-ttu-id="8e536-238">Om en server inte lämnar data 3 veckor tas bort.</span><span class="sxs-lookup"><span data-stu-id="8e536-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="8e536-239">*Vad är hello process som hello datainsamling hello namn?*</span><span class="sxs-lookup"><span data-stu-id="8e536-239">*What is hello name of hello process that does hello data collection?*</span></span>

* <span data-ttu-id="8e536-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="8e536-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="8e536-241">*Hur lång tid tar det för toobe för data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="8e536-241">*How long does it take for data toobe collected?*</span></span>

* <span data-ttu-id="8e536-242">hello faktiska datainsamling på hello servern tar ungefär en timme.</span><span class="sxs-lookup"><span data-stu-id="8e536-242">hello actual data collection on hello server takes about 1 hour.</span></span> <span data-ttu-id="8e536-243">Det kan ta längre tid på servrar som har ett stort antal databaser eller SQL-instanser.</span><span class="sxs-lookup"><span data-stu-id="8e536-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="8e536-244">*Vilken typ av data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="8e536-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="8e536-245">hello följande typer av data samlas in:</span><span class="sxs-lookup"><span data-stu-id="8e536-245">hello following types of data are collected:</span></span>
  * <span data-ttu-id="8e536-246">WMI</span><span class="sxs-lookup"><span data-stu-id="8e536-246">WMI</span></span>
  * <span data-ttu-id="8e536-247">Registret</span><span class="sxs-lookup"><span data-stu-id="8e536-247">Registry</span></span>
  * <span data-ttu-id="8e536-248">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="8e536-248">Performance counters</span></span>
  * <span data-ttu-id="8e536-249">SQL dynamiska hanteringsvyer (DMV).</span><span class="sxs-lookup"><span data-stu-id="8e536-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="8e536-250">*Finns det en tooconfigure sätt när data samlas in?*</span><span class="sxs-lookup"><span data-stu-id="8e536-250">*Is there a way tooconfigure when data is collected?*</span></span>

* <span data-ttu-id="8e536-251">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="8e536-251">Not at this time.</span></span>

<span data-ttu-id="8e536-252">*Varför måste jag tooconfigure en Kör som-konto?*</span><span class="sxs-lookup"><span data-stu-id="8e536-252">*Why do I have tooconfigure a Run As Account?*</span></span>

* <span data-ttu-id="8e536-253">För SQL Server körs ett litet antal SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="8e536-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="8e536-254">För att de toorun en Kör som-konto med VIEW SERVER STATE behörigheter tooSQL måste användas.</span><span class="sxs-lookup"><span data-stu-id="8e536-254">In order for them toorun, a Run As Account with VIEW SERVER STATE permissions tooSQL must be used.</span></span>  <span data-ttu-id="8e536-255">Dessutom i ordning tooquery WMI krävs lokal administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="8e536-255">In addition, in order tooquery WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="8e536-256">*Varför visas endast hello topp 10 rekommendationer?*</span><span class="sxs-lookup"><span data-stu-id="8e536-256">*Why display only hello top 10 recommendations?*</span></span>

* <span data-ttu-id="8e536-257">I stället för att ge dig en överväldigande uttömmande förteckning över aktiviteter, rekommenderar vi att du fokusera på adressering hello prioriteras rekommendationer först.</span><span class="sxs-lookup"><span data-stu-id="8e536-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing hello prioritized recommendations first.</span></span> <span data-ttu-id="8e536-258">När du åtgärda dem blir ytterligare rekommendationer tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="8e536-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="8e536-259">Om du föredrar toosee hello detaljerad lista kan du visa alla rekommendationer hello OMS loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="8e536-259">If you prefer toosee hello detailed list, you can view all recommendations using hello OMS log search.</span></span>

<span data-ttu-id="8e536-260">*Finns det ett sätt tooignore en rekommendation?*</span><span class="sxs-lookup"><span data-stu-id="8e536-260">*Is there a way tooignore a recommendation?*</span></span>

* <span data-ttu-id="8e536-261">Ja, se [Ignorera rekommendationer](#ignore-recommendations) ovan.</span><span class="sxs-lookup"><span data-stu-id="8e536-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e536-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e536-262">Next steps</span></span>
* <span data-ttu-id="8e536-263">[Söka i loggar](log-analytics-log-searches.md) tooview detaljerade SQL-bedömning data och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="8e536-263">[Search logs](log-analytics-log-searches.md) tooview detailed SQL Assessment data and recommendations.</span></span>
