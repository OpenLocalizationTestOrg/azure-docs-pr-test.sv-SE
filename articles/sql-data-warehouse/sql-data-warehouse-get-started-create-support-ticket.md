---
title: "aaaHow toocreate ett supportärende för SQL Data Warehouse | Microsoft Docs"
description: "Hur biljett toocreate stöd i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a>Hur toocreate en biljett för SQL Data Warehouse
Om du har problem med din SQL Data Warehouse, ber vi dig skapa ett supportärende så att våra tekniker kan hjälpa dig.

> [!NOTE] 
> Från och med 2016/12/20 stämmer inte hello resurs hälsokontroll i hello Azure-portalen. Vi arbetar aktivt toofix problemet. 


## <a name="create-a-support-ticket"></a>Skapa ett supportärende
1. Öppna hello [Azure-portalen][Azure portal].
2. Klicka på startsidan för hello hello **hjälp + support** panelen.
   
    ![Hjälp + support](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. Klicka på hello hjälp + Support-bladet, **skapa supportbegäran**.
   
    ![Ny supportbegäran](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Välj hello **begära typen**.
   
    ![Typ av begäran](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Varje SQL Server (t.ex. myserver.database.windows.net) har som standard en **DTU-kvot** på 45 000. Kvoten är helt enkelt en säkerhetsgräns. Du kan öka din kvot genom att skapa ett supportärende och välja *kvot* som hello typ av begäran. toocalculate din DTU måste, multiplicera hello 7.5 av hello totala [DWU] [ DWU] behövs. Till exempel som toohost två DW6000s på en SQL-server kan du begära en DTU-kvot på 90,000.  Du kan visa din aktuella DTU-förbrukning från hello SQL server-bladet i hello-portalen. Både pausas och icke pausas räknas in hello DTU-kvot. 
   > 
   > 
5. Välj hello **prenumeration** att värdar hello databasen med hello problem som du rapporterar.
   
    ![Prenumeration](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Välj **SQL Data Warehouse** som hello resurs.
   
    ![Resurs](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Välj din [Azure-supportplan][Azure support plan].
   
   * Stöd för **fakturerings-, kvot- och prenumerationhantering** finns tillgängligt på alla supportnivåer.
   * **Reparations**-support tillhandahålls via [Utvecklare][Developer], [Standard][Standard], [Professional Direct][Professional Direct] eller [Premier][Premier] support. Reparations problem är problem som kunder upplever när du använder Azure där det finns en rimligen kan antas att Microsoft orsakade hello problem.
   * **Utvecklarhandledning** och **rådgivningstjänster** finns på hello [Professional Direct] [ Professional Direct] och [Premier] [ Premier] supportnivåer. 
     
     Om du har en Premier-supportplan kan du även rapportera SQL Data Warehouse problem på hello [Microsoft Premier-onlineportalen][Microsoft Premier online portal].  Se [Azure-supportplaner] [ Azure support plan] toolearn mer om hello olika stöder planer, inklusive omfattning, svarstider, prissättning, osv.  Vanliga frågor och svar om Azure-support finns på [Vanliga frågor och svar om Azure-support][Azure support FAQs].  
     
     ![Supportplan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Välj hello **problemtyp** och **kategori**. I det här exemplet har vi valt hello problemtyp ”verktygen” och ”klientverktyg” som hello kategori. 
   
    ![Problemtyp-kategori](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Beskriv problemet hello och välj hello nivå av företagsinverkan.
   
    ![Problembeskrivning](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Din **kontaktinformation** för det här supportärendet fylls i automatiskt. Uppdatera den vid behov.
    
    ![Kontaktuppgifter](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Klicka på **skapa** toosubmit hello supportbegäran.

## <a name="monitor-a-support-ticket"></a>Övervaka ett supportärende
När du har skickat hello supportbegäran hello Azure-teamet kommer att kontakta dig. toocheck din begärandestatus och information klickar du på **hantera supportärenden** på hello instrumentpanel.

![Kontrollera status](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Andra resurser
Dessutom kan du ansluta med hello SQL Data Warehouse-communityn på [Stack Overflow] [ Stack Overflow] eller på hello [Azure SQL Data Warehouse MSDN-forum] [ Azure SQL Data Warehouse MSDN forum].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

