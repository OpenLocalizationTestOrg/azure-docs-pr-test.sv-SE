---
title: aaaThreat identifiering - Azure SQL Database | Microsoft Docs
description: Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen.
services: sql-database
documentationcenter: 
author: rmatchoro
manager: jhubbard
editor: v-romcal
ms.assetid: b50d232a-4225-46ed-91e7-75288f55ee84
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/19/2017
ms.author: ronmat; ronitr
ms.openlocfilehash: 0879d20eff515a4e69358b5a98ceccf57fbd0ea2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-threat-detection"></a><span data-ttu-id="7e50b-103">Hotidentifiering för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="7e50b-103">SQL Database Threat Detection</span></span>

<span data-ttu-id="7e50b-104">SQL-Hotidentifiering identifierar avvikande aktiviteter som anger ovanliga och potentiellt skadliga försök tooaccess eller utnyttja databaser.</span><span class="sxs-lookup"><span data-stu-id="7e50b-104">SQL Threat Detection detects anomalous activities indicating unusual and potentially harmful attempts tooaccess or exploit databases.</span></span>

## <a name="overview"></a><span data-ttu-id="7e50b-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="7e50b-105">Overview</span></span>

<span data-ttu-id="7e50b-106">SQL-Hotidentifiering ger ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7e50b-106">SQL Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span>  <span data-ttu-id="7e50b-107">Användarna får en avisering vid misstänkt databasaktiviteter, potentiella säkerhetsproblem och SQL injection attacker samt avvikande databasen åtkomstmönster.</span><span class="sxs-lookup"><span data-stu-id="7e50b-107">Users will receive an alert upon suspicious database activities, potential vulnerabilities, and SQL injection attacks, as well as anomalous database access patterns.</span></span> <span data-ttu-id="7e50b-108">SQL-Hotidentifiering aviseringar ger detaljerad information om misstänkt aktivitet och rekommenderar åtgärd på hur tooinvestigate och minimera hotet hello.</span><span class="sxs-lookup"><span data-stu-id="7e50b-108">SQL Threat Detection alerts provide details of suspicious activity and recommend action on how tooinvestigate and mitigate hello threat.</span></span> <span data-ttu-id="7e50b-109">Användare kan utforska hello misstänkta händelser med hjälp av [SQL Database Auditing](sql-database-auditing.md) toodetermine om de kommer från en försök tooaccess bryta mot eller utnyttja data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="7e50b-109">Users can explore hello suspicious events using [SQL Database Auditing](sql-database-auditing.md) toodetermine if they result from an attempt tooaccess, breach, or exploit data in hello database.</span></span> <span data-ttu-id="7e50b-110">Hotidentifiering gör det enkelt tooaddress potentiella hot toohello databas utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.</span><span class="sxs-lookup"><span data-stu-id="7e50b-110">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="7e50b-111">Till exempel är SQL injection en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program.</span><span class="sxs-lookup"><span data-stu-id="7e50b-111">For example, SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="7e50b-112">Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet fält, brott mot eller ändra data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="7e50b-112">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, breaching or modifying data in hello database.</span></span>

<span data-ttu-id="7e50b-113">SQL-Hotidentifiering integreras med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), och varje skyddade SQL Database-server som kommer att debiteras på samma pris hello som Azure Security Center Standard nivå på $15/nod/månad, där var och en skyddad SQL Databasservern räknas som en nod.</span><span class="sxs-lookup"><span data-stu-id="7e50b-113">SQL Threat Detection integrates alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), and, each protected SQL Database server will be billed at hello same price as Azure Security Center Standard tier, at $15/node/month, where each protected SQL Database server is counted as one node.</span></span> <span data-ttu-id="7e50b-114">Vi ber dig tootry det. i 60 dagar för ledig.</span><span class="sxs-lookup"><span data-stu-id="7e50b-114">We invite you tootry it out for 60 days for free.</span></span> 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="7e50b-115">Ställ in hotidentifiering för din databas i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7e50b-115">Set up threat detection for your database in hello Azure portal</span></span>
1. <span data-ttu-id="7e50b-116">Starta hello Azure portal på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7e50b-116">Launch hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="7e50b-117">Navigera toohello configuration bladet för hello SQL-databas som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="7e50b-117">Navigate toohello configuration blade of hello SQL Database you want toomonitor.</span></span> <span data-ttu-id="7e50b-118">Välj i hello inställningsbladet **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="7e50b-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> 
    <span data-ttu-id="7e50b-119">![Navigeringsfönstret][1]</span><span class="sxs-lookup"><span data-stu-id="7e50b-119">![Navigation pane][1]</span></span>
3. <span data-ttu-id="7e50b-120">I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning som visar hello threat detection inställningar.</span><span class="sxs-lookup"><span data-stu-id="7e50b-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** Auditing, which will display hello threat detection settings.</span></span>
  
    ![Navigeringsfönstret][2]
4. <span data-ttu-id="7e50b-122">Aktivera **på** Hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="7e50b-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="7e50b-123">Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7e50b-123">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>
6. <span data-ttu-id="7e50b-124">Klicka på **spara** i hello **Auditing & Threat detection** bladet toosave hello nya eller uppdaterade granskning och hotidentifiering identifieringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="7e50b-124">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection settings.</span></span>
       
    ![Navigeringsfönstret][3]

## <a name="set-up-threat-detection-using-powershell"></a><span data-ttu-id="7e50b-126">Ställ in hotidentifiering med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e50b-126">Set up threat detection using PowerShell</span></span>

<span data-ttu-id="7e50b-127">Ett exempel på skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7e50b-127">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="7e50b-128">Utforska avvikande databasaktiviteter vid identifiering av en misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="7e50b-128">Explore anomalous database activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="7e50b-129">Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="7e50b-129">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="7e50b-130">hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasens namn, servernamn, programnamn och hello tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="7e50b-130">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name, application name, and hello event time.</span></span> <span data-ttu-id="7e50b-131">Dessutom hello e-post ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas.</span><span class="sxs-lookup"><span data-stu-id="7e50b-131">In addition, hello email will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
     
    ![Navigeringsfönstret][4]
2. <span data-ttu-id="7e50b-133">hello e-postavisering innehåller en direktlänk toohello SQL granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="7e50b-133">hello email alert includes a direct link toohello SQL Audit log.</span></span> <span data-ttu-id="7e50b-134">Klicka på den här länken öppnar hello Azure portal och öppnar hello SQL granskningsposter runt hello tidpunkt hello misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="7e50b-134">Clicking on this link launches hello Azure portal and opens hello SQL Audit records around hello time of hello suspicious event.</span></span> <span data-ttu-id="7e50b-135">Klicka på en post tooview audit mer information om hello misstänkt databasaktiviteter, vilket gör det enklare toofind hello SQL-instruktioner som utfördes (som nås vad tyckte och när) och kontrollera om hello händelse legitima eller skadliga (t.ex. ett program säkerhetsproblem tooSQL injektion var de utnyttjas, någon utsatts för intrång känsliga data, osv.).</span><span class="sxs-lookup"><span data-stu-id="7e50b-135">Click on an audit record tooview more details on hello suspicious database activities, making it easier toofind hello SQL statements that were executed (who accessed, what they did and when) and determine if hello event was legitimate or malicious (e.g. application vulnerability tooSQL injection was exploited, someone breached sensitive data, etc.).</span></span><br/><span data-ttu-id="7e50b-136">
   ![Navigeringsfönstret][5]</span><span class="sxs-lookup"><span data-stu-id="7e50b-136">
![Navigation pane][5]</span></span>


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a><span data-ttu-id="7e50b-137">Utforska hotidentifieringsaviseringar för din databas i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7e50b-137">Explore threat detection alerts for your database in hello Azure portal</span></span>

<span data-ttu-id="7e50b-138">SQL-databasen Hotidentifiering integrerar dess aviseringar med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span><span class="sxs-lookup"><span data-stu-id="7e50b-138">SQL Database Threat Detection integrates its alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span></span> <span data-ttu-id="7e50b-139">En levande SQL-säkerhetsrutan i hello databasbladet i hello Azure portal spårar hello status för aktiva hot.</span><span class="sxs-lookup"><span data-stu-id="7e50b-139">A live SQL security tile within hello database blade in hello Azure portal tracks hello status of active threats.</span></span> 

   ![Navigeringsfönstret][6]
   
1. <span data-ttu-id="7e50b-141">När du klickar på hello SQL säkerhetsrutan startar hello Azure Security Center aviseringar bladet och ger en översikt över aktiva SQL hot upptäckts på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="7e50b-141">Clicking on hello SQL security tile launches hello Azure Security Center alerts blade and provides an overview of active SQL threats detected on hello database.</span></span> 

  ![Navigeringsfönstret][7]

2. <span data-ttu-id="7e50b-143">Klicka på en specifik avisering innehåller ytterligare information och åtgärder för att undersöka det här hotet och uppdateringen framtida problem.</span><span class="sxs-lookup"><span data-stu-id="7e50b-143">Clicking on a specific alert provides additional details and actions for investigating this threat and remediating future threats.</span></span>

  ![Navigeringsfönstret][8]


## <a name="next-steps"></a><span data-ttu-id="7e50b-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e50b-145">Next steps</span></span>

* <span data-ttu-id="7e50b-146">Mer information om Hotidentifiering går du till hello [Azure blogg](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span><span class="sxs-lookup"><span data-stu-id="7e50b-146">Learn more about Threat Detection, visit hello [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span></span> 
* <span data-ttu-id="7e50b-147">Lär dig mer om [Azure SQL Database Auditing](sql-database-auditing.md)</span><span class="sxs-lookup"><span data-stu-id="7e50b-147">Learn more about [Azure SQL Database Auditing](sql-database-auditing.md)</span></span>
* <span data-ttu-id="7e50b-148">Lär dig mer om [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span><span class="sxs-lookup"><span data-stu-id="7e50b-148">Learn more about [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span></span>
* <span data-ttu-id="7e50b-149">Mer information om priser finns i hello [prissättning för SQL-databas](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span><span class="sxs-lookup"><span data-stu-id="7e50b-149">For more details on pricing, please see hello [SQL Database Pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span></span>  
* <span data-ttu-id="7e50b-150">Exempel en PowerShell-skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7e50b-150">For a PowerShell script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span></span>



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


