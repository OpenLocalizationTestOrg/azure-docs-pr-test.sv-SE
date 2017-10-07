---
title: aaaManaging utskalat moln databaser | Microsoft Docs
description: "Visar hello elastiska jobb databastjänsten"
metakeywords: azure sql database elastic databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 6fa47cf2-1162-4534-a206-6e2d95b78580
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: b6d330cd712421b8cba781e835830772e6e5b77e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-scaled-out-cloud-databases"></a>Hantera databaser som skalats ut moln
toomanage utskalat delat databaser, hello **elastisk databas jobb** funktion (förhandsgranskning) gör du tooreliably köra ett skript för Transact-SQL (T-SQL) över flera databaser, inklusive:

* ett egendefinierat mängd databaser (beskrivs nedan)
* alla databaser i en [elastisk pool](sql-database-elastic-pool.md)
* en uppsättning Fragmentera (skapat med hjälp av [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)). 

## <a name="documentation"></a>Dokumentation
* [Installera hello elastiska jobb](sql-database-elastic-jobs-service-installation.md). 
* [Kom igång med elastisk databas jobb](sql-database-elastic-jobs-getting-started.md).
* [Skapa och hantera jobb med hjälp av PowerShell](sql-database-elastic-jobs-powershell.md).
* [Skapa och hantera skaländras ut Azure SQL-databaser](sql-database-elastic-jobs-getting-started.md)

**Den elastiska databasen jobb** är för närvarande en kund-värdbaserad Azure Cloud Service som gör att hello körning av ad hoc- och schemalagda administrativa uppgifter, som kallas **jobb**. Med jobb kan enkelt och tillförlitligt sätt hantera stora grupper med Azure SQL-databaser genom att köra Transact-SQL-skript tooperform administrativa åtgärder. 

![Jobb-tjänsten för elastisk databas][1]

## <a name="why-use-jobs"></a>Varför använda jobb?
**Hantera**

Enkelt göra schemaändringar, hantering av autentiseringsuppgifter, referens uppdateringar, insamling av prestandadata eller klient (kunden) telemetri samling.

**Rapporter**

Samla in data från en samling av Azure SQL-databaser till en enda måltabell.

**Minska omkostnader**

Vanligtvis måste du ansluta tooeach databasen oberoende i ordning toorun Transact-SQL-uttryck eller utföra andra administrativa uppgifter. Ett jobb hanterar hello uppgiften att loggning i tooeach databas i hello målgruppen. Du också definiera, underhålla och bevara Transact-SQL-skript toobe utförs i en grupp med Azure SQL-databaser.

**Redovisning**

Jobben körs hello skript och logg hello status för körning för varje databas. Du kan också få automatiska försök igen när fel uppstår.

**Flexibilitet**

Definiera anpassade grupper med Azure SQL-databaser och definiera scheman för att köra ett jobb.

> [!NOTE]
> Hello Azure-portalen, bara en uppsättning funktioner begränsad tooSQL Azure elastiska pooler är tillgänglig i. Använd hello PowerShell APIs tooaccess hello fullständig uppsättning aktuella funktionalitet.
> 
> 

## <a name="applications"></a>Program
* Utföra administrativa uppgifter, till exempel att distribuera ett nytt schema.
* Uppdatera data produkten Referensinformation vanliga över alla databaser. Eller scheman automatiska uppdateringar varje veckodag efter timmar.
* Återskapa index tooimprove frågeprestanda. hello återskapa kan vara konfigurerade tooexecute över en mängd databaser med jämna mellanrum, som vid låg belastning.
* Samla in frågeresultat från en uppsättning databaser till en central tabell på grundval av pågående. Prestandafrågor kan köras kontinuerligt och konfigurerat tootrigger ytterligare aktiviteter toobe utförs.
* Utför längre databearbetning frågor som körs i ett stort utbud av databaser, till exempel hello samling kunden telemetri. Samlas in resultaten till en enda måltabell för ytterligare analys.

## <a name="elastic-database-jobs-end-to-end"></a>Den elastiska databasen jobb: slutpunkt till slutpunkt
1. Installera hello **elastisk databas jobb** komponenter. Mer information finns i [installerar elastisk databas jobb](sql-database-elastic-jobs-service-installation.md). Om hello installationen misslyckas, se [hur toouninstall](sql-database-elastic-jobs-uninstall.md).
2. Använd hello PowerShell APIs tooaccess fler funktioner, till exempel skapa anpassade databasen samlingar, lägga till scheman och/eller samla in resultatet anger. Använd hello portal för enkel installation och skapa/övervakning av jobb begränsad tooexecution mot en **elastisk pool**. 
3. Skapa krypterade autentiseringsuppgifter för jobbkörningen och [Lägg till hello användare (eller roll) tooeach databas i gruppen hello](sql-database-security-overview.md).
4. Skapa en idempotent T-SQL-skript som kan köras mot varje databas i hello-gruppen. 
5. Följ dessa steg toocreate jobb med hjälp av hello Azure-portalen: [skapa och hantera jobb för elastisk databas](sql-database-elastic-jobs-create-and-manage.md). 
6. Eller Använd PowerShell-skript: [skapa och hantera en SQL-databas för elastisk databas jobb med hjälp av PowerShell (förhandsgranskning)](sql-database-elastic-jobs-powershell.md).

## <a name="idempotent-scripts"></a>Idempotent skript
hello skript måste vara [idempotent](https://en.wikipedia.org/wiki/Idempotence). Enkelt uttryckt innebär ”idempotent” att om hello skriptet lyckas och kör igen, hello samma resultat sker. Ett skript kan misslyckas på grund av nätverksproblem tootransient. I så fall försöker automatiskt hello jobbet körs hello skript ett förinställt antal gånger innan desisting. Ett skript för idempotent har hello samma resultat även om har utförts två gånger. 

En enkel skräpposten är tootest för hello förekomsten av ett objekt innan du skapar den.  

    IF NOT EXIST (some_object)
    -- Create hello object 
    -- If it exists, drop hello object before recreating it.

På liknande sätt kan ett skript måste vara kan tooexecute har genom att logiskt och motverka eventuella villkor som hittas.

## <a name="failures-and-logs"></a>Fel och loggar
Om ett skript misslyckas efter flera försök hello jobb loggar hello fel och fortsätter. När ett jobb har upphört kan (vilket innebär en körning mot alla databaser i gruppen hello) du kontrollera listan över misslyckade försök. hello-loggarna ger information toodebug felaktiga skript. 

## <a name="group-types-and-creation"></a>Gruppen typer och skapa
Det finns två typer av grupper: 

1. Fragmentera uppsättningar
2. Anpassade grupper

Fragmentera uppsättning grupper skapas med hjälp av hello [elastisk Databasverktyg](sql-database-elastic-scale-introduction.md). När du skapar en Fragmentera set grupp databaser läggs till eller tas bort från hello gruppen automatiskt. Till exempel blir en ny Fragmentera automatiskt i hello gruppen när du lägger till den toohello Fragmentera kartan. Sedan kan du köra ett jobb mot hello grupp.

Anpassade grupper på Hej andra sidan, strikt definierad. Du måste uttryckligen lägga till eller ta bort databaser från anpassade grupper. Om en databas i gruppen hello bryts försöker hello jobbet toorun hello skript mot hello databasen, vilket resulterar i ett eventuella fel. Grupper som skapats med hello Azure-portalen för närvarande är anpassade grupper. 

## <a name="components-and-pricing"></a>Komponenter och prissättning
hello fungerar följande komponenter tillsammans toocreate en Azure-molntjänst som aktiverar ad hoc-körning av administrativa jobb. hello-komponenter installeras och konfigureras automatiskt under installationen, i din prenumeration. Du kan identifiera hello tjänster som alla har hello samma automatiskt genererade namnet. hello namn är unikt och består av hello prefixet ”edj” följt av 21 slumpmässigt genererat tecken.

* **Molntjänsten Azure**: elastisk databas jobb (förhandsgranskning) levereras som en kund-värdbaserad Azure Cloud service tooperform körning av hello begärda uppgifter. Från hello-portalen hello-tjänsten distribueras och hanteras i Microsoft Azure-prenumerationen. hello distribueras standardtjänsten körs med hello minst två arbetsroller för hög tillgänglighet. hello standardstorleken på varje worker-rollen (ElasticDatabaseJobWorker) körs på en A0-instans. Priser, finns i [molntjänster priser](https://azure.microsoft.com/pricing/details/cloud-services/). 
* **Azure SQL Database**: hello tjänsten använder Azure SQL-databas kallas hello **databasen** toostore alla hello jobbet metadata. hello standard-tjänstnivå är en S0. Priser, finns i [priser för SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* **Azure Service Bus**: ett Azure Service Bus är för samordning av hello arbetet inom hello Azure Cloud Service. Se [Service Bus priser](https://azure.microsoft.com/pricing/details/service-bus/).
* **Azure Storage**: ett Azure Storage-konto används toostore diagnostiska utdata loggning i hello händelse som ett problem som kräver ytterligare felsökning (se [aktiverar diagnostik i Azure-molntjänster och virtuella datorer](../cloud-services/cloud-services-dotnet-diagnostics.md)). Priser, finns i [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-elastic-database-jobs-work"></a>Så här fungerar jobb för elastisk databas
1. En Azure SQL Database är tilldelad en **databasen** som lagrar alla data för metadata och tillstånd.
2. databasen för hello används av hello **jobbet service** tooexecute toolaunch och spåra jobb.
3. Två olika roller kommunicerar med databasen för hello: 
   * Domänkontrollant: Avgör vilka jobb som kräver uppgifter tooperform hello begärda jobbet och försök misslyckade jobb genom att skapa nya jobbuppgifter.
   * Jobb för körning av aktiviteten: Utför hello jobbuppgifter.

### <a name="job-task-types"></a>Uppgiften jobbtyper
Det finns flera typer av jobbuppgifter som utför körningen av jobb:

* ShardMapRefresh: Frågor hello Fragmentera kartan toodetermine alla hello-databaser som används som delar
* ScriptSplit: Delningar hello skriptet över 'Gå'-instruktioner i batchar
* ExpandJob: Skapar underordnade jobb för varje databas från ett jobb som riktar sig till en grupp databaser
* ScriptExecution: Kör ett skript mot en viss databas med angivna autentiseringsuppgifter
* Dacpac: Gäller en DACPAC tooa viss databas med viss autentiseringsuppgifter

## <a name="end-to-end-job-execution-work-flow"></a>Jobb för slutpunkt till slutpunkt-arbetsflöde för körning
1. Med hello portalen eller hello PowerShell API kan infogas ett jobb i hello **databasen**. hello jobbet begär körningen av en Transact-SQL-skript mot en grupp databaser med hjälp av autentiseringsuppgifter.
2. hello controller identifierar hello nytt jobb. Uppgifter i jobbet har skapats och köras toosplit hello skript och toorefresh hello grupp databaser. Slutligen ett nytt jobb skapas och körs tooexpand hello jobbet och skapa nya underordnade jobb där varje underordnad jobb är angivna tooexecute hello Transact-SQL-skript mot en individuell databas i hello gruppen.
3. hello controller identifierar hello skapa underordnade jobb. För varje jobb hello domänkontrollant skapar och utlöser ett jobb uppgiften tooexecute hello skript mot en databas. 
4. När alla aktiviteter i jobbet har slutförts, uppdaterar hello controller hello jobb tooa slutfört tillstånd. 
   När som helst under jobbkörningen hello PowerShell API kan använda tooview hello aktuell status för jobbkörningen. Alla tider returneras av hello PowerShell APIs representeras i UTC. Om du vill, vara en begäran om att avbryta initierade toostop ett jobb. 

## <a name="next-steps"></a>Nästa steg
[Installera komponenter för hello](sql-database-elastic-jobs-service-installation.md), sedan [skapa och Lägg till en logg i tooeach databas i hello grupp databaser](sql-database-manage-logins.md). toofurther förstå jobbet skapande och hantering, se [skapa och hantera jobb för elastisk databas](sql-database-elastic-jobs-create-and-manage.md). Se även [komma igång med elastisk databas jobb](sql-database-elastic-jobs-getting-started.md).

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-overview/elastic-jobs.png
<!--anchors-->


