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
# <a name="sql-database-threat-detection"></a>Hotidentifiering för SQL-databas

SQL-Hotidentifiering identifierar avvikande aktiviteter som anger ovanliga och potentiellt skadliga försök tooaccess eller utnyttja databaser.

## <a name="overview"></a>Översikt

SQL-Hotidentifiering ger ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.  Användarna får en avisering vid misstänkt databasaktiviteter, potentiella säkerhetsproblem och SQL injection attacker samt avvikande databasen åtkomstmönster. SQL-Hotidentifiering aviseringar ger detaljerad information om misstänkt aktivitet och rekommenderar åtgärd på hur tooinvestigate och minimera hotet hello. Användare kan utforska hello misstänkta händelser med hjälp av [SQL Database Auditing](sql-database-auditing.md) toodetermine om de kommer från en försök tooaccess bryta mot eller utnyttja data i hello-databas. Hotidentifiering gör det enkelt tooaddress potentiella hot toohello databas utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.

Till exempel är SQL injection en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program. Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet fält, brott mot eller ändra data i hello-databas.

SQL-Hotidentifiering integreras med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/), och varje skyddade SQL Database-server som kommer att debiteras på samma pris hello som Azure Security Center Standard nivå på $15/nod/månad, där var och en skyddad SQL Databasservern räknas som en nod. Vi ber dig tootry det. i 60 dagar för ledig. 

## <a name="set-up-threat-detection-for-your-database-in-hello-azure-portal"></a>Ställ in hotidentifiering för din databas i hello Azure-portalen
1. Starta hello Azure portal på [https://portal.azure.com](https://portal.azure.com).
2. Navigera toohello configuration bladet för hello SQL-databas som du vill toomonitor. Välj i hello inställningsbladet **granskning och Hotidentifiering**. 
    ![Navigeringsfönstret][1]
3. I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning som visar hello threat detection inställningar.
  
    ![Navigeringsfönstret][2]
4. Aktivera **på** Hotidentifiering.
5. Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande databasaktiviteter.
6. Klicka på **spara** i hello **Auditing & Threat detection** bladet toosave hello nya eller uppdaterade granskning och hotidentifiering identifieringsinställningar.
       
    ![Navigeringsfönstret][3]

## <a name="set-up-threat-detection-using-powershell"></a>Ställ in hotidentifiering med hjälp av PowerShell

Ett exempel på skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="explore-anomalous-database-activities-upon-detection-of-a-suspicious-event"></a>Utforska avvikande databasaktiviteter vid identifiering av en misstänkt aktivitet
1. Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter. <br/>
   hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasens namn, servernamn, programnamn och hello tidpunkt. Dessutom hello e-post ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas.<br/>
     
    ![Navigeringsfönstret][4]
2. hello e-postavisering innehåller en direktlänk toohello SQL granskningslogg. Klicka på den här länken öppnar hello Azure portal och öppnar hello SQL granskningsposter runt hello tidpunkt hello misstänkta händelsen. Klicka på en post tooview audit mer information om hello misstänkt databasaktiviteter, vilket gör det enklare toofind hello SQL-instruktioner som utfördes (som nås vad tyckte och när) och kontrollera om hello händelse legitima eller skadliga (t.ex. ett program säkerhetsproblem tooSQL injektion var de utnyttjas, någon utsatts för intrång känsliga data, osv.).<br/>
   ![Navigeringsfönstret][5]


## <a name="explore-threat-detection-alerts-for-your-database-in-hello-azure-portal"></a>Utforska hotidentifieringsaviseringar för din databas i hello Azure-portalen

SQL-databasen Hotidentifiering integrerar dess aviseringar med [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/). En levande SQL-säkerhetsrutan i hello databasbladet i hello Azure portal spårar hello status för aktiva hot. 

   ![Navigeringsfönstret][6]
   
1. När du klickar på hello SQL säkerhetsrutan startar hello Azure Security Center aviseringar bladet och ger en översikt över aktiva SQL hot upptäckts på hello-databasen. 

  ![Navigeringsfönstret][7]

2. Klicka på en specifik avisering innehåller ytterligare information och åtgärder för att undersöka det här hotet och uppdateringen framtida problem.

  ![Navigeringsfönstret][8]


## <a name="next-steps"></a>Nästa steg

* Mer information om Hotidentifiering går du till hello [Azure blogg](https://azure.microsoft.com/en-us/blog/azure-sql-database-threat-detection-general-availability-in-spring-2017/) 
* Lär dig mer om [Azure SQL Database Auditing](sql-database-auditing.md)
* Lär dig mer om [Azure Security Center](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro)
* Mer information om priser finns i hello [prissättning för SQL-databas](https://azure.microsoft.com/en-us/pricing/details/sql-database/)  
* Exempel en PowerShell-skript finns [konfigurera granskning och hotidentifiering identifiering med hjälp av PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md)



<!--Image references-->
[1]: ./media/sql-database-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-database-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-database-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-database-threat-detection/4_td_email.png
[5]: ./media/sql-database-threat-detection/5_td_audit_record_details.png
[6]: ./media/sql-database-threat-detection/6_td_security_tile_view_alerts.png
[7]: ./media/sql-database-threat-detection/7_td_SQL_security_alerts_list.png
[8]: ./media/sql-database-threat-detection/8_td_SQL_security_alert_details.png


