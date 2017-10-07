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
# <a name="get-started-with-threat-detection"></a>Kom igång med hotidentifiering
> [!div class="op_single_selector"]
> * [Granskning](sql-data-warehouse-auditing-overview.md)
> * [Hotidentifiering](sql-data-warehouse-security-threat-detection.md)
> 
> 

## <a name="overview"></a>Översikt
Hotidentifiering identifierar avvikande databasaktiviteter som indikerar potentiella hot toohello databasen. Hotidentifiering är en förhandsversion och stöds för SQL Data Warehouse.

Hotidentifiering innehåller ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter. Användare kan utforska hello misstänkta händelser med hjälp av [Azure SQL Data Warehouse-granskning](sql-data-warehouse-auditing-overview.md) toodetermine om de kommer från en försök tooaccess bryta mot eller utnyttja data i hello-datalagret.
Hotidentifiering gör det enkelt tooaddress potentiella hot toohello data warehouse utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.

Hotidentifiering identifierar till exempel vissa avvikande databasaktiviteter som indikerar potentiella försök med SQL injection. SQL injection är en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program. Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet post fält för brott mot eller ändra data i hello-databas.

## <a name="set-up-threat-detection-for-your-database"></a>Ställ in hotidentifiering för din databas
1. Starta hello Azure-portalen på [https://portal.azure.com](https://portal.azure.com).
2. Navigera toohello configuration bladet för hello SQL Data Warehouse som du vill toomonitor. Välj i hello inställningsbladet **granskning och Hotidentifiering**.
   
    ![Navigeringsfönstret][1]
3. I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning, som visar hello Threat detection inställningar.
   
    ![Navigeringsfönstret][2]
4. Aktivera **på** Hotidentifiering.
5. Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande data warehouse aktiviteter.
6. Klicka på **spara** i hello **Auditing & Threat detection** configuration bladet toosave hello nya eller uppdaterade granskning och hotidentifiering princip.
   
    ![Navigeringsfönstret][3]

## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Utforska data avvikande aktiviteter vid identifiering av en misstänkt aktivitet
1. Du får ett e-postmeddelande vid identifiering av avvikande databasaktiviteter. <br/>
   hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasnamn, namn och hello händelse servertid. Dessutom den ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas.<br/>
   
    ![Navigeringsfönstret][4]
2. I hello e-post, klickar du på hello **Azure SQL-granskning logg** länk som startar hello klassiska Azure-portalen och visa hello relevanta granskning poster runt hello tidpunkt hello misstänkta händelsen.
   
    ![Navigeringsfönstret][5]
3. Klicka på hello audit poster tooview mer information om hello misstänkt databasaktiviteter, till exempel SQL-instruktionen, fel orsak och klient-IP.
   
    ![Navigeringsfönstret][6]
4. I hello granskning poster bladet, klickar du på **öppna i Excel** tooopen förkonfigurerad excel mallen tooimport och kör djupare analys av hello granskningsloggen runt hello tidpunkt hello misstänkta händelsen.<br/>
   **Obs:** i Excel 2010 eller senare, Power Query och hello **snabb kombinera** inställningen är obligatorisk
   
    ![Navigeringsfönstret][7]
5. tooconfigure hello **snabb kombinera** inställningen - i hello **POWER QUERY** menyfliksområdet fliken **alternativ** toodisplay hello dialogrutan. Välj hello sekretess avsnitt och välj hello andra alternativet - ”Ignorera hello sekretessnivåerna och förbättra prestanda”:
   
    ![Navigeringsfönstret][8]
6. tooload granskningsloggarna för SQL, kontrollera att hello parametrar på fliken Inställningar för hello är korrekt inställda Välj hello ”uppgifter” menyfliksområdet och klickar på ”Uppdatera alla' hello-knappen.
   
    ![Navigeringsfönstret][9]
7. hello resultat visas i hello **SQL granskningsloggar** blad som gör att du toorun djupare analys av hello avvikande aktiviteter som har upptäckts och minska hello effekten av hello säkerhetshändelse i ditt program.

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
