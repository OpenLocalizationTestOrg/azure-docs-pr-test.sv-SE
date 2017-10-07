---
title: aaaTransparent kryptering av Data i SQL Data Warehouse (Portal) | Microsoft Docs
description: Transparent datakryptering (TDE) i SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 8233886ecf170844104e0d1459e2a829cafa9b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Kom igång med Transparent Data kryptering (TDE) i SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Översikt över säkerheten](sql-data-warehouse-overview-manage-security.md)
> * [Autentisering](sql-data-warehouse-authentication.md)
> * [Kryptering (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Kryptering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Nödvändig behörighet
tooenable Transparent Data kryptering (TDE), du måste vara administratör eller medlem i hello dbmanager roll.

## <a name="enabling-encryption"></a>Aktivera kryptering
tooenable TDE för ett SQL Data Warehouse gör hello nedan:

1. Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)
2. Klicka på hello i hello databasbladet **inställningar** knappen
3. Välj hello **Transparent datakryptering** alternativet![][1]
4. Välj hello **på** inställning![][2]
5. Välj **spara**
   ![][3]  

## <a name="disabling-encryption"></a>Om du inaktiverar kryptering
toodisable TDE för ett SQL Data Warehouse gör hello nedan:

1. Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)
2. Klicka på hello i hello databasbladet **inställningar** knappen
3. Välj hello **Transparent datakryptering** alternativet![][1]
4. Välj hello **av** inställning![][4]
5. Välj **spara**
   ![][5]  

## <a name="encryption-dmvs"></a>Kryptering av DMV: er
Kryptering kan bekräftas med hello följande av DMV: er:

* [sys.Databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
