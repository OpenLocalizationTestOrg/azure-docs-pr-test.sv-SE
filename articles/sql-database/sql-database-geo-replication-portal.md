---
title: 'Azure-portalen: SQL Database geo-replikering | Microsoft Docs'
description: "Konfigurera geo-replikering för Azure SQL Database i hello Azure-portalen och initiera redundans"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: d0b29822-714f-4633-a5ab-fb1a09d43ced
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/06/2016
ms.author: carlrab
ms.openlocfilehash: 09cbbdb040f36c42593e3be87ce6db2238f36656
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-hello-azure-portal-and-initiate-failover"></a>Konfigurera aktiv geo-replikering för Azure SQL Database i hello Azure-portalen och initiera redundans

Den här artikeln beskrivs hur du tooconfigure aktiv geo-replikering för SQL-databas i hello [Azure-portalen](http://portal.azure.com) och tooinitiate redundans.

tooinitiate redundans med hello Azure-portalen finns [starta en planerad eller oplanerad växling för Azure SQL Database med hello Azure-portalen](sql-database-geo-replication-portal.md).

tooconfigure aktiv geo-replikering med hjälp av hello Azure-portalen, behöver du hello följande resurser:

* En Azure SQL database: hello primära databasen som du vill tooreplicate tooa olika geografiska region.

> [!Note]
Aktiv geo-replikering måste vara mellan databaser i hello samma prenumeration.

## <a name="add-a-secondary-database"></a>Lägg till en sekundär databas
hello skapa följande en ny sekundär databas i ett partnerskap geo-replikering.  

tooadd en sekundär databas måste du vara hello prenumerationsägare eller Medägare.

hello sekundära databasen har samma namn som den primära databasen i hello hello och har, som standard hello samma servicenivå. hello sekundära databasen kan vara en enskild databas eller en databas i en elastisk pool. Mer information finns i [tjänstnivåer](sql-database-service-tiers.md).
Hello sekundära skapas och dirigeras börjar data när replikering från hello primära toohello nya sekundära databasen.

> [!NOTE]
> Hello kommandot misslyckas om hello partner databas redan finns (till exempel till följd av avslutar relationen för en tidigare geo-replikering).
> 

1. I hello [Azure-portalen](http://portal.azure.com), bläddra toohello databasen som du vill tooset för geo-replikering.
2. Hello SQL-databas på sidan Välj **georeplikering**, och välj sedan hello region toocreate hello sekundär databas. Du kan välja en annan region än hello region värd hello primära databasen, men vi rekommenderar hello [parad region](../best-practices-availability-paired-regions.md).
   
    ![Konfigurera geo-replikering](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Välj eller konfigurera hello-servern och prisnivå för hello sekundär databas.
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Du kan också kan du lägga till en sekundär tooan elastisk databaspool. toocreate hello sekundär databas i en pool, klickar du på **elastisk pool** och välj en pool på hello målserver. En pool måste redan finnas på hello målserver. Det här arbetsflödet kan inte skapa en pool.
5. Klicka på **skapa** tooadd hello sekundär.
6. hello sekundär databas skapas och hello seeding processen påbörjas.
   
    ![Konfigurera sekundära](./media/sql-database-geo-replication-portal/seeding0.png)
7. När hello seeding processen är klar visas dess status hello sekundär databas.
   
    ![Seeding klar](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Initiera redundans

hello sekundära databasen kan vara avstängda toobecome hello primära.  

1. I hello [Azure-portalen](http://portal.azure.com), bläddra toohello primära databasen hello geo-replikering tillsammans.
2. På bladet för hello SQL-databas, Välj **alla inställningar** > **georeplikering**.
3. I hello **SEKUNDÄRSERVRAR** listan, Välj hello-databas som du vill toobecome hello nya primära och på **redundans**.
   
    ![växling vid fel](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Klicka på **Ja** toobegin hello redundans.

hello växlar omedelbart hello sekundär databas i hello primära rollen. 

Det finns en kort period under vilken båda databaserna är inte tillgängliga (på hello ordning 0 too25 sekunder) när hello roller växlas. Om hello primära databasen har flera sekundära databaser, hello kommandot automatiskt hello Omkonfigurerar andra sekundärservrar tooconnect toohello nya primära servern. hello hela åtgärden bör ta mindre än en minut toocomplete under normala omständigheter. 

> [!NOTE]
> Det här kommandot har utformats för snabb återställning av databasen hello vid ett strömavbrott. Utlöser redundans utan datasynkronisering (tvingas redundans).  Om hello primära är online och acceptera transaktioner när hello-kommandot har utfärdats dataförluster kan uppstå. 
> 
> 

## <a name="remove-secondary-database"></a>Ta bort sekundära databas
Den här åtgärden avbryter permanent hello replikering toohello sekundära databas och ändringar hello roll hello sekundära tooa vanlig skrivskyddad databas. Om hello anslutningen toohello sekundära databasen är skadad, hello kommando lyckas men hello sekundära har inte blir oskyddad tills när anslutningen har återställts.  

1. I hello [Azure-portalen](http://portal.azure.com), bläddra toohello primära databasen hello geo-replikering tillsammans.
2. Hello SQL-databas på sidan Välj **georeplikering**.
3. I hello **SEKUNDÄRSERVRAR** listan, Välj hello-databasen som du vill använda tooremove från hello geo-replikering partnerskap.
4. Klicka på **Replikeringsstopp**.
   
    ![Ta bort sekundär](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. En bekräftelse öppnas. Klicka på **Ja** tooremove hello-databas från hello geo-replikering partnerskap. (Ange det tooa oskyddad databas inte en del av all replikering.)

## <a name="next-steps"></a>Nästa steg
* toolearn mer information om aktiv geo-replikering, se [aktiv geo-replikering](sql-database-geo-replication-overview.md).
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).

