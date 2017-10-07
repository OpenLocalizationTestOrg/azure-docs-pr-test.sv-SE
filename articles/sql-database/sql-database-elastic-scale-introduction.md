---
title: aaaScaling ut med Azure SQL Database | Microsoft Docs
description: "Programvara som en tjänst (SaaS)-utvecklare kan enkelt skapa elastiska, skalbara databaser i hello molnet med hjälp av dessa verktyg"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: d15a2e3f-5adf-41f0-95fa-4b945448e184
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: 82a561e07389d8619727a540fa9424248c087eda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-out-with-azure-sql-database"></a>Skala ut med Azure SQL Database
Du kan enkelt skala ut Azure SQL-databaser med hjälp av hello **elastisk databas** verktyg. Dessa verktyg och funktioner kan du använda hello praktiskt taget obegränsade databasen resurser av **Azure SQL Database** toocreate lösningar för transaktionell arbetsbelastningar och särskilt programvara som en tjänst (SaaS)-program. Elastiska databasfunktioner består av hello följande:

* [Klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md): hello klientbiblioteket är en funktion som gör att du toocreate och underhålla delat databaser.  Se [Kom igång med elastiska Databasverktyg](sql-database-elastic-scale-get-started.md).
* [Elastisk databas dela merge tool](sql-database-elastic-scale-overview-split-and-merge.md): flyttar data mellan delat. Detta är användbart för att flytta data från en flera innehavare databas tooa stöd för en innehavare databasen (eller vice versa). Se [elastisk databas delade dokument verktyget kursen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
* [Den elastiska databasen jobb](sql-database-elastic-jobs-overview.md) (förhandsversion): Använd jobb toomanage stort antal Azure SQL-databaser. Enkelt utföra administrativa åtgärder, till exempel schemaändringar, hantering av autentiseringsuppgifter, referens uppdateringar, insamling av prestandadata eller klient (kunden) telemetri samling med hjälp av jobben.
* [Elastisk databasfrågan](sql-database-elastic-query-overview.md) (förhandsversion): gör toorun Transact-SQL-fråga som sträcker sig över flera databaser. Detta gör anslutningen tooreporting verktyg som Excel, PowerBI, Tableau, osv.
* [Elastisk transaktioner](sql-database-elastic-transactions-overview.md): den här funktionen kan du toorun transaktioner som sträcker sig över flera databaser i Azure SQL Database. Elastisk databastransaktioner är tillgängliga för .NET-program med hjälp av ADO .NET och integrera med hello bekant programmering med hello [System.Transaction klasser](https://msdn.microsoft.com/library/system.transactions.aspx).

hello bilden nedan visar en arkitektur som innehåller hello **elastiska databasfunktioner** i relationen tooa samling av databaser.

I den här bilden representerar färger hello databasen scheman. Databaser med hello hello samma färg delar samma schema.

1. En uppsättning **Azure SQL-databaser** finns på Azure med hjälp av arkitekturen för horisontell partitionering.
2. Hej **klientbibliotek för elastisk databas** anges används toomanage en Fragmentera.
3. En delmängd av hello databaser placeras i en **elastisk pool**. (Se [vad är en pool?](sql-database-elastic-pool.md)).
4. En **elastisk databas jobbet** körs schemalagda eller ad hoc-T-SQL-skript mot alla databaser.
5. Hej **för delade sökvägssammanslagning** är används toomove data från en Fragmentera tooanother.
6. Hej **elastisk databasfrågan** kan du toowrite en fråga som omfattar alla databaser i hello Fragmentera uppsättningen.
7. **Elastisk transaktioner** kan du toorun transaktioner som sträcker sig över flera databaser. 

![Elastic Database-verktyg][1]

## <a name="why-use-hello-tools"></a>Varför använda hello verktyg?
Achieving elasticitet och skala för molnprogram har enkla för virtuella datorer och blob-lagring – bara lägga till eller ta bort enheter, eller öka power. Men det har varit en utmaning för tillståndskänsliga databearbetning i relationsdatabaser. Det här utmaningarna i följande scenarier:

* Växande och minska kapacitet för hello relationsdatabas en del av din arbetsbelastning.
* Hantera anslutningar som kan uppstå som påverkar en delmängd av data -, till exempel ett särskilt upptagen end-kund (klient).

Traditionellt berörs scenarier som dessa av investera i större skala servrar toosupport hello databasprogram. Det här alternativet är dock begränsad hello moln där all bearbetning som händer på fördefinierade vanlig hårdvara. I stället ger distribuerar data och bearbeta över flera identiskt strukturerad databaser (en skalbar mönstret kallas ”delning”) ett alternativt tootraditional skala upp metoder, både vad gäller kostnad och elasticitet.

## <a name="horizontal-and-vertical-scaling"></a>Vågräta och lodräta skalning
hello bilden nedan visar hello vågräta och lodräta dimensioner för skalning som hello olika sätt hello elastiska databaser kan skalas.

![Vågräta och lodräta Scaleout][2]

Teckenbredden refererar tooadding eller ta bort databaser i ordning tooadjust kapacitet eller prestandan. Detta kallas även ”skalning”. Horisontell partitionering, där data är partitionerad över en mängd identiskt strukturerade databaser är ett vanligt sätt tooimplement vågräta skalning.  

Lodrät skalning refererar tooincreasing eller minska hello prestandanivåerna för en individuell databas – kallas även ”skala upp”.

De flesta databasprogram för skalbar molnlagring använder en kombination av följande två metoder. En programvara som en tjänst kan till exempel använda vågräta skalning tooprovision nya end-kunder och lodrät skalning tooallow varje end-kund toogrow eller förminskas databasresurser som krävs av hello arbetsbelastning.

* Teckenbredden hanteras via hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md).
* Lodrät skalning åstadkoms med hjälp av Azure PowerShell-cmdlets toochange hello tjänstnivå eller genom att placera databaser i en elastisk pool.

## <a name="sharding"></a>Horisontell partitionering
*Horisontell partitionering* är en teknik toodistribute stora mängder identiskt strukturerade data över ett antal oberoende databaser. Det är särskilt populära med molnet utvecklare som skapar programvara som en tjänst (SAAS)-erbjudanden för slutanvändare och kunder eller företag. End kunderna är ofta hänvisade tooas ”innehavare”. Horisontell partitionering kan krävas av olika orsaker:  

* hello total mängd data är för stor toofit inom hello begränsningar för en enskild databas
* hello transaktion genomströmning av hello övergripande arbetsbelastning överskrider hello funktioner för en enskild databas
* Klienter kan kräva fysiska isolerade från varandra, så separata databaser krävs för varje klient
* Olika avsnitt i en databas kan behöva tooreside i olika geografiska områden för kompatibilitet, prestanda eller geopolitiska orsaker.

I andra scenarier, till exempel införandet av data från distribuerade enheter vara horisontell partitionering används toofill en uppsättning databaser som är ordnade närvarande. En separat databas kan till exempel vara dedikerade tooeach dag eller vecka. I så fall hello horisontell partitionering nyckeln kan vara ett heltal som representerar hello datum (finns i alla rader i hello delat tabeller) och frågor som hämtar information för ett datum måste skickas hello programmet toohello delmängd av databaser som täcker hello intervallet i fråga.

Delning fungerar bäst när varje transaktion i ett program kan vara begränsad tooa enstaka värde för en nyckel för horisontell partitionering. Detta säkerställer att alla transaktioner kommer att lokala tooa viss databas.

## <a name="multi-tenant-and-single-tenant"></a>Flera innehavare och single-klient
Vissa program använder hello enklaste sättet att skapa en separat databas för varje klient. Detta är hello **mönster för enstaka klient horisontell partitionering** som ger isolering, möjlighet för säkerhetskopiering/återställning och resurs skalning på hello Granulariteten för hello-klient. Varje databas är kopplad till en viss klient ID-värde (eller kunden nyckelvärde) med enda innehavare delning, men nyckeln behöver inte alltid finns i själva hello informationen. Det är hello programmets ansvar tooroute varje begäran toohello lämplig databas - och hello klientbiblioteket kan förenkla detta.

![En organisation jämfört med flera innehavare][4]

Andra scenarier packa ihop flera innehavare i databaser i stället för att isolera dem i separata databaser. Det här är en typisk **mönster för flera innehavare horisontell partitionering** - och den styrs av hello faktum att ett program hanterar många mycket små innehavare. I flera innehavare delning är hello rader i hello databastabeller alla utformade toocarry en nyckel som identifierar hello-ID eller horisontell partitionering klientnyckel. Återigen hello programmet nivå är ansvarig för routning en klients begäran toohello rätt databas och detta kan stödjas av hello klientbibliotek för elastisk databas. Säkerhet på radnivå kan dessutom använda toofilter vilka rader som varje klient kan komma åt - mer information finns i [program med flera klienter med elastiska Databasverktyg och säkerhet på radnivå](sql-database-elastic-tools-multi-tenant-row-level-security.md). Distribuera data mellan databaser kan behövas med hello flera innehavare horisontell partitionering mönster och detta möjliggörs med hjälp av hello elastisk databas dela merge tool. toolearn mer om designmönster för SaaS-program med elastiska pooler finns [designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="move-data-from-multiple-toosingle-tenancy-databases"></a>Flytta data från flera innehavare toosingle databaser
När du skapar ett SaaS-program, är typiska toooffer potentiella kunder en utvärderingsversion av hello programvara. I det här fallet är det kostnadseffektiv toouse en databas för flera innehavare för hello data. När en potentiell kund blir en kund, är en enskild klient-databas dock bättre eftersom det ger bättre prestanda. Om kunden hello hade skapat data under utvärderingsperioden hello, använda hello [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) toomove hello data från hello flera innehavare toohello ny enskild klient databas.

## <a name="next-steps"></a>Nästa steg
En exempelapp som visar hello klientbiblioteket finns [Kom igång med elastisk Datababase verktyg](sql-database-elastic-scale-get-started.md).

tooconvert befintliga databaser toouse hello verktyg, se [migrera befintliga databaser tooscale ut](sql-database-elastic-convert-to-use-elastic-tools.md).

toosee hello specifika hello elastisk pool, se [pris- och prestandaöverväganden för en elastisk pool](sql-database-elastic-pool.md), eller skapa en ny pool med [elastiska pooler](sql-database-elastic-pool-manage-portal.md).  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-introduction/tools.png
[2]:./media/sql-database-elastic-scale-introduction/h_versus_vert.png
[3]:./media/sql-database-elastic-scale-introduction/overview.png
[4]:./media/sql-database-elastic-scale-introduction/single_v_multi_tenant.png

