---
title: Hotidentifiering - Azure SQL Database | Microsoft Docs
description: "Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella säkerhetshot mot databasen."
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
ms.openlocfilehash: bd3de9ed0131edc683763b0fe7f4a2ae74533944
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sql-database-threat-detection"></a><span data-ttu-id="6ff61-103">Hotidentifiering för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="6ff61-103">SQL Database Threat Detection</span></span>

<span data-ttu-id="6ff61-104">SQL-Hotidentifiering identifierar avvikande aktiviteter som anger ovanliga och potentiellt skadliga försök att komma åt eller utnyttja databaser.</span><span class="sxs-lookup"><span data-stu-id="6ff61-104">SQL Threat Detection detects anomalous activities indicating unusual and potentially harmful attempts to access or exploit databases.</span></span>

## <a name="overview"></a><span data-ttu-id="6ff61-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="6ff61-105">Overview</span></span>

<span data-ttu-id="6ff61-106">SQL-Hotidentifiering ger ett nytt lager av säkerhet som ger kunder möjlighet att identifiera och svara på potentiella hot allteftersom de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6ff61-106">SQL Threat Detection provides a new layer of security, which enables customers to detect and respond to potential threats as they occur by providing security alerts on anomalous activities.</span></span>  <span data-ttu-id="6ff61-107">Användarna får en avisering vid misstänkt databasaktiviteter, potentiella säkerhetsproblem och SQL injection attacker samt avvikande databasen åtkomstmönster.</span><span class="sxs-lookup"><span data-stu-id="6ff61-107">Users will receive an alert upon suspicious database activities, potential vulnerabilities, and SQL injection attacks, as well as anomalous database access patterns.</span></span> <span data-ttu-id="6ff61-108">SQL-Hotidentifiering aviseringar ger detaljerad information om misstänkt aktivitet och rekommenderar åtgärd att undersöka och minska risken.</span><span class="sxs-lookup"><span data-stu-id="6ff61-108">SQL Threat Detection alerts provide details of suspicious activity and recommend action on how to investigate and mitigate the threat.</span></span> <span data-ttu-id="6ff61-109">Användare kan utforska misstänkta händelser med hjälp av [SQL Database Auditing](sql-database-auditing.md) att avgöra om de härrör från ett försök att komma åt, bryta mot eller utnyttja data i databasen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-109">Users can explore the suspicious events using [SQL Database Auditing](sql-database-auditing.md) to determine if they result from an attempt to access, breach, or exploit data in the database.</span></span> <span data-ttu-id="6ff61-110">Hotidentifiering gör det enkelt att adressen potentiella hot mot databasen utan att behöva vara en expert säkerhet eller hantera avancerade säkerhetsövervakning system.</span><span class="sxs-lookup"><span data-stu-id="6ff61-110">Threat Detection makes it simple to address potential threats to the database without the need to be a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="6ff61-111">Till exempel är SQL injection en av säkerhetsproblem för vanliga Web program på Internet, används till att attackera datadrivna program.</span><span class="sxs-lookup"><span data-stu-id="6ff61-111">For example, SQL injection is one of the common Web application security issues on the Internet, used to attack data-driven applications.</span></span> <span data-ttu-id="6ff61-112">Angripare dra nytta av programmet säkerhetsproblem att mata in skadlig SQL-instruktioner i programmet fält, brott mot eller ändra data i databasen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-112">Attackers take advantage of application vulnerabilities to inject malicious SQL statements into application entry fields, breaching or modifying data in the database.</span></span>

<span data-ttu-id="6ff61-113">SQL-Hotidentifiering integreras med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), och varje skyddade SQL Database-server kommer att debiteras på samma pris som Azure Security Center Standard nivå på $15/nod/månad, där var och en skyddad SQL Databasservern räknas som en nod.</span><span class="sxs-lookup"><span data-stu-id="6ff61-113">SQL Threat Detection integrates alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), and, each protected SQL Database server will be billed at the same price as Azure Security Center Standard tier, at $15/node/month, where each protected SQL Database server is counted as one node.</span></span> <span data-ttu-id="6ff61-114">Vi erbjuder dig att testa kostnadsfritt i 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="6ff61-114">We invite you to try it out for 60 days for free.</span></span> 

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a><span data-ttu-id="6ff61-115">Ställ in hotidentifiering för din databas i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6ff61-115">Set up threat detection for your database in the Azure portal</span></span>
1. <span data-ttu-id="6ff61-116">Starta Azure-portalen på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6ff61-116">Launch the Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6ff61-117">Navigera till bladet för konfiguration av SQL-databas som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="6ff61-117">Navigate to the configuration blade of the SQL Database you want to monitor.</span></span> <span data-ttu-id="6ff61-118">I bladet inställningar väljer **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="6ff61-118">In the Settings blade, select **Auditing & Threat Detection**.</span></span> 
    <span data-ttu-id="6ff61-119">![Navigeringsfönstret][1]</span><span class="sxs-lookup"><span data-stu-id="6ff61-119">![Navigation pane][1]</span></span>
3. <span data-ttu-id="6ff61-120">I den **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning som visar inställningarna för identifiering av hot.</span><span class="sxs-lookup"><span data-stu-id="6ff61-120">In the **Auditing & Threat Detection** configuration blade turn **ON** Auditing, which will display the threat detection settings.</span></span>
  
    ![Navigeringsfönstret][2]
4. <span data-ttu-id="6ff61-122">Aktivera **på** Hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="6ff61-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="6ff61-123">Konfigurera en lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6ff61-123">Configure the list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>
6. <span data-ttu-id="6ff61-124">Klicka på **spara** i den **Auditing & Threat detection** bladet för att spara den nya eller uppdaterade gransknings- och hot identifieringsinställningar.</span><span class="sxs-lookup"><span data-stu-id="6ff61-124">Click **Save** in the **Auditing & Threat detection** blade to save the new or updated auditing and threat detection settings.</span></span>
       
    ![Navigeringsfönstret][3]

## <a name="set-up-threat-detection-using-powershell"></a><span data-ttu-id="6ff61-126">Ställ in hotidentifiering med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="6ff61-126">Set up threat detection using PowerShell</span></span>

<span data-ttu-id="6ff61-127">Ett exempel på skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6ff61-127">For a script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).</span></span>

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="6ff61-128">Utforska avvikande databasaktiviteter vid identifiering av en misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="6ff61-128">Explore anomalous database activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="6ff61-129">Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6ff61-129">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="6ff61-130">E-postmeddelandet ger information om händelsen misstänkta säkerhet inklusive uppbyggnad avvikande aktiviteter, databasens namn, servernamn, programnamn och tidpunkt för händelsen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-130">The email will provide information on the suspicious security event including the nature of the anomalous activities, database name, server name, application name, and the event time.</span></span> <span data-ttu-id="6ff61-131">Dessutom ger information om möjliga orsaker e-postmeddelandet och rekommenderade åtgärder för att undersöka och minska den potentiella risken till databasen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-131">In addition, the email will provide information on possible causes and recommended actions to investigate and mitigate the potential threat to the database.</span></span><br/>
     
    ![Navigeringsfönstret][4]
2. <span data-ttu-id="6ff61-133">E-postavisering innehåller en direktlänk till SQL granskningsloggen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-133">The email alert includes a direct link to the SQL Audit log.</span></span> <span data-ttu-id="6ff61-134">När du klickar på den här länken startar Azure portal och öppnar SQL granskningsposter vid ungefär samma tidpunkt misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-134">Clicking on this link launches the Azure portal and opens the SQL Audit records around the time of the suspicious event.</span></span> <span data-ttu-id="6ff61-135">Klicka på en granskningspost om du vill visa mer information om misstänkt databasaktiviteter, vilket gör det lättare att hitta SQL-instruktioner som utfördes (som nås vad tyckte och när) och ta reda på om händelsen var ogiltigt eller skadliga (t.ex. ett program säkerhetsproblem för SQL injection var de utnyttjas, någon utsatts för intrång känsliga data, osv.).</span><span class="sxs-lookup"><span data-stu-id="6ff61-135">Click on an audit record to view more details on the suspicious database activities, making it easier to find the SQL statements that were executed (who accessed, what they did and when) and determine if the event was legitimate or malicious (e.g. application vulnerability to SQL injection was exploited, someone breached sensitive data, etc.).</span></span><br/><span data-ttu-id="6ff61-136">
   ![Navigeringsfönstret][5]</span><span class="sxs-lookup"><span data-stu-id="6ff61-136">
![Navigation pane][5]</span></span>


## <a name="explore-threat-detection-alerts-for-your-database-in-the-azure-portal"></a><span data-ttu-id="6ff61-137">Utforska hotidentifieringsaviseringar för din databas i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6ff61-137">Explore threat detection alerts for your database in the Azure portal</span></span>

<span data-ttu-id="6ff61-138">SQL-databasen Hotidentifiering integrerar dess aviseringar med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span><span class="sxs-lookup"><span data-stu-id="6ff61-138">SQL Database Threat Detection integrates its alerts with [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).</span></span> <span data-ttu-id="6ff61-139">En levande panel SQL i databasbladet i Azure portal spårar status för aktiva hot.</span><span class="sxs-lookup"><span data-stu-id="6ff61-139">A live SQL security tile within the database blade in the Azure portal tracks the status of active threats.</span></span> 

   ![Navigeringsfönstret][6]
   
1. <span data-ttu-id="6ff61-141">Klicka på SQL säkerhetsrutan startar bladet med säkerhetsaviseringar i Azure Security Center och ger en översikt över aktiva SQL hot upptäckts på databasen.</span><span class="sxs-lookup"><span data-stu-id="6ff61-141">Clicking on the SQL security tile launches the Azure Security Center alerts blade and provides an overview of active SQL threats detected on the database.</span></span> 

  ![Navigeringsfönstret][7]

2. <span data-ttu-id="6ff61-143">Klicka på en specifik avisering innehåller ytterligare information och åtgärder för att undersöka det här hotet och uppdateringen framtida problem.</span><span class="sxs-lookup"><span data-stu-id="6ff61-143">Clicking on a specific alert provides additional details and actions for investigating this threat and remediating future threats.</span></span>

  ![Navigeringsfönstret][8]


## <a name="next-steps"></a><span data-ttu-id="6ff61-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ff61-145">Next steps</span></span>

* <span data-ttu-id="6ff61-146">Läs mer om Hotidentifiering den [Azure blogg](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span><span class="sxs-lookup"><span data-stu-id="6ff61-146">Learn more about Threat Detection, visit the [Azure blog](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/)</span></span> 
* <span data-ttu-id="6ff61-147">Lär dig mer om [Azure SQL Database Auditing](sql-database-auditing.md)</span><span class="sxs-lookup"><span data-stu-id="6ff61-147">Learn more about [Azure SQL Database Auditing](sql-database-auditing.md)</span></span>
* <span data-ttu-id="6ff61-148">Lär dig mer om [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span><span class="sxs-lookup"><span data-stu-id="6ff61-148">Learn more about [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)</span></span>
* <span data-ttu-id="6ff61-149">Mer information om priser finns i [prissättning för SQL-databas](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span><span class="sxs-lookup"><span data-stu-id="6ff61-149">For more details on pricing, please see the [SQL Database Pricing page](https://azure.microsoft.com/en-us/pricing/details/sql-database/)</span></span>  
* <span data-ttu-id="6ff61-150">Exempel en PowerShell-skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="6ff61-150">For a PowerShell script example, see [Configure auditing and threat detection using PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)</span></span>



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


