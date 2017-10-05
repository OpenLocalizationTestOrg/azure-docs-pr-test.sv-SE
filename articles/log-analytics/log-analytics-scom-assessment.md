---
title: "Optimera din miljö för System Center Operations Manager med Azure Log Analytics | Microsoft Docs"
description: "Du kan använda System Center Operations Manager Assessment-lösning för att bedöma risker och hälsotillståndet för server-miljöer med regelbundna intervall."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: tysonn
ms.assetid: 49aad8b1-3e05-4588-956c-6fdd7715cda1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4992d98397da409f7c1cfbdeb40fdb0cdd0d2f19
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="optimize-your-environment-with-the-system-center-operations-manager-assessment-preview-solution"></a><span data-ttu-id="be63c-103">Optimera din miljö med System Center Operations Manager Assessment (förhandsgranskning)-lösning</span><span class="sxs-lookup"><span data-stu-id="be63c-103">Optimize your environment with the System Center Operations Manager Assessment (Preview) solution</span></span>

![System Center Operations Manager Assessment symbol](./media/log-analytics-scom-assessment/scom-assessment-symbol.png)

<span data-ttu-id="be63c-105">Du kan använda System Center Operations Manager Assessment-lösning för att bedöma risken och hälsotillståndet för ditt System Center Operations Manager server-miljöer med regelbundna intervall.</span><span class="sxs-lookup"><span data-stu-id="be63c-105">You can use the System Center Operations Manager Assessment solution to assess the risk and health of your System Center Operations Manager server environments on a regular interval.</span></span> <span data-ttu-id="be63c-106">Den här artikeln hjälper dig att installera, konfigurera och använda lösningen så att du kan vidta åtgärder för möjliga problem.</span><span class="sxs-lookup"><span data-stu-id="be63c-106">This article helps you install, configure, and use the solution so that you can take corrective actions for potential problems.</span></span>

<span data-ttu-id="be63c-107">Den här lösningen ger en prioriterad lista med rekommendationer som är specifika för din distribuerade serverinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="be63c-107">This solution provides a prioritized list of recommendations specific to your deployed server infrastructure.</span></span> <span data-ttu-id="be63c-108">Rekommendationerna kategoriseras i fyra fokusområden, som hjälper dig att snabbt förstå risken och vidta åtgärder.</span><span class="sxs-lookup"><span data-stu-id="be63c-108">The recommendations are categorized across four focus areas, which help you quickly understand the risk and take corrective action.</span></span>

<span data-ttu-id="be63c-109">Rekommendationer baserat på kunskap och erfarenhet av Microsoft-tekniker från tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="be63c-109">The recommendations made are based on the knowledge and experience gained by Microsoft engineers from thousands of customer visits.</span></span> <span data-ttu-id="be63c-110">Varje rekommendation innehåller information om varför ett problem kan verkligen är viktiga och hur du implementerar de föreslagna ändringarna.</span><span class="sxs-lookup"><span data-stu-id="be63c-110">Each recommendation provides guidance about why an issue might matter to you and how to implement the suggested changes.</span></span>

<span data-ttu-id="be63c-111">Du kan välja fokusområden som är viktigast för din organisation och spåra din framsteg mot kör en risk ledigt och hälsosam miljö.</span><span class="sxs-lookup"><span data-stu-id="be63c-111">You can choose focus areas that are most important to your organization and track your progress toward running a risk free and healthy environment.</span></span>

<span data-ttu-id="be63c-112">När du har lagt till lösningen och en bedömning är slutförd, Sammanfattning visas information för fokusområden på den **System Center Operations Manager Assessment** instrumentpanel för din infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="be63c-112">After you've added the solution and an assessment is completed, summary information for focus areas is shown on the **System Center Operations Manager Assessment** dashboard for your infrastructure.</span></span> <span data-ttu-id="be63c-113">I följande avsnitt beskrivs hur du använder informationen på den **System Center Operations Manager Assessment** instrumentpanel, där du kan visa och sedan vidta rekommenderade åtgärder för din SCOM-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="be63c-113">The following sections describe how to use the information on the **System Center Operations Manager Assessment** dashboard, where you can view and then take recommended actions for your SCOM infrastructure.</span></span>

![System Center Operations Manager-lösningen sida vid sida](./media/log-analytics-scom-assessment/scom-tile.png)

![System Center Operations Manager Assessment instrumentpanelen](./media/log-analytics-scom-assessment/scom-dashboard01.png)

## <a name="installing-and-configuring-the-solution"></a><span data-ttu-id="be63c-116">Installera och konfigurera lösningen</span><span class="sxs-lookup"><span data-stu-id="be63c-116">Installing and configuring the solution</span></span>

<span data-ttu-id="be63c-117">Lösningen fungerar med Microsoft System Operations Manager 2012 R2 och 2012 SP1.</span><span class="sxs-lookup"><span data-stu-id="be63c-117">The solution works with Microsoft System Operations Manager 2012 R2 and 2012 SP1.</span></span>

<span data-ttu-id="be63c-118">Använd följande information för att installera och konfigurera lösningen.</span><span class="sxs-lookup"><span data-stu-id="be63c-118">Use the following information to install and configure the solution.</span></span>

 - <span data-ttu-id="be63c-119">Innan du kan använda en lösning i OMS måste ha installerat lösningen.</span><span class="sxs-lookup"><span data-stu-id="be63c-119">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="be63c-120">Installera lösning från [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) eller genom att följa instruktionerna i [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="be63c-120">Install the solution from [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.SCOMAssessmentOMS?tab=Overview) or by following the instructions in [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span>

 - <span data-ttu-id="be63c-121">När du lägger till lösningen till arbetsytan visas bedömning av System Center Operations Manager-panelen på instrumentpanelen ytterligare konfiguration krävs.</span><span class="sxs-lookup"><span data-stu-id="be63c-121">After adding the solution to the workspace, the System Center Operations Manager Assessment tile on the dashboard displays the additional configuration required message.</span></span> <span data-ttu-id="be63c-122">Klicka på panelen och följ konfigurationsstegen anges på sidan</span><span class="sxs-lookup"><span data-stu-id="be63c-122">Click on the tile and follow the configuration steps mentioned in the page</span></span>

 ![System Center Operations Manager-instrumentpanelen](./media/log-analytics-scom-assessment/scom-configrequired-tile.png)

 <span data-ttu-id="be63c-124">Konfiguration av System Center Operations Manager kan göras via skriptet genom att följa anvisningarna i konfigurationssidan för lösningen i OMS.</span><span class="sxs-lookup"><span data-stu-id="be63c-124">Configuration of the System Center Operations Manager can be done through the script by following the steps mentioned in the configuration page of the solution in OMS.</span></span>

 <span data-ttu-id="be63c-125">Följ i stället för att konfigurera assessment via SCOM-konsolen, de nedanstående steg i samma ordning</span><span class="sxs-lookup"><span data-stu-id="be63c-125">Instead, to configure the assessment through SCOM Console, follow the below steps in the same order</span></span>
1. [<span data-ttu-id="be63c-126">Ange kör som-kontot för System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="be63c-126">Set the Run As account for System Center Operations Manager Assessment</span></span>](#operations-manager-run-as-accounts-for-oms)  
2. [<span data-ttu-id="be63c-127">Konfigurera System Center Operations Manager Assessment-regel</span><span class="sxs-lookup"><span data-stu-id="be63c-127">Configure the System Center Operations Manager Assessment rule</span></span>](#configure-the-assessment-rule)

## <a name="system-center-operations-manager-assessment-data-collection-details"></a><span data-ttu-id="be63c-128">System Center Operations Manager assessment data samling information</span><span class="sxs-lookup"><span data-stu-id="be63c-128">System Center Operations Manager assessment data collection details</span></span>

<span data-ttu-id="be63c-129">System Center Operations Manager-bedömning samlar in WMI-data, registerdata, EventLog data, Operations Manager data via Windows PowerShell, SQL-frågor, filen information insamlaren med den server som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="be63c-129">The System Center Operations Manager assessment collects WMI data, Registry data, EventLog data, Operations Manager data through Windows PowerShell, SQL Queries, File information collector using the server that you have enabled.</span></span>

<span data-ttu-id="be63c-130">I följande tabell visas data collection metoder för System Center Operations Manager, och hur ofta data samlas in av en agent.</span><span class="sxs-lookup"><span data-stu-id="be63c-130">The following table shows data collection methods for System Center Operations Manager Assessment, and how often data is collected by an agent.</span></span>

| <span data-ttu-id="be63c-131">Plattform</span><span class="sxs-lookup"><span data-stu-id="be63c-131">platform</span></span> | <span data-ttu-id="be63c-132">Styr Agent</span><span class="sxs-lookup"><span data-stu-id="be63c-132">Direct Agent</span></span> | <span data-ttu-id="be63c-133">SCOM-agent</span><span class="sxs-lookup"><span data-stu-id="be63c-133">SCOM agent</span></span> | <span data-ttu-id="be63c-134">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="be63c-134">Azure Storage</span></span> | <span data-ttu-id="be63c-135">SCOM krävs?</span><span class="sxs-lookup"><span data-stu-id="be63c-135">SCOM required?</span></span> | <span data-ttu-id="be63c-136">SCOM-agent data som skickas via management-grupp</span><span class="sxs-lookup"><span data-stu-id="be63c-136">SCOM agent data sent via management group</span></span> | <span data-ttu-id="be63c-137">Frekvens för samlingen</span><span class="sxs-lookup"><span data-stu-id="be63c-137">collection frequency</span></span> |
| --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="be63c-138">Windows</span><span class="sxs-lookup"><span data-stu-id="be63c-138">Windows</span></span> | | | | <span data-ttu-id="be63c-139">&#8226;</span><span class="sxs-lookup"><span data-stu-id="be63c-139">&#8226;</span></span> | | <span data-ttu-id="be63c-140">sju dagar</span><span class="sxs-lookup"><span data-stu-id="be63c-140">seven days</span></span> |

## <a name="operations-manager-run-as-accounts-for-oms"></a><span data-ttu-id="be63c-141">Operations Manager kör som-konton för OMS</span><span class="sxs-lookup"><span data-stu-id="be63c-141">Operations Manager run-as accounts for OMS</span></span>

<span data-ttu-id="be63c-142">OMS bygger på hanteringspaket för arbetsbelastningar att ge mervärde tjänster.</span><span class="sxs-lookup"><span data-stu-id="be63c-142">OMS builds on management packs for workloads to provide value-add services.</span></span> <span data-ttu-id="be63c-143">Varje arbetsbelastning kräver belastningsspecifikt behörighet att köra hanteringspaket i en annan säkerhetskontext, till exempel ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="be63c-143">Each workload requires workload-specific privileges to run management packs in a different security context, such as a domain account.</span></span> <span data-ttu-id="be63c-144">Konfigurera en Operations Manager kör som-konto för att tillhandahålla autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="be63c-144">Configure an Operations Manager Run As account to provide credential information.</span></span>

<span data-ttu-id="be63c-145">Använd följande information för att ange Operations Manager kör som-konto för System Center Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="be63c-145">Use the following information to set the Operations Manager run-as account for System Center Operations Manager Assessment.</span></span>

### <a name="set-the-run-as-account"></a><span data-ttu-id="be63c-146">Ange kör som-konto</span><span class="sxs-lookup"><span data-stu-id="be63c-146">Set the Run As account</span></span>

1. <span data-ttu-id="be63c-147">I Operations Manager-konsolen går du till den **Administration** fliken.</span><span class="sxs-lookup"><span data-stu-id="be63c-147">In the Operations Manager Console, go to the **Administration** tab.</span></span>
2. <span data-ttu-id="be63c-148">Under den **kör som-konfiguration**, klickar du på **konton**.</span><span class="sxs-lookup"><span data-stu-id="be63c-148">Under the **Run As Configuration**, click **Accounts**.</span></span>
3. <span data-ttu-id="be63c-149">Skapa kör som-kontot, följande via guiden skapar ett windowskonto.</span><span class="sxs-lookup"><span data-stu-id="be63c-149">Create the Run As Account, following through the Wizard, creating a Windows account.</span></span> <span data-ttu-id="be63c-150">Kontot som ska användas är den som identifierats och att alla nödvändiga komponenter nedan:</span><span class="sxs-lookup"><span data-stu-id="be63c-150">The account to use is the one identified and having all the prerequisites below:</span></span>

    >[!NOTE]
    <span data-ttu-id="be63c-151">Kör som-kontot måste uppfylla följande krav:</span><span class="sxs-lookup"><span data-stu-id="be63c-151">The Run As account must meet following requirements:</span></span>
    - <span data-ttu-id="be63c-152">En domänmedlem konto i den lokala gruppen Administratörer på alla servrar i miljön (alla Operations Manager-roller - hanteringsservern, OpsMgr-databasen, datalagret, rapportering, webbkonsolen, Gateway)</span><span class="sxs-lookup"><span data-stu-id="be63c-152">A domain account member of the local Administrators group on all servers in the environment (All Operations Manager Roles - Management Server, OpsMgr Database, Data Warehouse, Reporting, Web Console, Gateway)</span></span>
    - <span data-ttu-id="be63c-153">Åtgärden Managers administratörsroll för hanteringsgruppen som ska utvärderas</span><span class="sxs-lookup"><span data-stu-id="be63c-153">Operation Manager Administrator Role for the management group being assessed</span></span>
    - <span data-ttu-id="be63c-154">Köra den [skriptet](#sql-script-to-grant-granular-permissions-to-the-run-as-account) att ge detaljerade behörigheter till kontot för SQL-instans som används av Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="be63c-154">Execute the [script](#sql-script-to-grant-granular-permissions-to-the-run-as-account) to grant granular permissions to the account on SQL instance used by Operations Manager.</span></span>
      <span data-ttu-id="be63c-155">Obs: Om det här kontot har redan sysadmin-behörighet, går vidare körningen av skriptet.</span><span class="sxs-lookup"><span data-stu-id="be63c-155">Note: If this account has sysadmin rights already, then skip the script execution.</span></span>

4. <span data-ttu-id="be63c-156">Under **Distributionssäkerhet**väljer **säkrare**.</span><span class="sxs-lookup"><span data-stu-id="be63c-156">Under **Distribution Security**, select **More secure**.</span></span>
5. <span data-ttu-id="be63c-157">Ange den hanteringsserver där kontot är distribuerade.</span><span class="sxs-lookup"><span data-stu-id="be63c-157">Specify the management server where the account is distributed.</span></span>
3. <span data-ttu-id="be63c-158">Gå tillbaka till det kör som-konfiguration och klicka på **profiler**.</span><span class="sxs-lookup"><span data-stu-id="be63c-158">Go back to the Run As Configuration and click **Profiles**.</span></span>
4. <span data-ttu-id="be63c-159">Sök efter den *SCOM Assessment profil*.</span><span class="sxs-lookup"><span data-stu-id="be63c-159">Search for the *SCOM Assessment Profile*.</span></span>
5. <span data-ttu-id="be63c-160">Namnet på profilen bör vara: *Microsoft System Center Advisor SCOM Assessment kör som-profilen*.</span><span class="sxs-lookup"><span data-stu-id="be63c-160">The profile name should be: *Microsoft System Center Advisor SCOM Assessment Run As Profile*.</span></span>
6. <span data-ttu-id="be63c-161">Högerklicka på och uppdatera dess egenskaper och lägga till nyligen har skapat kör som-kontot du skapade i steg 3.</span><span class="sxs-lookup"><span data-stu-id="be63c-161">Right-click and update its properties and add the recently created Run As Account you created in step 3.</span></span>

### <a name="sql-script-to-grant-granular-permissions-to-the-run-as-account"></a><span data-ttu-id="be63c-162">SQL-skript för att ge detaljerade behörigheter till Kör som-konto</span><span class="sxs-lookup"><span data-stu-id="be63c-162">SQL script to grant granular permissions to the Run As account</span></span>

<span data-ttu-id="be63c-163">Kör följande SQL-skript för att bevilja behörighet till Kör som-konto för SQL-instans som används av Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="be63c-163">Execute the following SQL script to grant required permissions to the Run As account on the SQL instance used by Operations Manager.</span></span>

```
-- Replace <UserName> with the actual user name being used as Run As Account.
USE master

-- Create login for the user, comment this line if login is already created.
CREATE LOGIN [UserName] FROM WINDOWS


--GRANT permissions to user.
GRANT VIEW SERVER STATE TO [UserName]
GRANT VIEW ANY DEFINITION TO [UserName]
GRANT VIEW ANY DATABASE TO [UserName]

-- Add database user for all the databases on SQL Server Instance, this is required for connecting to individual databases.
-- NOTE: This command must be run anytime new databases are added to SQL Server instances.
EXEC sp_msforeachdb N'USE [?]; CREATE USER [UserName] FOR LOGIN [UserName];'

Use msdb
GRANT SELECT To [UserName]
Go

--Give SELECT permission on all Operations Manager related Databases

--Replace the Operations Manager database name with the one in your environment
Use [OperationsManager];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager DatawareHouse database name with the one in your environment
Use [OperationsManagerDW];
GRANT SELECT To [UserName]
GO

--Replace the Operations Manager Audit Collection database name with the one in your environment
Use [OperationsManagerAC];
GRANT SELECT To [UserName]
GO

--Give db_owner on [OperationsManager] DB
--Replace the Operations Manager database name with the one in your environment
USE [OperationsManager]
GO
ALTER ROLE [db_owner] ADD MEMBER [UserName]

```


### <a name="configure-the-assessment-rule"></a><span data-ttu-id="be63c-164">Konfigurera assessment-regel</span><span class="sxs-lookup"><span data-stu-id="be63c-164">Configure the assessment rule</span></span>

<span data-ttu-id="be63c-165">Management pack för System Center Operations Manager Assessment-lösning innehåller en regel med namnet *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln*.</span><span class="sxs-lookup"><span data-stu-id="be63c-165">The System Center Operations Manager Assessment solution’s management pack includes a rule named *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule*.</span></span> <span data-ttu-id="be63c-166">Den här regeln är ansvarig för att köra bedömningen.</span><span class="sxs-lookup"><span data-stu-id="be63c-166">This rule is responsible for running the assessment.</span></span> <span data-ttu-id="be63c-167">Använd procedurerna nedan om du vill aktivera regeln och ange hur ofta.</span><span class="sxs-lookup"><span data-stu-id="be63c-167">To enable the rule and configure the frequency, use the procedures below.</span></span>

<span data-ttu-id="be63c-168">Microsoft System Center Advisor SCOM Assessment kör Assessment regeln är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="be63c-168">By default, the Microsoft System Center Advisor SCOM Assessment Run Assessment Rule is disabled.</span></span> <span data-ttu-id="be63c-169">Om du vill köra bedömningen, måste du aktivera regeln på en hanteringsserver.</span><span class="sxs-lookup"><span data-stu-id="be63c-169">To run the assessment, you must enable the rule on a management server.</span></span> <span data-ttu-id="be63c-170">Använd följande steg.</span><span class="sxs-lookup"><span data-stu-id="be63c-170">Use the following steps.</span></span>

#### <a name="enable-the-rule-for-a-specific-management-server"></a><span data-ttu-id="be63c-171">Aktivera regeln för en viss hanteringsserver</span><span class="sxs-lookup"><span data-stu-id="be63c-171">Enable the rule for a specific management server</span></span>

1. <span data-ttu-id="be63c-172">I den **redigering** arbetsytan i Operations Manager-konsolen, Sök efter regeln *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln* i den **regler** fönstret.</span><span class="sxs-lookup"><span data-stu-id="be63c-172">In the **Authoring** workspace of the Operations Manager console, search for the rule *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* in the **Rules** pane.</span></span>
2. <span data-ttu-id="be63c-173">I sökresultaten väljer du det alternativ som innehåller texten *typ: hanteringsservern*.</span><span class="sxs-lookup"><span data-stu-id="be63c-173">In the search results, select the one that includes the text *Type: Management Server*.</span></span>
3. <span data-ttu-id="be63c-174">Högerklicka på regeln och klickar sedan på **åsidosätter** > **för ett specifikt objekt i klassen: hanteringsservern**.</span><span class="sxs-lookup"><span data-stu-id="be63c-174">Right-click the rule and then click **Overrides** > **For a specific object of class: Management Server**.</span></span>
4.  <span data-ttu-id="be63c-175">Välj hanteringsservern där regeln ska köras i listan tillgängliga management-servrar.</span><span class="sxs-lookup"><span data-stu-id="be63c-175">In the available management servers list, select the management server where the rule should run.</span></span>
5.  <span data-ttu-id="be63c-176">Se till att du ändrar åsidosättningsvärde till **SANT** för den **aktiverad** parametervärdet.</span><span class="sxs-lookup"><span data-stu-id="be63c-176">Ensure that you change override value to **True** for the **Enabled** parameter value.</span></span>  
    <span data-ttu-id="be63c-177">![åsidosätt parametern](./media/log-analytics-scom-assessment/rule.png)</span><span class="sxs-lookup"><span data-stu-id="be63c-177">![override parameter](./media/log-analytics-scom-assessment/rule.png)</span></span>

<span data-ttu-id="be63c-178">Konfigurera hur ofta kör med nästa procedur vid fortfarande i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="be63c-178">While still in this window, configure the frequency of the run using the next procedure.</span></span>

#### <a name="configure-the-run-frequency"></a><span data-ttu-id="be63c-179">Konfigurera kör frekvens</span><span class="sxs-lookup"><span data-stu-id="be63c-179">Configure the run frequency</span></span>

<span data-ttu-id="be63c-180">Bedömning är konfigurerad för att köras var 10 080 minuter (eller sju dagar) standardintervallet.</span><span class="sxs-lookup"><span data-stu-id="be63c-180">The assessment is configured to run every 10,080 minutes (or seven days), the default interval.</span></span> <span data-ttu-id="be63c-181">Du kan åsidosätta värdet till ett minsta värde för 1440 minuter (eller en dag).</span><span class="sxs-lookup"><span data-stu-id="be63c-181">You can override the value to a minimum value of 1440 minutes (or one day).</span></span> <span data-ttu-id="be63c-182">Värdet som representerar minimitid mellanrummet krävs mellan på varandra följande assessment körs.</span><span class="sxs-lookup"><span data-stu-id="be63c-182">The value represents the minimum time gap required between successive assessment runs.</span></span> <span data-ttu-id="be63c-183">Följ anvisningarna nedan om du vill åsidosätta intervallet.</span><span class="sxs-lookup"><span data-stu-id="be63c-183">To override the interval, use the steps below.</span></span>

1. <span data-ttu-id="be63c-184">I den **redigering** arbetsytan i Operations Manager-konsolen, Sök efter regeln *Microsoft System Center Advisor SCOM Assessment kör Assessment regeln* i den **regler** fönstret.</span><span class="sxs-lookup"><span data-stu-id="be63c-184">In the **Authoring** workspace of the Operations Manager console, search for the rule *Microsoft System Center Advisor SCOM Assessment Run Assessment Rule* in the **Rules** pane.</span></span>
2. <span data-ttu-id="be63c-185">I sökresultaten väljer du det alternativ som innehåller texten *typ: hanteringsservern*.</span><span class="sxs-lookup"><span data-stu-id="be63c-185">In the search results, select the one that includes the text *Type: Management Server*.</span></span>
3. <span data-ttu-id="be63c-186">Högerklicka på regeln och klickar sedan på **Åsidosätt regeln** > **för alla objekt i klassen: hanteringsservern**.</span><span class="sxs-lookup"><span data-stu-id="be63c-186">Right-click the rule and then click **Override the Rule** > **For all objects of class: Management Server**.</span></span>
4. <span data-ttu-id="be63c-187">Ändra den **intervall** parametervärde för din önskade intervall.</span><span class="sxs-lookup"><span data-stu-id="be63c-187">Change the **Interval** parameter value to your desired interval value.</span></span> <span data-ttu-id="be63c-188">I exemplet nedan anges värdet till 1 440 minuter (en dag).</span><span class="sxs-lookup"><span data-stu-id="be63c-188">In the example below, the value is set to 1440 minutes (one day).</span></span>  
    <span data-ttu-id="be63c-189">![intervall för parametern](./media/log-analytics-scom-assessment/interval.png)</span><span class="sxs-lookup"><span data-stu-id="be63c-189">![interval parameter](./media/log-analytics-scom-assessment/interval.png)</span></span>  
    <span data-ttu-id="be63c-190">Om värdet anges till mindre än 1 440 minuter sedan körs regeln vid ett intervall med en dag.</span><span class="sxs-lookup"><span data-stu-id="be63c-190">If the value is set to less than 1440 minutes, then the rule runs at a one day interval.</span></span> <span data-ttu-id="be63c-191">I det här exemplet regeln ignoreras värdet för intervallet och körs med en frekvens som en dag.</span><span class="sxs-lookup"><span data-stu-id="be63c-191">In this example, the rule ignores the interval value and runs at a frequency of one day.</span></span>


## <a name="understanding-how-recommendations-are-prioritized"></a><span data-ttu-id="be63c-192">Förstå hur rekommendationer prioriteras</span><span class="sxs-lookup"><span data-stu-id="be63c-192">Understanding how recommendations are prioritized</span></span>

<span data-ttu-id="be63c-193">Varje rekommendation är ett givet värde som identifierar en avvägning mellan kraven för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="be63c-193">Every recommendation made is given a weighting value that identifies the relative importance of the recommendation.</span></span> <span data-ttu-id="be63c-194">Endast de 10 viktigaste rekommendationerna visas.</span><span class="sxs-lookup"><span data-stu-id="be63c-194">Only the 10 most important recommendations are shown.</span></span>

### <a name="how-weights-are-calculated"></a><span data-ttu-id="be63c-195">Hur vikterna beräknas</span><span class="sxs-lookup"><span data-stu-id="be63c-195">How weights are calculated</span></span>

<span data-ttu-id="be63c-196">Viktningar är sammanställda värden som baseras på tre viktiga faktorer:</span><span class="sxs-lookup"><span data-stu-id="be63c-196">Weightings are aggregate values based on three key factors:</span></span>

- <span data-ttu-id="be63c-197">Den *sannolikhet* att ett problem som identifieras ska orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="be63c-197">The *probability* that an issue identified will cause problems.</span></span> <span data-ttu-id="be63c-198">En högre sannolikhet är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="be63c-198">A higher probability equates to a larger overall score for the recommendation.</span></span>
- <span data-ttu-id="be63c-199">Den *inverkan* av problemet i din organisation om det uppstår ett problem.</span><span class="sxs-lookup"><span data-stu-id="be63c-199">The *impact* of the issue on your organization if it does cause a problem.</span></span> <span data-ttu-id="be63c-200">En högre inverkan är lika med en större övergripande poäng för rekommendationen.</span><span class="sxs-lookup"><span data-stu-id="be63c-200">A higher impact equates to a larger overall score for the recommendation.</span></span>
- <span data-ttu-id="be63c-201">Den *arbete* krävs för att implementera denna rekommendation.</span><span class="sxs-lookup"><span data-stu-id="be63c-201">The *effort* required to implement the recommendation.</span></span> <span data-ttu-id="be63c-202">Det är lika med en mindre övergripande poäng för rekommendationen högre prestanda.</span><span class="sxs-lookup"><span data-stu-id="be63c-202">A higher effort equates to a smaller overall score for the recommendation.</span></span>

<span data-ttu-id="be63c-203">Värde för varje rekommendation uttrycks i procent av den sammanlagda poängen för varje fokusområde.</span><span class="sxs-lookup"><span data-stu-id="be63c-203">The weighting for each recommendation is expressed as a percentage of the total score available for each focus area.</span></span> <span data-ttu-id="be63c-204">Till exempel ökar implementera denna rekommendation om en rekommendation i området för tillgänglighet och affärskontinuitet fokus har ett resultat på 5%, din övergripande tillgänglighet och affärskontinuitet poäng av 5%.</span><span class="sxs-lookup"><span data-stu-id="be63c-204">For example, if a recommendation in the Availability and Business Continuity focus area has a score of 5%, implementing that recommendation increases your overall Availability and Business Continuity score by 5%.</span></span>

### <a name="focus-areas"></a><span data-ttu-id="be63c-205">Fokusområden</span><span class="sxs-lookup"><span data-stu-id="be63c-205">Focus areas</span></span>

<span data-ttu-id="be63c-206">**Tillgänglighet och affärskontinuitet** -fokus visas här rekommendationer för tjänstetillgänglighet, återhämtning för din infrastruktur och business-skydd.</span><span class="sxs-lookup"><span data-stu-id="be63c-206">**Availability and Business Continuity** - This focus area shows recommendations for service availability, resiliency of your infrastructure, and business protection.</span></span>

<span data-ttu-id="be63c-207">**Prestanda och skalbarhet** -fokus visas här rekommendationer som hjälper din organisations IT-infrastruktur växer, se till att din IT-miljö uppfyller aktuella prestandakrav och kan svara på förändrade infrastrukturbehov.</span><span class="sxs-lookup"><span data-stu-id="be63c-207">**Performance and Scalability** - This focus area shows recommendations to help your organization's IT infrastructure grow, ensure that your IT environment meets current performance requirements, and is able to respond to changing infrastructure needs.</span></span>

<span data-ttu-id="be63c-208">**Uppgradering och migrering distribution** -fokus visas här rekommendationer när du uppgraderar, migrera och distribuera SQL Server till den befintliga infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="be63c-208">**Upgrade, Migration, and Deployment** - This focus area shows recommendations to help you upgrade, migrate, and deploy SQL Server to your existing infrastructure.</span></span>

<span data-ttu-id="be63c-209">**Åtgärder och övervakning** -fokus visas här rekommendationer för att effektivisera IT-avdelningen, implementera förebyggande underhåll och maximera prestanda.</span><span class="sxs-lookup"><span data-stu-id="be63c-209">**Operations and Monitoring** - This focus area shows recommendations to help streamline your IT operations, implement preventative maintenance, and maximize performance.</span></span>

### <a name="should-you-aim-to-score-100-in-every-focus-area"></a><span data-ttu-id="be63c-210">Bör du försöka poängsätta 100% överallt fokus i?</span><span class="sxs-lookup"><span data-stu-id="be63c-210">Should you aim to score 100% in every focus area?</span></span>

<span data-ttu-id="be63c-211">Inte nödvändigtvis.</span><span class="sxs-lookup"><span data-stu-id="be63c-211">Not necessarily.</span></span> <span data-ttu-id="be63c-212">Rekommendationerna baseras på kunskaper och erfarenheter som gjorts av Microsoft-tekniker över tusentals kunden besök.</span><span class="sxs-lookup"><span data-stu-id="be63c-212">The recommendations are based on the knowledge and experiences gained by Microsoft engineers across thousands of customer visits.</span></span> <span data-ttu-id="be63c-213">Dock inga två server-infrastrukturen är samma och specifika rekommendationer kan vara mer eller mindre relevant för dig.</span><span class="sxs-lookup"><span data-stu-id="be63c-213">However, no two server infrastructures are the same, and specific recommendations may be more or less relevant to you.</span></span> <span data-ttu-id="be63c-214">Exempelvis kanske vissa säkerhetsrekommendationer mindre relevant om de virtuella datorerna inte utsätts för Internet.</span><span class="sxs-lookup"><span data-stu-id="be63c-214">For example, some security recommendations might be less relevant if your virtual machines are not exposed to the Internet.</span></span> <span data-ttu-id="be63c-215">Vissa tillgänglighet rekommendationer kan vara relevanta för tjänster som erbjuder med låg prioritet ad hoc-insamling och rapportering.</span><span class="sxs-lookup"><span data-stu-id="be63c-215">Some availability recommendations may be less relevant for services that provide low priority ad hoc data collection and reporting.</span></span> <span data-ttu-id="be63c-216">Problem som är viktiga för en mogen företag kanske mindre viktiga för en Startup.</span><span class="sxs-lookup"><span data-stu-id="be63c-216">Issues that are important to a mature business may be less important to a start-up.</span></span> <span data-ttu-id="be63c-217">Du kanske vill identifiera vilka fokusområden är dina prioriteter och titta på hur ditt resultat förändras över tid.</span><span class="sxs-lookup"><span data-stu-id="be63c-217">You may want to identify which focus areas are your priorities and then look at how your scores change over time.</span></span>

<span data-ttu-id="be63c-218">Varje rekommendation innehåller information om varför det är viktigt.</span><span class="sxs-lookup"><span data-stu-id="be63c-218">Every recommendation includes guidance about why it is important.</span></span> <span data-ttu-id="be63c-219">Använd den här vägledningen för att utvärdera om implementera rekommendationen är lämplig för dig, baserat på typ av din IT-tjänster och affärsbehoven för din organisation.</span><span class="sxs-lookup"><span data-stu-id="be63c-219">Use this guidance to evaluate whether implementing the recommendation is appropriate for you, given the nature of your IT services and the business needs of your organization.</span></span>

## <a name="use-assessment-focus-area-recommendations"></a><span data-ttu-id="be63c-220">Använd assessment fokus området rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be63c-220">Use assessment focus area recommendations</span></span>

<span data-ttu-id="be63c-221">Innan du kan använda en lösning i OMS måste ha installerat lösningen.</span><span class="sxs-lookup"><span data-stu-id="be63c-221">Before you can use an assessment solution in OMS, you must have the solution installed.</span></span> <span data-ttu-id="be63c-222">Du kan läsa mer om hur du installerar lösningar finns [lägga till logganalys lösningar från galleriet lösningar](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="be63c-222">To read more about installing solutions, see [Add Log Analytics solutions from the Solutions Gallery](log-analytics-add-solutions.md).</span></span> <span data-ttu-id="be63c-223">När den har installerats kan du visa sammanfattning av rekommendationer med hjälp av System Center Operations Manager Assessment panelen på översiktssidan i OMS.</span><span class="sxs-lookup"><span data-stu-id="be63c-223">After it is installed, you can view the summary of recommendations by using the System Center Operations Manager Assessment tile on the Overview page in OMS.</span></span>

<span data-ttu-id="be63c-224">Visa sammanfattade efterlevnad bedömningar för din infrastruktur och gå till rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be63c-224">View the summarized compliance assessments for your infrastructure and then drill-into recommendations.</span></span>

### <a name="to-view-recommendations-for-a-focus-area-and-take-corrective-action"></a><span data-ttu-id="be63c-225">Visa rekommendationer för en Fokusområde och vidta åtgärder</span><span class="sxs-lookup"><span data-stu-id="be63c-225">To view recommendations for a focus area and take corrective action</span></span>

1. <span data-ttu-id="be63c-226">På den **översikt** klickar du på den **System Center Operations Manager Assessment** panelen.</span><span class="sxs-lookup"><span data-stu-id="be63c-226">On the **Overview** page, click the **System Center Operations Manager Assessment** tile.</span></span>
2. <span data-ttu-id="be63c-227">På den **System Center Operations Manager Assessment** , Granska sammanfattningen i ett fokus området blad och klickar sedan på en om du vill visa rekommendationer för området fokus.</span><span class="sxs-lookup"><span data-stu-id="be63c-227">On the **System Center Operations Manager Assessment** page, review the summary information in one of the focus area blades and then click one to view recommendations for that focus area.</span></span>
3. <span data-ttu-id="be63c-228">På någon av sidorna fokus område, kan du visa prioriterad rekommendationer för din miljö.</span><span class="sxs-lookup"><span data-stu-id="be63c-228">On any of the focus area pages, you can view the prioritized recommendations made for your environment.</span></span> <span data-ttu-id="be63c-229">Klicka på en rekommendation enligt **påverkade objekt** att visa information om varför rekommendationen görs.</span><span class="sxs-lookup"><span data-stu-id="be63c-229">Click a recommendation under **Affected Objects** to view details about why the recommendation is made.</span></span>  
    <span data-ttu-id="be63c-230">![Fokusområde](./media/log-analytics-scom-assessment/focus-area.png)</span><span class="sxs-lookup"><span data-stu-id="be63c-230">![focus area](./media/log-analytics-scom-assessment/focus-area.png)</span></span>
4. <span data-ttu-id="be63c-231">Du kan vidta åtgärder i **föreslagna åtgärder**.</span><span class="sxs-lookup"><span data-stu-id="be63c-231">You can take corrective actions suggested in **Suggested Actions**.</span></span> <span data-ttu-id="be63c-232">När objektet har behandlats registrerar senare bedömningar den rekommenderade åtgärder som utförts och kompatibilitet poängen ökar.</span><span class="sxs-lookup"><span data-stu-id="be63c-232">When the item has been addressed, later assessments will record that recommended actions were taken and your compliance score will increase.</span></span> <span data-ttu-id="be63c-233">Korrigerade objekt visas som **skickades objekt**.</span><span class="sxs-lookup"><span data-stu-id="be63c-233">Corrected items appear as **Passed Objects**.</span></span>

## <a name="ignore-recommendations"></a><span data-ttu-id="be63c-234">Ignorera rekommendationer</span><span class="sxs-lookup"><span data-stu-id="be63c-234">Ignore recommendations</span></span>

<span data-ttu-id="be63c-235">Om du har rekommendationer som du vill ignorera, kan du skapa en textfil som OMS används för att förhindra rekommendationer visas i sökresultatet assessment.</span><span class="sxs-lookup"><span data-stu-id="be63c-235">If you have recommendations that you want to ignore, you can create a text file that OMS uses to prevent recommendations from appearing in your assessment results.</span></span>

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

### <a name="to-identify-recommendations-that-you-want-to-ignore"></a><span data-ttu-id="be63c-236">Att identifiera rekommendationer som du vill ignorera</span><span class="sxs-lookup"><span data-stu-id="be63c-236">To identify recommendations that you want to ignore</span></span>

1. <span data-ttu-id="be63c-237">Logga in på din arbetsyta och öppna loggen sökning.</span><span class="sxs-lookup"><span data-stu-id="be63c-237">Sign in to your workspace and open Log Search.</span></span> <span data-ttu-id="be63c-238">Använd följande fråga för att lista över rekommendationer som har misslyckats för datorer i din miljö.</span><span class="sxs-lookup"><span data-stu-id="be63c-238">Use the following query to list recommendations that have failed for computers in your environment.</span></span>

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Failed | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

    <span data-ttu-id="be63c-239">Här är en skärmbild som visar frågan loggen:</span><span class="sxs-lookup"><span data-stu-id="be63c-239">Here's a screen shot showing the Log Search query:</span></span>  
    ![loggsökning](./media/log-analytics-scom-assessment/scom-log-search.png)

2. <span data-ttu-id="be63c-241">Välj rekommendationer som du vill ignorera.</span><span class="sxs-lookup"><span data-stu-id="be63c-241">Choose recommendations that you want to ignore.</span></span> <span data-ttu-id="be63c-242">Du ska använda värdena för RecommendationId i nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="be63c-242">You'll use the values for RecommendationId in the next procedure.</span></span>

### <a name="to-create-and-use-an-ignorerecommendationstxt-text-file"></a><span data-ttu-id="be63c-243">Du skapar och använder en IgnoreRecommendations.txt textfil</span><span class="sxs-lookup"><span data-stu-id="be63c-243">To create and use an IgnoreRecommendations.txt text file</span></span>

1. <span data-ttu-id="be63c-244">Skapa en fil med namnet IgnoreRecommendations.txt.</span><span class="sxs-lookup"><span data-stu-id="be63c-244">Create a file named IgnoreRecommendations.txt.</span></span>
2. <span data-ttu-id="be63c-245">Klistra in eller ange varje RecommendationId för varje rekommendation som du vill OMS Ignorera på en separat rad och spara och stäng filen.</span><span class="sxs-lookup"><span data-stu-id="be63c-245">Paste or type each RecommendationId for each recommendation that you want OMS to ignore on a separate line and then save and close the file.</span></span>
3. <span data-ttu-id="be63c-246">Placera filen i följande mapp på varje dator där du vill att OMS att ignorera rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be63c-246">Put the file in the following folder on each computer where you want OMS to ignore recommendations.</span></span>
4. <span data-ttu-id="be63c-247">På hanteringsservern för Operations Manager - *SystemDrive*: \Program\Microsoft System Center 2012 R2\Operations Manager\Server.</span><span class="sxs-lookup"><span data-stu-id="be63c-247">On the Operations Manager management server - *SystemDrive*:\Program Files\Microsoft System Center 2012 R2\Operations Manager\Server.</span></span>

### <a name="to-verify-that-recommendations-are-ignored"></a><span data-ttu-id="be63c-248">Kontrollera att rekommendationer ignoreras</span><span class="sxs-lookup"><span data-stu-id="be63c-248">To verify that recommendations are ignored</span></span>

1. <span data-ttu-id="be63c-249">När nästa schemalagda utvärdering körs som standard var sjunde dag, angivna rekommendationerna markeras ignoreras och kommer inte att visas på instrumentpanelen assessment.</span><span class="sxs-lookup"><span data-stu-id="be63c-249">After the next scheduled assessment runs, by default every seven days, the specified recommendations are marked Ignored and will not appear on the assessment dashboard.</span></span>
2. <span data-ttu-id="be63c-250">Du kan använda följande loggen sökfrågor för att lista alla rekommendationer som ignoreras.</span><span class="sxs-lookup"><span data-stu-id="be63c-250">You can use the following Log Search queries to list all the ignored recommendations.</span></span>

    ```
    Type=SCOMAssessmentRecommendationRecommendationResult=Ignored | select  Computer, RecommendationId, Recommendation | sort  Computer
    ```

3. <span data-ttu-id="be63c-251">Om du senare bestämmer att du vill se ignoreras rekommendationer, ta bort IgnoreRecommendations.txt filer eller du kan ta bort RecommendationIDs från dem.</span><span class="sxs-lookup"><span data-stu-id="be63c-251">If you decide later that you want to see ignored recommendations, remove any IgnoreRecommendations.txt files, or you can remove RecommendationIDs from them.</span></span>

## <a name="system-center-operations-manager-assessment-solution-faq"></a><span data-ttu-id="be63c-252">System Center Operations Manager lösning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="be63c-252">System Center Operations Manager Assessment solution FAQ</span></span>

<span data-ttu-id="be63c-253">*Jag har lagt till lösningen på min OMS-arbetsyta. Men du ser inte rekommendationer. Varför inte?*</span><span class="sxs-lookup"><span data-stu-id="be63c-253">*I added the assessment solution to my OMS workspace. But I don’t see the recommendations. Why not?*</span></span> <span data-ttu-id="be63c-254">Använd följande steg vy rekommendationer när du lägger till lösningen på OMS-instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="be63c-254">After adding the solution, use the following steps view the recommendations on the OMS dashboard.</span></span>  

- [<span data-ttu-id="be63c-255">Ange kör som-kontot för System Center Operations Manager</span><span class="sxs-lookup"><span data-stu-id="be63c-255">Set the Run As account for System Center Operations Manager Assessment</span></span>](#operations-manager-run-as-accounts-for-oms)  
- [<span data-ttu-id="be63c-256">Konfigurera System Center Operations Manager Assessment-regel</span><span class="sxs-lookup"><span data-stu-id="be63c-256">Configure the System Center Operations Manager Assessment rule</span></span>](#configure-the-assessment-rule)


<span data-ttu-id="be63c-257">*Finns det ett sätt att konfigurera hur ofta bedömning körs?*</span><span class="sxs-lookup"><span data-stu-id="be63c-257">*Is there a way to configure how often the assessment runs?*</span></span> <span data-ttu-id="be63c-258">Ja.</span><span class="sxs-lookup"><span data-stu-id="be63c-258">Yes.</span></span> <span data-ttu-id="be63c-259">Se [konfigurera kör frekvensen](#configure-the-run-frequency).</span><span class="sxs-lookup"><span data-stu-id="be63c-259">See [Configure the run frequency](#configure-the-run-frequency).</span></span>

<span data-ttu-id="be63c-260">*Om en annan server identifieras när jag har lagt till System Center Operations Manager Assessment lösningen, kommer utvärderas det?*</span><span class="sxs-lookup"><span data-stu-id="be63c-260">*If another server is discovered after I’ve added the System Center Operations Manager Assessment solution, will it be assessed?*</span></span> <span data-ttu-id="be63c-261">Ja, efter identifiering, det bedöms fr.o.m--som standard var sjunde dag.</span><span class="sxs-lookup"><span data-stu-id="be63c-261">Yes, after discovery, it is assessed from then on--by default, every seven days.</span></span>

<span data-ttu-id="be63c-262">*Vad är namnet på processen som gör datainsamlingen?*</span><span class="sxs-lookup"><span data-stu-id="be63c-262">*What is the name of the process that does the data collection?*</span></span> <span data-ttu-id="be63c-263">AdvisorAssessment.exe</span><span class="sxs-lookup"><span data-stu-id="be63c-263">AdvisorAssessment.exe</span></span>

<span data-ttu-id="be63c-264">*Där körs AdvisorAssessment.exe processen?*</span><span class="sxs-lookup"><span data-stu-id="be63c-264">*Where does the AdvisorAssessment.exe process run?*</span></span> <span data-ttu-id="be63c-265">AdvisorAssessment.exe körs under HealthService för hanteringsservern där assessment regeln är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="be63c-265">AdvisorAssessment.exe runs under the HealthService of the management server where the assessment rule is enabled.</span></span> <span data-ttu-id="be63c-266">Med den här processen uppnås identifiering av hela miljön genom insamling av fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="be63c-266">Using that process, discovery of your entire environment is achieved through remote data collection.</span></span>

<span data-ttu-id="be63c-267">*Hur lång tid tar det för insamling av data?*</span><span class="sxs-lookup"><span data-stu-id="be63c-267">*How long does it take for data collection?*</span></span> <span data-ttu-id="be63c-268">Insamling av data på servern tar ungefär en timme.</span><span class="sxs-lookup"><span data-stu-id="be63c-268">Data collection on the server takes about one hour.</span></span> <span data-ttu-id="be63c-269">Det kan ta längre tid i miljöer som har många instanser för Operations Manager eller databaser.</span><span class="sxs-lookup"><span data-stu-id="be63c-269">It may take longer in environments that have many Operations Manager instances or databases.</span></span>

<span data-ttu-id="be63c-270">*Vad händer om jag ange intervallet av bedömningen till mindre än 1 440 minuter?*</span><span class="sxs-lookup"><span data-stu-id="be63c-270">*What if I set the interval of the assessment to less than 1440 minutes?*</span></span> <span data-ttu-id="be63c-271">Utvärderingen har förkonfigurerats så att köras på högst en gång per dag.</span><span class="sxs-lookup"><span data-stu-id="be63c-271">The assessment is pre-configured to run at a maximum of once per day.</span></span> <span data-ttu-id="be63c-272">Om du åsidosätter intervallvärdet till ett värde mindre än 1 440 minuter använder bedömningen 1440 minuter som värdet för intervallet.</span><span class="sxs-lookup"><span data-stu-id="be63c-272">If you override the interval value to a value less than 1440 minutes, then the assessment uses 1440 minutes as the interval value.</span></span>

<span data-ttu-id="be63c-273">*Hur vet jag om det finns fel som krävs?*</span><span class="sxs-lookup"><span data-stu-id="be63c-273">*How to know if there are pre-requisite failures?*</span></span> <span data-ttu-id="be63c-274">Om bedömningen kördes och du inte ser resultaten, är det troligt att vissa av kraven för att bedöma misslyckades.</span><span class="sxs-lookup"><span data-stu-id="be63c-274">If the assessment ran and you don't see results, then it is likely that some of the pre-requisites for the assessment failed.</span></span> <span data-ttu-id="be63c-275">Du kan köra frågor: `Type=Operation Solution=SCOMAssessment` och `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` i loggen Sök om du vill se misslyckade kraven.</span><span class="sxs-lookup"><span data-stu-id="be63c-275">You can execute queries: `Type=Operation Solution=SCOMAssessment` and `Type=SCOMAssessmentRecommendation FocusArea=Prerequisites` in Log Search to see the failed pre-requisites.</span></span>

<span data-ttu-id="be63c-276">*Det finns en `Failed to connect to the SQL Instance (….).` meddelandet i förutvärdering fel. Vad är problemet?*</span><span class="sxs-lookup"><span data-stu-id="be63c-276">*There is a `Failed to connect to the SQL Instance (….).` message in pre-requisite failures. What is the issue?*</span></span> <span data-ttu-id="be63c-277">AdvisorAssessment.exe, processen som samlar in data, körs under HealthService för hanteringsservern.</span><span class="sxs-lookup"><span data-stu-id="be63c-277">AdvisorAssessment.exe, the process that collects data, runs under the HealthService of the management server.</span></span> <span data-ttu-id="be63c-278">Som en del av utvärderingen, processen som försöker ansluta till SQL Server där Operations Manager-databasen finns.</span><span class="sxs-lookup"><span data-stu-id="be63c-278">As part of the assessment, the process attempts to connect to the SQL Server where the Operations Manager database is present.</span></span> <span data-ttu-id="be63c-279">Det här felet kan inträffa när brandväggsregler blockera anslutningen till SQL Server-instansen.</span><span class="sxs-lookup"><span data-stu-id="be63c-279">This error can occur when firewall rules block the connection to the SQL Server instance.</span></span>

<span data-ttu-id="be63c-280">*Vilken typ av data som samlas in?*</span><span class="sxs-lookup"><span data-stu-id="be63c-280">*What type of data is collected?*</span></span> <span data-ttu-id="be63c-281">Följande typer av data har samlats in: - WMI-data – data - EventLog-data – Operations Manager registerdata via Windows PowerShell, SQL-frågor och filen information insamlaren.</span><span class="sxs-lookup"><span data-stu-id="be63c-281">The following types of data are collected: - WMI data - Registry data - EventLog data - Operations Manager data through Windows PowerShell, SQL Queries and File information collector.</span></span>

<span data-ttu-id="be63c-282">*Varför måste jag konfigurera ett kör som-konto?*</span><span class="sxs-lookup"><span data-stu-id="be63c-282">*Why do I have to configure a Run As Account?*</span></span> <span data-ttu-id="be63c-283">Olika SQL-frågor körs för en Operations Manager-server.</span><span class="sxs-lookup"><span data-stu-id="be63c-283">For an Operations Manager server, various SQL queries are run.</span></span> <span data-ttu-id="be63c-284">För att kunna köra måste du använda ett kör som-konto med tillräcklig behörighet.</span><span class="sxs-lookup"><span data-stu-id="be63c-284">In order for them to run, you must use a Run As Account with necessary permissions.</span></span> <span data-ttu-id="be63c-285">Dessutom krävs lokal administratörsbehörighet för att fråga WMI.</span><span class="sxs-lookup"><span data-stu-id="be63c-285">In addition, local administrator credentials are required to query WMI.</span></span>

<span data-ttu-id="be63c-286">*Varför visas endast topp 10 rekommendationer?*</span><span class="sxs-lookup"><span data-stu-id="be63c-286">*Why display only the top 10 recommendations?*</span></span> <span data-ttu-id="be63c-287">I stället för att ge dig en fullständig, överväldigande lista över aktiviteter, rekommenderar vi att du fokusera på adressering prioriterad rekommendationerna först.</span><span class="sxs-lookup"><span data-stu-id="be63c-287">Instead of giving you an exhaustive, overwhelming list of tasks, we recommend that you focus on addressing the prioritized recommendations first.</span></span> <span data-ttu-id="be63c-288">När du åtgärda dem blir ytterligare rekommendationer tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="be63c-288">After you address them, additional recommendations will become available.</span></span> <span data-ttu-id="be63c-289">Du kan visa alla rekommendationer loggen sökning om du vill se en detaljerad lista.</span><span class="sxs-lookup"><span data-stu-id="be63c-289">If you prefer to see the detailed list, you can view all recommendations using Log Search.</span></span>

<span data-ttu-id="be63c-290">*Finns det ett sätt att ignorera en rekommendation?*</span><span class="sxs-lookup"><span data-stu-id="be63c-290">*Is there a way to ignore a recommendation?*</span></span> <span data-ttu-id="be63c-291">Ja, finns det [Ignorera rekommendationer](#Ignore-recommendations).</span><span class="sxs-lookup"><span data-stu-id="be63c-291">Yes, see the [Ignore recommendations](#Ignore-recommendations).</span></span>


## <a name="next-steps"></a><span data-ttu-id="be63c-292">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be63c-292">Next steps</span></span>

- <span data-ttu-id="be63c-293">[Söka i loggar](log-analytics-log-searches.md) att visa detaljerad information för System Center Operations Manager bedömning och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="be63c-293">[Search logs](log-analytics-log-searches.md) to view detailed System Center Operations Manager Assessment data and recommendations.</span></span>
