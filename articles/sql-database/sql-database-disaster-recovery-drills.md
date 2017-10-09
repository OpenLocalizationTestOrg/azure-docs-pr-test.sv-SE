---
title: aaaSQL databasen Haveriberedskap | Microsoft Docs
description: "Lär dig vägledning och bästa praxis för att använda Azure SQL Database tooperform disaster recovery flyttar toohelp behålla din uppdrag affärskritiska program flexibel toofailures och avbrott."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: bf17857a19fdebddf0d4f55e4db3a1b33efb4e8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performing-disaster-recovery-drill"></a><span data-ttu-id="850ff-103">Utför Återställningsgranskning för katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="850ff-103">Performing Disaster Recovery Drill</span></span>
<span data-ttu-id="850ff-104">Du rekommenderas att verifieringen av programmet beredskap för återställningsarbetsflöde utförs med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="850ff-104">It is recommended that validation of application readiness for recovery workflow is performed periodically.</span></span> <span data-ttu-id="850ff-105">Verifierar hello programmets beteende och följderna av att data går förlorade och/eller hello avbrott innebär att redundansväxlingen är bra engineering.</span><span class="sxs-lookup"><span data-stu-id="850ff-105">Verifying hello application behavior and implications of data loss and/or hello disruption that failover involves is a good engineering practice.</span></span> <span data-ttu-id="850ff-106">Det är också ett krav som de flesta branschstandarder som en del av business continuity certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="850ff-106">It is also a requirement by most industry standards as part of business continuity certification.</span></span>

<span data-ttu-id="850ff-107">Utför en katastrofåterställning återställningsgranskning består av:</span><span class="sxs-lookup"><span data-stu-id="850ff-107">Performing a disaster recovery drill consists of:</span></span>

* <span data-ttu-id="850ff-108">Simulering data nivå avbrott</span><span class="sxs-lookup"><span data-stu-id="850ff-108">Simulating data tier outage</span></span>
* <span data-ttu-id="850ff-109">Återställa</span><span class="sxs-lookup"><span data-stu-id="850ff-109">Recovering</span></span>
* <span data-ttu-id="850ff-110">Validera programåterställning integritet post</span><span class="sxs-lookup"><span data-stu-id="850ff-110">Validate application integrity post recovery</span></span>

<span data-ttu-id="850ff-111">Beroende på hur du [utformats för ditt program för affärskontinuitet](sql-database-business-continuity.md), hello arbetsflöde tooexecute hello ökad kan variera.</span><span class="sxs-lookup"><span data-stu-id="850ff-111">Depending on how you [designed your application for business continuity](sql-database-business-continuity.md), hello workflow tooexecute hello drill can vary.</span></span> <span data-ttu-id="850ff-112">Nedan beskrivs hello metodtips utför en katastrofåterställning återställningsgranskning hello gäller Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="850ff-112">Below we describe hello best practices conducting a disaster recovery drill in hello context of Azure SQL Database.</span></span>

## <a name="geo-restore"></a><span data-ttu-id="850ff-113">Geo-återställning</span><span class="sxs-lookup"><span data-stu-id="850ff-113">Geo-restore</span></span>
<span data-ttu-id="850ff-114">tooprevent hello potentiell dataförlust när du utför en katastrofåterställning återställningsgranskning, rekommenderar vi hello ökad använder en testmiljö genom att skapa en kopia av hello produktionsmiljön och använder den tooverify hello programmet redundans arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="850ff-114">tooprevent hello potential data loss when conducting a disaster recovery drill, we recommend performing hello drill using a test environment by creating a copy of hello production environment and using it tooverify hello application’s failover workflow.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="850ff-115">Avbrott simulering</span><span class="sxs-lookup"><span data-stu-id="850ff-115">Outage simulation</span></span>
<span data-ttu-id="850ff-116">toosimulate hello avbrott, kan du ta bort eller byta namn på hello källdatabasen.</span><span class="sxs-lookup"><span data-stu-id="850ff-116">toosimulate hello outage, you can delete or rename hello source database.</span></span> <span data-ttu-id="850ff-117">Detta medför anslutningsfel till programmet.</span><span class="sxs-lookup"><span data-stu-id="850ff-117">This causes application connectivity failures.</span></span>

#### <a name="recovery"></a><span data-ttu-id="850ff-118">Återställning</span><span class="sxs-lookup"><span data-stu-id="850ff-118">Recovery</span></span>
* <span data-ttu-id="850ff-119">Utföra hello geo-återställning av hello-databas till en annan server enligt beskrivningen [här](sql-database-disaster-recovery.md).</span><span class="sxs-lookup"><span data-stu-id="850ff-119">Perform hello geo-restore of hello database into a different server as described [here](sql-database-disaster-recovery.md).</span></span>
* <span data-ttu-id="850ff-120">Ändra hello programmet configuration tooconnect toohello återställda databasen och följ hello [konfigurera en databas efter återställningen](sql-database-disaster-recovery.md) guide toocomplete hello återställning.</span><span class="sxs-lookup"><span data-stu-id="850ff-120">Change hello application configuration tooconnect toohello recovered database and follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="850ff-121">Validering</span><span class="sxs-lookup"><span data-stu-id="850ff-121">Validation</span></span>
* <span data-ttu-id="850ff-122">Fullständig hello detaljnivån genom att verifiera hello integritet efter programåterställning (inklusive anslutningssträngar, inloggningar, grundläggande funktioner testning eller andra verifieringar som en del av standardprogrammet signeringar procedurer).</span><span class="sxs-lookup"><span data-stu-id="850ff-122">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="geo-replication"></a><span data-ttu-id="850ff-123">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="850ff-123">Geo-replication</span></span>
<span data-ttu-id="850ff-124">För en databas som skyddas med geo-replikering hello ökad omfattar Övning planerad redundans toohello sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="850ff-124">For a database that is protected using geo-replication hello drill exercise involves planned failover toohello secondary database.</span></span> <span data-ttu-id="850ff-125">hello säkerställer planerad redundans att hello primära och sekundära databaser i hello är synkroniserade när hello roller växlas.</span><span class="sxs-lookup"><span data-stu-id="850ff-125">hello planned failover ensures that hello primary and hello secondary databases remain in sync when hello roles are switched.</span></span> <span data-ttu-id="850ff-126">Till skillnad från Hej oplanerad växling vid fel, åtgärden inte leder till dataförlust, så hello ökad kan utföras i hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="850ff-126">Unlike hello unplanned failover, this operation does not result in data loss, so hello drill can be performed in hello production environment.</span></span>

#### <a name="outage-simulation"></a><span data-ttu-id="850ff-127">Avbrott simulering</span><span class="sxs-lookup"><span data-stu-id="850ff-127">Outage simulation</span></span>
<span data-ttu-id="850ff-128">toosimulate hello avbrott, kan du inaktivera hello webbprogram eller virtuella datorn ansluten toohello databas.</span><span class="sxs-lookup"><span data-stu-id="850ff-128">toosimulate hello outage, you can disable hello web application or virtual machine connected toohello database.</span></span> <span data-ttu-id="850ff-129">Detta resulterar i anslutningsfel till hello för hello webbklienter.</span><span class="sxs-lookup"><span data-stu-id="850ff-129">This results in hello connectivity failures for hello web clients.</span></span>

#### <a name="recovery"></a><span data-ttu-id="850ff-130">Återställning</span><span class="sxs-lookup"><span data-stu-id="850ff-130">Recovery</span></span>
* <span data-ttu-id="850ff-131">Gör att hello programkonfigurationen i hello DR region punkter toohello f.d. sekundära som blir hello fullt tillgängliga nya primära.</span><span class="sxs-lookup"><span data-stu-id="850ff-131">Make sure hello application configuration in hello DR region points toohello former secondary which becomes hello fully accessible new primary.</span></span>
* <span data-ttu-id="850ff-132">Utföra [planerad redundans](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello sekundära databasen en ny primär</span><span class="sxs-lookup"><span data-stu-id="850ff-132">Perform [planned failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) toomake hello secondary database a new primary</span></span>
* <span data-ttu-id="850ff-133">Följ hello [konfigurera en databas efter återställningen](sql-database-disaster-recovery.md) guide toocomplete hello återställning.</span><span class="sxs-lookup"><span data-stu-id="850ff-133">Follow hello [Configure a database after recovery](sql-database-disaster-recovery.md) guide toocomplete hello recovery.</span></span>

#### <a name="validation"></a><span data-ttu-id="850ff-134">Validering</span><span class="sxs-lookup"><span data-stu-id="850ff-134">Validation</span></span>
* <span data-ttu-id="850ff-135">Fullständig hello detaljnivån genom att verifiera hello integritet efter programåterställning (inklusive anslutningssträngar, inloggningar, grundläggande funktioner testning eller andra verifieringar som en del av standardprogrammet signeringar procedurer).</span><span class="sxs-lookup"><span data-stu-id="850ff-135">Complete hello drill by verifying hello application integrity post recovery (including connection strings, logins, basic functionality testing or other validations part of standard application signoffs procedures).</span></span>

## <a name="next-steps"></a><span data-ttu-id="850ff-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="850ff-136">Next steps</span></span>
* <span data-ttu-id="850ff-137">toolearn om business continuity scenarier finns [kontinuitet scenarier](sql-database-business-continuity.md)</span><span class="sxs-lookup"><span data-stu-id="850ff-137">toolearn about business continuity scenarios, see [Continuity scenarios](sql-database-business-continuity.md)</span></span>
* <span data-ttu-id="850ff-138">toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="850ff-138">toolearn about Azure SQL Database automated backups, see [SQL Database automated backups](sql-database-automated-backups.md)</span></span>
* <span data-ttu-id="850ff-139">toolearn om hur du använder automatisk säkerhetskopiering för återställning finns [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="850ff-139">toolearn about using automated backups for recovery, see [restore a database from hello service-initiated backups](sql-database-recovery-using-backups.md)</span></span>
* <span data-ttu-id="850ff-140">toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="850ff-140">toolearn about faster recovery options, see [active geo-replication](sql-database-geo-replication-overview.md)</span></span>  
