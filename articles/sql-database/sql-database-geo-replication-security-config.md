---
title: "aaaConfigure Azure SQL Database-säkerhet för katastrofåterställning | Microsoft Docs"
description: "Det här avsnittet beskriver säkerhetsaspekter för att konfigurera och hantera säkerhet efter en återställning av databasen eller en växling vid fel tooa sekundär server i hello händelse av ett avbrott för data center eller andra katastrofåterställning"
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: c7c898c9-69d4-4e16-8b7e-720bbb3353dd
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/13/2016
ms.author: sashan
ms.openlocfilehash: c3172568e1b8ad2a53958200aa6cf19b4a9434ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a>Konfigurera och hantera Azure SQL Database-säkerhet för geo-återställning eller växling vid fel 

> [!NOTE]
> [Aktiv geo-replikering](sql-database-geo-replication-overview.md) är nu tillgänglig för alla databaser i alla servicenivåer.
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a>Översikt över autentiseringskraven för katastrofåterställning
Det här avsnittet beskrivs hello autentisering krav tooconfigure och kontroll [aktiv geo-replikering](sql-database-geo-replication-overview.md) och hello steg krävs tooset användaren åtkomst toohello sekundär databas. Här beskrivs också hur Aktivera åtkomst toohello återställda databasen när du använder [geo-återställning](sql-database-recovery-using-backups.md#geo-restore). Mer information om alternativ för återställning finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).

## <a name="disaster-recovery-with-contained-users"></a>Katastrofåterställning med inneslutna användare
Till skillnad från traditionella användare som måste mappas toologins i hello master-databasen, en innesluten användare hanteras helt av själva hello-databasen. Detta har två fördelar. I hello katastrofåterställning kan hello användare fortsätta tooconnect toohello nya primära eller hello databasen återställas med hjälp av geo-återställning utan någon ytterligare konfiguration eftersom hello databasen hanterar hello användare. Det finns också potentiella skalbarhet och prestandafördelar från den här konfigurationen ur inloggningen. Mer information finns i [Oberoende databasanvändare – göra databasen portabel](https://msdn.microsoft.com/library/ff929188.aspx). 

hello huvudsakliga kompromiss är att hantera hello återställningsprocessen i skala mer utmanande. När du har flera databaser att använda hello finns i samma inloggning, underhålla hello autentiseringsuppgifterna med hjälp av kan användare i flera databaser negera hello fördelarna med inneslutna användare. Till exempel kräver hello lösenordsprincip rotation att ändringar görs konsekvent i flera databaser i stället ändra hello lösenord för inloggning hello en gång i hello master-databasen. Därför, om du har flera databaser att använda hello samma användarnamn och lösenord med hjälp av inneslutna användare rekommenderas inte. 

## <a name="how-tooconfigure-logins-and-users"></a>Hur tooconfigure inloggningar och användare
Om du använder inloggningar och användare (i stället för inneslutna användare), måste du utföra ytterligare åtgärder för tooinsure som hello samma inloggningar finns i hello master-databasen. hello beskriver följande avsnitt hello steg ingår och ytterligare överväganden.

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a>Ställ in användaren åtkomst tooa sekundär eller återställda databas
För hello sekundära databasen toobe kan användas som en skrivskyddad sekundär och tooensure tillgång toohello nya primära databasen eller hello databasen återskapats med geo-återställning, hello master måste databasen för hello-målservern ha hello lämplig säkerhetskonfiguration på plats innan hello återställning.

hello specifika behörigheter för varje steg beskrivs senare i det här avsnittet.

Förbereda användaren åtkomst tooa ska sekundär geo-replikering utföras som en del konfigurera geo-replikering. Förbereda användaråtkomst toohello geo återställa databaser ska utföras när som helst när hello ursprungliga servern är online (t.ex. som en del av hello DR-återställningsgranskning).

> [!NOTE]
> Om du inte över geo-återställning tooa server eller som inte har korrekt konfigurerad inloggningar åtkomst tooit kommer att vara begränsade toohello server-administratörskonto.
> 
> 

Konfigurera inloggningar på hello målserver omfattar tre steg som beskrivs nedan:

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a>1. Avgör inloggningar med åtkomst toohello primära databasen:
hello första steget i processen hello är toodetermine vilka inloggningar måste kopieras på hello målserver. Detta åstadkoms med ett par SELECT-uttryck, en i hello logiska master-databasen på källservern för hello och en i hello primära själva databasen.

Endast hello serveradministratör eller en medlem av hello **LoginManager** -serverrollen kan fastställa hello inloggningar på hello källservern med hello följande SELECT-instruktion. 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

Endast en medlem av hello databasrollen db_owner, hello dbo-användaren eller -administratören kan bestämma hello databasen användaren säkerhetsobjekt i hello primära databasen.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a>2. Hitta hello SID för hello inloggningar som identifierades i steg 1:
Du kan mappa hello server-inloggning toodatabase genom att jämföra hello utdata från hello frågor från hello föregående avsnitt och matchande hello SID. Inloggningar som har en databasanvändare med ett matchande SID har användaren åtkomst toothat databas som den primära databas användaren. 

hello följande fråga kan vara används toosee alla huvudobjekt Hej som användare och deras SID i en databas. Endast en medlem av Hej db_owner databasen roll- eller administratör kan köra den här frågan.

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> Hej **INFORMATION_SCHEMA** och **sys** användare har *NULL* SID och hello **gäst** SID är **0x00**. Hej **dbo** SID kan börja med *0x01060000000001648000000000048454*om hello-databasskaparen var hello-administratören i stället för en medlem av **DbManager**.
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a>3. Skapa hello inloggningar på hello målservern:
hello sista steget är toogo toohello målservern eller servrar och generera hello inloggningar med hello lämpliga SID. grundläggande hello-syntaxen är som följer.

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> Om du vill att toogrant användaren åtkomst toohello sekundär, men inte toohello är primär kan göra du det genom att ändra hello användarinloggning på hello primära servern med hjälp av hello följande syntax.
> 
> ALTER INLOGGNINGEN <login name> INAKTIVERA
> 
> INAKTIVERA ändras inte hello lösenord, så du kan aktivera den alltid om det behövs.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du hanterar åtkomst till databasen och inloggningar finns [SQL Database-säkerhet: hantera åtkomst och logga in databassäkerhet](sql-database-manage-logins.md).
* Mer information om oberoende databasanvändare finns [innehöll databasanvändare - göra din databas bärbara](https://msdn.microsoft.com/library/ff929188.aspx).
* Information om hur du använder och konfigurerar aktiv geo-replikering finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)
* Information om hur du använder geo-återställning finns [geo-återställning](sql-database-recovery-using-backups.md#geo-restore)

