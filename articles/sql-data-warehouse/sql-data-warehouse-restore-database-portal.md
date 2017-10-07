---
title: aaaRestore Azure SQL Data Warehouse (Azure portal) | Microsoft Docs
description: "Azure portal uppgifter för att återställa Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cb225d2a21b61acab70a51b69c266f8d3ffacc9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Återställa Azure SQL Data Warehouse (portal)
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
I den här artikeln får du lära dig hur toorestore Azure SQL Data Warehouse med hjälp av hello Azure-portalen.

## <a name="before-you-begin"></a>Innan du börjar
**Kontrollera DTU-kapacitet.** Varje instans av SQL Data Warehouse finns i en SQL-server (till exempel myserver.database.windows.net) som har en kvot som standard data genomströmning enhet (DTU). Innan du kan återställa SQL Data Warehouse, kan du kontrollera att SQLServer har tillräckligt återstående DTU-kvot för hello-databas som du återställa. toolearn hur toocalculate DTU-kvot eller toorequest mer dtu: er Se [begär en ändring för DTU-kvot][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Återställa en databas för aktiva eller pausats
toorestore en-databas:

1. Logga in toohello [Azure-portalen][Azure portal].
2. I hello vänster och välj **Bläddra**, och välj sedan **SQL-servrar**.

    ![Välj Bläddra > SQL-servrar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Hitta din server och markerar den.

    ![Markera din server](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Hitta hello instans av SQL Data Warehouse som du vill toorestore från och markerar den.

    ![Välj hello instans av SQL Data Warehouse toorestore](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Överst hello i hello Data Warehouse-bladet, Välj **återställa**.

    ![Välj återställning](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Ange ett nytt **databasnamnet**.
7. Välj hello senaste **återställningspunkt**.

   Kontrollera att du väljer hello senaste återställningspunkt. Eftersom återställningspunkter visas i Coordinated Universal Time (UTC), kanske inte hello standardalternativet hello senaste återställningspunkt.

      ![Välj en återställningspunkt](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Välj **OK**.
9. hello databasen återställningsprocessen startar och du kan använda **meddelanden** toomonitor hello processen.

> [!NOTE]
> När hello återställning har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Återställa en borttagen databas
toorestore en borttagen databas:

1. Logga in toohello [Azure-portalen][Azure portal].
2. I hello vänster och välj **Bläddra**, och välj sedan **SQL-servrar**.

    ![Välj Bläddra > SQL-servrar](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Hitta din server och markerar den.

    ![Markera din server](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Bläddra nedåt toohello **Operations** avsnitt på bladet för din server.
5. Välj hello **bort databaser** panelen.

    ![Välj hello borttagna databaser sida vid sida](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Välj hello bort databasen som du vill toorestore.

    ![Välj en databas toorestore](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Ange ett nytt **databasnamnet**.

    ![Lägga till ett namn för hello-databas](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Välj **OK**.
9. hello databasen återställningsprocessen startar och du kan använda **meddelanden** toomonitor hello processen.

> [!NOTE]
> tooconfigure databasen efter hello återställning har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Nästa steg
toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version, läsa hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
