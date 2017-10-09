---
title: aaaTransparent kryptering av Data i SQL Data Warehouse (T-SQL) | Microsoft Docs
description: Transparent datakryptering (TDE) i SQL Data Warehouse (T-SQL)
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: barbkess
editor: 
ms.assetid: 8ccefef3-1308-41ee-b336-5e491d1098ae
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 3894431c76f14b217f3a6b9a42dbf2f4d216bad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-transparent-data-encryption-tde"></a>Kom igång med Transparent Data kryptering (TDE)
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
Följ dessa steg tooenable TDE för ett SQL Data Warehouse:

1. Ansluta toohello *master* databasen på värdservern för hello hello database med en inloggning som administratör eller medlem i hello **dbmanager** roll i hello master-databasen
2. Köra hello följande instruktion tooencrypt hello-databasen.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Om du inaktiverar kryptering
Följ dessa steg toodisable TDE för ett SQL Data Warehouse:

1. Ansluta toohello *master* databasen med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen
2. Köra hello följande instruktion tooencrypt hello-databasen.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [!NOTE]
> Att måste återuppta ett pausat SQL Data Warehouse innan du ändrar toohello TDE inställningar.
> 
> 

## <a name="verifying-encryption"></a>Verifiering av kryptering
tooverify krypteringsstatus för en SQL Data Warehouse gör hello nedan:

1. Ansluta toohello *master* eller instans database med en inloggning som administratör eller medlem i hello **dbmanager** roll i hello master-databasen
2. Köra hello följande instruktion tooencrypt hello-databasen.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Ett resultat av ```1``` anger en krypterad databas ```0``` anger en icke-krypterad databas.

## <a name="encryption-dmvs"></a>Kryptering av DMV: er
* [sys.Databases][sys.databases] 
* [sys.dm_pdw_nodes_database_encryption_keys][sys.dm_pdw_nodes_database_encryption_keys]

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->
