---
title: "aaaSQL databasen – automatisk justering | Microsoft Docs"
description: "SQL-databas analyserar SQL-fråga och automatiskt anpassas efter dina behov toouser arbetsbelastning."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: jovanpop
ms.openlocfilehash: 897a8deaabedb6727dc49958c64d0433c5f2e4f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-tuning"></a>Automatisk inställning

Azure SQL Database är en helt hanterad datatjänst som övervakar hello frågor som körs på hello databasen och automatiskt kan förbättra prestanda för hello arbetsbelastning. Azure SQL-databas har en inbyggd intelligens mekanism som automatiskt kan justera och förbättra prestanda för dina frågor genom att dynamiskt anpassa hello databasen tooyour arbetsbelastning. Automatisk justering i Azure SQL Database kan vara något av hello de viktigaste funktionerna som du kan aktivera på Azure SQL Database toooptimize prestanda av dina frågor.

Finns den här artikeln hello anvisningar för[aktivera automatisk justering](sql-database-automatic-tuning-enable.md) med hello Azure-portalen.

## <a name="why-automatic-tuning"></a>Varför automatisk justering?

En av hello huvuduppgifter i klassiska databasadministration övervakar hello arbetsbelastning, identifiera kritiska SQL frågar index som ska läggas till tooimprove prestanda och används sällan index. Azure SQL Database ger detaljerad inblick i hello frågor och index som du behöver toomonitor. Det kan vara svårt att konstant övervaka databasen, särskilt när vi hanterar flera databaser. Hantera ett stort antal databaser kan vara omöjligt toodo effektivt även med alla tillgängliga verktyg och rapporter som ger Azure SQL Database och Azure-portalen. I stället för att övervaka och justera databasen manuellt, kan du delegera vissa av hello övervaka och justera åtgärder tooAzure SQL-databas med hjälp av funktionen för automatisk justering. 


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="how-does-automatic-tuning-work"></a>Hur fungerar automatisk justering arbete?

Azure SQL-databas har en kontinuerlig prestanda och analysverktyg process som hela tiden lär sig hello egenskap hos din arbetsbelastning och identifiera potentiella problem och förbättringar.

![Automatisk justering process](./media/sql-database-automatic-tuning/tuning-process.png)

Den här processen möjliggör för Azure SQL Database toodynamically anpassa tooyour arbetsbelastning genom att hitta vilka index och planer kan förbättra prestanda för din arbetsbelastning och vad index påverka dina arbetsbelastningar. Baserat på dessa resultat, gäller automatisk justering prestandajustering åtgärder som förbättrar prestandan för din arbetsbelastning. Dessutom övervakar kontinuerligt Azure SQL Database prestanda efter varje ändring av automatisk justering tooensure det förbättrar prestandan för din arbetsbelastning. En åtgärd som inte har bättre prestanda återställas automatiskt. Åtgärden är en nyckelfunktion som garanterar att alla ändringar som gjorts av automatisk justering inte minska hello prestandan för din arbetsbelastning.

Det finns två automatisk justering aspekter som är tillgängliga i Azure SQL Database:

 -  **Automatisk indexhantering** som identifierar index som ska läggas till i din databas och index som ska tas bort.
 -  **Plan för automatisk korrigering** (kommer snart, redan finns i SQL Server 2017) som identifierar problematiska planer och korrigeringar SQL planera prestandaproblem.

## <a name="automatic-index-management"></a>Automatisk indexhantering

I Azure SQL Database är indexhantering enkelt eftersom Azure SQL Database lär sig din arbetsbelastning och garanterar att dina data alltid optimalt indexeras. Design för rätt indexet är avgörande för optimala prestanda av din arbetsbelastning och automatisk indexhantering kan hjälpa dig att optimera ditt index. Automatisk indexhantering antingen åtgärda problem med prestanda i felaktigt indexerade databaser eller underhålla och förbättra index för hello befintliga databasschemat. 

### <a name="why-do-you-need-index-management"></a>Varför behöver du indexhantering?

Index påskynda vissa av dina frågor som läser data från hello tabeller. dock kan dessa sakta ned hello frågor som uppdaterar data. Du behöver toocarefully analysera när toocreate ett index och vilka kolumner som du behöver tooinclude i hello index. Några index kan inte behövas efter en stund. Därför behöver du tooperiodically identifiera och släpp hello index som inte tar några fördelar. Om du ignorerar Hej oanvända index, prestanda för hello frågor som uppdaterar data skulle minskas utan en förmån för hello frågor som läser data. Oanvända index också påverka prestandan hello system eftersom ytterligare uppdateringar kräver onödig loggning.

Hitta hello optimala uppsättningen av index som förbättrar prestanda för frågor i hello läsa data från tabeller som har minimal inverkan på uppdateringar kan kräva kontinuerliga och komplex analys.

Azure SQL Database använder inbyggd intelligens och avancerade regler som analysera dina frågor, identifiera index som är optimala för din aktuella arbetsbelastningar och hello index kan tas bort. Azure SQL Database garanterar att du har en minimal uppsättning nödvändiga index som optimerar hello frågor som läser data, hello hello minimera inverkan på andra frågor.

### <a name="how-tooidentify-indexes-that-need-toobe-changed-in-your-database"></a>Hur tooidentify index som behöver toobe ändras i databasen?

Azure SQL Database gör det enkelt att index hanteringen. I stället för hello tråkigt processen för manuell arbetsbelastning analys och index övervakning, Azure SQL Database analyserar din arbetsbelastning, identifierar hello frågor som kan utföras snabbare med ett nytt index och identifierar oanvända eller duplicerade index. Mer information om identifiering av index som ska ändras på [hitta index-rekommendationer i Azure-portalen](sql-database-advisor-portal.md).

### <a name="automatic-index-management-considerations"></a>Automatisk index hanteringsanmärkningar

Om du upptäcker att hello inbyggda regler förbättras databasens prestanda hello, kan du låta Azure SQL-databas som automatiskt hanterar ditt index.

Åtgärder som krävs toocreate nödvändiga index i Azure SQL-databaser kan använda resurser och närvarande påverka arbetsbelastningens prestanda. toominimize hello inverkan för skapande av index på arbetsbelastningsprestanda, Azure SQL Database hittar hello lämpliga tidsfönstret för någon åtgärd för hantering av indexet. Justera åtgärd är uppskjuten om hello databasen måste resurser tooexecute din arbetsbelastning och påbörjas när hello databasen har tillräckligt med oanvända resurser som kan användas för hello underhållsåtgärden. En viktig funktion i automatisk indexhantering är en kontroll av hello åtgärder. När Azure SQL Database skapar eller släpper index, analyserar en övervakningsprocessen prestandan för din arbetsbelastning tooverify att hello åtgärd bättre hello prestanda. Om den inte gör betydande förbättring – hello åtgärd omedelbart har återställts. På så sätt kan säkerställer Azure SQL Database att automatiska åtgärder inte negativt påverka prestandan för din arbetsbelastning. Index som skapats med automatisk justering är transparenta för hello Underhåll på hello underliggande schema. Schemaändringar, till exempel släppa eller byter namn på kolumner blockeras inte av hello förekomst av automatiskt skapade index. Index som skapas automatiskt av Azure SQL Database släpps omedelbart när relaterad tabell eller kolumner har släppts.

## <a name="automatic-plan-choice-correction"></a>Automatisk plan val korrigering

Plan för automatisk korrigering är en ny automatisk justering funktion som lades till i SQL Server 2017 CTP2.0 som identifierar SQL serviceplaner programfel uppstår. Det här alternativet om automatisk justering snart på Azure SQL Database. Mer information på [automatisk justering i SQL Server 2017](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="next-steps"></a>Nästa steg

- tooenable automatisk justering i Azure SQL Database och gör att automatisk justering funktion fullständigt hantera din arbetsbelastning, se [aktivera automatisk justering](sql-database-automatic-tuning-enable.md).
- toouse manuell inställning, kan du granska [justera rekommendationerna i Azure-portalen](sql-database-advisor-portal.md) och tillämpa manuellt hello som förbättra prestanda för dina frågor.
