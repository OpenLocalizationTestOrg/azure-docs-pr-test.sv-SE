---
title: "aaaEnable Transparent datakryptering för Stretch Database TSQL - Azure | Microsoft Docs"
description: "Aktivera Transparent datakryptering (TDE) för SQL Server Stretch Database på Azure TSQL"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 27753d91-9ca2-4d47-b34d-b5e2c2f029bb
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: anvang
ms.openlocfilehash: a9ba23649656fb344480d79438a1115f0eb353bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure (Transact-SQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello programmet.

TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen. Hej databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat. hello inbyggda servercertifikatet är unikt för varje Azure-servern. Microsoft roteras automatiskt dessa certifikat minst var 90: e dag. En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].

## <a name="enabling-encryption"></a>Aktivera kryptering
tooenable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:

1. Ansluta toohello *master* databasen på hello Azure server värd hello-databas med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen
2. Köra hello följande instruktion tooencrypt hello-databasen.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Om du inaktiverar kryptering
toodisable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:

1. Ansluta toohello *master* databasen med en inloggning som är administratör eller medlem i hello **dbmanager** roll i hello master-databasen
2. Köra hello följande instruktion tooencrypt hello-databasen.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

## <a name="verifying-encryption"></a>Verifiering av kryptering
tooverify krypteringsstatus för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:

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

<!--Anchors-->
[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->
