---
title: "aaaEnable Transparent datakryptering för Stretch Database - Azure | Microsoft Docs"
description: "Aktivera Transparent datakryptering (TDE) för SQL Server Stretch Database på Azure"
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: 1d6bff455030ac8851b2184c1e8097afd61361d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Aktivera Transparent datakryptering (TDE) för Stretch Database på Azure
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av hello databas, tillhörande säkerhetskopior och transaktionsloggfiler vilande utan ändringar toohello programmet.

TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen. Hej databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat. hello inbyggda servercertifikatet är unikt för varje Azure-servern. Microsoft roteras automatiskt dessa certifikat minst var 90: e dag. En allmän beskrivning av TDE finns [Transparent Data kryptering (TDE)].

## <a name="enabling-encryption"></a>Aktivera kryptering
tooenable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:

1. Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)
2. Klicka på hello i hello databasbladet **inställningar** knappen
3. Välj hello **Transparent datakryptering** alternativet![][1]
4. Välj hello **på** inställningen och välj sedan **spara**
   ![][2]

## <a name="disabling-encryption"></a>Om du inaktiverar kryptering
toodisable TDE för en Azure-databas som lagrar data som migrerats från en Stretch-aktiverade SQL Server-databas, hello hello följande saker:

1. Öppna hello databasen i hello [Azure-portalen](https://portal.azure.com)
2. Klicka på hello i hello databasbladet **inställningar** knappen
3. Välj hello **Transparent datakryptering** alternativet
4. Välj hello **av** inställningen och välj sedan **spara**

<!--Anchors-->
[Transparent Data kryptering (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
