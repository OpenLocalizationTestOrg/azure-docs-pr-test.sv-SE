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
# <a name="configure-and-manage-azure-sql-database-security-for-geo-restore-or-failover"></a><span data-ttu-id="76a64-103">Konfigurera och hantera Azure SQL Database-säkerhet för geo-återställning eller växling vid fel</span><span class="sxs-lookup"><span data-stu-id="76a64-103">Configure and manage Azure SQL Database security for geo-restore or failover</span></span> 

> [!NOTE]
> <span data-ttu-id="76a64-104">[Aktiv geo-replikering](sql-database-geo-replication-overview.md) är nu tillgänglig för alla databaser i alla servicenivåer.</span><span class="sxs-lookup"><span data-stu-id="76a64-104">[Active geo-replication](sql-database-geo-replication-overview.md) is now available for all databases in all service tiers.</span></span>
>  

## <a name="overview-of-authentication-requirements-for-disaster-recovery"></a><span data-ttu-id="76a64-105">Översikt över autentiseringskraven för katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="76a64-105">Overview of authentication requirements for disaster recovery</span></span>
<span data-ttu-id="76a64-106">Det här avsnittet beskrivs hello autentisering krav tooconfigure och kontroll [aktiv geo-replikering](sql-database-geo-replication-overview.md) och hello steg krävs tooset användaren åtkomst toohello sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="76a64-106">This topic describes hello authentication requirements tooconfigure and control [active geo-replication](sql-database-geo-replication-overview.md) and hello steps required tooset up user access toohello secondary database.</span></span> <span data-ttu-id="76a64-107">Här beskrivs också hur Aktivera åtkomst toohello återställda databasen när du använder [geo-återställning](sql-database-recovery-using-backups.md#geo-restore).</span><span class="sxs-lookup"><span data-stu-id="76a64-107">It also describes how enable access toohello recovered database after using [geo-restore](sql-database-recovery-using-backups.md#geo-restore).</span></span> <span data-ttu-id="76a64-108">Mer information om alternativ för återställning finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="76a64-108">For more information on recovery options, see [Business Continuity Overview](sql-database-business-continuity.md).</span></span>

## <a name="disaster-recovery-with-contained-users"></a><span data-ttu-id="76a64-109">Katastrofåterställning med inneslutna användare</span><span class="sxs-lookup"><span data-stu-id="76a64-109">Disaster recovery with contained users</span></span>
<span data-ttu-id="76a64-110">Till skillnad från traditionella användare som måste mappas toologins i hello master-databasen, en innesluten användare hanteras helt av själva hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="76a64-110">Unlike traditional users, which must be mapped toologins in hello master database, a contained user is managed completely by hello database itself.</span></span> <span data-ttu-id="76a64-111">Detta har två fördelar.</span><span class="sxs-lookup"><span data-stu-id="76a64-111">This has two benefits.</span></span> <span data-ttu-id="76a64-112">I hello katastrofåterställning kan hello användare fortsätta tooconnect toohello nya primära eller hello databasen återställas med hjälp av geo-återställning utan någon ytterligare konfiguration eftersom hello databasen hanterar hello användare.</span><span class="sxs-lookup"><span data-stu-id="76a64-112">In hello disaster recovery scenario, hello users can continue tooconnect toohello new primary database or hello database recovered using geo-restore without any additional configuration, because hello database manages hello users.</span></span> <span data-ttu-id="76a64-113">Det finns också potentiella skalbarhet och prestandafördelar från den här konfigurationen ur inloggningen.</span><span class="sxs-lookup"><span data-stu-id="76a64-113">There are also potential scalability and performance benefits from this configuration from a login perspective.</span></span> <span data-ttu-id="76a64-114">Mer information finns i [Oberoende databasanvändare – göra databasen portabel](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="76a64-114">For more information, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span> 

<span data-ttu-id="76a64-115">hello huvudsakliga kompromiss är att hantera hello återställningsprocessen i skala mer utmanande.</span><span class="sxs-lookup"><span data-stu-id="76a64-115">hello main trade-off is that managing hello disaster recovery process at scale is more challenging.</span></span> <span data-ttu-id="76a64-116">När du har flera databaser att använda hello finns i samma inloggning, underhålla hello autentiseringsuppgifterna med hjälp av kan användare i flera databaser negera hello fördelarna med inneslutna användare.</span><span class="sxs-lookup"><span data-stu-id="76a64-116">When you have multiple databases that use hello same login, maintaining hello credentials using contained users in multiple databases may negate hello benefits of contained users.</span></span> <span data-ttu-id="76a64-117">Till exempel kräver hello lösenordsprincip rotation att ändringar görs konsekvent i flera databaser i stället ändra hello lösenord för inloggning hello en gång i hello master-databasen.</span><span class="sxs-lookup"><span data-stu-id="76a64-117">For example, hello password rotation policy requires that changes be made consistently in multiple databases rather than changing hello password for hello login once in hello master database.</span></span> <span data-ttu-id="76a64-118">Därför, om du har flera databaser att använda hello samma användarnamn och lösenord med hjälp av inneslutna användare rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="76a64-118">For this reason, if you have multiple databases that use hello same user name and password, using contained users is not recommended.</span></span> 

## <a name="how-tooconfigure-logins-and-users"></a><span data-ttu-id="76a64-119">Hur tooconfigure inloggningar och användare</span><span class="sxs-lookup"><span data-stu-id="76a64-119">How tooconfigure logins and users</span></span>
<span data-ttu-id="76a64-120">Om du använder inloggningar och användare (i stället för inneslutna användare), måste du utföra ytterligare åtgärder för tooinsure som hello samma inloggningar finns i hello master-databasen.</span><span class="sxs-lookup"><span data-stu-id="76a64-120">If you are using logins and users (rather than contained users), you must take extra steps tooinsure that hello same logins exist in hello master database.</span></span> <span data-ttu-id="76a64-121">hello beskriver följande avsnitt hello steg ingår och ytterligare överväganden.</span><span class="sxs-lookup"><span data-stu-id="76a64-121">hello following sections outline hello steps involved and additional considerations.</span></span>

### <a name="set-up-user-access-tooa-secondary-or-recovered-database"></a><span data-ttu-id="76a64-122">Ställ in användaren åtkomst tooa sekundär eller återställda databas</span><span class="sxs-lookup"><span data-stu-id="76a64-122">Set up user access tooa secondary or recovered database</span></span>
<span data-ttu-id="76a64-123">För hello sekundära databasen toobe kan användas som en skrivskyddad sekundär och tooensure tillgång toohello nya primära databasen eller hello databasen återskapats med geo-återställning, hello master måste databasen för hello-målservern ha hello lämplig säkerhetskonfiguration på plats innan hello återställning.</span><span class="sxs-lookup"><span data-stu-id="76a64-123">In order for hello secondary database toobe usable as a read-only secondary database, and tooensure proper access toohello new primary database or hello database recovered using geo-restore, hello master database of hello target server must have hello appropriate security configuration in place before hello recovery.</span></span>

<span data-ttu-id="76a64-124">hello specifika behörigheter för varje steg beskrivs senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="76a64-124">hello specific permissions for each step are described later in this topic.</span></span>

<span data-ttu-id="76a64-125">Förbereda användaren åtkomst tooa ska sekundär geo-replikering utföras som en del konfigurera geo-replikering.</span><span class="sxs-lookup"><span data-stu-id="76a64-125">Preparing user access tooa geo-replication secondary should be performed as part configuring geo-replication.</span></span> <span data-ttu-id="76a64-126">Förbereda användaråtkomst toohello geo återställa databaser ska utföras när som helst när hello ursprungliga servern är online (t.ex. som en del av hello DR-återställningsgranskning).</span><span class="sxs-lookup"><span data-stu-id="76a64-126">Preparing user access toohello geo-restored databases should be performed at any time when hello original server is online (e.g. as part of hello DR drill).</span></span>

> [!NOTE]
> <span data-ttu-id="76a64-127">Om du inte över geo-återställning tooa server eller som inte har korrekt konfigurerad inloggningar åtkomst tooit kommer att vara begränsade toohello server-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="76a64-127">If you fail over or geo-restore tooa server that does not have properly configured logins, access tooit will be limited toohello server admin account.</span></span>
> 
> 

<span data-ttu-id="76a64-128">Konfigurera inloggningar på hello målserver omfattar tre steg som beskrivs nedan:</span><span class="sxs-lookup"><span data-stu-id="76a64-128">Setting up logins on hello target server involves three steps outlined below:</span></span>

#### <a name="1-determine-logins-with-access-toohello-primary-database"></a><span data-ttu-id="76a64-129">1. Avgör inloggningar med åtkomst toohello primära databasen:</span><span class="sxs-lookup"><span data-stu-id="76a64-129">1. Determine logins with access toohello primary database:</span></span>
<span data-ttu-id="76a64-130">hello första steget i processen hello är toodetermine vilka inloggningar måste kopieras på hello målserver.</span><span class="sxs-lookup"><span data-stu-id="76a64-130">hello first step of hello process is toodetermine which logins must be duplicated on hello target server.</span></span> <span data-ttu-id="76a64-131">Detta åstadkoms med ett par SELECT-uttryck, en i hello logiska master-databasen på källservern för hello och en i hello primära själva databasen.</span><span class="sxs-lookup"><span data-stu-id="76a64-131">This is accomplished with a pair of SELECT statements, one in hello logical master database on hello source server and one in hello primary database itself.</span></span>

<span data-ttu-id="76a64-132">Endast hello serveradministratör eller en medlem av hello **LoginManager** -serverrollen kan fastställa hello inloggningar på hello källservern med hello följande SELECT-instruktion.</span><span class="sxs-lookup"><span data-stu-id="76a64-132">Only hello server admin or a member of hello **LoginManager** server role can determine hello logins on hello source server with hello following SELECT statement.</span></span> 

    SELECT [name], [sid] 
    FROM [sys].[sql_logins] 
    WHERE [type_desc] = 'SQL_Login'

<span data-ttu-id="76a64-133">Endast en medlem av hello databasrollen db_owner, hello dbo-användaren eller -administratören kan bestämma hello databasen användaren säkerhetsobjekt i hello primära databasen.</span><span class="sxs-lookup"><span data-stu-id="76a64-133">Only a member of hello db_owner database role, hello dbo user, or server admin, can determine all of hello database user principals in hello primary database.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

#### <a name="2-find-hello-sid-for-hello-logins-identified-in-step-1"></a><span data-ttu-id="76a64-134">2. Hitta hello SID för hello inloggningar som identifierades i steg 1:</span><span class="sxs-lookup"><span data-stu-id="76a64-134">2. Find hello SID for hello logins identified in step 1:</span></span>
<span data-ttu-id="76a64-135">Du kan mappa hello server-inloggning toodatabase genom att jämföra hello utdata från hello frågor från hello föregående avsnitt och matchande hello SID.</span><span class="sxs-lookup"><span data-stu-id="76a64-135">By comparing hello output of hello queries from hello previous section and matching hello SIDs, you can map hello server login toodatabase user.</span></span> <span data-ttu-id="76a64-136">Inloggningar som har en databasanvändare med ett matchande SID har användaren åtkomst toothat databas som den primära databas användaren.</span><span class="sxs-lookup"><span data-stu-id="76a64-136">Logins that have a database user with a matching SID have user access toothat database as that database user principal.</span></span> 

<span data-ttu-id="76a64-137">hello följande fråga kan vara används toosee alla huvudobjekt Hej som användare och deras SID i en databas.</span><span class="sxs-lookup"><span data-stu-id="76a64-137">hello following query can be used toosee all of hello user principals and their SIDs in a database.</span></span> <span data-ttu-id="76a64-138">Endast en medlem av Hej db_owner databasen roll- eller administratör kan köra den här frågan.</span><span class="sxs-lookup"><span data-stu-id="76a64-138">Only a member of hello db_owner database role or server admin can run this query.</span></span>

    SELECT [name], [sid]
    FROM [sys].[database_principals]
    WHERE [type_desc] = 'SQL_USER'

> [!NOTE]
> <span data-ttu-id="76a64-139">Hej **INFORMATION_SCHEMA** och **sys** användare har *NULL* SID och hello **gäst** SID är **0x00**.</span><span class="sxs-lookup"><span data-stu-id="76a64-139">hello **INFORMATION_SCHEMA** and **sys** users have *NULL* SIDs, and hello **guest** SID is **0x00**.</span></span> <span data-ttu-id="76a64-140">Hej **dbo** SID kan börja med *0x01060000000001648000000000048454*om hello-databasskaparen var hello-administratören i stället för en medlem av **DbManager**.</span><span class="sxs-lookup"><span data-stu-id="76a64-140">hello **dbo** SID may start with *0x01060000000001648000000000048454*, if hello database creator was hello server admin instead of a member of **DbManager**.</span></span>
> 
> 

#### <a name="3-create-hello-logins-on-hello-target-server"></a><span data-ttu-id="76a64-141">3. Skapa hello inloggningar på hello målservern:</span><span class="sxs-lookup"><span data-stu-id="76a64-141">3. Create hello logins on hello target server:</span></span>
<span data-ttu-id="76a64-142">hello sista steget är toogo toohello målservern eller servrar och generera hello inloggningar med hello lämpliga SID.</span><span class="sxs-lookup"><span data-stu-id="76a64-142">hello last step is toogo toohello target server, or servers, and generate hello logins with hello appropriate SIDs.</span></span> <span data-ttu-id="76a64-143">grundläggande hello-syntaxen är som följer.</span><span class="sxs-lookup"><span data-stu-id="76a64-143">hello basic syntax is as follows.</span></span>

    CREATE LOGIN [<login name>]
    WITH PASSWORD = <login password>,
    SID = <desired login SID>

> [!NOTE]
> <span data-ttu-id="76a64-144">Om du vill att toogrant användaren åtkomst toohello sekundär, men inte toohello är primär kan göra du det genom att ändra hello användarinloggning på hello primära servern med hjälp av hello följande syntax.</span><span class="sxs-lookup"><span data-stu-id="76a64-144">If you want toogrant user access toohello secondary, but not toohello primary, you can do that by altering hello user login on hello primary server by using hello following syntax.</span></span>
> 
> <span data-ttu-id="76a64-145">ALTER INLOGGNINGEN <login name> INAKTIVERA</span><span class="sxs-lookup"><span data-stu-id="76a64-145">ALTER LOGIN <login name> DISABLE</span></span>
> 
> <span data-ttu-id="76a64-146">INAKTIVERA ändras inte hello lösenord, så du kan aktivera den alltid om det behövs.</span><span class="sxs-lookup"><span data-stu-id="76a64-146">DISABLE doesn’t change hello password, so you can always enable it if needed.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="76a64-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76a64-147">Next steps</span></span>
* <span data-ttu-id="76a64-148">Mer information om hur du hanterar åtkomst till databasen och inloggningar finns [SQL Database-säkerhet: hantera åtkomst och logga in databassäkerhet](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="76a64-148">For more information on managing database access and logins, see [SQL Database security: Manage database access and login security](sql-database-manage-logins.md).</span></span>
* <span data-ttu-id="76a64-149">Mer information om oberoende databasanvändare finns [innehöll databasanvändare - göra din databas bärbara](https://msdn.microsoft.com/library/ff929188.aspx).</span><span class="sxs-lookup"><span data-stu-id="76a64-149">For more information on contained database users, see [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx).</span></span>
* <span data-ttu-id="76a64-150">Information om hur du använder och konfigurerar aktiv geo-replikering finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="76a64-150">For information about using and configuring active geo-replication, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>
* <span data-ttu-id="76a64-151">Information om hur du använder geo-återställning finns [geo-återställning](sql-database-recovery-using-backups.md#geo-restore)</span><span class="sxs-lookup"><span data-stu-id="76a64-151">For information about using geo-restore, see [geo-restore](sql-database-recovery-using-backups.md#geo-restore)</span></span>

