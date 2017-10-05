---
title: "Optimera SQL Server-miljön med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda SQL-bedömning lösning med Azure logganalys bedöma risker och hälsotillståndet för din SQL server-miljöer med regelbundna intervall."
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
ms.openlocfilehash: d2aed3315fe60ace46dfb4176dc13aa417257b0c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-sql-server-environment-with-the-sql-assessment-solution-in-log-analytics"></a><span data-ttu-id="66593-103">Optimera SQL Server-miljön med SQL-bedömning lösningen i logganalys</span><span class="sxs-lookup"><span data-stu-id="66593-103">Optimize your SQL Server environment with the SQL Assessment solution in Log Analytics</span></span>

![SQL-bedömning symbol](./media/log-analytics-sql-assessment/sql-assessment-symbol.png)

<span data-ttu-id="66593-105">Du kan använda SQL-bedömning-lösning för att bedöma risken och hälsotillståndet för server-miljöer med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="66593-105">You can use the SQL Assessment solution to assess the risk and health of your server environments on a regular interval.</span></span> <span data-ttu-id="66593-106">Den här artikeln beskriver hur du installerar lösningen så att du kan vidta åtgärder för möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="66593-106">This article will help you install the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="66593-107">Den här lösningen ger en prioriterad lista med rekommendationer som är specifika för din distribuerade serverinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="66593-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="66593-108">Rekommendationerna kategoriseras i sex fokusområden som hjälper dig att snabbt förstå risken och vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="66593-108">The recommendations are categorized across six focus areas which help you quickly understand the risk and take corrective action.</span></span>

<span data-ttu-id="66593-109">Rekommendationer baserat på kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="66593-109">The recommendations made are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="66593-110">Varje rekommendation innehåller information om varför ett problem kan verkligen är viktiga och hur du implementerar de föreslagna ändringarna.</span><span class="sxs-lookup"><span data-stu-id="66593-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="66593-111">Du kan välja fokusområden som är viktigast för din organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.</span><span class="sxs-lookup"><span data-stu-id="66593-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="66593-112">När du har lagt till lösningen och en bedömning är slutförd, Sammanfattning visas information för fokusområden på den **SQL-bedömning** instrumentpanel för infrastrukturen i din miljö.</span><span class="sxs-lookup"><span data-stu-id="66593-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **SQL Assessment** dashboard for the infrastructure in your environment.</span></span> <span data-ttu-id="66593-113">I följande avsnitt beskrivs hur du använder informationen på den **SQL-bedömning** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din SQL server-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="66593-113">The following sections describe how to use the information on the **SQL Assessment** dashboard, where you can view and then take recommended actions for your SQL server infrastructure.</span></span>

![Bild av SQL-bedömning sida vid sida](./media/log-analytics-sql-assessment/sql-assess-tile.png)

![Bild av SQL-bedömning instrumentpanelen](./media/log-analytics-sql-assessment/sql-assess-dash.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="66593-116">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="66593-116">Installing and configuring the solution</span></span>
<span data-ttu-id="66593-117">SQL-bedömning fungerar med alla versioner av SQL Server för Standard, Developer och Enterprise-versioner som stöds för närvarande.</span><span class="sxs-lookup"><span data-stu-id="66593-117">SQL Assessment works with all currently supported versions of SQL Server for the Standard, Developer, and Enterprise editions.</span></span>

<span data-ttu-id="66593-118">Använd följande information för att installera och konfigurera lösningen.</span><span class="sxs-lookup"><span data-stu-id="66593-118">Use the following information to install and configure the solution.</span></span>

* <span data-ttu-id="66593-119">Agenterna måste installeras på servrar som har installerat SQL Server.</span><span class="sxs-lookup"><span data-stu-id="66593-119">Agents must be installed on servers that have SQL Server installed.</span></span>
* <span data-ttu-id="66593-120">SQL-bedömning lösning kräver en version av .NET Framework 4 installeras på varje dator som har en OMS-agent som stöds.</span><span class="sxs-lookup"><span data-stu-id="66593-120">The SQL Assessment solution requires a supported version of .NET Framework 4 installed on each computer that has an OMS agent.</span></span>
* <span data-ttu-id="66593-121">För att kunna installera lösningen måste användaren vara administratör eller deltagare i Azure-prenumeration när du använder Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="66593-121">In order to install the solution, the user must be an administrator or contributor to the Azure subscription when using the Azure portal.</span></span> <span data-ttu-id="66593-122">Användaren måste dessutom ha bidragsgivar- eller ha administratörsrollen för OMS-arbetsytan i OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="66593-122">In addition, the user must be a member of the OMS workspace contributor or administrator role in the OMS portal.</span></span>
* <span data-ttu-id="66593-123">När du använder Operations Manager-agenten med SQL-bedömning, måste du använda ett konto för Operations Manager Run-As.</span><span class="sxs-lookup"><span data-stu-id="66593-123">When using the Operations Manager agent with SQL Assessment, you'll need to use an Operations Manager Run-As account.</span></span> <span data-ttu-id="66593-124">Se [Operations Manager kör som-konton för OMS](#operations-manager-run-as-accounts-for-oms) nedan för mer information.</span><span class="sxs-lookup"><span data-stu-id="66593-124">See [Operations Manager run-as accounts for OMS](#operations-manager-run-as-accounts-for-oms) below for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="66593-125">MMA agenten har inte stöd för Operations Manager Run-As konton.</span><span class="sxs-lookup"><span data-stu-id="66593-125">The MMA agent does not support Operations Manager Run-As accounts.</span></span>
  >
  >
* <span data-ttu-id="66593-126">Lägga till SQL-bedömning lösningen till OMS-arbetsytan med processen som beskrivs i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="66593-126">Add the SQL Assessment solution to your OMS workspace using the process described in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="66593-127">Det krävs ingen ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="66593-127">There is no further configuration required.</span></span>

> [!NOTE]
> <span data-ttu-id="66593-128">När du har lagt till lösningen läggs filen AdvisorAssessment.exe till servrar med agenter.</span><span class="sxs-lookup"><span data-stu-id="66593-128">After you've added the solution, the AdvisorAssessment.exe file is added to servers with agents.</span></span> <span data-ttu-id="66593-129">Konfigurationsdata läsa och sedan skickas till OMS-tjänsten i molnet för bearbetning.</span><span class="sxs-lookup"><span data-stu-id="66593-129">Configuration data is read and then sent to the OMS service in the cloud for processing.</span></span> <span data-ttu-id="66593-130">Molntjänsten innehåller data att logik tillämpas för mottagna data.</span><span class="sxs-lookup"><span data-stu-id="66593-130">Logic is applied to the received data and the cloud service records the data.</span></span>

## <a name="sql-assessment-data-collection-details"></a><span data-ttu-id="66593-131">SQL-bedömning information för samlingen</span><span class="sxs-lookup"><span data-stu-id="66593-131">SQL Assessment data collection details</span></span>
<span data-ttu-id="66593-132">SQL-bedömning samlar in WMI-data, registerdata, prestandadata och SQL Server hantering av dynamiska visa resultat med hjälp av de agenter som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="66593-132">SQL Assessment collects WMI data, registry data, performance data, and SQL Server dynamic management view results using the agents that you have enabled.</span></span>

<span data-ttu-id="66593-133">I följande tabell visas data collection metoder för agenter, om Operations Manager (SCOM) krävs och hur ofta data samlas in av en agent.</span><span class="sxs-lookup"><span data-stu-id="66593-133">The following table shows data collection methods for agents, whether Operations Manager (SCOM) is required, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="66593-134">Plattform</span><span class="sxs-lookup"><span data-stu-id="66593-134">platform</span></span> | <span data-ttu-id="66593-135">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="66593-135">Direct Agent</span></span> | <span data-ttu-id="66593-136">SCOM-agent</span><span class="sxs-lookup"><span data-stu-id="66593-136">SCOM agent</span></span> | <span data-ttu-id="66593-137">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="66593-137">Azure Storage</span></span> | <span data-ttu-id="66593-138">SCOM krävs?</span><span class="sxs-lookup"><span data-stu-id="66593-138">SCOM required?</span></span> | <span data-ttu-id="66593-139">SCOM-agent data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="66593-139">SCOM agent data sent via management group</span></span> | <span data-ttu-id="66593-140">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="66593-140">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="66593-141">Windows</span><span class="sxs-lookup"><span data-stu-id="66593-141">Windows</span></span> | <span data-ttu-id="66593-142">&#8226;</span><span class="sxs-lookup"><span data-stu-id="66593-142">&#8226;</span></span> | <span data-ttu-id="66593-143">&#8226;</span><span class="sxs-lookup"><span data-stu-id="66593-143">&#8226;</span></span> |  |  | <span data-ttu-id="66593-144">&#8226;</span><span class="sxs-lookup"><span data-stu-id="66593-144">&#8226;</span></span> |<span data-ttu-id="66593-145">7 dagar</span><span class="sxs-lookup"><span data-stu-id="66593-145">7 days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="66593-146">Operations Manager kör som-konton för OMS</span><span class="sxs-lookup"><span data-stu-id="66593-146">Operations Manager run-as accounts for OMS</span></span>
<span data-ttu-id="66593-147">Logganalys i OMS använder Operations Manager-agent och hantering av gruppen för att samla in och skicka data till OMS-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="66593-147">Log Analytics in OMS uses the Operations Manager agent and management group to collect and send data to the OMS service.</span></span> <span data-ttu-id="66593-148">OMS-versioner på hanteringspaket för arbetsbelastningar att ge mervärde tjänster.</span><span class="sxs-lookup"><span data-stu-id="66593-148">OMS builds upon management packs for workloads to provide value-add services.</span></span> <span data-ttu-id="66593-149">Varje arbetsbelastning kräver belastningsspecifikt behörighet att köra hanteringspaket i en annan säkerhetskontext, till exempel ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="66593-149">Each workload requires workload-specific privileges to run management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="66593-150">Du måste ange autentiseringsuppgifter genom att konfigurera en Operations Manager kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="66593-150">You need to provide credential information by configuring an Operations Manager Run As account.</span></span>

<span data-ttu-id="66593-151">Använd följande information för att ange Operations Manager kör som-konto för SQL.</span><span class="sxs-lookup"><span data-stu-id="66593-151">Use the following information to set the Operations Manager run-as account for SQL Assessment.</span></span>

### <a name="set-the-run-as-account-for-sql-assessment"></a><span data-ttu-id="66593-152">Ange kör som-konto för SQL-bedömning</span><span class="sxs-lookup"><span data-stu-id="66593-152">Set the Run As account for SQL assessment</span></span>
 <span data-ttu-id="66593-153">Om du redan använder SQL Server management pack, bör du använda Kör som-kontot.</span><span class="sxs-lookup"><span data-stu-id="66593-153">If you are already using the SQL Server management pack, you should use that Run As account.</span></span>

#### <a name="to-configure-the-sql-run-as-account-in-the-operations-console"></a><span data-ttu-id="66593-154">Så här konfigurerar du SQL kör som-kontot i Operations-konsolen</span><span class="sxs-lookup"><span data-stu-id="66593-154">To configure the SQL Run As account in the Operations console</span></span>
> [!NOTE]
> <span data-ttu-id="66593-155">Om du använder direkt OMS-agenten, i stället för SCOM-agent körs alltid management pack i säkerhetskontexten för kontot Lokalt System.</span><span class="sxs-lookup"><span data-stu-id="66593-155">If you are using the OMS direct agent, rather than the SCOM agent, the management pack always runs in the security context of the Local System account.</span></span> <span data-ttu-id="66593-156">Hoppa över steg 1-5 nedan och köra T-SQL eller Powershell-exempel som anger att NT AUTHORITY\SYSTEM användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="66593-156">Skip steps 1-5 below, and run either the T-SQL or Powershell sample, specifying NT AUTHORITY\SYSTEM as the user name.</span></span>
>
>

1. <span data-ttu-id="66593-157">Öppna driftkonsolen i Operations Manager och klicka sedan på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="66593-157">In Operations Manager, open the Operations console, and then click **Administration**.</span></span>
2. <span data-ttu-id="66593-158">Under **kör som-konfiguration**, klickar du på **profiler**, och öppna **OMS SQL Assessment kör som-profilen**.</span><span class="sxs-lookup"><span data-stu-id="66593-158">Under **Run As Configuration**, click **Profiles**, and open **OMS SQL Assessment Run As Profile**.</span></span>
3. <span data-ttu-id="66593-159">På den **kör som-konton** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="66593-159">On the **Run As Accounts** page, click **Add**.</span></span>
4. <span data-ttu-id="66593-160">Välj ett Windows kör som-konto som innehåller de autentiseringsuppgifter som krävs för SQL Server, eller klicka på **ny** att skapa en.</span><span class="sxs-lookup"><span data-stu-id="66593-160">Select a Windows Run As account that contains the credentials needed for SQL Server, or click **New** to create one.</span></span>

   > [!NOTE]
   > <span data-ttu-id="66593-161">Kör som-kontotyp måste vara Windows.</span><span class="sxs-lookup"><span data-stu-id="66593-161">The Run As account type must be Windows.</span></span> <span data-ttu-id="66593-162">Kör som-kontot måste också ingå i gruppen lokala administratörer på alla Windows-servrar som är värd för SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="66593-162">The Run As account must also be part of Local Administrators group on all Windows Servers hosting SQL Server Instances.</span></span>
   >
   >
5. <span data-ttu-id="66593-163">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="66593-163">Click **Save**.</span></span>
6. <span data-ttu-id="66593-164">Ändra och kör följande T-SQL-exempel på varje SQL Server-instans för att bevilja lägsta behörighet som krävs för Kör som-konto att utföra SQL-bedömning.</span><span class="sxs-lookup"><span data-stu-id="66593-164">Modify and then execute the following T-SQL sample on each SQL Server Instance to grant minimum permissions required to Run As Account to perform SQL Assessment.</span></span> <span data-ttu-id="66593-165">Dock behöver du inte göra det om ett kör som-konto redan är en del av serverrollen sysadmin på SQL Server-instanser.</span><span class="sxs-lookup"><span data-stu-id="66593-165">However, you don’t need to do this if a Run As Account is already part of the sysadmin server role on SQL Server Instances.</span></span>

```
---
    -- Replace <UserName> with the actual user name being used as Run As Account.
    USE master

    -- Create login for the user, comment this line if login is already created.
    CREATE LOGIN [<UserName>] FROM WINDOWS

    -- Grant permissions to user.
    GRANT VIEW SERVER STATE TO [<UserName>]
    GRANT VIEW ANY DEFINITION TO [<UserName>]
    GRANT VIEW ANY DATABASE TO [<UserName>]

    -- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
    -- NOTE: This command must be run anytime new databases are added to SQL Server instances.
    EXEC sp_msforeachdb N'USE [?]; CREATE USER [<UserName>] FOR LOGIN [<UserName>];'

```
#### <a name="to-configure-the-sql-run-as-account-using-windows-powershell"></a><span data-ttu-id="66593-166">Så här konfigurerar du SQL kör som-kontot med Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="66593-166">To configure the SQL Run As account using Windows PowerShell</span></span>
<span data-ttu-id="66593-167">Öppna ett PowerShell-fönster och kör följande skript när du har uppdaterat den med din information:</span><span class="sxs-lookup"><span data-stu-id="66593-167">Open a PowerShell window and run the following script after you’ve updated it with your information:</span></span>

```

    import-module OperationsManager
    New-SCOMManagementGroupConnection "<your management group name>"

    $profile = Get-SCOMRunAsProfile -DisplayName "OMS SQL Assessment Run As Profile"
    $account = Get-SCOMrunAsAccount | Where-Object {$_.Name -eq "<your run as account name>"}
    Set-SCOMRunAsProfile -Action "Add" -Profile $Profile -Account $Account
```

## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="66593-168">Förstå hur rekommendationer prioriteras</span><span class="sxs-lookup"><span data-stu-id="66593-168">Understanding how recommendations are prioritized</span></span>
<span data-ttu-id="66593-169">Varje rekommendation är ett givet värde som identifierar en avvägning mellan kraven för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="66593-169">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="66593-170">Bara de tio viktigaste rekommendationerna visas.</span><span class="sxs-lookup"><span data-stu-id="66593-170">Only the ten most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="66593-171">Hur vikterna beräknas</span><span class="sxs-lookup"><span data-stu-id="66593-171">How weights are calculated</span></span>
<span data-ttu-id="66593-172">Viktningar är sammanställda värden som baseras på tre viktiga faktorer:</span><span class="sxs-lookup"><span data-stu-id="66593-172">Weightings are aggregate values based on three key factors:</span></span>

* <span data-ttu-id="66593-173">Den *sannolikhet* att ett problem som identifieras ska orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="66593-173">The *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="66593-174">En högre sannolikhet är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="66593-174">A higher probability equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="66593-175">Den *inverkan* av problemet i din organisation om det uppstår ett problem.</span><span class="sxs-lookup"><span data-stu-id="66593-175">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="66593-176">En högre inverkan är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="66593-176">A higher impact equates to a larger overall score for the recommendation.</span></span>
* <span data-ttu-id="66593-177">Den *arbete* krävs för att implementera denna rekommendation.</span><span class="sxs-lookup"><span data-stu-id="66593-177">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="66593-178">Det är lika med en mindre övergripande poäng för rekommendationen högre prestanda.</span><span class="sxs-lookup"><span data-stu-id="66593-178">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="66593-179">Värde för varje rekommendation uttrycks i procent av den sammanlagda poängen för varje fokusområde.</span><span class="sxs-lookup"><span data-stu-id="66593-179">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="66593-180">Till exempel om en rekommendation i området för säkerhet och efterlevnad fokus har ett resultat på 5%, ökar implementera denna rekommendation din övergripande säkerhet och efterlevnad poäng av 5%.</span><span class="sxs-lookup"><span data-stu-id="66593-180">For example, if a recommendation in the Security and Compliance focus area has a score of 5%, implementing that recommendation will increase your overall Security and Compliance score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="66593-181">Fokusområden</span><span class="sxs-lookup"><span data-stu-id="66593-181">Focus areas</span></span>
<span data-ttu-id="66593-182">**Säkerhet och efterlevnad** -fokus visas här rekommendationer för potentiella säkerhetshot och överträdelser, företagets principer och krav på teknisk, juridisk och regelmässig efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="66593-182">**Security and Compliance** - This focus area shows recommendations for potential security threats and breaches, corporate policies, and technical, legal and regulatory compliance requirements.</span></span>

<span data-ttu-id="66593-183">**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.</span><span class="sxs-lookup"><span data-stu-id="66593-183">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="66593-184">**Prestanda och skalbarhet** -fokus visas här rekommendationer som hjälper din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan svara på förändrade infrastrukturbehov.</span><span class="sxs-lookup"><span data-stu-id="66593-184">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="66593-185">**Uppgradera migrering och distribution** -fokus visas här rekommendationer när du uppgraderar, migrera och distribuera SQL Server till den befintliga infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="66593-185">**Upgrade, Migration and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.</span></span>

<span data-ttu-id="66593-186">**Åtgärder och övervakning** -fokus visas här rekommendationer för att effektivisera IT-avdelningen, implementera förebyggande underhåll och maximera prestanda.</span><span class="sxs-lookup"><span data-stu-id="66593-186">**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

<span data-ttu-id="66593-187">**Ändrings- och konfigurationshantering** -fokus visas här rekommendationer för att skydda dagliga uppgifter, se till att ändringar inte negativt påverka din infrastruktur kan upprätta ändringskontroll, och för att spåra och granska systemkonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="66593-187">**Change and Configuration Management** - This focus area shows recommendations to help protect day-to-day operations, ensure that changes don't negatively affect your infrastructure, establish change control procedures, and to track and audit system configurations.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="66593-188">Bör du försöka poängsätta 100% överallt fokus i?</span><span class="sxs-lookup"><span data-stu-id="66593-188">Should you aim to score 100% in every focus area?</span></span>
<span data-ttu-id="66593-189">Inte nödvändigtvis.</span><span class="sxs-lookup"><span data-stu-id="66593-189">Not necessarily.</span></span> <span data-ttu-id="66593-190">Rekommendationerna baseras på kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="66593-190">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="66593-191">Dock inga två server-infrastrukturen är samma och specifika rekommendationer kan vara mer eller mindre relevant för dig.</span><span class="sxs-lookup"><span data-stu-id="66593-191">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="66593-192">Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte utsätts för Internet.</span><span class="sxs-lookup"><span data-stu-id="66593-192">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="66593-193">Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering.</span><span class="sxs-lookup"><span data-stu-id="66593-193">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="66593-194">Problem som är viktiga för en mogen företag kanske mindre viktiga för en Startup.</span><span class="sxs-lookup"><span data-stu-id="66593-194">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="66593-195">Du kanske vill identifiera vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="66593-195">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="66593-196">Varje rekommendation innehåller information om varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="66593-196">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="66593-197">Du bör använda den här vägledningen för att utvärdera om implementera rekommendationen är lämplig för dig, baserat på typ av din IT-tjänster och affärsbehoven för din organisation.</span><span class="sxs-lookup"><span data-stu-id="66593-197">You should use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="66593-198">Använd assessment fokus området rekommendationer</span><span class="sxs-lookup"><span data-stu-id="66593-198">Use assessment focus area recommendations</span></span>
<span data-ttu-id="66593-199">Innan du kan använda en lösning i OMS måste ha installerat lösningen.</span><span class="sxs-lookup"><span data-stu-id="66593-199">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="66593-200">Du kan läsa mer om hur du installerar lösningar finns [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="66593-200">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="66593-201">När den har installerats kan du visa sammanfattning av rekommendationer med hjälp av SQL-bedömning panelen på översiktssidan i OMS.</span><span class="sxs-lookup"><span data-stu-id="66593-201">After it is installed, you can view the summary of recommendations by using the SQL Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="66593-202">Visa sammanfattade efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="66593-202">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="66593-203">Visa rekommendationer för en Fokusområde och vidta åtgärder</span><span class="sxs-lookup"><span data-stu-id="66593-203">To view recommendations for a focus area and take corrective action</span></span>
1. <span data-ttu-id="66593-204">På den **översikt** klickar du på den **SQL-bedömning** panelen.</span><span class="sxs-lookup"><span data-stu-id="66593-204">On the **Overview** page, click the **SQL Assessment** tile.</span></span>
2. <span data-ttu-id="66593-205">På den **SQL-bedömning** , Granska sammanfattningen i ett fokus området blad och klickar sedan på en om du vill visa rekommendationer för området fokus.</span><span class="sxs-lookup"><span data-stu-id="66593-205">On the **SQL Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="66593-206">På någon av sidorna fokus område, kan du visa prioriterad rekommendationer för din miljö.</span><span class="sxs-lookup"><span data-stu-id="66593-206">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="66593-207">Klicka på en rekommendation enligt **påverkade objekt** att visa information om varför rekommendationen görs.</span><span class="sxs-lookup"><span data-stu-id="66593-207">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="66593-208">![Bild av SQL-bedömning rekommendationer](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span><span class="sxs-lookup"><span data-stu-id="66593-208">![image of SQL Assessment recommendations](./media/log-analytics-sql-assessment/sql-assess-focus.png)</span></span>
4. <span data-ttu-id="66593-209">Du kan vidta åtgärder i **föreslagna åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="66593-209">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="66593-210">När objektet har behandlats registrerar senare bedömningar den rekommenderade åtgärder som utförts och kompatibilitet poängen ökar.</span><span class="sxs-lookup"><span data-stu-id="66593-210">When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="66593-211">Korrigerade objekt visas som **skickades objekt**.</span><span class="sxs-lookup"><span data-stu-id="66593-211">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="66593-212">Ignorera rekommendationer</span><span class="sxs-lookup"><span data-stu-id="66593-212">Ignore recommendations</span></span>
<span data-ttu-id="66593-213">Om du har rekommendationer som du vill ignorera, kan du skapa en textfil som OMS använder för att förhindra rekommendationer visas i sökresultatet assessment.</span><span class="sxs-lookup"><span data-stu-id="66593-213">If you have recommendations that you want to ignore, you can create a text file that OMS will use to prevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-will-ignore"></a><span data-ttu-id="66593-214">Att identifiera rekommendationer som kommer att ignorera</span><span class="sxs-lookup"><span data-stu-id="66593-214">To identify recommendations that you will ignore</span></span>
1. <span data-ttu-id="66593-215">Logga in på din arbetsyta och öppna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="66593-215">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="66593-216">Använd följande fråga för att lista över rekommendationer som har misslyckats för datorer i din miljö.</span><span class="sxs-lookup"><span data-stu-id="66593-216">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```

   <span data-ttu-id="66593-217">Här är en skärmbild som visar loggen sökfråga: ![misslyckades rekommendationer](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span><span class="sxs-lookup"><span data-stu-id="66593-217">Here's a screen shot showing the Log Search query: ![failed recommendations](./media/log-analytics-sql-assessment/sql-assess-failed-recommendations.png)</span></span>
2. <span data-ttu-id="66593-218">Välj rekommendationer som du vill ignorera.</span><span class="sxs-lookup"><span data-stu-id="66593-218">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="66593-219">Du ska använda värdena för RecommendationId i nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="66593-219">You’ll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="66593-220">Du skapar och använder en IgnoreRecommendations.txt textfil</span><span class="sxs-lookup"><span data-stu-id="66593-220">To create and use an IgnoreRecommendations.txt text file</span></span>
1. <span data-ttu-id="66593-221">Skapa en fil med namnet IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="66593-221">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="66593-222">Klistra in eller ange varje RecommendationId för varje rekommendation som du vill OMS Ignorera på en separat rad och spara och stäng filen.</span><span class="sxs-lookup"><span data-stu-id="66593-222">Paste or type each RecommendationId for each recommendation that you want OMS to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="66593-223">Placera filen i följande mapp på varje dator där du vill att OMS att ignorera rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="66593-223">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
   * <span data-ttu-id="66593-224">På datorer med Microsoft Monitoring Agent (ansluten direkt eller via Operations Manager) - *SystemDrive*: \Program\Microsoft övervakning Agent\Agent</span><span class="sxs-lookup"><span data-stu-id="66593-224">On computers with the Microsoft Monitoring Agent (connected directly or through Operations Manager) - *SystemDrive*:\Program Files\Microsoft Monitoring Agent\Agent</span></span>
   * <span data-ttu-id="66593-225">På hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server</span><span class="sxs-lookup"><span data-stu-id="66593-225">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="66593-226">Kontrollera att rekommendationer ignoreras</span><span class="sxs-lookup"><span data-stu-id="66593-226">To verify that recommendations are ignored</span></span>
1. <span data-ttu-id="66593-227">När nästa schemalagda utvärdering körs som standard var sjunde dag, angivna rekommendationerna markeras ignoreras och kommer inte att visas på instrumentpanelen assessment.</span><span class="sxs-lookup"><span data-stu-id="66593-227">After the next scheduled assessment runs, by default every 7 days, the specified recommendations are marked Ignored and will not appear on the assessment dashboard.</span></span>
2. <span data-ttu-id="66593-228">Du kan använda följande loggen sökfrågor för att lista alla rekommendationer som ignoreras.</span><span class="sxs-lookup"><span data-stu-id="66593-228">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

   ```
   Type=SQLAssessmentRecommendation RecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
   ```
3. <span data-ttu-id="66593-229">Om du senare bestämmer att du vill se ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.</span><span class="sxs-lookup"><span data-stu-id="66593-229">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="sql-assessment-solution-faq"></a><span data-ttu-id="66593-230">SQL lösning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="66593-230">SQL Assessment solution FAQ</span></span>
<span data-ttu-id="66593-231">*Hur ofta körs en utvärdering?*</span><span class="sxs-lookup"><span data-stu-id="66593-231">*How often does an assessment run?*</span></span>

* <span data-ttu-id="66593-232">Utvärderingen körs var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="66593-232">The assessment runs every 7 days.</span></span>

<span data-ttu-id="66593-233">*Finns det ett sätt att konfigurera hur ofta bedömning körs?*</span><span class="sxs-lookup"><span data-stu-id="66593-233">*Is there a way to configure how often the assessment runs?*</span></span>

* <span data-ttu-id="66593-234">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="66593-234">Not at this time.</span></span>

<span data-ttu-id="66593-235">*Om en annan server identifieras när jag har lagt till SQL-bedömning lösning, kommer utvärderas det?*</span><span class="sxs-lookup"><span data-stu-id="66593-235">*If another server is discovered after I’ve added the SQL assessment solution, will it be assessed?*</span></span>

* <span data-ttu-id="66593-236">Ja, när det upptäcks att det bedöms från sedan, var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="66593-236">Yes, once it is discovered it is assessed from then on, every 7 days.</span></span>

<span data-ttu-id="66593-237">*Om en server tas ur drift, när den tas bort från bedömningen?*</span><span class="sxs-lookup"><span data-stu-id="66593-237">*If a server is decommissioned, when will it be removed from the assessment?*</span></span>

* <span data-ttu-id="66593-238">Om en server inte lämnar data 3 veckor tas bort.</span><span class="sxs-lookup"><span data-stu-id="66593-238">If a server does not submit data for 3 weeks, it is removed.</span></span>

<span data-ttu-id="66593-239">*Vad är namnet på processen som gör datainsamlingen?*</span><span class="sxs-lookup"><span data-stu-id="66593-239">*What is the name of the process that does the data collection?*</span></span>

* <span data-ttu-id="66593-240">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="66593-240">AdvisorAssessment.exe</span></span>

<span data-ttu-id="66593-241">*Hur lång tid tar det för data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="66593-241">*How long does it take for data to be collected?*</span></span>

* <span data-ttu-id="66593-242">Samlingen faktiska data på servern tar ungefär en timme.</span><span class="sxs-lookup"><span data-stu-id="66593-242">The actual data collection on the server takes about 1 hour.</span></span> <span data-ttu-id="66593-243">Det kan ta längre tid på servrar som har ett stort antal databaser eller SQL-instanser.</span><span class="sxs-lookup"><span data-stu-id="66593-243">It may take longer on servers that have a large number of SQL instances or databases.</span></span>

<span data-ttu-id="66593-244">*Vilken typ av data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="66593-244">*What type of data is collected?*</span></span>

* <span data-ttu-id="66593-245">Följande typer av data samlas in:</span><span class="sxs-lookup"><span data-stu-id="66593-245">The following types of data are collected:</span></span>
  * <span data-ttu-id="66593-246">WMI</span><span class="sxs-lookup"><span data-stu-id="66593-246">WMI</span></span>
  * <span data-ttu-id="66593-247">Registret</span><span class="sxs-lookup"><span data-stu-id="66593-247">Registry</span></span>
  * <span data-ttu-id="66593-248">Prestandaräknare</span><span class="sxs-lookup"><span data-stu-id="66593-248">Performance counters</span></span>
  * <span data-ttu-id="66593-249">SQL dynamiska hanteringsvyer (DMV).</span><span class="sxs-lookup"><span data-stu-id="66593-249">SQL dynamic management views (DMV).</span></span>

<span data-ttu-id="66593-250">*Finns det ett sätt att konfigurera när data samlas in?*</span><span class="sxs-lookup"><span data-stu-id="66593-250">*Is there a way to configure when data is collected?*</span></span>

* <span data-ttu-id="66593-251">Inte just nu.</span><span class="sxs-lookup"><span data-stu-id="66593-251">Not at this time.</span></span>

<span data-ttu-id="66593-252">*Varför måste jag konfigurera ett kör som-konto?*</span><span class="sxs-lookup"><span data-stu-id="66593-252">*Why do I have to configure a Run As Account?*</span></span>

* <span data-ttu-id="66593-253">För SQL Server körs ett litet antal SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="66593-253">For SQL Server, a small number of SQL queries are run.</span></span> <span data-ttu-id="66593-254">För att kunna köra måste en Kör som-konto med VIEW SERVER STATE behörighet till SQL användas.</span><span class="sxs-lookup"><span data-stu-id="66593-254">In order for them to run, a Run As Account with VIEW SERVER STATE permissions to SQL must be used.</span></span>  <span data-ttu-id="66593-255">Dessutom för att fråga WMI krävs lokal administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="66593-255">In addition, in order to query WMI, local administrator credentials are required.</span></span>

<span data-ttu-id="66593-256">*Varför visas endast topp 10 rekommendationer?*</span><span class="sxs-lookup"><span data-stu-id="66593-256">*Why display only the top 10 recommendations?*</span></span>

* <span data-ttu-id="66593-257">I stället för att ge dig en överväldigande uttömmande förteckning över aktiviteter, rekommenderar vi att du fokusera på adressering prioriterad rekommendationerna först.</span><span class="sxs-lookup"><span data-stu-id="66593-257">Instead of giving you an exhaustive overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="66593-258">När du åtgärda dem blir ytterligare rekommendationer tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="66593-258">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="66593-259">Du kan visa alla rekommendationer OMS loggen sökning om du vill se en detaljerad lista.</span><span class="sxs-lookup"><span data-stu-id="66593-259">If you prefer to see the detailed list, you can view all recommendations using the OMS log search.</span></span>

<span data-ttu-id="66593-260">*Finns det ett sätt att ignorera en rekommendation?*</span><span class="sxs-lookup"><span data-stu-id="66593-260">*Is there a way to ignore a recommendation?*</span></span>

* <span data-ttu-id="66593-261">Ja, se [Ignorera rekommendationer](#ignore-recommendations) ovan.</span><span class="sxs-lookup"><span data-stu-id="66593-261">Yes, see [Ignore recommendations](#ignore-recommendations) section above.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66593-262">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66593-262">Next steps</span></span>
* <span data-ttu-id="66593-263">[Söka i loggar](log-analytics-log-searches.md) att visa detaljerad information i SQL-bedömning och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="66593-263">[Search logs](log-analytics-log-searches.md) to view detailed SQL Assessment data and recommendations.</span></span>
