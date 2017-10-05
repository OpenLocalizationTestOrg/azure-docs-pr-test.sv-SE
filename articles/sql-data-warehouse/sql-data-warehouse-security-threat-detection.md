---
title: "Kom igång med SQL Data Warehouse Hotidentifiering"
description: "Hur du kommer igång med Hotidentifiering"
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: c9073dd9-6c62-4735-8457-dfb9f859c900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: f4a2376fe4fb710d031c35ca7fdbf4c7bb0f3caa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-threat-detection"></a><span data-ttu-id="62961-103">Kom igång med hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="62961-103">Get started with threat detection</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62961-104">Granskning</span><span class="sxs-lookup"><span data-stu-id="62961-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="62961-105">Hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="62961-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="62961-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="62961-106">Overview</span></span>
<span data-ttu-id="62961-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella säkerhetshot mot databasen.</span><span class="sxs-lookup"><span data-stu-id="62961-107">Threat Detection detects anomalous database activities indicating potential security threats to the database.</span></span> <span data-ttu-id="62961-108">Hotidentifiering är en förhandsversion och stöds för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="62961-108">Threat Detection is in preview and is supported for SQL Data Warehouse.</span></span>

<span data-ttu-id="62961-109">Hotidentifiering innehåller ett nytt lager av säkerhet som ger kunder möjlighet att identifiera och svara på potentiella hot allteftersom de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="62961-109">Threat Detection provides a new layer of security, which enables customers to detect and respond to potential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="62961-110">Användare kan utforska misstänkta händelser med hjälp av [Azure SQL Data Warehouse-granskning](sql-data-warehouse-auditing-overview.md) att avgöra om de härrör från ett försök att komma åt, bryta mot eller utnyttja data i datalagret.</span><span class="sxs-lookup"><span data-stu-id="62961-110">Users can explore the suspicious events using [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) to determine if they result from an attempt to access, breach or exploit data in the data warehouse.</span></span>
<span data-ttu-id="62961-111">Hotidentifiering gör det enkelt att adressen potentiella hot till datalagret utan att behöva vara en expert säkerhet eller hantera avancerade säkerhetsövervakning system.</span><span class="sxs-lookup"><span data-stu-id="62961-111">Threat Detection makes it simple to address potential threats to the data warehouse without the need to be a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="62961-112">Hotidentifiering identifierar till exempel vissa avvikande databasaktiviteter som indikerar potentiella försök med SQL injection.</span><span class="sxs-lookup"><span data-stu-id="62961-112">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="62961-113">SQL injection är en av säkerhetsproblem för vanliga Web program på Internet, används till att attackera datadrivna program.</span><span class="sxs-lookup"><span data-stu-id="62961-113">SQL injection is one of the common Web application security issues on the Internet, used to attack data-driven applications.</span></span> <span data-ttu-id="62961-114">Angripare kan utnyttja säkerhetsproblem för programmet att mata in skadlig SQL-instruktioner i programmet post fält för brott mot eller ändra data i databasen.</span><span class="sxs-lookup"><span data-stu-id="62961-114">Attackers take advantage of application vulnerabilities to inject malicious SQL statements into application entry fields, for breaching or modifying data in the database.</span></span>

## <a name="set-up-threat-detection-for-your-database"></a><span data-ttu-id="62961-115">Ställ in hotidentifiering för din databas</span><span class="sxs-lookup"><span data-stu-id="62961-115">Set up threat detection for your database</span></span>
1. <span data-ttu-id="62961-116">Starta Azure-portalen på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="62961-116">Launch the Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="62961-117">Navigera till bladet för konfiguration av SQL Data Warehouse som du vill övervaka.</span><span class="sxs-lookup"><span data-stu-id="62961-117">Navigate to the configuration blade of the SQL Data Warehouse you want to monitor.</span></span> <span data-ttu-id="62961-118">I bladet inställningar väljer **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="62961-118">In the Settings blade, select **Auditing & Threat Detection**.</span></span>
   
    ![Navigeringsfönstret][1]
3. <span data-ttu-id="62961-120">I den **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning, som visar inställningarna för identifiering av hot.</span><span class="sxs-lookup"><span data-stu-id="62961-120">In the **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display the Threat detection settings.</span></span>
   
    ![Navigeringsfönstret][2]
4. <span data-ttu-id="62961-122">Aktivera **på** Hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="62961-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="62961-123">Konfigurera en lista över e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande data warehouse aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="62961-123">Configure the list of emails that will receive security alerts upon detection of anomalous data warehouse activities.</span></span>
6. <span data-ttu-id="62961-124">Klicka på **spara** i den **Auditing & Threat detection** configuration bladet för att spara den nya eller uppdaterade granskning och hot princip.</span><span class="sxs-lookup"><span data-stu-id="62961-124">Click **Save** in the **Auditing & Threat detection** configuration blade to save the new or updated auditing and threat detection policy.</span></span>
   
    ![Navigeringsfönstret][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="62961-126">Utforska data avvikande aktiviteter vid identifiering av en misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="62961-126">Explore anomalous data warehouse activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="62961-127">Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="62961-127">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="62961-128">E-postmeddelandet ger information om händelsen misstänkta säkerhet inklusive avvikande aktiviteter, databasens namn, servernamn och tidpunkt för händelsen.</span><span class="sxs-lookup"><span data-stu-id="62961-128">The email will provide information on the suspicious security event including the nature of the anomalous activities, database name, server name and the event time.</span></span> <span data-ttu-id="62961-129">Dessutom ger information om möjliga orsaker och rekommenderade åtgärder för att undersöka och minska den potentiella risken till databasen.</span><span class="sxs-lookup"><span data-stu-id="62961-129">In addition, it will provide information on possible causes and recommended actions to investigate and mitigate the potential threat to the database.</span></span><br/>
   
    ![Navigeringsfönstret][4]
2. <span data-ttu-id="62961-131">I e-post, klickar du på den **Azure SQL-granskning logg** länk som startar den klassiska Azure-portalen och visa de granskning register vid ungefär samma tidpunkt misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="62961-131">In the email, click on the **Azure SQL Auditing Log** link, which will launch the Azure Classic Portal and show the relevant Auditing records around the time of the suspicious event.</span></span>
   
    ![Navigeringsfönstret][5]
3. <span data-ttu-id="62961-133">Klicka på granskningsposter om du vill visa mer information om misstänkt databasaktiviteter, till exempel SQL-instruktionen, fel orsak och klient-IP.</span><span class="sxs-lookup"><span data-stu-id="62961-133">Click on the audit records to view more details on the suspicious database activities such as SQL statement, failure reason and client IP.</span></span>
   
    ![Navigeringsfönstret][6]
4. <span data-ttu-id="62961-135">I bladet granskning poster klickar du på **öppna i Excel** för att öppna en förkonfigurerad excel-mall som du vill importera och kör djupare analys av granskningsloggen vid ungefär samma tidpunkt misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="62961-135">In the Auditing Records blade, click  **Open in Excel** to open a pre-configured excel template to import and run deeper analysis of the audit log around the time of the suspicious event.</span></span><br/><span data-ttu-id="62961-136">
   **Obs:** i Excel 2010 eller senare, Power Query och **snabb kombinera** inställningen är obligatorisk</span><span class="sxs-lookup"><span data-stu-id="62961-136">
**Note:** In Excel 2010 or later, Power Query and the **Fast Combine** setting is required</span></span>
   
    ![Navigeringsfönstret][7]
5. <span data-ttu-id="62961-138">Så här konfigurerar du den **snabb kombinera** - inställningen i den **POWER QUERY** menyfliksområdet fliken **alternativ** att visa dialogrutan Alternativ.</span><span class="sxs-lookup"><span data-stu-id="62961-138">To configure the **Fast Combine** setting - In the **POWER QUERY** ribbon tab, select **Options** to display the Options dialog.</span></span> <span data-ttu-id="62961-139">Välj avsnittet sekretess och väljer det andra alternativet - ”Ignorera sekretessnivåerna och förbättra prestanda”:</span><span class="sxs-lookup"><span data-stu-id="62961-139">Select the Privacy section and choose the second option - 'Ignore the Privacy Levels and potentially improve performance':</span></span>
   
    ![Navigeringsfönstret][8]
6. <span data-ttu-id="62961-141">Kontrollera att parametrarna i inställningar på fliken är korrekt och välj ”uppgifter” menyfliksområdet och klicka på knappen 'Uppdatera alla' för att läsa in SQL-granskningsloggar.</span><span class="sxs-lookup"><span data-stu-id="62961-141">To load SQL audit logs, ensure that the parameters in the settings tab are set correctly and then select the 'Data' ribbon and click the 'Refresh All' button.</span></span>
   
    ![Navigeringsfönstret][9]
7. <span data-ttu-id="62961-143">Resultatet visas i den **SQL granskningsloggar** blad som gör det möjligt att köra djupare analys av avvikande aktiviteter som upptäckts och minimera effekten av säkerhetshändelse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="62961-143">The results appear in the **SQL Audit Logs** sheet which enables you to run deeper analysis of the anomalous activities that were detected, and mitigate the impact of the security event in your application.</span></span>

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
