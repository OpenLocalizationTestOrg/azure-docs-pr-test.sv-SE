---
title: "aaaGet igång med SQL Data Warehouse Hotidentifiering"
description: "Hur tooget igång med Hotidentifiering"
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
ms.openlocfilehash: dec0b734849e7f52434e099db0b38fbf0bf6ad53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-threat-detection"></a><span data-ttu-id="a2f70-103">Kom igång med hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="a2f70-103">Get started with threat detection</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a2f70-104">Granskning</span><span class="sxs-lookup"><span data-stu-id="a2f70-104">Auditing</span></span>](sql-data-warehouse-auditing-overview.md)
> * [<span data-ttu-id="a2f70-105">Hotidentifiering</span><span class="sxs-lookup"><span data-stu-id="a2f70-105">Threat detection</span></span>](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="a2f70-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="a2f70-106">Overview</span></span>
<span data-ttu-id="a2f70-107">Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen.</span><span class="sxs-lookup"><span data-stu-id="a2f70-107">Threat Detection detects anomalous database activities indicating potential security threats toohello database.</span></span> <span data-ttu-id="a2f70-108">Hotidentifiering är en förhandsversion och stöds för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a2f70-108">Threat Detection is in preview and is supported for SQL Data Warehouse.</span></span>

<span data-ttu-id="a2f70-109">Hotidentifiering innehåller ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a2f70-109">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="a2f70-110">Användare kan utforska hello misstänkta händelser med hjälp av [Azure SQL Data Warehouse-granskning](sql-data-warehouse-auditing-overview.md) toodetermine om de kommer från en försök tooaccess bryta mot eller utnyttja data i hello-datalagret.</span><span class="sxs-lookup"><span data-stu-id="a2f70-110">Users can explore hello suspicious events using [Azure SQL Data Warehouse Auditing](sql-data-warehouse-auditing-overview.md) toodetermine if they result from an attempt tooaccess, breach or exploit data in hello data warehouse.</span></span>
<span data-ttu-id="a2f70-111">Hotidentifiering gör det enkelt tooaddress potentiella hot toohello data warehouse utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.</span><span class="sxs-lookup"><span data-stu-id="a2f70-111">Threat Detection makes it simple tooaddress potential threats toohello data warehouse without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>

<span data-ttu-id="a2f70-112">Hotidentifiering identifierar till exempel vissa avvikande databasaktiviteter som indikerar potentiella försök med SQL injection.</span><span class="sxs-lookup"><span data-stu-id="a2f70-112">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="a2f70-113">SQL injection är en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program.</span><span class="sxs-lookup"><span data-stu-id="a2f70-113">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="a2f70-114">Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet post fält för brott mot eller ändra data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="a2f70-114">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

## <a name="set-up-threat-detection-for-your-database"></a><span data-ttu-id="a2f70-115">Ställ in hotidentifiering för din databas</span><span class="sxs-lookup"><span data-stu-id="a2f70-115">Set up threat detection for your database</span></span>
1. <span data-ttu-id="a2f70-116">Starta hello Azure-portalen på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a2f70-116">Launch hello Azure Portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a2f70-117">Navigera toohello configuration bladet för hello SQL Data Warehouse som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="a2f70-117">Navigate toohello configuration blade of hello SQL Data Warehouse you want toomonitor.</span></span> <span data-ttu-id="a2f70-118">Välj i hello inställningsbladet **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="a2f70-118">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>
   
    ![Navigeringsfönstret][1]
3. <span data-ttu-id="a2f70-120">I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning, som visar hello Threat detection inställningar.</span><span class="sxs-lookup"><span data-stu-id="a2f70-120">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello Threat detection settings.</span></span>
   
    ![Navigeringsfönstret][2]
4. <span data-ttu-id="a2f70-122">Aktivera **på** Hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="a2f70-122">Turn **ON** Threat detection.</span></span>
5. <span data-ttu-id="a2f70-123">Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande data warehouse aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a2f70-123">Configure hello list of emails that will receive security alerts upon detection of anomalous data warehouse activities.</span></span>
6. <span data-ttu-id="a2f70-124">Klicka på **spara** i hello **Auditing & Threat detection** configuration bladet toosave hello nya eller uppdaterade granskning och hotidentifiering princip.</span><span class="sxs-lookup"><span data-stu-id="a2f70-124">Click **Save** in hello **Auditing & Threat detection** configuration blade toosave hello new or updated auditing and threat detection policy.</span></span>
   
    ![Navigeringsfönstret][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a><span data-ttu-id="a2f70-126">Utforska data avvikande aktiviteter vid identifiering av en misstänkt aktivitet</span><span class="sxs-lookup"><span data-stu-id="a2f70-126">Explore anomalous data warehouse activities upon detection of a suspicious event</span></span>
1. <span data-ttu-id="a2f70-127">Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="a2f70-127">You will receive an email notification upon detection of anomalous database activities.</span></span> <br/>
   <span data-ttu-id="a2f70-128">hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasnamn, namn och hello händelse servertid.</span><span class="sxs-lookup"><span data-stu-id="a2f70-128">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="a2f70-129">Dessutom den ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas.</span><span class="sxs-lookup"><span data-stu-id="a2f70-129">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span><br/>
   
    ![Navigeringsfönstret][4]
2. <span data-ttu-id="a2f70-131">I hello e-post, klickar du på hello **Azure SQL-granskning logg** länk som startar hello klassiska Azure-portalen och visa hello relevanta granskning poster runt hello tidpunkt hello misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="a2f70-131">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure Classic Portal and show hello relevant Auditing records around hello time of hello suspicious event.</span></span>
   
    ![Navigeringsfönstret][5]
3. <span data-ttu-id="a2f70-133">Klicka på hello audit poster tooview mer information om hello misstänkt databasaktiviteter, till exempel SQL-instruktionen, fel orsak och klient-IP.</span><span class="sxs-lookup"><span data-stu-id="a2f70-133">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>
   
    ![Navigeringsfönstret][6]
4. <span data-ttu-id="a2f70-135">I hello granskning poster bladet, klickar du på **öppna i Excel** tooopen förkonfigurerad excel mallen tooimport och kör djupare analys av hello granskningsloggen runt hello tidpunkt hello misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="a2f70-135">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span><br/><span data-ttu-id="a2f70-136">
   **Obs:** i Excel 2010 eller senare, Power Query och hello **snabb kombinera** inställningen är obligatorisk</span><span class="sxs-lookup"><span data-stu-id="a2f70-136">
**Note:** In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required</span></span>
   
    ![Navigeringsfönstret][7]
5. <span data-ttu-id="a2f70-138">tooconfigure hello **snabb kombinera** inställningen - i hello **POWER QUERY** menyfliksområdet fliken **alternativ** toodisplay hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a2f70-138">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="a2f70-139">Välj hello sekretess avsnitt och välj hello andra alternativet - ”Ignorera hello sekretessnivåerna och förbättra prestanda”:</span><span class="sxs-lookup"><span data-stu-id="a2f70-139">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>
   
    ![Navigeringsfönstret][8]
6. <span data-ttu-id="a2f70-141">tooload granskningsloggarna för SQL, kontrollera att hello parametrar på fliken Inställningar för hello är korrekt inställda Välj hello ”uppgifter” menyfliksområdet och klickar på ”Uppdatera alla' hello-knappen.</span><span class="sxs-lookup"><span data-stu-id="a2f70-141">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>
   
    ![Navigeringsfönstret][9]
7. <span data-ttu-id="a2f70-143">hello resultat visas i hello **SQL granskningsloggar** blad som gör att du toorun djupare analys av hello avvikande aktiviteter som har upptäckts och minska hello effekten av hello säkerhetshändelse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="a2f70-143">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>

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
