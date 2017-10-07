---
title: "aaaPerformance räknare för Fragmentera mappa manager"
description: "ShardMapManager klass- och beroende routning prestandaräknare"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: b090aba0-2e30-454c-96b3-dffa281f539a
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: ddove
ms.openlocfilehash: d24198563d9fa88d12e6c464dbe89bc300e72ca0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-counters-for-shard-map-manager"></a>Prestandaräknare för karthanteraren för shard
Du kan avbilda hello prestanda för en [Fragmentera kartan manager](sql-database-elastic-scale-shard-map-management.md), särskilt när du använder [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md). Räknare skapas med metoderna i hello Microsoft.Azure.SqlDatabase.ElasticScale.Client klass.  

Räknare anges används tootrack hello prestanda för [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) åtgärder. Dessa räknare är tillgängliga i hello Prestandaövervakaren under hello ”elastisk: Fragmentera databashantering” kategori.

**Senaste versionen av hello:** gå för[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Se även [uppgradera en app toouse hello senaste elastisk databas klientbiblioteket](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Krav
* toocreate hello prestandakategorin och räknare, hello användaren måste vara en del av hello lokala **administratörer** på hello-dator som är värd hello program.  
* toocreate en prestanda prestandaräknaren instans och uppdatera hello räknare, hello användaren måste vara medlem i antingen hello **administratörer** eller **Prestandaövervakningsanvändare** grupp. 

## <a name="create-performance-category-and-counters"></a>Skapa prestandakategorin och räknare
toocreate hello räknare anropa hello CreatePeformanceCategoryAndCounters-metoden i hello [ShardMapManagmentFactory klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Endast administratörer kan köra hello-metoden: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Du kan också använda [detta](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) tooexecute hello metod för PowerShell-skript. hello-metoden skapar hello följande prestandaräknare:  

* **Cachelagrade mappningar**: antalet avbildningar som cachelagrats för hello Fragmentera kartan.
* **DDR per sekund**: antal beroende routning dataåtgärder för hello Fragmentera kartan. Den här räknaren uppdateras när ett anrop för[OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) resulterar i en lyckad anslutning toohello mål Fragmentera. 
* **Mappning av sökning träffar/sek**: antalet lyckade cache-sökning åtgärder för avbildningar i hello Fragmentera kartan. 
* **Mappning av sökning missar/sek i cache**: antal misslyckade cache-sökning åtgärder för avbildningar i hello Fragmentera kartan.
* **Mappningar läggs till eller uppdateras i/sek i cache**: frekvens vilka mappningar läggs eller uppdaterats i cacheminnet för hello Fragmentera kartan. 
* **Mappningar tas bort från cache/sek**: hastighet med vilken mappningar tas bort från cachen för hello Fragmentera kartan. 

Prestandaräknare skapas för varje cachelagrade Fragmentera mappning per process.  

## <a name="notes"></a>Anteckningar
hello utlösa följande händelser hello skapandet av hello prestandaräknare:  

* Initieringen av hello [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) med [gärna inläsning](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx)om hello ShardMapManager innehåller alla Fragmentera maps. Dessa inkluderar hello [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) och hello [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) metoder.
* Lyckad sökning på en Fragmentera-mappning (med hjälp av [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) eller [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Har skapats Fragmentera karta med hjälp av CreateShardMap().

hello prestandaräknare kommer att uppdateras av alla cache-åtgärder som utförs på hello Fragmentera kartan och mappningar. Lyckad borttagning av hello Fragmentera karta med DeleteShardMap () reults i borttagning av hello prestandainstans räknare.  

## <a name="best-practices"></a>Bästa praxis
* Skapandet av hello prestandakategorin och räknare bör bara utföras en gång innan hello skapandet av ShardMapManager objektet. Varje körning av hello kommando CreatePerformanceCategoryAndCounters() rensar hello tidigare räknare (förlorar data som rapporteras av alla instanser) och nya skapas.  
* Prestandaräknaren instanser skapas per process. Alla program kraschar eller borttagning av en Fragmentera mappning från hello cache leder borttagning av hello prestandaräknare instanser.  

### <a name="see-also"></a>Se även
[Översikt över funktioner i elastisk databas](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

