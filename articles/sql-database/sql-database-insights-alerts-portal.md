---
title: "Använd Azure-portalen för att skapa SQL-databas aviseringar | Microsoft Docs"
description: "Använd Azure-portalen för att skapa SQL-databas aviseringar som kan utlösa meddelanden eller automation när angivna villkor uppfylls."
author: aamalvea
manager: jhubbard
editor: 
services: sql-database
documentationcenter: 
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: sql-database
ms.custom: monitor and tune
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: aamalvea
ms.openlocfilehash: fd21c9b5e573ac6a47fef88c2a9d31c52618ecb8
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2017
---
# <a name="use-azure-portal-to-create-alerts-for-azure-sql-database-and-data-warehouse"></a>Använd Azure-portalen för att skapa aviseringar för Azure SQL Database och datalagret

## <a name="overview"></a>Översikt
Den här artikeln visar hur du ställer in Azure SQL Database och datalagret aviseringar med Azure-portalen. Den här artikeln innehåller även rekommendationer för att Avisera punkter.    

Du kan ta emot en avisering baserat på övervakning mätvärden för eller händelser på Azure-tjänster.

* **Måttvärden** -aviseringen utlöses när värdet för ett visst mått överskrider ett tröskelvärde som du tilldelar i båda riktningarna. Det vill säga den utlöser både när villkoret uppfylls först och sedan efteråt när villkor som inte längre är uppfyllt.    
* **Aktiviteten logghändelser** -utlösa en avisering på *varje* händelse eller endast när ett visst antal händelser inträffar.

Du kan konfigurera en avisering när den utlöser gör du följande:

* Skicka e-postmeddelanden till tjänstadministratören och medadministratörer
* Skicka e-post till ytterligare e-postmeddelanden som du anger.
* anropa en webhook

Du kan konfigurera och få information om aviseringen regler med hjälp av

* [Azure Portal](../monitoring-and-diagnostics/insights-alerts-portal.md)
* [PowerShell](../monitoring-and-diagnostics/insights-alerts-powershell.md)
* [kommandoradsgränssnittet (CLI)](../monitoring-and-diagnostics/insights-alerts-command-line-interface.md)
* [Azure-Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Skapa en aviseringsregel på ett mått med Azure-portalen
1. I den [portal](https://portal.azure.com/), leta upp den resurs som du är intresserad av övervakning och markera den.
2. Det här steget är olika för SQL-databas och elastiska pooler jämfört med SQL DW: 

   - **SQL DB & endast elastiska pooler**: Välj **aviseringar** eller **Varna regler** under avsnittet övervakning. Text och ikon kan variera något mellan olika resurser.  
   
     ![Övervakning](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButton.png)
  
   - **ENDAST SQL DW**: Välj **övervakning** under avsnittet vanliga uppgifter. Klicka på den **DWU användning** diagram.

     ![VANLIGA AKTIVITETER](../monitoring-and-diagnostics/media/insights-alerts-portal/AlertRulesButtonDW.png)

3. Välj den **Lägg till avisering** kommando och Fyll i fälten.
   
    ![Lägg till avisering](../monitoring-and-diagnostics/media/insights-alerts-portal/AddDBAlertPage.png)
4. **Namnet** aviseringen regel och väljer en **beskrivning**, som visar även i e-postmeddelanden.
5. Välj den **mått** du vill övervaka och väljer sedan en **villkoret** och **tröskelvärdet** värdet för måttet. Också välja den **Period** som mått regeln måste uppfyllas innan aviseringen utlösare. Exempelvis om du använder period ”PT5M” och aviseringen söker efter CPU över 80 procent, aviseringen utlöses när den **genomsnittlig** CPU har varit över 80 procent under 5 minuter. När den första utlösaren infaller utlöses igen när den genomsnittliga CPU är lägre än 80% än 5 minuter. CPU-mätning inträffar var 1 minut. Se tabellen nedan för stöds tidsfönster och aggregering skriver att varje varning använder inte alla aviseringar använder medelvärdet.   
6. Kontrollera **e-ägare...**  om du vill att administratörer och medadministratörer kan skickas när aviseringen utlöses.
7. Om du vill att ytterligare e-postmeddelanden ett meddelande när aviseringen utlöses, lägga till dem i den **ytterligare administratören email(s)** fältet. Avgränsa flera e-postmeddelanden med semikolon -  *email@contoso.com;email2@contoso.com*
8. Placera i en giltig URI i den **Webhook** om du vill att den anropas när aviseringen utlöses.
9. Välj **OK** när du är klar för att skapa aviseringen.   

Inom några minuter aviseringen är aktiv och utlöser som beskrivits tidigare.

## <a name="managing-your-alerts"></a>Hantera aviseringar
När du har skapat en avisering, kan du välja den och:

* Visa ett diagram som visar mått tröskelvärdet och faktiska värden från föregående dag.
* Redigera eller ta bort den.
* **Inaktivera** eller **aktivera** den om du vill att tillfälligt stoppa eller återuppta tar emot meddelanden om den här aviseringen.


## <a name="sql-database-alert-values"></a>Aviseringen värden för SQL-databas

| Resurstyp | Måttnamnet | Eget namn | Sammansättningstyp | Minsta avisering tidsfönstret|
| --- | --- | --- | --- | --- |
| SQL-databas | cpu_percent | CPU-procent | Genomsnittlig | 5 minuter |
| SQL-databas | physical_data_read_percent | Data IO-procent | Genomsnittlig | 5 minuter |
| SQL-databas | log_write_percent | Loggen IO-procent | Genomsnittlig | 5 minuter |
| SQL-databas | dtu_consumption_percent | DTU-procent | Genomsnittlig | 5 minuter |
| SQL-databas | Lagring | Totalt antal databasens storlek | Maximalt | 30 minuter |
| SQL-databas | connection_successful | Lyckade anslutningar | Totalt | 10 minuter |
| SQL-databas | connection_failed | Misslyckade anslutningar | Totalt | 10 minuter |
| SQL-databas | blocked_by_firewall | Blockeras av brandvägg | Totalt | 10 minuter |
| SQL-databas | deadlock | Deadlocks | Totalt | 10 minuter |
| SQL-databas | storage_percent | Databasstorlek i procent | Maximalt | 30 minuter |
| SQL-databas | xtp_storage_percent | Minnesintern OLTP lagring percent(Preview) | Genomsnittlig | 5 minuter |
| SQL-databas | workers_percent | Procentsatsen för arbetare | Genomsnittlig | 5 minuter |
| SQL-databas | sessions_percent | Sessioner procent | Genomsnittlig | 5 minuter |
| SQL-databas | dtu_limit | DTU gräns | Genomsnittlig | 5 minuter |
| SQL-databas | dtu_used | DTU används | Genomsnittlig | 5 minuter |
||||||
| Elastisk pool | cpu_percent | CPU-procent | Genomsnittlig | 10 minuter |
| Elastisk pool | physical_data_read_percent | Data IO-procent | Genomsnittlig | 10 minuter |
| Elastisk pool | log_write_percent | Loggen IO-procent | Genomsnittlig | 10 minuter |
| Elastisk pool | dtu_consumption_percent | DTU-procent | Genomsnittlig | 10 minuter |
| Elastisk pool | storage_percent | Lagringsprocent | Genomsnittlig | 10 minuter |
| Elastisk pool | workers_percent | Procentsatsen för arbetare | Genomsnittlig | 10 minuter |
| Elastisk pool | eDTU_limit | eDTU gräns | Genomsnittlig | 10 minuter |
| Elastisk pool | storage_limit | Lagringsgränsen | Genomsnittlig | 10 minuter |
| Elastisk pool | eDTU_used | eDTU används | Genomsnittlig | 10 minuter |
| Elastisk pool | storage_used | Använt lagringsutrymme | Genomsnittlig | 10 minuter |
||||||               
| SQL data warehouse | cpu_percent | CPU-procent | Genomsnittlig | 10 minuter |
| SQL data warehouse | physical_data_read_percent | Data IO-procent | Genomsnittlig | 10 minuter |
| SQL data warehouse | Lagring | Totalt antal databasens storlek | Maximalt | 10 minuter |
| SQL data warehouse | connection_successful | Lyckade anslutningar | Totalt | 10 minuter |
| SQL data warehouse | connection_failed | Misslyckade anslutningar | Totalt | 10 minuter |
| SQL data warehouse | blocked_by_firewall | Blockeras av brandvägg | Totalt | 10 minuter |
| SQL data warehouse | service_level_objective | Servicenivåmålet för databasen | Totalt | 10 minuter |
| SQL data warehouse | dwu_limit | dwu gräns | Maximalt | 10 minuter |
| SQL data warehouse | dwu_consumption_percent | DWU-procent | Genomsnittlig | 10 minuter |
| SQL data warehouse | dwu_used | DWU används | Genomsnittlig | 10 minuter |
||||||


## <a name="next-steps"></a>Nästa steg
* [Få en översikt över Azure övervakning](../monitoring-and-diagnostics/monitoring-overview.md) inklusive typerna av information som du kan samla in och övervaka.
* Lär dig mer om [hur du konfigurerar webhooks i aviseringar](../monitoring-and-diagnostics/insights-webhooks-alerts.md).
* Hämta en [översikt över diagnostikloggar](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) och samla in detaljerade hög frekvens mått på din tjänst.
* Hämta en [översikt över mått samling](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) att kontrollera att tjänsten är tillgänglig och svarstid.
