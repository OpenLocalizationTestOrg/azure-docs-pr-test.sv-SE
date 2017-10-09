---
title: aaaRestore en Azure SQL Data Warehouse (REST-API) | Microsoft Docs
description: "REST API-uppgifterna för att återställa en Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: cf6678d71aafff71b1ea715f447e41e25f20d1b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Återställa en Azure SQL Data Warehouse (REST-API)
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

I den här artikeln du lära dig hur hello toorestore en Azure SQL Data Warehouse med hjälp av REST API.

## <a name="before-you-begin"></a>Innan du börjar
**Kontrollera DTU-kapacitet.** Varje SQL Data Warehouse finns på en SQLServer (t.ex. myserver.database.windows.net) som har en standard DTU-kvot.  Innan du kan återställa en SQL Data Warehouse, kontrollera att hello din SQLServer har tillräckligt återstående DTU-kvot för hello databasen som återställs. toolearn hur toocalculate DTU behövs eller toorequest mer DTU finns [begär en ändring för DTU-kvot][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Återställa en databas för aktiva eller pausats
toorestore en-databas:

1. Hämta hello lista över återställningspunkter i databasen med hjälp av hello Återställningspunkter för Get-databasen igen.
2. Börja din återställning med hjälp av hello [begäran om att skapa databasen återställning] [ Create database restore request] igen.
3. Spåra hello status för återställningspunkten med hjälp av hello [databasen Åtgärdsstatus] [ Database operation status] igen.

> [!NOTE]
> När hello återställningen har slutförts kan du konfigurera din återställda databas genom att följa [konfigurera databasen efter återställningen][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Återställa en borttagen databas
toorestore en borttagen databas:

1. Lista över alla dina återställningsbara borttagna databaser med hjälp av hello [lista återställningsbara bort databaser] [ List restorable dropped databases] igen.
2. Hämta hello information för hello bort databasen du vill använda toorestore med hjälp av hello [Get återställningsbara släppa databasen] [ Get restorable dropped database] igen.
3. Börja din återställning med hjälp av hello [begäran om att skapa databasen återställning] [ Create database restore request] igen.
4. Spåra hello status för återställningspunkten med hjälp av hello [databasen Åtgärdsstatus] [ Database operation status] igen.

> [!NOTE]
> tooconfigure databasen efter hello återställningen har slutförts, se [konfigurera databasen efter återställningen][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Nästa steg
Läs toolearn om funktionerna för verksamhetskontinuitet hello Azure SQL Database version hello [översikt över verksamhetskontinuitet i Azure SQL Database][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
